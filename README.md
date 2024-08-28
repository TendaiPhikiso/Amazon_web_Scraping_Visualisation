<div align="center">

<h1> Visualising Amazon Data Books | Power BI </h1>

![image](https://github.com/user-attachments/assets/7cd0574a-6be3-49f9-ab26-f15f20bfaae7)

</div>

## Overview 📊

This dashboard was developed to visualize data related to books scraped from Amazon. The dashboard provides insights into the trends, top authors & books, and key metrics related to books focused on data science, data analysis, and related fields.

---

---
## Trends in Data Book Publishing

The dashboard highlights a noticeable trend in the increasing number of data-related books published starting in 2019. This reflects the growing interest in data science, machine learning, and artificial intelligence as these fields have become central to many industries and professions.

---

### Correlation Between Print Length and Review Count
To explore how the length of books correlates with their popularity (measured by review count), we calculated correlation coefficients for each format:

Paperback: Correlation of 0.05
Kindle Edition: Data unavailable (blank)
Hardcover: Correlation of -0.06


### Correlation Insights for Individual Book formats

**Paperback (0.05 correlation)**:
    
A correlation coefficient of 0.05 indicates a very weak positive linear relationship between print length and review count for paperback books. This means that as the print length increases, the review count increases slightly, but the relationship is very weak.

**Hardcover (-0.06 correlation)**:
A correlation coefficient of -0.06 indicates a very weak negative linear relationship between print length and review count for hardcover books. The negative correlation for hardcover books, though weak, could suggest that longer hardcover books tend to receive slightly fewer reviews.

---

![image](https://github.com/user-attachments/assets/ab4820f7-3ced-41eb-ad26-c6ccc312c242)

### DAX Code 
```sql

Correlation Coefficient = 
VAR __CORRELATION_TABLE = VALUES('Data_Books'[BookTitle])
VAR __COUNT =
	COUNTX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('Data_Books'[PrintLength]) * SUM('Data_Books'[ReviewCount]))
	)
-- Calculating the Sum of X value: (PrintLength)
VAR __SUM_X =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('Data_Books'[PrintLength]))
	)

-- Calculating the Sum of Y value: (ReviewCount)
VAR __SUM_Y =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('Data_Books'[ReviewCount]))
	)
-- Calculating the sum of the products of PrintLength and ReviewCount for each BookTitle
VAR __SUM_XY =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(
			SUM('Data_Books'[PrintLength])
				* SUM('Data_Books'[ReviewCount]) * 1.
		)
	)
-- Calculating the Sum of the squares of PrintLength for each BookTtitle 
VAR __SUM_X2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('Data_Books'[PrintLength]) ^ 2)
	)

-- Calculating the Sum of the squares of ReviewCount for each BookTtitle 
VAR __SUM_Y2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('Data_Books'[ReviewCount]) ^ 2)
	)
-- Using the above variables to calculate Correlation Coefficient
VAR __CORRELATION = 
    DIVIDE(
		__COUNT * __SUM_XY - __SUM_X * __SUM_Y * 1.,
		SQRT(
			(__COUNT * __SUM_X2 - __SUM_X ^ 2)
				* (__COUNT * __SUM_Y2 - __SUM_Y ^ 2)
		    )
    )
RETURN
-- Handling blank Values where the correlation cannot be calculated
    IF(
        ISBLANK(__CORRELATION), "N/A",
        __CORRELATION
    )


```

## Limitations and Considerations

While this dashboard provides valuable insights, there are some limitations and considerations to keep in mind:

1. Sample Size:
2. Data Availability

