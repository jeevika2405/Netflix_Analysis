# Netflix Content Analysis Dashboard (Power BI)

An interactive Power BI dashboard that analyzes Netflix's catalog of movies and TV shows: content mix, genres, countries, ratings, people, and how content additions changed over time.

## Problem Statement
What does Netflix's catalog contain, and how has it changed? This project answers questions such as:
- How has the mix of Movies vs TV Shows changed?
- Which genres and countries dominate the catalog?
- When did Netflix add the most content, and how old was that content when added?
- Which directors and actors appear most often?

## Dataset
- **Source:** Netflix Movies and TV Shows dataset (Kaggle)
- **Size:** 8,807 titles, added between 2008 and 2021
- **Columns:** show_id, type, title, director, cast, country, date_added, release_year, rating, duration, listed_in (genres), description

## Tools Used
Power BI Desktop, Power Query (data cleaning), DAX (measures), Data Modeling (star schema)

## Project Steps
1. **Data cleaning (Power Query):** trimmed and cleaned text, replaced null values with "Unknown", fixed data types, removed duplicates and one corrupted row.
2. **Feature engineering:** split `duration` into Duration Mins (movies) and Seasons (TV shows), grouped ratings into an Audience column (Kids / Teens / Adults / Other), and created Content Age (year added minus release year).
3. **Bridge tables:** split multi-value columns (genre, country, cast, director) into separate tables, one value per row.
4. **Data model:** star schema with a Titles fact table, four bridge tables and a custom Date table, linked on `show_id` and `date_added`.
5. **DAX measures:** KPIs, year-over-year growth and averages.
6. **Dashboard:** 5 report pages with slicers, drill-through, custom tooltips, bookmarks and a page navigator, styled in a dark Netflix theme.

## Key DAX Measures
```
Total Titles = DISTINCTCOUNT(Titles[show_id])

Movies = CALCULATE([Total Titles], Titles[type] = "Movie")

TV Shows = CALCULATE([Total Titles], Titles[type] = "TV Show")

% Movies = DIVIDE([Movies], [Total Titles])

Avg Movie Duration = CALCULATE(AVERAGE(Titles[Duration Mins]), Titles[type] = "Movie")

Avg Content Age = AVERAGE(Titles[content_age])

Titles Last Year = CALCULATE([Total Titles], SAMEPERIODLASTYEAR('Date'[Date]))

YoY % = DIVIDE([Total Titles] - [Titles Last Year], [Titles Last Year])
```

## Dashboard Pages
| Page | What it shows |
|---|---|
| Overview | KPI cards, titles by audience and type, titles added per year |
| Genre and Rating | Top 10 genres, rating distribution, genre by year heatmap |
| Geography | Map and Top 10 countries, with genre, type and year slicers |
| People | Top 10 directors, top 10 actors, searchable titles table |
| Insights | Key findings and recommendations |

**Interactive features:** slicers synced across pages, drill-through from genre to title list, tooltip page with genre statistics, bookmark buttons (All / Movies / TV Shows), and page navigation.

## Key Insights
- Movies make up about 70% of the catalog (6,131 titles); TV shows are about 30% (2,676).
- Content additions peaked in 2019 with 2,016 titles, then fell to 1,879 in 2020 and 1,498 in 2021.
- TV-MA is the most common rating (3,207 titles), so the catalog leans toward adult and teen audiences; Kids content is the smallest group.
- International Movies is the largest genre (2,752 titles), followed by Dramas (2,427) and Comedies (1,674).
- The United States (2,751 titles) and India (962) lead production, followed by the United Kingdom, Canada and France.
- Titles are added on average about 4.7 years after release.

## Recommendations
- Grow the TV show catalog, since TV shows are only about 30% of titles but keep viewers returning.
- Invest in international content, especially international TV shows.
- Reduce dependence on the US by spreading investment across regions.
- Expand Kids and family content, the smallest audience group.
- Favor fresher releases and keep additions steady after the 2019 peak.

*Note: this analysis is based on catalog data only (no viewership data) and covers 2008 to 2021.*

## Screenshots
![Overview](screenshots/01_Overview.png)
![Genre and Rating](screenshots/02_Genre_Rating.png)
![Geography](screenshots/03_Geography.png)
![People](screenshots/04_People.png)
![Insights](screenshots/05_Insights.png)

## Files
- `Netflix_Analysis.pbix`: the Power BI report (open with Power BI Desktop)
- `Netflix_Analysis.pdf`: PDF export of the report
- `screenshots/`: images of each report page

## Author
**Jeevikish**
GitHub: https://github.com/jeevikish-commits
