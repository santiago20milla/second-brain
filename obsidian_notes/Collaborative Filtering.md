# Collaborative Filtering
Approach of [[Recommendation Systems]] where historic preference between users and items is used to predict the preference between other users and items. 
Can be user-based, where the similarity of users is used to predict the preference of a new item. Or Item-based, where the similarity of items is used to predict the preference of a new user. 
It is relatively easy to implement, but has problems on handling with scalability (big systems explode in permutations), sparsity (not enough recommendations in a complex system) and cold start (users and items don't have enough history to make accurate recommendations). 