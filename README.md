# Health Analytics Case Study
Answering business questions using SQL  
By Jeremy Anderson

![Red heart made out of binary digits](img/image1.jpeg)

**Table of Contents**
  - [Situation](#situation)
  - [Task](#task)
  - [Action](#action)
  - [Results](#results)

## Situation

In this case study, I play the role of a data analyst at Health Co. The General Manager of Analytics has requested my assistance with their analysis of the ```health.user_logs``` dataset. 

>_Hello Jeremy,_
>
>_I have a board meeting tomorrow and I need your help with answering a few business questions about the active users in our new health app. Will you please degbug this SQL script and send me the working code by the end of today? Our analytics team ran into a few bugs that they couldn't fix._
>
>_Thank you!_  
>
>_Georgina,_    
_GM of Analytics_


## Task 
My task is to identify and resolve the issues in the SQL script that the General Manager sent to me. 


## Action 
Using a PostgreSQL database system, I ran each script, reviewed the error messages, and used my knowledge of SQL to write correct and working code. 

## Results 

>_Hello Georgina,_
>
>_Good news! I have successfully debuged the SQL script. Please find the updated and working code below._
>
>_Thank you!_
>
>_Jeremy_

***

1. How many unique users exist in the logs dataset?
```sql
SELECT
  COUNT (DISTINCT id) AS unique_users
FROM health.user_logs;
```

| unique_users 
| ----------- | 
| 554      |        


>_For questions 2, 3, 4, 5, 7 & 8 we created a temporary table_
```sql
DROP TABLE IF EXISTS user_measure_count;
CREATE TEMP TABLE user_measure_count AS
SELECT
    id,
    COUNT(*) AS measure_count,
    COUNT(DISTINCT measure) as unique_measures
  FROM health.user_logs
  GROUP BY 1; 
```

2. How many total measurements do we have per user on average?
```sql
SELECT ROUND(AVG(measure_count)) AS avg_total_measurements_per_user
FROM user_measure_count;
```

| avg_total_measurements_per_user
| ----------- | 
| 79      |   

3. What about the median number of measurements per user?
```sql
SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY measure_count) AS median_number_measurements
FROM user_measure_count;
```
| median_number_measurements
| ----------- | 
| 2      |  


4. How many users have 3 or more measurements?
```sql
SELECT
  COUNT(id)
FROM user_measure_count
WHERE measure_count >= 3;
```

| count
| ----------- | 
| 209      | 

5. How many users have 1,000 or more measurements?
```sql
SELECT
  COUNT(id)
FROM user_measure_count
WHERE measure_count >= 1000;
```
| count
| ----------- | 
| 5      | 


6. How many users have logged blood glucose measurements?
```sql
SELECT 
  COUNT (DISTINCT id)
FROM health.user_logs
WHERE measure = 'blood_glucose';
```

| count
| ----------- | 
| 325      | 


7. How many users have at least 2 types of measurements?
```sql
SELECT
  COUNT(id)
FROM user_measure_count
WHERE unique_measures >= 2;
```
| count
| ----------- | 
| 204     | 


8. Have all 3 measures - blood glucose, weight and blood pressure?
```sql
SELECT
  COUNT(id)
FROM user_measure_count
WHERE unique_measures =3;
```

| count
| ----------- | 
| 50     | 



9.  What is the median systolic/diastolic blood pressure values?**
```sql
SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY systolic) AS median_systolic,
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY diastolic) AS median_diastolic
FROM health.user_logs
WHERE measure = 'blood_pressure';
```

| median_systolic      | median_diastolic |
| ----------- | ----------- |
| 126      | 79       |

***
Thank you!

![Yellow stethoscope with red paper heart](img/image2.jpeg)

***
Note: I completed this case study as part of the course [Serious SQL](https://www.datawithdanny.com/courses/serious-sql) taught by Danny Ma. 
