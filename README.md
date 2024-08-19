# DVD Rental Store Analysis


## Table of Contents
- [Project Overview](project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaningpreparation)
- [Descriptive Data Analysis](#descriptive-data-analysis)
- [Data Analysis](#data-analysis)
- [Results and Findings](#results-and-findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)


### Project Overview

This project aims to analyze the DVD rental data from a fictional DVD rental company to uncover insights that can help improve business operations, customer satisfaction, and profitability. The analysis focuses on several key areas, including customer behavior, rental patterns, inventory management, and sales trends.


### Data Sources

DVD Rental Data: The primary database used for this analysis is the "dvdrental.tar" database file, containing detailed information about the dvdrental store.


### Tools

- Command Prompt - Importing the database to pgadmin
- PostgresSQL Sever - Data analysis


### Data Cleaning/Preparation

In the intial data preparation phase we perfromed the following tasks
1. Data loading and inspection
2. Review the database schema and identify the primary keys, foreign keys, and relationships between tables.
3. Understand the purpose of each table and its role in the database


### Descriptive Data Analysis

This involved exploring the dvdrental database to retrive the following information:
- Total number of films in each category.
- Top 5 customers who have rented the most films.
- Number of rentals per month for the past year.
- Total revenue generated by each store.
- Average rental duration for each film.
- Films that have not been rented in the last 90 days.
- Update the rental table to mark rentals that are overdue.
- Rental patterns to identify popular films, categories, and rental trends.
- Patterns in late returns and overdue rentals.
- Insights and recommendations for the rental store based on the analysis.


### Data Analysis

The data analysis for the DVD Rental project was conducted using PostgreSQL to explore customer behaviors, rental patterns, and revenue trends. SQL queries were used to extract, manipulate and transform, and analyze data from various tables, providing insights into customer demographics, popular film genres, peak rental times, and sales performance. The analysis results help inform strategies for inventory management, customer engagement, and operational efficiency.

```sql
Top 5 customers who have rented the most films
SELECT 
    c.customer_id,
    c.first_name,
    c.last_name,
    COUNT(r.rental_id) AS total_rentals
FROM 
    customer c
JOIN 
    rental r ON c.customer_id = r.customer_id
GROUP BY 
    c.customer_id, c.first_name, c.last_name
ORDER BY 
    total_rentals DESC
LIMIT 5;
```
```sql
Total revenue generated by each store
SELECT 
    s.store_id,
    SUM(p.amount) AS total_revenue
FROM 
    store s
JOIN 
    staff st ON s.store_id = st.store_id
JOIN 
    payment p ON st.staff_id = p.staff_id
GROUP BY 
    s.store_id
ORDER BY 
    total_revenue DESC
```
```sql
a. Most Rented Films
SELECT film.title, COUNT(rental.rental_id) AS rental_count
FROM rental
JOIN inventory ON rental.inventory_id = inventory.inventory_id
JOIN film ON inventory.film_id = film.film_id
GROUP BY film.title
ORDER BY rental_count DESC
LIMIT 10

b. Most Popular Categories
SELECT category.name, COUNT(rental.rental_id) AS rental_count
FROM rental
JOIN inventory ON rental.inventory_id = inventory.inventory_id
JOIN film ON inventory.film_id = film.film_id
JOIN film_category ON film.film_id = film_category.film_id
JOIN category ON film_category.category_id = category.category_id
GROUP BY category.name
ORDER BY rental_count DESC
LIMIT 10

c. Rental trends over time
SELECT DATE_TRUNC('month', rental.rental_date) AS month, COUNT(rental.rental_id) AS rental_count
FROM rental
GROUP BY month
ORDER BY month
```

### Results/Findings

The analysis results are summarised as follows:
- The analysis identified several films that are consistently popular among customers. These films are frequently rented, indicating a high demand.
- Certain categories, such as Action, Sports, and Animation, stand out as the most rented genres. These categories drive a significant portion of the rental activity.
- The data shows noticeable rental spikes during specific months, suggesting seasonal trends. This could be due to holidays, vacation periods, or other factors that increase customer interest in renting films during these times.
- A significant number of rentals are returned late, particularly for specific films. This may suggest that the rental durations for these films are too short, or customers may require reminders to return films on time.
- There are several cases where rentals remain overdue, indicating a need for follow-up with customers. Overdue rentals can lead to inventory shortages and lost revenue if not managed properly.
- A small group of customers accounts for a large portion of the rentals. These top customers are valuable to the store, and targeted loyalty programs or personalized offers could encourage even more frequent rentals.
- Based on the popularity of certain films, it’s recommended that the store increases the stock of these high-demand titles to prevent stockouts and maximize rental opportunities.
- The findings on late returns suggest that the store might need to review and potentially adjust its rental duration policies or late fee structure to encourage timely returns.


### Recommendations

Based on the analysis we recommend the following actions:
- Ensure that high-demand films are always in stock to meet customer needs and maximize rentals.
- Tailor marketing efforts around the most popular categories to attract more customers.
- Implement reminder systems or revise rental policies to reduce the incidence of late and overdue returns.
- Develop loyalty programs and personalized offers to retain and reward the store's most active customers.
- Anticipate busy periods by adjusting inventory and staffing levels to handle increased demand.


### Limitations

1. The analysis is descriptive and does not include predictive modeling to forecast future rental trends or customer behavior. Predictive analytics could provide more forward-looking insights.
2. The analysis does not account for external factors such as changes in the competitive landscape, economic conditions, or shifts in consumer behavior that could impact rental patterns.
3. The analysis assumes that past rental patterns will continue, but it does not account for changing customer preferences, such as new film releases or emerging trends in genres.
4. The analysis assumes that all films are equally available for rent, but it does not account for inventory limitations that might have prevented customers from renting certain titles.

### Reference

1. SQL Query script used has been attached
2. PostgresSQL v.16.4
3. DVDrental Store Data Dictionary 
4. Database ERD Diagram
