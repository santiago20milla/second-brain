# ING - Campaign Optimization
Consulting project done in [[Lynx Analytics]] from March 2020 to February 2021.
## Glossary
- Activation Ratio: % of active users over total on-boarded users (with account).
- KYC: Know Your Customer.
- CLP: Customer Life-cycle Project.
## STAR
#storytelling #interviews
### Situation
Second project done with Lynx. ING is entering with a Digital Bank into Philippines. They don't have the expected results, users are downloading their app but not putting money on. They want to grow no matter how their Activation Ratio (AR). Moh (from France) is a very angry man who is in charge of making this happen in Philippines. Covid has just started and at first we can travel every week to Philippines, from Monday to Friday. We do this for 2-3 months. When we travel, we work easily 14-17 hours per day. First project under Gyorgy and I realize he is not sane, he can't see people in the eye while he talks and has very low evidences of empathy. He pushes us to work up to 2-3 in the morning and wake up at 8 am. After a big presentation in which we did very good he took us to Starbucks for lunch (get a quick sandwich and go back quickly). When Covid starts, we stop going to Philippines and move on to the office in Singapore. Where we start building this [[Campaign Optimization Framework]] which determines which users get a promotion based on probability of them going for that promotion and their probability of being active (have higher balance). Joren joins the company and pushes even further with a optimization for the values (no longer only for the users). We finish the round of campaigns (6 campaigns) because we are bringing so many new users that the KYC operations team and customer interaction team cannot handle the number of users that are coming in. We stop the campaigns and start with other minor tasks. Project finished with Ben (PM), Tuck (DE), Paolo (DS) for a moment and Richard (DS) for the rest. 
### Task
Phase 1: Convince them of the methodology.
Phase 2: Increase the activation ratio from 26% to 45% (44% achieved) in 6 months.
Phase 3: Ad-Hoc various analysis. NPS (Net Promoter Score) evaluation.
### Actions
- Promo optimization framework:
	- Feature engineering (SQL) #machine_learning 
	- Define label (SQL)
	- [[Classification Model]] (Python - [[XG Boost]])
	- Select the optimized users and randomly assign them to reward, no reward and no communication groups (SQL)
	- Optimize the promotions to be sent to each group (Excel - Solver, Python) #statistics 
	- Evaluate the performance of the promotions by comparing it to the control groups (SQL, Excel, Python)
	- Evaluate the performance comparing churn of promo takers vs controls (and natural takers) #statistics
- Dashboards (SQL + Excel)
- Ad-hoc analysis (greatest part of our time in the late project).
- Queue analysis and simulation for KYC Workload and Customer Service with the previously developed [[Queue Simulator]] #queueing_theory.
- NPS evaluation:
	- Classification model (Python - xgboost)
	- [[Customer Happinex Index]] to simulate the effect of the features on NPS (what happens to the NPS when I change the time for KYC).
### Results
 Increased the Activation Ratio to 44%. So many new users were coming that we had to stop the CLP Promotions. Too many users were coming in and we were giving problems to the KYC Operations and Customer Service teams. Really good relationship with customer and grew from DS to Senior DS in Lynx. Improved code skills and got to understand better xgboost #machine_learning and shap #machine_learning. Started to become the leader of the team and had to teach a lot to Richard (new DS). 
### Reflections
Second round of proving myself that my worth is a lot higher than what I expected. Slightly disappointed by the level of other Data Scientist (even senior levels) in my company, maybe in general it is like this. Lot's to improve on the Python coding, and learning from Heiko (ING side) to keep things in order and don't over complicate things. Important skill to break down complex analysis into simple steps. Very important iterative process of the [[Classification Model]] that starts with a [[Minimum Value Product]] and improves after each iteration #design_thinking #agility.
