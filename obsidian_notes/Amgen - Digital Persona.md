# Amgen - Digital Persona
Consulting project done in [[Lynx Analytics]] from March 2021 to June 2021.
## Glossary
- HCP (Health Care Practitioner): doctor
- MR (Medical Representative): sales representative from Amgen
- MR Reach: channels where an active invitation comes from the MR
- Non MR Reach: channels where there's no active invitation, HCP goes to the information themselves.
## STAR
#storytelling #interviews
### Situation
Third project done in Lynx. First project that I started as a Senior DS, having bigger expectations on the handling of the team. Team conformed by Jeroen (PM), Corinne (DE) and me. Amgen wants to improve the efficiency of their marketing and sales team by determining the HCP's preference over channel types (digital vs traditional & MR-Reach vs Non-MR Reach). Work together with global analytics team to develop and deploy the methodology using Databricks #data_engineering. Towards the end, Jeroen left the company and Nicole came in with minor involvement in the tasks of PM, mainly focusing on client engagement.
### Task
Use the methodology developed in a previous project and data from interactions (i.e. emails, phone calls, face to face meetings) to define Digital Persona via a clustering algorithm and predict for HCPs that are not yet involved with Amgen. Deploy the code via Databricks connected to an AWS S3 Bucket #data_engineering.
### Actions
- Determine a methodology to calculate how _digitally advanced_ channels are based purely on data. Based mainly on adoption frequency (% of users adopted) and adoption order (position of first interaction).
- Calculate the HCPs affinity towards Digital, Traditional, MR Reach and Non MR Reach channels.
- Develop a KNN [[Clustering Algorithm]] #machine_learning notebook which considers each pair of scores (digital vs traditional & MR Reach vs Non MR Reach) to group all HCPs with enough interactions into clusters (3 and 4).
- Predict via multiple [[Regression Model]]s #machine_learning the affinity scores for users who do not have enough interactions (<=5).
- For users with 1-5 interactions, combine the calculated and predicted affinity scores into a combined score depending on the interaction number {1:0.1, 2:0.3, 3:0.5, 4:0.7, 5:0.9}.
- Deploy the code into Databricks by translating the steps into functions and merge them into single notebook.
- Create dashboard notebook that compares previous vs current results. 
- Survey MRs about the digital/traditional affinity of their HCPs to compare to our results.
### Results
 Deployed the semi-automated code into Databricks (not schedule to run, somebody had to upload certain csv files into the S3 bucket to make it run). Towards the end, I had to handle most of the PM related tasks, including client management and tasks division. Developed a good relationship with Arshad, the global data scientist which was in charge of our project. 
### Reflections
Fairly easy project, had everything scoped down and methodology already almost finalized from the start. First project to actually deploy to a cloud based system. Surprised about the low quality of the DS for Amgen (Nima, not Arshad). I am not bad at doing PM related work but it's not my favorite, enjoy more the ideation part #agility #design_thinking #creativity.
