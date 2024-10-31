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

'''sql
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
'''

