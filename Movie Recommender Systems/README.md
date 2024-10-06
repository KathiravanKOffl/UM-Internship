# **Movie Recommender System**

## **Table of Contents**
- [Project Overview](#project-overview)
- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Technologies Used](#technologies-used)
- [Installation](#installation)
- [Usage](#usage)
- [Model and Algorithms](#model-and-algorithms)
- [Evaluation](#evaluation)
- [Deployment](#deployment)
- [Future Enhancements](#future-enhancements)
- [Contributors](#contributors)

---

## **Project Overview**
The **Movie Recommender System** is a machine learning project aimed at providing personalized movie recommendations to users based on their previous ratings and preferences. The system leverages collaborative filtering techniques using the **Singular Value Decomposition (SVD)** algorithm to predict the ratings a user may give to movies they haven’t yet rated. It is designed to suggest movies that the user is likely to enjoy.

---

## **Project Structure**

```
Movie-Recommender-System/
│
├── data/
│   ├── movies_metadata.csv       # Movies metadata dataset
│   ├── ratings_small.csv         # User ratings dataset
│
├── notebooks/
│   ├── EDA.ipynb                 # Exploratory Data Analysis
│   ├── Model_Building.ipynb      # Model building and training
│
├── app/
│   ├── app.py                    # Flask API for recommendation
│
├── requirements.txt              # Required libraries and dependencies
├── README.md                     # Project documentation
└── LICENSE                       # License for the project
```

---

## **Dataset**
This project uses the **MovieLens** dataset, which includes metadata for movies and user ratings for movies. The dataset consists of:
- **movies_metadata.csv**: Contains details of over 45,000 movies such as title, genres, release date, etc.
- **ratings_small.csv**: Contains 100,000 movie ratings from 700 users.

You can download the dataset from [MovieLens Dataset](https://grouplens.org/datasets/movielens/latest/).

---

## **Technologies Used**
- **Programming Language**: Python
- **Libraries**:
  - `pandas`, `numpy`: For data manipulation and cleaning.
  - `matplotlib`, `seaborn`: For data visualization.
  - `scikit-learn`: For machine learning model development and evaluation.
  - `surprise`: For collaborative filtering with the SVD algorithm.
  - `Flask`: For deploying the recommendation system as an API.

---

## **Installation**

### **Step 1: Clone the Repository**
To start with the project, clone the repository by running the following command in your terminal:
```bash
git clone https://github.com/your-username/Movie-Recommender-System.git
cd Movie-Recommender-System
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

### **Step 1: Jupyter Notebook**
You can explore the **EDA** and **model building** process in the notebooks provided. The steps include:
1. **EDA.ipynb**: Contains exploratory data analysis of the dataset.
2. **Model_Building.ipynb**: Contains the collaborative filtering model training using the SVD algorithm.

### **Step 2: Flask Application**
To run the Flask app locally:
```bash
cd app
python app.py
```
This will start a local Flask server at `http://127.0.0.1:5000`.

You can access the recommendation API by making a GET request to:
```
http://127.0.0.1:5000/recommend?user_id=1
```
This will return a JSON response containing movie recommendations for the specified user.

---

## **Model and Algorithms**

### **Collaborative Filtering**:  
This project implements **collaborative filtering** using the **Singular Value Decomposition (SVD)** algorithm from the `surprise` library. SVD is a matrix factorization technique used to predict missing values in user-item matrices.

```python
from surprise import SVD, Dataset, Reader
from surprise.model_selection import cross_validate

# Load the dataset
reader = Reader(rating_scale=(0.5, 5.0))
data = Dataset.load_from_df(ratings[['userId', 'movieId', 'rating']], reader)

# Build and train the SVD model
svd = SVD()
cross_validate(svd, data, measures=['RMSE', 'MAE'], cv=5, verbose=True)
trainset = data.build_full_trainset()
svd.fit(trainset)
```

### **Recommendation Function**
The function `recommend_movies(user_id, num_recommendations)` predicts the ratings for movies a user hasn’t rated and recommends the top-rated ones.

```python
def recommend_movies(user_id, num_recommendations=10):
    movie_ids = movies['id'].unique()
    movie_ratings = [svd.predict(user_id, movie_id).est for movie_id in movie_ids]
    recommendations = pd.DataFrame({'movieId': movie_ids, 'predicted_rating': movie_ratings})
    recommendations = recommendations.sort_values(by='predicted_rating', ascending=False)
    return recommendations.head(num_recommendations)
```

---

## **Evaluation**
The model's performance is evaluated using two metrics:
1. **Root Mean Square Error (RMSE)**
2. **Mean Absolute Error (MAE)**

Both metrics provide an understanding of how well the model predicts movie ratings.

```python
from surprise.model_selection import cross_validate

# Cross-validation results
results = cross_validate(svd, data, measures=['RMSE', 'MAE'], cv=5, verbose=True)
print("Average RMSE:", np.mean(results['test_rmse']))
print("Average MAE:", np.mean(results['test_mae']))
```

---

## **Deployment**
The recommender system is deployed using a **Flask** web application. It provides an API endpoint that accepts a user ID and returns a list of recommended movies in JSON format.

### **Steps to Deploy**
1. Run the Flask application locally using the command:
   ```bash
   python app.py
   ```

2. Make GET requests to the `/recommend` endpoint to receive movie recommendations.

For production deployment, you can consider deploying the Flask app on cloud services like **Heroku** or **AWS**.

---

## **Future Enhancements**
- **Content-Based Filtering**: Add metadata features like genres, directors, and cast to enhance recommendations.
- **Hybrid Recommender System**: Combine collaborative filtering with content-based filtering to provide more robust recommendations.
- **User Interface**: Develop a front-end interface that allows users to interact with the recommender system more intuitively.

---

## **Contributors**
- **Kathiravan K**: [GitHub Profile](https://github.com/kathir-btech)
