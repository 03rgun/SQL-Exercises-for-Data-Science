# Filtering, Sorting and Calculating Data
For all the questions in this practice set, you will be using the Salary by Job Range Table. This is a single table titled: salary_range_by_job_classification. This table contains the following columns:



- SetID
- Job_Code
- Eff_Date
- Sal_End_Date
- Salary_setID
- Sal_Plan
- Grade
- Step
- Biweekly_High_Rate
- Biweekly_Low_Rate
- Union_Code
- Extended_Step
- Pay_Type
\
\
**1. Find the distinct values for the extended step.** 
\
*Code*
```SQL
SELECT DISTINCT Extended_step
FROM salary_range_by_job_classification; 
```
*Result*
```
+---------------+
| Extended_Step |
+---------------+
| 0             |
| 11            |
| 6             |
| 2             |
+---------------+
```
\
**2. Excluding $0.00, what is the minimum bi-weekly high rate of pay (please include the dollar sign and decimal point in your answer)?**
\
*Code*
```SQL
SELECT MIN(Biweekly_high_Rate)
FROM salary_range_by_job_classification
WHERE NOT Biweekly_high_Rate = "$0.00";
```
*Result*
```
+-------------------------+
| MIN(Biweekly_high_Rate) |
+-------------------------+
| $100.00                 |
+-------------------------+
```
\
**3. What is the maximum biweekly high rate of pay (please include the dollar sign and decimal point in your answer)?**
\
*Code*
```SQL
SELECT 
MAX(Biweekly_high_Rate) 
FROM salary_range_by_job_classification;
```
*Result*
```
+-------------------------+
| MAX(Biweekly_high_Rate) |
+-------------------------+
| $9726.38                |
+-------------------------+
```
\
**4. What is the pay type for all the job codes that start with '03'?**
\
*Code*
```SQL
SELECT job_code, pay_type
FROM salary_range_by_job_classification
WHERE job_code LIKE "03%";
```
*Result*
```
+----------+----------+
| Job_Code | Pay_Type |
+----------+----------+
| 0380     | B        |
| 0381     | B        |
| 0382     | B        |
| 0390     | B        |
| 0395     | B        |
| 0380     | B        |
| 0381     | B        |
| 0382     | B        |
+----------+----------+
```
\
**5. Run a query to find the Effective Date (eff_date) or Salary End Date (sal_end_date) for grade Q90H0?**
\
*Code*
```SQL
SELECT grade, eff_date, sal_end_date
FROM salary_range_by_job_classification
WHERE grade = "Q90H0";
```
*Result*
```
+-------+------------------------+------------------------+
| Grade | Eff_Date               | Sal_End_Date           |
+-------+------------------------+------------------------+
| Q90H0 | 12/26/2009 12:00:00 AM | 06/30/2010 12:00:00 AM |
+-------+------------------------+------------------------+
```
\
**6. Sort the Biweekly low rate in ascending order.**
\
*Code*
```SQL
SELECT Biweekly_Low_Rate
FROM salary_range_by_job_classification
ORDER BY Biweekly_Low_Rate ASC;
```
*Result*
```
+-------------------+
| Biweekly_Low_Rate |
+-------------------+
| $0.00             |
| $0.00             |
| $0.00             |
| $0.00             |
| $100.00           |
| $100.00           |
| $10059.00         |
| $10376.00         |
| $1052.00          |
| $10630.00         |
| $10843.00         |
| $1088.00          |
| $1112.00          |
| $11255.00         |
| $11405.00         |
| $1162.00          |
| $12120.77         |
| $1280.00          |
| $1284.00          |
| $1298.00          |
| $1299.00          |
| $1381.00          |
| $1384.00          |
| $1405.00          |
| $1464.00          |
+-------------------+
(Output limit exceeded, 25 of 1356 total rows shown)
```
\
**7. Write and run a query, with no starter code to answer this question: What Step are Job Codes 0110-0400?**
\
*Code*
```SQL
SELECT Step, Job_Code
FROM salary_range_by_job_classification
WHERE Job_Code = "0110" OR Job_Code = "0400";
```
*Result*
```
+------+----------+
| Step | Job_Code |
+------+----------+
| 1    | 0110     |
| 1    | 0400     |
+------+----------+
```
\
**8. What is the Biweekly High Rate minus the Biweekly Low Rate for job Code 0170?**
\
*Code*
```SQL
SELECT job_code, Biweekly_High_Rate, Biweekly_Low_Rate, (Biweekly_High_Rate - Biweekly_Low_Rate) AS biweekly_minus
FROM salary_range_by_job_classification
WHERE Job_Code = "0170";
```
*Result*
```
+----------+--------------------+-------------------+----------------+
| Job_Code | Biweekly_High_Rate | Biweekly_Low_Rate | biweekly_minus |
+----------+--------------------+-------------------+----------------+
| 0170     | $4142.00           | $4142.00          |              0 |
+----------+--------------------+-------------------+----------------+
```
\
**9. What is the Extended Step for Pay Types M, H, and D?**
\
*Code*
```SQL
SELECT Extended_Step, Pay_type
FROM salary_range_by_job_classification
WHERE Pay_type In ("M", "H", "D");
```
*Result*
```
+---------------+----------+
| Extended_Step | Pay_Type |
+---------------+----------+
| 0             | D        |
| 0             | D        |
| 0             | D        |
| 0             | M        |
| 0             | D        |
| 0             | D        |
| 0             | M        |
| 0             | H        |
| 0             | H        |
| 0             | H        |
| 0             | H        |
| 0             | H        |
| 0             | H        |
| 0             | H        |
| 0             | H        |
+---------------+----------+
```
\
**10.  What is the step for Union Code 990 and a Set ID of SFMTA or COMMN?**
\
*Code*
```SQL
SELECT Step, Union_Code, SetID
FROM salary_range_by_job_classification
WHERE Union_Code = "990" AND (SetId = "SFMTA" OR SetId = "COMMN");
```
*Result*
```
+------+------------+-------+
| Step | Union_Code | SetID |
+------+------------+-------+
| 1    | 990        | COMMN |
+------+------------+-------+
```
