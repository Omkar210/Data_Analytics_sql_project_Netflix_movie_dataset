# Netflix Tv Shows and Movies Data Analysis Project

![Netflix Logo](https://raw.githubusercontent.com/Omkar210/Netflix_sql_project/refs/heads/main/2560px-Netflix_logo.svg.webp)

## Overview 

The Netflix dataset insights provide a comprehensive view of the platform’s content strategy, revealing patterns in content type (Movies vs. TV Shows), popular genres, and geographic trends. Analyzing the distribution of genres and the balance between Movies and TV Shows offers insight into Netflix’s appeal across various audience segments, while the growth in titles added each year highlights its expansion and evolving catalog. Regional analysis shows the most prominent content-producing countries, indicating Netflix’s focus on global and local content appeal. Duration and rating distributions reveal how Netflix caters to different age groups and viewing preferences, from family-friendly to mature-rated content. Additionally, identifying top creators and popular titles helps pinpoint key contributors to Netflix’s library, shaping future content acquisition and recommendation strategies to enhance viewer satisfaction.

## Objective

- Understanding content variety helps in content planning and budgeting.
- Netflix’s reach and content variety from different countries.
- Understanding rating distribution can inform decisions on content targeting specific age groups.
- Recognize popular directors, actors, and genres to promote key titles and enhance recommendations.

## Schema
**Objective:** Creating Table with listed Attribute and before that checking if Table is already exist or not and droping that Table.

```sql
DROP TABLE netflix;
CREATE TABLE netflix (
	show_id	varchar(10),
	type varchar(10),
	title varchar(150),
	director varchar(220),
	casts varchar(800),
	country	varchar(150),
	date_added varchar(50),
	release_year int,
	rating	varchar(10),
	duration varchar(30),
	listed_in	varchar(80),
	description varchar(250)
);
```

## Data Insight 
**Objective:** Getting Insight of Data.
```sql
SELECT * FROM netflix
LIMIT 10;
```
## Business Problem and their Solution 
### 1. Count the number of Movies vs TV Shows
**Objective:** Counting the Total Number of Movies and TV Shows.

```sql
SELECT 
	type,
	count(*) as total_content
FROM netflix 
GROUP BY type;
```


### 2. Find the most common rating for movies and TV shows
**Objective:** Finding the most common rating given to the Movies and TV Shows.

```sql
SELECT
	type,
	rating
FROM
	(
		SELECT
			type,
			rating,
			COUNT(*),
			RANK() OVER (PARTITION BY type ORDER BY COUNT(*) DESC) AS ranking
		FROM
			netflix
		GROUP BY
			1, 2
	) AS table1
WHERE
	ranking = 1
```


### 3. List all movies released in a specific year (e.g., 2020)
**Objective:** Identifing all Movies that are release in specific year.

```sql
SELECT * FROM netflix 
WHERE 
	type = 'Movie' AND 
	release_year = 2020
```


### 4. Find the top 5 countries with the most content on Netflix
**Objective:** Finding Top 5 countries where most of the content of Netflix is Released.

```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(country, ',')) as listed_country,
	COUNT(show_id) as total_content
FROM 
	netflix
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```


### 5. Identify the longest movie
**Objective:** 

```sql
SELECT * FROM netflix 
WHERE 
	type = 'Movie' AND
	duration = (SELECT MAX(duration) FROM netflix)
```


### 6. Find content added in the last 5 years
```sql
SELECT * FROM netflix
WHERE 
	TO_DATE(date_added, 'Month DD, YYYY') >= CURRENT_DATE - INTERVAL '5 years'
```


### 7. Find all the movies/TV shows by director 'Rajiv Chilaka'!
```sql
SELECT * FROM netflix 
WHERE 
	director ILIKE '%Rajiv Chilaka%'
```


### 8. List all TV shows with more than 5 seasons
```sql
SELECT * FROM netflix
WHERE 
	type = 'TV Show' AND
	SPLIT_PART(duration, ' ', 1)::numeric > 5
```


### 9. Count the number of content items in each genre
```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(listed_in, ',')) as genre,
	COUNT(show_id) as total_content
FROM netflix
GROUP BY 1
```


### 10.Find each year and the average numbers of content release in India on netflix. return top 5 year with highest avg content release!
```sql
SELECT 
	EXTRACT(YEAR FROM TO_DATE(date_added, 'Month DD, YYYY')) AS year,
	ROUND(COUNT(*)::NUMERIC/(SELECT COUNT(*) FROM netflix WHERE country = 'India')::NUMERIC * 100, 2) AS avg_content_per_year
FROM netflix
WHERE country = 'India'
GROUP BY 1
```


### 11. List all movies that are documentaries
```sql
SELECT * FROM netflix
WHERE listed_in ILIKE '%documentaries%'
```


### 12. Find all content without a director
```sql
SELECT * FROM netflix
WHERE director IS NULL
```


### 13. Find how many movies actor 'Salman Khan' appeared in last 10 years!
```sql
SELECT * FROM netflix 
WHERE 
	casts ILIKE '%salman khan%'AND
	release_year > EXTRACT(YEAR FROM CURRENT_DATE) - 10
```


### 14. Find the top 10 actors who have appeared in the highest number of movies produced in India.
```sql
SELECT 
	UNNEST(STRING_TO_ARRAY(casts, ',')) AS movies_of_casts,
	COUNT(show_id) AS total_content
FROM netflix
WHERE country ILIKE '%india%'
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10
```


### 15. Categorize the content based on the presence of the keywords 'kill' and 'violence' in the description field. Label content containing these keywords as 'Bad' and all other content as 'Good'. Count how many items fall into each category.
```sql
WITH new_table AS (
SELECT 
	*,
	CASE 
	WHEN description ILIKE '%kill%' OR
	description ILIKE '%voilence%' THEN 'Bad-Content: Not Recommended'
	ELSE 'Good-Content'
	END category
FROM netflix
)
SELECT 
	category,
	COUNT(*) AS total_content
FROM new_table
GROUP BY 1
```

## Conclusion 

-Content Mix: Netflix maintains a strategic balance between Movies and TV Shows, appealing to varied audience preferences.
-Genre Focus: The platform prioritizes popular genres to cater to a diverse and global user base.
-Content Growth: Netflix has shown consistent growth in its content library, expanding significantly over the years.
-International Content: There is a strong emphasis on acquiring and promoting international and regional content to appeal to global audiences.
-Viewer Preferences: Duration and rating patterns indicate a strategy to engage a wide range of viewers, from family-friendly content to mature-rated series.
-Influential Creators: Netflix invests in top directors and actors, using their influence to attract and retain viewers.
-Adaptive Strategy: The content catalog is continuously updated to meet changing audience demands and trends.
