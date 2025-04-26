import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
sns.set(style="whitegrid")
from google.colab import files
uploaded = files.upload()
filename = list(uploaded.keys())[0]
df = pd.read_csv(filename)
df['country'] = df['country'].fillna('Unknown')
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce')
df['release_year'] = pd.to_numeric(df['release_year'], errors='coerce')
df['listed_in'] = df['listed_in'].fillna('Unknown')

plt.figure(figsize=(6, 4))
sns.countplot(data=df, x='type', palette='Set2')
plt.title("Number of Movies vs TV Shows on Netflix")
plt.xlabel("Type")
plt.ylabel("Count")
plt.tight_layout()
plt.show()

top_countries = df['country'].value_counts().head(10)
plt.figure(figsize=(8, 5))
top_countries.plot(kind='bar', color='coral')
plt.title("Top 10 Countries with Most Netflix Titles")
plt.xlabel("Country")
plt.ylabel("Number of Titles")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

df['year_added'] = df['date_added'].dt.year
titles_per_year = df['year_added'].value_counts().sort_index()
plt.figure(figsize=(10, 5))
titles_per_year.plot(kind='line', marker='o')
plt.title("Netflix Titles Added Over the Years")
plt.xlabel("Year")
plt.ylabel("Number of Titles Added")
plt.grid(True)
plt.tight_layout()
plt.show()

print("Top 10 Categories on Netflix:")
print(df['listed_in'].value_counts().head(10))

release_counts = df['release_year'].dropna().value_counts().sort_index()
plt.figure(figsize=(14, 6))
release_counts.plot(kind='bar', color='skyblue')
plt.title("Number of Titles Released by Year")
plt.xlabel("Release Year")
plt.ylabel("Count")
plt.tight_layout()
plt.show()

release_years = df['release_year'].dropna().astype(int)
plt.figure(figsize=(14,6))
sns.histplot(release_years, bins=50, kde=True, color='skyblue')
plt.title("Distribution of Release Years")
plt.xlabel("Release Year")
plt.ylabel("Frequency")
plt.tight_layout()
plt.show()

plt.figure(figsize=(10,5))
sns.countplot(data=df, x='rating', order=df['rating'].value_counts().index[:10], palette="coolwarm")
plt.title("Top Content Ratings")
plt.xticks(rotation=50)
plt.tight_layout()
plt.show()

from collections import Counter

genres_series = df['listed_in'].dropna().apply(lambda x: x.split(', '))
genre_counts = Counter([genre for sublist in genres_series for genre in sublist])

top_genres = pd.DataFrame(genre_counts.most_common(10), columns=['Genre', 'Count'])

plt.figure(figsize=(10, 6))
sns.barplot(x='Count', y='Genre', data=top_genres, palette='magma')
plt.title("Top 10 Most Common Genres on Netflix")
plt.xlabel("Number of Titles")
plt.ylabel("Genre")
plt.tight_layout()
plt.show()

movies = df[df['type'] == 'Movie']
movies['minutes'] = movies['duration'].str.replace(' min', '').replace('Unknown', np.nan).astype(float)
plt.figure(figsize=(10,4))
sns.histplot(movies['minutes'].dropna(), bins=40, color='green')
plt.title("Distribution of Movie Durations")
plt.xlabel("Duration (minutes)")
plt.tight_layout()
plt.show()

