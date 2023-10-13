# Gender-Pay-Gap
--Q1 How many companies are in the data set?
SELECT count(employername)
FROM gender_pay_gap_21_22;

A: There are 10174 companies in the data set.

--Q2 How many of them submitted their data after the reporting deadline?
SELECT count(submittedafterthedeadline)
FROM gender_pay_gap_21_22
WHERE submittedafterthedeadline='true';

SELECTemployername, submittedafterthedeadline
FROM gender_pay_gap_21_22
WHERE submittedafterthedeadline='true';

A: 361 companies did not submit the report on time.

--Q3 How many companies have not provided a URL?
SELECT count(companylinktogpginfo)
FROM gender_pay_gap_21_22
WHERE companylinktogpginfo='0';

A: 3700 companies did not provide an URL. 

--Q4 Which measures of pay gap contain too much missing data, and should not be used in our analysis?
SELECT
count(DiffMedianHourlyPercent)
FROM gender_pay_gap_21_22
where DiffMedianHourlyPercent = 0.0 ;

SELECT count(diffmeanbonuspercent)
FROM gender_pay_gap_21_22
WHERE diffmeanbonuspercent='0';


A: The ‘Diffmedianhourlypercent’ column contains a lot of nulls( 861) therefore and it will not be used for this analysis as it might give us inaccurate results.  The Diffmeanbonuspercent column contains 2837 null values, this could be due to some companies not having a bonus scheme, and so it will not be used for this analysis.



--Q5 Choose which column you will use to calculate the pay gap. Will you use DiffMeanHourlyPercent or DiffMedianHourlyPercent? Can you justify your choice?

SELECT
STDDEV_POP(diffmeanhourlypercent) as stddev_mean, STDDEV_POP(diffmedianhourlypercent) as stddev_median
FROM gender_pay_gap_21_22;

A: I will use DiffMeanHourlyPercent because the mean it will be more accurate than the median as it gives us the average of the values. And the standard deviation is lower which means is closer to 0.

--Q6 Use an appropriate metric to find the average gender pay gap across all the companies in the data set.
SELECT avg(diffmeanhourlypercent)
FROM gender_pay_gap_21_22;

--Q7 What are some caveats we need to be aware of when reporting the figure we’ve just calculated?
We always need to check both median and mean standard deviation. And the one closest to 0 will give us the more accurate results. After checking this I chose to use avg.

--Q8 What are the 10 companies with the largest pay gaps skewed towards men?
SELECT employername, diffmeanhourlypercent
FROM gender_pay_gap_21_22
WHERE diffmeanhourlypercent >'0'
ORDER BY diffmeanhourlypercent DESC
LIMIT 10;

--Q9 What do you notice about the results? Are these well-known companies?
They’re companies who are predominately run by men and have men as employees. This can be due to the sector that’s in… construction, sports such as golf, football clubs.

--Q10 Apply some additional filtering to pick out the most significant companies with large pay gaps.
SELECT employername, diffmeanhourlypercent
FROM gender_pay_gap_21_22
WHERE diffmeanhourlypercent >'0' AND diffmeanhourlypercent >95
ORDER BY diffmeanhourlypercent DESC;

--Q11 How would you report on the results? Can we say that these companies are engaging in unlawful pay discrimination?
A: Wouldn’t say these companies are engaging in an unlawful pay discrimination as we don’t know the job positions of each employee and the correspondent gender for those.  But it is possible to observe a general bias when it comes to gender across companies.



--Q12 What’s the average pay gap in London versus outside London?
SELECT avg(diffmeanhourlypercent)
FROM gender_pay_gap_21_22
WHERE address NOT LIKE '%London%'; 
A: Average outside London is 13.045

SELECT avg(diffmeanhourlypercent)
FROM gender_pay_gap_21_22
WHERE address like '%London%'; Average in London is 15.701

A: Average pay gap in London is 15.7 and outside London is 13.1, showing a bigger discrepancy in companies based outside the big city.

Q13 What’s the average pay gap in London versus Birmingham?
SELECT avg(diffmeanhourlypercent)
FROM gender_pay_gap_21_22
WHERE address LIKE '%Birmingham%'; 
A: Average pay gap  in Birmingham 13.2, being 2.5% less than in London. 

--Q14 What is the average pay gap within schools?
SELECT avg(diffmeanhourlypercent)
FROM gender_pay_gap_21_22
WHERE companylinktogpginfo LIKE '%ac%';  
A: Average pay gap within schools 14.179.

--Q15 What is the average pay gap within banks?
SELECT avg(diffmeanhourlypercent)
FROM gender_pay_gap_21_22
WHERE employername LIKE '%BANK%'; 
A: Average 27.171

--Q16 Is there a relationship between the number of employees at a company and the average pay gap?

SELECT distinct employersize as count_of_employersize, avg(diffmeanhourlypercent)
FROM gender_pay_gap_21_22
GROUP BY employersize;

