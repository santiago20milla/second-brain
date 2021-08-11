# DBS - Queue Estimation
Consulting project done in [[Lynx Analytics]] from October 2018 to February 2019. 
#queueing_theory #fintech #banking #statistics
## Glossary
 - T1: Time to enter queue
 - T2: Time to enter system 
 - T3: Time to exit system
 - RWT (Relative Waiting Time): % of the maximum queue time that a user experienced.
## STARR
### Situation
First project worked in Lynx. Started theoretically when I was in Barcelona working mainly in the app and _getting ready._ Driven mainly by Gabor and worked with Ben (PM), Zoltan (Senior DS) and me (DS). DBS wants to improve the UX for their users by controlling the queues in their ATMs. No clear way of calculating the Queue size and waiting times. 
### Task
Find a way to estimate the presence of queues in the ATMs. Based on .txt files with the transactions log and the transactions db (from the perspective of the bank). 
### Actions
 - Physically survey some sites (with previously designed app).
- Build an ETL (Extract Transform and Load) notebook to process the .txt logs that are saved by the ATMs. 
 - Compare ATMs logs results with export from the DB and to clean up non existent transactions from the ATMs log.
 - Manually clean the survey results and match the survey records with the cleaned transactions of the ATMs logs.
 - Find the optimal time between arrivals (time for a user to walk from the queue to the ATM and start a transaction) and use it to combine the usage into usage chains.
 - Use the T1 (time to enter the queue) to determine the statistical parameters of the queue (make sure the queue follows a standard MM1 parameters).
 - Build a [[Queue Simulator]] using python and simulate thousands data points of similar queues.
 - Calculate the Relative Waiting Time as the percentage of possible waiting time (maximum waiting time is when user arrives at exactly the same time as previous user, minimum waiting time is when user arrives at the time that the previous user finishes).
 - Determine the distribution of the Relative Waiting Time based on user's position in the queue chain and queue chain size (how many users went one after the other without a stop). 
	 - Relative Waiting Time of users at the extremes of the queue chains follow a uniform distribution.
	 - Relative Waiting Time of the rest of the users follow an approximate to Power [[Probability Distribution]] depending on position of that user on the chain and the chain length.
 - Calculate the Relative Waiting Time for *all* transactions in the past month, compute the respective T1 and calculate the Waiting Time and Queue Size.
 - Use the Waiting Time and Queue Size to compare with results of survey done by DBS and compare the 3 KPIs:
	 - Queue presence in 15 min window.
	 - Queue > 5 in 15 min window.
	 - Average Waiting Time in 15 min window.
### Results
Project lasted from November to February. I took care of the simple ATMs that only have regular transactions, Zoltan took care of the ATM+ which you can do more transactions (and were more complicated). Some of the outputs were:
 - ETL program to transform the logs of the ATMs into a .csv of transactions.
 - Semi automated code to calculate the KPIs for a defined time.
 - Estimation of the wait time experienced by users in the 3 ATMs for the past ~6 months with some insights.
 - Comparison of KPIs with surveyed outputs (ground truth):
	 - Queue Presence: ~95% accuracy
	 - Queue > 5: ~80% accuracy
	 - Average Wait Time: ~60%  accuracy +-20%  (when taking into consideration that short wait times would need a higher tolerance of accuracy)
### Reflections
First real size project done by me. Lot's to learn from Zoltan but not as much as I would've initially thought a Senior DS would've given. Proved myself that I am a lot smarter that I thought and I'm able to pull through complicated tasks. I manage to get things done. Gained trust from Gabor on talking with the client. Was surprised on the speed of the process and the ideas. 