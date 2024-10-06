# **Netflix Data: Cleaning, Analysis, and Visualization**

## **Table of Contents**
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Technologies Used](#technologies-used)
- [Installation](#installation)
- [Usage](#usage)
- [Data Cleaning](#data-cleaning)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Visualization](#visualization)
- [Future Enhancements](#future-enhancements)
- [Contributors](#contributors)

---

## **Project Overview**
This project focuses on cleaning, analyzing, and visualizing a dataset of Netflix content, covering titles available on the platform between 2008 and 2021. The primary goal is to handle missing values, clean the data, and then perform exploratory data analysis (EDA) to uncover trends such as content distribution, popular genres, and growth trends. The project concludes with data visualizations that provide insights into the Netflix catalog.

---

## **Dataset**
The **Netflix Titles Dataset** contains metadata about movies and TV shows available on Netflix. The dataset is publicly available on [Kaggle](https://www.kaggle.com/shivamb/netflix-shows). It includes the following columns:
- **show_id**: Unique identifier for each title.
- **type**: Whether the content is a movie or TV show.
- **title**: Title of the content.
- **director**: Name of the director(s) of the content.
- **cast**: List of actors in the title.
- **country**: The country where the content was produced.
- **date_added**: Date the content was added to Netflix.
- **release_year**: Year when the content was released.
- **rating**: Age-based content rating.
- **duration**: Duration of the content.
- **listed_in**: Genres/categories the title is listed under.

---

## **Technologies Used**
- **Programming Language**: Python
- **Libraries**:
  - `pandas`: For data manipulation and cleaning.
  - `numpy`: For numerical computations.
  - `matplotlib`, `seaborn`: For data visualization.
  - `wordcloud`: For creating word clouds of popular genres and titles.
- **Visualization Tool**: Tableau for interactive data visualization.

---

## **Installation**

### **Step 1: Clone the Repository**
To start with the project, clone the repository by running the following command in your terminal:
```bash
git clone https://github.com/your-username/Netflix-Data-Cleaning-Analysis-Visualization.git
cd Netflix-Data-Cleaning-Analysis-Visualization
```

### **Step 2: Create a Virtual Environment (Optional)**
It is recommended to create a virtual environment to manage dependencies:
```bash
python -m venv venv
source venv/bin/activate      # For MacOS/Linux
venv\Scripts\activate         # For Windows
```

### **Step 3: Install Dependencies**
Install all required dependencies using the `requirements.txt` file:
```bash
pip install -r requirements.txt
```

---

## **Usage**

### **Step 1: Jupyter Notebooks**
The project includes two Jupyter notebooks that contain all code and visualizations:

1. **Data_Cleaning.ipynb**: This notebook includes all the data cleaning steps, including handling missing values, duplicates, and data type corrections.
2. **EDA_and_Visualization.ipynb**: This notebook performs exploratory data analysis and visualizes the insights using matplotlib, seaborn, and word clouds.

### **Step 2: Tableau Dashboard**
The project also includes a Tableau dashboard for interactive visualizations. You can open the **`netflix_visualization.twb`** file in Tableau to explore the data further and interact with the visualizations.

---

## **Data Cleaning**
The dataset required significant data cleaning before proceeding with the analysis. Key steps included:
- **Handling Missing Values**: Some columns like `director` and `country` had missing values, which were either dropped or filled with the most frequent values.
- **Data Type Conversion**: The `date_added` column was converted to a datetime format for time-based analysis.
- **Removing Duplicates**: Duplicate entries were dropped to ensure the dataset was clean and ready for analysis.

```python
# Load dataset
import pandas as pd

data = pd.read_csv('netflix_titles.csv')

# Handle missing values and data type conversion
data_cleaned = data.dropna(subset=['director', 'country'])
data_cleaned['date_added'] = pd.to_datetime(data_cleaned['date_added'])

# Remove duplicates
data_cleaned.drop_duplicates(subset=['show_id'], inplace=True)

print(data_cleaned.info())
```

---

## **Exploratory Data Analysis (EDA)**
In the **EDA_and_Visualization.ipynb** notebook, various analyses were conducted, including:

### **1. Content Type Distribution**
Analyzed the split between movies and TV shows on Netflix.

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Distribution of content type
plt.figure(figsize=(8,6))
sns.countplot(x='type', data=data_cleaned, palette='Set2')
plt.title('Distribution of Movies vs. TV Shows on Netflix')
plt.show()
```

### **2. Yearly Content Addition**
Tracked the number of movies and TV shows added to Netflix each year.

```python
# Content added by year
data_cleaned['year_added'] = data_cleaned['date_added'].dt.year
plt.figure(figsize=(12,6))
sns.countplot(x='year_added', data=data_cleaned, palette='coolwarm')
plt.title('Number of Titles Added to Netflix by Year')
plt.xticks(rotation=45)
plt.show()
```

### **3. Top Genres**
Identified the most popular genres/categories on Netflix.

```python
# Analyze genres
data_cleaned['genres'] = data_cleaned['listed_in'].apply(lambda x: x.split(', '))
all_genres = sum(data_cleaned['genres'], [])

# Visualize most common genres
genre_counts = pd.Series(all_genres).value_counts().head(10)
plt.figure(figsize=(10,6))
sns.barplot(x=genre_counts.values, y=genre_counts.index, palette='Set3')
plt.title('Top 10 Most Common Genres on Netflix')
plt.show()
```

### **4. Word Cloud of Movie Titles**
Created a word cloud from the movie titles to show the most frequent words in the titles.

```python
from wordcloud import WordCloud

# Word cloud for movie titles
movie_titles = data_cleaned[data_cleaned['type'] == 'Movie']['title']
wordcloud = WordCloud(width=800, height=400, background_color='black').generate(' '.join(movie_titles))

# Display word cloud
plt.figure(figsize=(10,6))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.show()
```

---

## **Visualization**
Using Tableau, we built interactive dashboards to analyze Netflix's content more dynamically. Key dashboards include:
1. **Content Added by Year**: A line chart tracking the number of titles added each year.
2. **Content Breakdown by Type**: Pie charts and bar charts showing the distribution between movies and TV shows.
3. **Top Genres**: Visualizations of the most common genres on Netflix.
4. **Content Distribution by Country**: Map visualizations showing the number of titles available by country.

You can open the `netflix_visualization.twb` file in Tableau to explore the interactive dashboard.

---

## **Future Enhancements**
- **Advanced Insights**: Use machine learning techniques to predict content trends, such as which genres might dominate in the future.
- **Recommendations**: Develop a recommendation system that suggests content to users based on their viewing history and preferences.
- **Integration with Viewer Data**: If user viewership data were available, we could analyze which content is most popular among different demographics or regions.

---

## **Contributors**
- **Kathiravan K**: [GitHub Profile](https://github.com/kathir-btech)

---
