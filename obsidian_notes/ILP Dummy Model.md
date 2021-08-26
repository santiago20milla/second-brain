# ILP Dummy Model
#operations_research #python
```
import pandas as pd
import numpy as np
import pulp
import itertools
from sklearn.impute import SimpleImputer
import tqdm

def retro_dictify(frame, overlapping_leafs=False):
    '''
	Transform a dataframe into a dictionary
	Params:
	frame: pandas dataframe
	overlapping_leafs: boolean that indicates if there are multiple leafs per index
	'''
    result = {}
	for lst in frame.values:
	    leaf = result
		for path in lst[:-2]:
		    leaf = leaf.setdefault(path, {})
		if overlapping_leafs:
		    leaf.setdefault(lst[-2], list()).append(lst[-1])
		else:
		    leaf[lst[-2]] = lst[-1]
	return result
			
def get_assorted_month_range(x):
    '''
	Returns the months assorted given a placement for a period
	Params:
	x: placement of an item in a specific month for a specific period
	'''
	return list(
	    range(
		    x['placement_month'].astype('int'),
	        x['months_placed'].astype('int') + x['placement_month'].astype('int')
			)
		)

class Optimizer(object):

    def __init__(self, name, **kwargs):
	    self.model = pulp.LpProblem(name=name, sense=pulp.LpMaximize)
		self.placements = kwargs.get('placements')
		self.stores = kwargs.get('stores')
		self.months = kwargs.get('months')
		self.pc9_list = kwargs.get('pc9_list')
		self.pc5_list = kwargs.get('pc5_list')
		self.placement_df = kwargs.get('placement_df')
		self.prod_hierarchy_df = kwargs.get('prod_hierarchy')
		self.store_hierarchy_df = kwargs.get('store_hierarchy')
		self.corresponding_months = kwargs.get('corresponding_months')
		self.monthly_pc9_contribution = kwargs.get('monthly_pc9_contribution')
		self.create_placement_contribution_df()
		self.create_contribution_dict()
		self.create_placement_tuples() 
		self.create_decision_vars()
		self.create_objective_function()
		self.warm_start = 0 
		
	def create_placement_contribution_df(self):
	    '''
		Transform monthly contribution to placement contribution
		'''
		median_imputer = SimpleImputer(missing_values=np.nan, strategy='median')
		
		df = self.placement_df.merge(
		    self.monthly_pc9_contribution,
			how='left',
			left_on=['month_assorted','store_code'],
			right_on=['month','store_code']
		).reset_index(drop=True)
		
		df[self.pc9_list] = median_imputer.fit_transform(df[self.pc9_list])
		
		melted_df = df.melt(
		    id_vars=[
			    'placement',
				'placement_month',
				'months_placed',
				'months_assorted',
				'store_code'
				],
			value_vars=self.pc9_list,
			value_name='pc9_contribution',
			var_name='pc9_code'
		)
		
		self.placement_contribution = melted_df\
		.groupby(['placement','store_code','pc9_code'])\
		.agg({'pc9_contribution':'sum'})\
		.reset_index(drop=False)
		
		self.placement_contribution = self.placement_contribution.merge(
		    self.prod_hierarchy_df[['pc9','pc5']],
			how='left',
			left_on='pc9_code',
			right_on='pc9'
		).drop(columns='pc9')\
		.rename(columns={'corresponding_pc5';'pc5_code'})
		
	def create_contribution_dict(self):
	    '''
		Transform the contribution dataframe into a dictionary
		'''
		self.contribution_dict = rectro_dictify(
		    self.placement_contribution[[
			    'placement','store_code','pc9_code','pc9_contribution'
		    ]])
			
	def create_placement_tuples(self):
	    self.placed = [
		    (p,store,pc9) 
			for p in self.placements 
			for store in stores 
			for pc9 in pc9_list
		]
		
	def create_decision_vars(self):
	    self.vars = pulp.LpVariable.dicts(
		    'place',
			(self.placements, self.stores, self.pc9_list),
			0,
			None,
			pulp.LpBinary
		)
		
		self.aux_pc5 = pulp.LpVariable.dicts(
		    'pc5',
			(self.months, self.stores, self.pc5_list),
			0,
			None,
			pulp.LpBinary
		)
	
	def create_objective_function(self):
	    self.model += pulp.lpSum([
		    self.vars[p][store][pc9]*self.contribution_dict[p][store][pc9]
			for (p,store,pc9) in self.placed
		]), 'Sum_of_PC9_Contribution'
		
	def set_monthly_capacity_constraints(self, max_store_capacity, min_store_capacity):
	    '''
		Add monthly total capacity constraints per store
		Every store has to be present in both maximum an dminimum capacity dictionaries
		current code doesn't let capacity to change over months
		Params:
		max_store_capacity: dictionary of maximum monthly capacity per store
		min_store_capacity: dictionary of minimum monthly capacity per store
		'''
		corresponding_months_dict = retro_dictify(
		    self.corresponding_months,
			overlapping_leafs=True
		)
		
		for s in self.stores:
		    for m in self.months:
			    self.model += pulp.lpSum([
				    self.vars[p][s][pc9]
					for p in self.placements
					for pc9 in self.pc9_list
					if p in corresponding_months_dict[m]
				]) <= max_store_capacity[s]
			    
				self.model += pulp.lpSum([
				    self.vars[p][s][pc9]
					for p in self.placements
					for pc9 in self.pc9_list
					if p in corresponding_months_dict[m]
				]) >= min_store_capacity[s]
				
	def set_seasonal_capacity_constraints(self, max_store_capacity, min_store_capacity):
	    '''
		Add seasonal total capacity constraints per store
		Every store has to be present in both maximum and minimum capacity dictionaries
		Params:
		max_store_capacity: dictionary of maximum seasonal capacity per store
		min_store_capacity: dictionary of minimum seasonal capacity per store
		'''
		for s in self.stores:
		    self.model += pulp.lpSum([
			    self.vars[p][s][pc9]
				for p in self.placements
				for pc9 in self.pc9_list
			]) <= max_store_capacity[s]
			
			self.model += pulp.lpSum([
			    self.vars[p][s][pc9]
				for p in self.placements
				for pc9 in self.pc9_list
			]) >= min_store_capacity[s]
			
	def set_monthly_department_capacity_constraints(
	    self, max_pc9_department_month, min_pc9_department_month
	):
	    '''
		Add monthly maximum number of pc9s per department per store
		Every store has to be present in both maximum and minimum capacity dictionaries
		Not yet differentiated by month or store
		Params:
		max_pc9_department_month: dictionary of maximum monthly capacity per store per department
		min_pc9_department_month: dictionary of minimum monthly capacity per store per department
		'''
		
		corresponding_months_dict = retro_dictify(
		    self.corresponding_months,
			overlapping_leafs=True
		)
		pc9_dept = retro_dictify(
		    self.prod_hierarchy_df[['prod_department','pc9']],
			overlapping_leafs=True
		)
		
		for s in self.stores:
		    for m in self.months:
			    for dep in self.prod_hierarchy_df['prod_department'].unique():
				    self.model += pulp.lpSum([
					    self.vars[p][s][pc9]
						for p in self.placements
						for pc9 in self.pc9_list
						if (p in corresponding_months_dict[m]
						    and 
							pc9 in pc9_dept[dep])
					]) <= max_pc9_department_month[dep]
					
					self.model += pulp.lpSum([
					    self.vars[p][s][pc9]
						for p in self.placements
						for pc9 in self.pc9_list
						if (p in corresponding_months_dict[m]
						    and 
							pc9 in pc9_dept[dep])
					]) >= min_pc9_department_month[dep]
					
	def set_minimum_prod_assortment(self, min_pc9_total_assortment):
	    '''
		Add seasonal minimum number of assortments per pc9 across all stores
		Every product has to be present (fill with 0s those with no minimum)
		Params:
		min_pc9_total_assortment: dictionary of minimum total assortments per pc9
		'''
		corresponding_months_dict = retro_dictify(
		    self.corresponding_months,
			overlapping_leafs=True
		)
		
		for pc9 in self.pc9_list:
		    for m in self.months:
			    self.model += pulp.lpSum([
				    self.vars[p][s][pc9]
					for p in self.placements
					for s in self.stores
					if p in corresponding_months_dict[m]
				])*len(corresponding_months_dict[m]) >= min_pc9_total_assortment[pc9]
				
	def set_minimin_options_per_pc5(self, min_options_per_pc5_per_dept):
	    '''
		Add minimum number of options (pc9s) per pc5 per store per month if pc5 is present
		Params:
		min_options_per_pc5_per_dept: dictionary of minimum number of pc9s per pc5
		'''
		
		corresponding_months_dict = retro_dictify(
		    self.corresponding_months,
			overlapping_leafs=True
		)
		
		pc5_options = self.prod_hierarchy_df.copy()
		pc5_options['min_options'] = pc5_options['prod_department']\
		.map(min_options_per_pc5_per_dept)
		
		pc5_min_options = retro_dictify(pc5_options[['pc5','min_options']])
		
		pc5_grp = retro_dictify(
		    self.prod_hierarchy_df[['pc5','pc9']],
			overlapping_leafs=True
		)
		
		max_options = max([len(value) for key, value in pc5_grp.items()])
		
		pbar = tqdm.tqdm(total=len(self.stores)*len(self.months)*len(self.pc5_list))
		
		for s in self.stores:
		    for m in self.months:
			    for pc5 in self.pc5_list:
				    self.model += pulp.lpSum([
					    self.vars[p][s][pc9]
						for pc9 in self.pc9_list
						for p in self.placements
						if (pc9 in pc5_grp[pc5]
						    and 
							p in corresponding_months_dict[m])
					]) >= pc5_min_options[pc5] * self.aux_pc5[m][s][pc5]
					
					self.model += pulp.lpSum([
					    self.vars[p][s][pc9]
						for pc9 in self.pc9_list
						for p in self.placements
						if (pc9 in pc5_grp[pc5]
						    and 
							p in corresponding_months_dict[m])
					]) <= max_options * self.aux_pc5[m][s][pc5]
					
					pbar.update(1)
					
	    pbar.close()
					
```