# Product Cannibalization
Multiple products affect negatively each other's demand when assorted (in a store) at the same time. Modeled by a [[Geometric Series]] in the form of:

![[Pasted image 20210927170456.png]]

With three components:
 - a: Base target value.  Can be the 1st ranked item's target variable.
 - r: Decay rate, how does the target variable flattens out for that particular set of items.
 - n: Number of options that set of items include.

To include this in a demand forecast, it is necessary to break the model into two pieces. First predict the A value (rank the items within each group and find the first ranked item) and then fit the R value via a curve fitting algorithm (scipy.optimize.curve_fit).

Used in [[Levi's - Micro Assortment]] project to predict the demand of a set of products.