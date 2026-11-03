Movie Recommendations using Machine Learning
=======

This project implements a movie recommendation system using the MovieLens
ml-latest-small dataset. It combines content-based filtering and item-based
collaborative filtering to estimate movie ratings and recommend movies that a
user has not already rated.

The ml-latest-small dataset contains 100836 ratings and 3683 tag applications
across 9742 movies. The ratings were created by 610 users between March 29,
1996 and September 24, 2018. The dataset was generated on September 26, 2018.

The project uses the files `movies.csv`, `ratings.csv`, `tags.csv` and
`links.csv`. Movie genres and user-generated tags are combined to create
metadata for the content-based recommendation model. User ratings are used
to build the collaborative filtering model.

The main output is a ranked list of movies recommended for a selected user,
along with predicted ratings and movie genres.


Project Files
=============

The project directory should contain the following files:

* `Movie_recommendations_commented.ipynb` - Jupyter Notebook containing the
  complete implementation of the recommendation system.
* `movies.csv` - Movie identifiers, titles and genres.
* `ratings.csv` - User ratings for movies.
* `tags.csv` - User-generated tags applied to movies.
* `links.csv` - Movie identifiers used to connect to IMDb and TMDb.
* `README.txt` - Project description and instructions.


Dataset Information
===================

The dataset is MovieLens ml-latest-small, provided by GroupLens Research at
the University of Minnesota.

The dataset contains anonymized user identifiers and does not include
demographic information. Each included user rated at least 20 movies.

The four CSV files use UTF-8 encoding and contain a header row. Movie IDs are
consistent across the movies, ratings, tags and links files.

The dataset is a development dataset. Refer to the official GroupLens
documentation for complete dataset information and usage conditions:

http://grouplens.org/datasets/
http://movielens.org


Content and Use of Files
========================

Movies Data File (movies.csv)
-----------------------------

This file contains information about each movie. Its columns are:

    movieId,title,genres

The `movieId` column identifies the movie, `title` contains the movie title
and release year, and `genres` contains a pipe-separated list of genres.

Ratings Data File (ratings.csv)
-------------------------------

This file contains the ratings given by users. Its columns are:

    userId,movieId,rating,timestamp

Ratings use a five-star scale with half-star increments, from 0.5 to 5.0.
Each row represents one user's rating of one movie.

Tags Data File (tags.csv)
-------------------------

This file contains tags added by users to movies. Its columns are:

    userId,movieId,tag,timestamp

Tags are user-generated descriptions, usually consisting of a word or short
phrase. In this project, tags are converted to lowercase and grouped by movie
to help form movie metadata.

Links Data File (links.csv)
---------------------------

This file contains identifiers that connect MovieLens movies to external
movie databases. Its columns are:

    movieId,imdbId,tmdbId

The current recommendation workflow loads this file along with the other
dataset files. The main recommendation models use movie genres, tags and
ratings.


Requirements
============

The project uses Python and the following libraries:

* NumPy
* pandas
* Matplotlib
* Seaborn
* scikit-learn
* SciPy
* Jupyter Notebook

Install the required libraries with:

    pip install numpy pandas matplotlib seaborn scikit-learn scipy jupyter


Running the Project
===================

1. Keep the notebook and all four CSV files in the same working directory.
2. Install the required Python libraries.
3. Open a terminal or command prompt in the project directory.
4. Start Jupyter Notebook:

       jupyter notebook

5. Open `Movie_recommendations_commented.ipynb`.
6. Run the notebook cells in order, from the first cell to the last cell.
7. The final cell generates the top five movie recommendations for user ID 4.

To generate recommendations for a different user, change the `user_id`
argument in the final cell. Change `top_n` to request a different number of
recommendations.


Recommendation Methods
======================

Content-Based Filtering
-----------------------

The content-based model uses movie genres and tags as movie metadata.
`TfidfVectorizer` converts this text into numerical features, and cosine
similarity measures how similar movies are.

The model predicts a user's rating for a movie by comparing it with movies
the user has already rated.

Item-Based Collaborative Filtering
----------------------------------

The collaborative filtering model uses the rating patterns of users across
movies. It creates a user-movie rating matrix, centres ratings around each
user's average, and calculates movie-to-movie cosine similarities.

The prediction is based on the ratings the user gave to similar movies.
The model considers up to 20 of the most similar rated movies.

Dynamic Hybrid Model
--------------------

The hybrid model combines the content-based and collaborative filtering
predictions. It adjusts the contribution of each model according to the
number of ratings available for the target movie.

Movies with more training ratings receive more weight from collaborative
filtering. Movies with fewer ratings receive more weight from content-based
filtering.


Evaluation
==========

The notebook splits the ratings into training and testing sets using an
80:20 split and a fixed random state of 42.

It calculates a user-mean baseline and evaluates prediction performance
using Root Mean Squared Error (RMSE).

The notebook also includes:
* A rating distribution and movie popularity exploration.
* A cold-start comparison using a sample of test ratings.
* A comparison between an evaluation based on training data and a
  deliberately leaky user-mean calculation using all ratings.

The leaky calculation is included to demonstrate data leakage. It should not
be treated as a valid estimate of model performance because it uses rating
information from the test data.

The cold-start section samples 200 test records. It compares collaborative
filtering predictions with content-based predictions for those records; it
does not remove movies from the original dataset.


Usage and Dataset License
=========================

The MovieLens dataset is provided by GroupLens Research. Please follow the
usage conditions supplied with the original dataset. The dataset must not be
presented as endorsed by the University of Minnesota or GroupLens Research.

For research publications using the dataset, acknowledge MovieLens and cite
the following paper:

F. Maxwell Harper and Joseph A. Konstan. 2015. The MovieLens Datasets:
History and Context. ACM Transactions on Interactive Intelligent Systems
(TiiS) 5, 4: 19:1-19:19.
https://doi.org/10.1145/2827872

The dataset is provided without a guarantee that it is correct or suitable
for every purpose. Use of external movie resources is subject to the terms
of their respective providers.


Further Information
====================

MovieLens:
http://movielens.org

GroupLens datasets:
http://grouplens.org/datasets/

GroupLens Research:
https://grouplens.org/
