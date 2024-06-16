# Nobel Prize Data Analysis

### Project Description

This project aims to analyze data related to Nobel Prizes, containing information such as year, category, motivation, prize share, personal data of laureates, place of birth, place of work, and, if applicable, place and date of death. The analysis includes initial data exploration, data cleaning, various analyses, and visualizations.

## Data

The data is located in the file `data/netflix_data.csv` and contains the following columns:

- `year`: Year of the award
- `category`: Category of the award
- `prize`: Name of the award
- `motivation`: Motivation for the award
- `prize_share`: Prize share
- `laureate_id`: Laureate ID
- `laureate_type`: Type of laureate (individual, organization)
- `full_name`: Full name of the laureate
- `birth_date`: Birth date of the laureate
- `birth_city`: Birth city of the laureate
- `birth_country`: Birth country of the laureate
- `sex`: Gender of the laureate
- `organization_name`: Name of the laureate's organization
- `organization_city`: City of the laureate's organization
- `organization_country`: Country of the laureate's organization
- `death_date`: Death date of the laureate (if applicable)
- `death_city`: Death city of the laureate (if applicable)
- `death_country`: Death country of the laureate (if applicable)

## Analyses Conducted

1. Data import
2. Initial data mining
3. Distribution of prizes by gender
  - Percentage of women and men among the laureates
  - Number of Awards Granted to Women and Men by Year
4. Prize distribution by category
  - The most popular award categories
  - Nobel Prizes Distribution by Category Over the Years
5. Distribution of Nobel Prizes by year
6. The most common countries of origin of the winners
7. Number of Nobel Prize Winners from USA Over the Years
8. Age distribution of winners at the time of receiving the award
9. Age at death of winners
10. Number of awards in individual cities (Top 10)
11. Most common organizations that won awards (Top 10)
12. Nobel Prize winners from Poland
13. Nobel Prize winners from Poland by Gender
14. Number of Nobel Prizes by Category winners from Poland
15. Distribution of Nobel Prizes by year winners from Poland
    
## Tools and Technologies

- Pandas: Data manipulation and analysis
- NumPy: Numerical operations
- Matplotlib: Data visualization
- Seaborn: Statistical data visualization
- Plotly: Interactive data visualization
