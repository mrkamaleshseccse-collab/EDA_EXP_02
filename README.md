Netflix Shows & Movies

Name: kamaleshkumar k

Register Number:212225040164

Aim

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

Procedure / Algorithm

1)Load dataset (netflix_titles.csv). 2)Count movies vs TV shows. 3)Group by country → top contributors. 4)Create pivot table (release year vs type). 5)Visualize with bar & line charts.

Program

Write your code here
~~~
import pandas as pd
import numpy as np

url = "https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"

df = pd.read_csv(url)

print("\n========== FIRST 5 ROWS ==========")
print(df.head())

print("\n========== LAST 5 ROWS ==========")
print(df.tail())

print("\n========== SHAPE ==========")
print(df.shape)

print("\n========== COLUMN NAMES ==========")
print(df.columns)

print("\n========== DATA TYPES ==========")
print(df.dtypes)

print("\n========== DATASET INFORMATION ==========")
df.info()

print("\n========== STATISTICAL SUMMARY ==========")
print(df.describe(include="all"))

print("\n========== MISSING VALUES ==========")

missing_count = df.isnull().sum()
missing_percentage = (df.isnull().sum() / len(df)) * 100

missing_report = pd.DataFrame({
    "Missing Values": missing_count,
    "Percentage": missing_percentage
})

print(missing_report)

# Replace missing values
df["director"] = df["director"].fillna("Unknown")
df["country"] = df["country"].fillna("Unknown")
df["cast"] = df["cast"].fillna("Unknown")
df["rating"] = df["rating"].fillna("Not Rated")

print("\n========== MISSING VALUES AFTER fillna ==========")
print(df[["director", "country", "cast", "rating"]].isnull().sum())

# DataFrame after removing rows with missing director and country
df_drop = pd.read_csv(url)

df_no_missing = df_drop.dropna(subset=["director", "country"])

print("\n========== DATAFRAME AFTER dropna ==========")
print(df_no_missing.head())
print("Shape:", df_no_missing.shape)

print("\n========== MOVIES ==========")
movies = df[df["type"] == "Movie"]
print(movies.head())

print("\n========== TV SHOWS ==========")
tv_shows = df[df["type"] == "TV Show"]
print(tv_shows.head())

print("\n========== TITLE, COUNTRY, RELEASE YEAR ==========")
print(df[["title", "country", "release_year"]].head())

print("\n========== ROWS FROM INDEX 100 TO 120 ==========")
print(df.loc[100:120])

print("\n========== MOVIES RELEASED AFTER 2018 ==========")
print(df.loc[
    (df["type"] == "Movie") &
    (df["release_year"] > 2018)
][["title", "release_year"]].head(20))

print("\n========== TITLES PRODUCED IN INDIA ==========")
india_titles = df[df["country"].str.contains("India", case=False, na=False)]
print(india_titles[["title", "country", "type"]].head(20))

print("\n========== TV SHOWS FROM UNITED STATES ==========")
us_tv = df[
    (df["type"] == "TV Show") &
    (df["country"].str.contains("United States", case=False, na=False))
]
print(us_tv[["title", "country", "type"]].head(20))

print("\n========== MOVIES WITH PG-13 RATING ==========")
pg13 = df[
    (df["type"] == "Movie") &
    (df["rating"] == "PG-13")
]
print(pg13[["title", "rating"]].head(20))

df["Title Length"] = df["title"].str.len()

df["Movie Age"] = 2026 - df["release_year"]

df["Is Recent Content"] = df["release_year"] >= 2020

print("\n========== NEW COLUMNS ==========")
print(df[[
    "title",
    "release_year",
    "Title Length",
    "Movie Age",
    "Is Recent Content"
]].head())

# Longest title
longest_index = df["Title Length"].idxmax()
print("\n========== LONGEST TITLE ==========")
print(df.loc[longest_index, "title"])

# Shortest title
shortest_index = df["Title Length"].idxmin()
print("\n========== SHORTEST TITLE ==========")
print(df.loc[shortest_index, "title"])

# Average movie age
print("\n========== AVERAGE MOVIE AGE ==========")
print(df["Movie Age"].mean())

# Oldest content
oldest_index = df["release_year"].idxmin()
print("\n========== OLDEST CONTENT ==========")
print(df.loc[oldest_index, ["title", "release_year"]])

# Newest content
newest_index = df["release_year"].idxmax()
print("\n========== NEWEST CONTENT ==========")
print(df.loc[newest_index, ["title", "release_year"]])

print("\n========== TITLES IN UPPERCASE ==========")
print(df["title"].str.upper().head(10))

print("\n========== TITLES IN LOWERCASE ==========")
print(df["title"].str.lower().head(10))

print("\n========== LENGTH OF EACH TITLE ==========")
print(df[["title", "Title Length"]].head(10))

print("\n========== TITLES CONTAINING 'LOVE' ==========")
print(df[
    df["title"].str.contains("Love", case=False, na=False)
][["title", "type"]].head(20))

print("\n========== TITLES CONTAINING 'LIFE' ==========")
print(df[
    df["title"].str.contains("Life", case=False, na=False)
][["title", "type"]].head(20))

print("\n========== TITLES STARTING WITH 'THE' ==========")
print(df[
    df["title"].str.startswith("The", na=False)
][["title", "type"]].head(20))

print("\n========== TITLES ENDING WITH 'STORY' ==========")
print(df[
    df["title"].str.endswith("Story", na=False)
][["title", "type"]].head(20))

# Extract Primary Genre
df["Primary Genre"] = df["listed_in"].str.split(",").str[0].str.strip()

print("\n========== PRIMARY GENRE ==========")
print(df[["title", "listed_in", "Primary Genre"]].head(10))


print("\n========== NUMBER OF MOVIES AND TV SHOWS ==========")
content_count = df.groupby("type")["title"].count()
print(content_count)

print("\n========== AVERAGE RELEASE YEAR BY CONTENT TYPE ==========")
average_year = df.groupby("type")["release_year"].mean()
print(average_year)

print("\n========== TITLES IN EACH RATING CATEGORY ==========")
rating_count = df.groupby("rating")["title"].count().sort_values(
    ascending=False
)
print(rating_count)

# Countries
country_count = (
    df["country"]
    .str.split(",")
    .explode()
    .str.strip()
    .value_counts()
)

print("\n========== TOP 10 COUNTRIES ==========")
print(country_count.head(10))

# Directors
director_count = (
    df["director"]
    .str.split(",")
    .explode()
    .str.strip()
    .replace("Unknown", np.nan)
    .dropna()
    .value_counts()
)

print("\n========== TOP 10 DIRECTORS ==========")
print(director_count.head(10))

# Genres
genre_count = (
    df["listed_in"]
    .str.split(",")
    .explode()
    .str.strip()
    .value_counts()
)

print("\n========== TOP 10 GENRES ==========")
print(genre_count.head(10))

print("\n========== PIVOT TABLE: COUNTRY VS TYPE ==========")

country_type_pivot = pd.pivot_table(
    df,
    index="country",
    columns="type",
    values="title",
    aggfunc="count",
    fill_value=0
)

print(country_type_pivot.head(20))

print("\n========== PIVOT TABLE: RELEASE YEAR VS TYPE ==========")

year_type_pivot = pd.pivot_table(
    df,
    index="release_year",
    columns="type",
    values="title",
    aggfunc="count",
    fill_value=0
)

print(year_type_pivot)

# Use a copy so original df remains convenient for other operations
multi_df = df.copy()

multi_df["Country"] = multi_df["country"].str.split(",").str[0].str.strip()

multi_df = multi_df.set_index(["Country", "type"]).sort_index()

print("\n========== MULTIINDEX ==========")
print(multi_df.head(10))

print("\n========== MOVIES FROM INDIA ==========")

try:
    india_movies = multi_df.loc[("India", "Movie")]
    print(india_movies[["title", "release_year"]].head(20))
except KeyError:
    print("No India Movie records found.")

print("\n========== TV SHOWS FROM UNITED STATES ==========")

try:
    us_tv_multi = multi_df.loc[("United States", "TV Show")]
    print(us_tv_multi[["title", "release_year"]].head(20))
except KeyError:
    print("No United States TV Show records found.")

movies_df = df[df["type"] == "Movie"].copy()
tv_df = df[df["type"] == "TV Show"].copy()

print("\n========== MOVIES DATAFRAME ==========")
print(movies_df.head())

print("\n========== TV SHOWS DATAFRAME ==========")
print(tv_df.head())

# Concatenate Movies and TV Shows
combined_df = pd.concat([movies_df, tv_df], ignore_index=True)

print("\n========== CONCATENATED DATAFRAME ==========")
print(combined_df.head())
print("Shape:", combined_df.shape)

# Separate DataFrames
title_director = df[["title", "director"]].copy()
title_country = df[["title", "country"]].copy()

print("\n========== TITLE & DIRECTOR ==========")
print(title_director.head())

print("\n========== TITLE & COUNTRY ==========")
print(title_country.head())

# Merge
merged_df = pd.merge(
    title_director,
    title_country,
    on="title",
    how="inner"
)

print("\n========== MERGED DATAFRAME ==========")
print(merged_df.head())

# Convert date_added to datetime
df["date_added"] = pd.to_datetime(
    df["date_added"],
    errors="coerce"
)

# Extract year and month
df["Year Added"] = df["date_added"].dt.year

df["Month Added"] = df["date_added"].dt.month_name()

print("\n========== YEAR AND MONTH ADDED ==========")
print(df[[
    "title",
    "date_added",
    "Year Added",
    "Month Added"
]].head(10))

# Number added each year
year_added = (
    df.groupby(["Year Added", "type"])
    .size()
    .unstack(fill_value=0)
)

print("\n========== CONTENT ADDED EACH YEAR ==========")
print(year_added)

# Number added each month
month_added = (
    df.groupby(["Month Added", "type"])
    .size()
    .unstack(fill_value=0)
)

# Arrange months chronologically
month_order = [
    "January", "February", "March", "April",
    "May", "June", "July", "August",
    "September", "October", "November", "December"
]

month_added = month_added.reindex(month_order)

print("\n========== CONTENT ADDED EACH MONTH ==========")
print(month_added)

# Total additions by year
total_year_additions = df.groupby("Year Added").size()

print("\n========== TOTAL ADDITIONS BY YEAR ==========")
print(total_year_additions.sort_values(ascending=False))


print("\n")
print("=" * 60)
print("                 NETFLIX SUMMARY REPORT")
print("=" * 60)

# Highest content country
country_content = (
    df["country"]
    .str.split(",")
    .explode()
    .str.strip()
    .replace("Unknown", np.nan)
    .dropna()
    .value_counts()
)

highest_country = country_content.idxmax()
highest_country_count = country_content.max()

print("\n1. Country with highest Netflix content:")
print(highest_country, "-", highest_country_count, "titles")


# Year with highest additions
year_highest = total_year_additions.idxmax()
year_highest_count = total_year_additions.max()

print("\n2. Year with highest number of additions:")
print(year_highest, "-", year_highest_count, "titles")


# Most common rating
common_rating = df["rating"].value_counts().idxmax()
common_rating_count = df["rating"].value_counts().max()

print("\n3. Most common rating:")
print(common_rating, "-", common_rating_count, "titles")


# Most common genre
common_genre = genre_count.idxmax()
common_genre_count = genre_count.max()

print("\n4. Most common genre:")
print(common_genre, "-", common_genre_count, "titles")


# Percentage of Movies and TV Shows
type_percentage = df["type"].value_counts(normalize=True) * 100

print("\n5. Percentage of Movies and TV Shows:")
print(type_percentage.round(2))


# Five meaningful observations
print("\n6. MEANINGFUL OBSERVATIONS:")

print(
    "• Observation 1: The country with the highest number of Netflix titles is",
    highest_country + "."
)

print(
    "• Observation 2:",
    year_highest,
    "has the highest number of Netflix additions."
)

print(
    "• Observation 3: The most common rating is",
    common_rating + "."
)

print(
    "• Observation 4: The most common genre is",
    common_genre + "."
)

print(
    "• Observation 5:",
    round(type_percentage.get("Movie", 0), 2),
    "% of the content consists of Movies, while",
    round(type_percentage.get("TV Show", 0), 2),
    "% consists of TV Shows."
)

print(
    "• Observation 6: Netflix contains more Movies than TV Shows."
    if type_percentage.get("Movie", 0) > type_percentage.get("TV Show", 0)
    else
    "• Observation 6: Netflix contains more TV Shows than Movies."
)

print(
    "• Observation 7: The dataset contains",
    df.shape[0],
    "Netflix titles across",
    df.shape[1],
    "columns."
)

print("=" * 60)
print("                 END OF REPORT")
print("=" * 60)
```
output
~~~
<img width="710" height="896" alt="image" src="https://github.com/user-attachments/assets/8930d215-f101-4fb2-91f9-825295732d8e" />
<img width="703" height="901" alt="image" src="https://github.com/user-attachments/assets/b55c04ed-fa4e-408c-a7ac-9c1324806637" />
<img width="699" height="905" alt="image" src="https://github.com/user-attachments/assets/8d758c10-5f91-4b9c-bd59-4b0fa2002a70" />
<img width="709" height="902" alt="image" src="https://github.com/user-attachments/assets/a4b5f8d0-8030-4a88-ad4d-01cc1118046f" />
<img width="719" height="902" alt="image" src="https://github.com/user-attachments/assets/dbd58890-8b9e-4dd8-96fc-f69b2c65893a" />
<img width="714" height="900" alt="image" src="https://github.com/user-attachments/assets/222a25e6-d724-4f9a-8c28-0360572e4fd8" />
<img width="706" height="887" alt="image" src="https://github.com/user-attachments/assets/de9e4ece-633b-45d1-84ff-f6a285ed149f" />
<img width="704" height="847" alt="image" src="https://github.com/user-attachments/assets/d3cbcbe4-51e5-48b3-ad8b-57888a18b897" />
<img width="716" height="906" alt="image" src="https://github.com/user-attachments/assets/a0391851-16ed-491e-b7fe-f4c8dd3ed8fa" />
<img width="724" height="908" alt="image" src="https://github.com/user-attachments/assets/f1e12609-c034-4898-9c96-2a7ea4768560" />
~~~

Result
Helps Netflix in content planning & investments.








