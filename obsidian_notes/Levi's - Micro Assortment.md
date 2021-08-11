# Levi's - Micro Assortment
Consulting project done in [[Lynx Analytics]] from June 2021 to October 2021 (last project done in Lynx). 
#retail #optimization 
## Glossary
- Assortment: product present in a store in a month.
- Placement: action of assorting in a store for an specific amount of months.
- PC9: product code 9 digits (digits denote detail).
- PC5: product code 5 digits (less detailed than PC9).
## STAR
#storytelling #interviews
### Situation
Fourth and last project done in Lynx. Working as a rented-out DS for the Levi's Global Analytics team (felt good that they picked me to work with them). Worked during the first months of living in Hamburg and was complicated to manage the time differences. Worked with Serena as a Project Manager, Stoddard as a Data Analyst, Tasnim as a Data Scientist, Aino as a Data Scientist, Inna as a Product Owner, and Ben as a Senior Data Scientist and Tech Lead (pretty big team). 
### Task
Use the learnings from previous Microassortment  projects done by Lynx in China-Japan and Europe and apply them in the stores from North America (US + CA). Send by the end of September a table of the assortment of PC9 products by store by month for the first 6 months of 2022. 
### Actions
- Cluster via a [[Clustering Algorithm]] the stores according to their internal data (Point of Sales transactions) into 3 store types (small enough to be business driven).
- Create a [[Machine Learning]] model that predicts which clusters do stores belong to. Better if it's via a [[Regression Model]] that predicts one internal feature rather than a [[Classification Model]] that predicts cluster belonging.
- Determine the final cluster for each store with and without POS data (new stores).
- Create a [[Gross Margin]] estimation model via a [[Regression Model]] that considers [[Product Cannibalization]]. 
- Build an [[Integer Linear Programming]] model that picks the best combination of placements for each store (the work-stream that I was in charge of).
### Results
Built a productionalizable system that would define the optimal set of placements per product per store considering all the business constraints. The most important ILP constraints where the [[Russian Doll Constraints]] and [[Flow Constraints]]. Both were managed by transforming the decision variables from month assorted to placement of x amount of months. The impact was measured to the estimated increase of last years' [[Gross Margin]] real vs estimated (running the assortment optimization on last year's data).
### Reflections
Not my favorite project. Was too isolated from the rest of the team due to hour difference so most of the decisions and team work was done with me. I was just assigned a part of the project and didn't have much interaction, which made me lose my motivation and just doing the basic requirements without putting any effort. Also, pushed me to quit and move on to [[Delivery Hero]]. Also, got to experience how unorganized teams are a waste of resources and can kill a project. Ben Saunders was in charge of leading the team and he was disorganized and prone to micro-manage each part of the project which led to slower development and very low quality results (specially in business communication).