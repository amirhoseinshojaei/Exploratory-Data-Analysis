# Exploratory Data Analysis (EDA) of Layoffs Data

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRIn6p_Svb0wSiLnOMNdgDNkylGYIEt-F35jw&s"></img>

## Overview

This project focuses on conducting exploratory data analysis (EDA) on the `layoffs_staging2` dataset. The dataset contains information on company layoffs, including details such as the number of employees laid off, the percentage of workforce laid off, industry, country, and more. The SQL queries provided offer insights into various aspects of the layoffs data.

## Project Structure

The project is structured around the following SQL queries, each designed to extract specific insights from the dataset:

1. **Basic Data Exploration**
   ```sql
   SELECT * FROM layoffs_staging2;
   ```

2. **Maximum Values Analysis**

   ```sql
   SELECT MAX(total_laid_off), MAX(percentage_laid_off) FROM layoffs_staging2;
   ```

3. **High-Impact Layoffs**

   ````sql
   SELECT * FROM layoffs_staging2 WHERE percentage_laid_off = 1 ORDER BY total_laid_off DESC;
   ````
   

4. **Company-wise Layoffs**

   ````sql
   SELECT company, SUM(total_laid_off) FROM layoffs_staging2 GROUP BY company ORDER BY 2 DESC;
   ````
   

5. **Date Range of Layoffs**

   ````sql
   SELECT MIN(`date`), MAX(`date`) FROM layoffs_staging2;
   ````
   

6. **Industry-wise Layoffs**

   ````sql
   SELECT industry, SUM(total_laid_off) FROM layoffs_staging2 GROUP BY industry ORDER BY 2 DESC;
   ````
   

7. **Country-wise Layoffs**

   ````sql
   SELECT country, SUM(total_laid_off) FROM layoffs_staging2 GROUP BY country ORDER BY 2 DESC;
   ````
   

8. **Year-wise Layoffs**

   ````sql
   SELECT YEAR(`date`), SUM(total_laid_off) FROM layoffs_staging2 GROUP BY YEAR(`date`) ORDER BY 1 DESC;
   ````


9. **Country and Year-wise Layoffs**

   ````sql
   SELECT country, YEAR(`date`), SUM(total_laid_off) FROM layoffs_staging2 GROUP BY YEAR(`date`), country ORDER BY SUM(total_laid_off) DESC;
   ````
   

10. **Stage-wise Layoffs**

   ````sql
   SELECT stage, SUM(total_laid_off) FROM layoffs_staging2 GROUP BY stage ORDER BY 2 DESC;
   ````

11. **Monthly Layoffs Trend**

   ````sql
   SELECT SUBSTRING(`date`, 1, 7) AS `MONTH`, SUM(total_laid_off) FROM layoffs_staging2 WHERE SUBSTRING(`date`, 1, 7) IS NOT NULL GROUP BY `MONTH` ORDER BY 1 ASC;
   ````


12. **Rolling Total of Layoffs**

   ````sql
   WITH rolling_total AS (
    SELECT SUBSTRING(`date`, 1, 7) AS `MONTH`, SUM(total_laid_off) AS total_laid_off
    FROM layoffs_staging2
    WHERE SUBSTRING(`date`, 1, 7) IS NOT NULL
    GROUP BY `MONTH`
    ORDER BY 1 ASC
)
SELECT `MONTH`, total_laid_off, SUM(total_laid_off) OVER(ORDER BY `MONTH`) AS rolling_total
FROM rolling_total;
````

13. **Company and Year-wise Layoffs**

   ````sql
   SELECT company, YEAR(`date`) AS `date`, SUM(total_laid_off) AS total_laid_off
FROM layoffs_staging2
GROUP BY company, `date`
ORDER BY 3 DESC;
````

14. **Company Ranking by Year**

   ````sql
   WITH company_year (company_name, years, total_laid_off) AS (
    SELECT company, YEAR(`date`) AS `date`, SUM(total_laid_off) AS total_laid_off
    FROM layoffs_staging2
    GROUP BY company, `date`
)
SELECT *, DENSE_RANK() OVER(PARTITION BY years ORDER BY total_laid_off DESC) AS ranks
FROM company_year
WHERE years IS NOT NULL;
````

15. **Top 5 Companies by Layoffs Per Year**

   ````sql
   WITH company_year (company_name, years, total_laid_off) AS (
    SELECT company, YEAR(`date`) AS `date`, SUM(total_laid_off) AS total_laid_off
    FROM layoffs_staging2
    GROUP BY company, `date`
), company_ranking AS (
    SELECT *, DENSE_RANK() OVER(PARTITION BY years ORDER BY total_laid_off DESC) AS ranks
    FROM company_year
    WHERE years IS NOT NULL 
)
SELECT * FROM company_ranking WHERE ranks <= 5;
````


## Getting Started

To run these queries, load your dataset into a SQL environment and execute the SQL statements provided. This will allow you to explore and analyze the layoffs data from different angles, helping you to gain a better understanding of the trends and patterns within the data.

