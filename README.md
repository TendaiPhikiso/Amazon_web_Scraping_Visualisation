<div align="center">

<h1> Visualising Amazon Data Books | Power BI </h1>

![image](https://github.com/user-attachments/assets/7cd0574a-6be3-49f9-ab26-f15f20bfaae7)

</div>


### Correlation Between Print Length and Review Count
To explore how the length of books correlates with their popularity (measured by review count), we calculated correlation coefficients for each format:

Paperback: Correlation of 0.05
Kindle Edition: Data unavailable (blank)
Hardcover: Correlation of -0.06


### Correlation Insights for Individual Book formats

**Paperback (0.05 correlation)**:
    
A correlation coefficient of 0.05 indicates a very weak positive linear relationship between print length and review count for paperback books. This means that as the print length increases, the review count increases slightly, but the relationship is very weak.

**Hardcover (-0.06 correlation)**:
A correlation coefficient of -0.06 indicates a very weak negative linear relationship between print length and review count for hardcover books. This means that as the print length increases, the review count decreases slightly.

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
VAR __SUM_X =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('Data_Books'[PrintLength]))
	)
VAR __SUM_Y =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('Data_Books'[ReviewCount]))
	)
VAR __SUM_XY =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(
			SUM('Data_Books'[PrintLength])
				* SUM('Data_Books'[ReviewCount]) * 1.
		)
	)
VAR __SUM_X2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('Data_Books'[PrintLength]) ^ 2)
	)
VAR __SUM_Y2 =
	SUMX(
		KEEPFILTERS(__CORRELATION_TABLE),
		CALCULATE(SUM('Data_Books'[ReviewCount]) ^ 2)
	)

VAR __CORRELATION = 
    DIVIDE(
		__COUNT * __SUM_XY - __SUM_X * __SUM_Y * 1.,
		SQRT(
			(__COUNT * __SUM_X2 - __SUM_X ^ 2)
				* (__COUNT * __SUM_Y2 - __SUM_Y ^ 2)
		    )
    )
RETURN
    IF(
        ISBLANK(__CORRELATION), "N/A",
        __CORRELATION
    )


```
