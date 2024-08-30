<div align="center">

<h1> Visualising Amazon Data Books | Power BI </h1>

![image](https://github.com/user-attachments/assets/7cd0574a-6be3-49f9-ab26-f15f20bfaae7)

</div>

## Overview 📊

This dashboard was developed to visualize data related to books scraped from Amazon, providing insights into trends and key metrics for books focused on data science, data analysis, and related fields. The dashboard features various visualizations, including tables displaying the top 5 authors and books, a treemap illustrating the distribution of book formats (paperback, hardcover, and Kindle edition), and an area chart showing the number of books published over time starting from 1992. Additionally, I applied a statistical method (Pearson correlation coefficient) to explore the relationship between print length and review count by book format.

The data was visualized as part of the analysis phase conducted previously using SQL ( [Click here to view Analysis](https://github.com/TendaiPhikiso/Amazon_web_scraping_Load_AnalysisPhase) ), highlighting key metrics such as the total number of books, average ratings, and average selling prices.

[Click to View Live Dashboard](https://app.powerbi.com/view?r=eyJrIjoiZWE4Y2U2ZjItNzRkMy00NWNjLTk3MWMtMWUwMjZlZjhhNzliIiwidCI6IjE2MmIyYmIwLWM3ZTEtNGZlZC04NzlkLTk2NTZhMWFjMzMyYiJ9)

---
### KPI Insights

- **Total Books and Authors:**  The dataset contains a total of 233 books authored by 218 individuals. This suggests that the data science community has a notable and diverse group of contributors, with some authors writing multiple books.
- **Average Rating (3.98):** The average rating of 3.98 indicates that the quality of books in this niche is relatively high. A rating near 4.0 suggests that most books are positively received, though there’s always room for improvement in terms of content quality or accessibility.
- **Average Selling Price (£20.63):** The average price is moderate, likely making data-related books accessible to a broad audience, including students, professionals, and enthusiasts.
---

### Trends in Data Book Publishing
The dashboard highlights a noticeable trend in the increasing number of data-related books published starting in 2019. This reflects the growing interest in data science, machine learning, and artificial intelligence as these fields have become central to many industries and professions.

---

### Correlation Between Print Length and Review Count
To explore how the length of books correlates with their popularity (measured by review count), I calculated correlation coefficients for each format:

- Paperback: Correlation of 0.05
- Kindle Edition: Data unavailable (blank)
- Hardcover: Correlation of -0.06

  
**1. Paperback (0.05 correlation)**:
    
A correlation coefficient of 0.05 indicates a very weak positive linear relationship between print length and review count for paperback books. This means that as the print length increases, the review count increases slightly, but the relationship is very weak.

**2. Hardcover (-0.06 correlation)**:
A correlation coefficient of -0.06 indicates a very weak negative linear relationship between print length and review count for hardcover books. The negative correlation for hardcover books, though weak, could suggest that longer hardcover books tend to receive slightly fewer reviews.

---

![image](https://github.com/user-attachments/assets/ab4820f7-3ced-41eb-ad26-c6ccc312c242)

### DAX Code 
```sql

Correlation Coefficient =
-- setting up a subset of data to perform calculations for each unique book title
VAR __CORRELATION_TABLE = VALUES('Data_Books'[BookTitle])

-- __COUNT variable represents the value of n, which is the number of data points (observations) used in the correlation calculation.
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

1. Sample Size: The dataset includes 233 books, which is substantial but still a relatively small sample. A larger dataset may reveal different trends and provide stronger conclusions.
2. Data Availability: One of the key limitations is the lack of complete data for Kindle editions. All Kindle edition books were out of stock at the time of scraping, which meant that their selling price could not be calculated, as out-of-stock books did not have a selling price attribute available during the data collection. Additionally, because Kindle editions are digital formats, they did not include print length as a variable during the scraping phase, which excluded them from the analysis of the correlation coefficient between print length and review count

