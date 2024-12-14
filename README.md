# FIFA Machine Learning Project
# Data Engineering
# Joseph Kennedy, Jacob Woolley, Micah Korinek, Viktor Bondarenko
This project tests a variety of Machine Learning regressions on FIFA players' in-game stats listed in FC 25 to predict their wages.
# Getting the Dataset
The dataset used was found by exploring Kaggle for sufficiently large datasets within our area of interest, sports statistics. Ultimately, we settled on a FIFA dataset uploaded Mukesh Manral (https://www.kaggle.com/datasets/mukeshmanral/fifa-data-for-eda-and-stats) which was developed by scraping SoFIFA's API, a third-party website for collecting in-game player information from the FIFA/FC videogame series across a wide variety of categories. This data was then reformatted into a usable CSV in the repository under the name updated_player_data_VV.csv.

![Sample Player Statistics](https://github.com/user-attachments/assets/8f65b906-3df4-4d91-98bb-5d03e98326aa)


# Initial Data Analysis

![Age vs Wage](https://github.com/user-attachments/assets/3affe13d-eeda-4ca0-8a37-4d64b310dc69)

![Age vs Wage](https://github.com/user-attachments/assets/52f7c3ff-d300-4b7e-81c2-cab06f8c31b0)

Age vs Wage - used initially to assess if the behavior of the data acts normally and in a bell curve. It gives us an idea of the average age for the dataset and shows that most players’ wages are under 100k. We can also see more outliers as age increases and the most extreme outliers in terms of wage are actually much older than the average age. This would make sense as these point represent the most popular players, whose international appeal might account for their continued success where many others in their age range are paid significantly less. Looking at the orange dots, the goalkeepers, we can also see an interesting trend, that they represent the very oldest members of the league – likely due to the less physical nature of goalkeeping when compared to other soccer positions.

![Stats vs Wage Goalkeepers](https://github.com/user-attachments/assets/edef4369-a080-43a2-880c-648cceb82a49)

All stats vs age – this was another quick test to see if the data was behaving normally. While we would expect the wages to increase as overall scores increase, we can notice an uptick in wages at a much lower total stat score than we might expect, in addition to the expected increase towards the higher scores. In order to explore the potential causes of this anomaly, we looked into the different positions to see if there was a position with a more unusual distribution that might be throwing off a regular curve. Ultimately, what we found was that Goalkeepers were the position advancing at a different rate. This makes sense, as the statistics only have 5 skills directly related to goalkeeping, compared to the 28 that aren’t related to goalkeeping. This means that a goal score for a goalkeeper is going to be lower than for other players and might have a significant effect on any model we might try to make if left in the dataset.

# PostGres
  Utilize tableschema.SQL to create the Player table in PostgreSQL under a new database. Once the table is set up, upload the cleaned CSV files into the newly created PostgreSQL table. 

# Pandas
  To retrieve data from PostgreSQL, begin to run MachineLearningTesting.ipynb with SQLAlchemy and psycopg2 installed. Edit the config.py to connect the code to your own Postgres server. Once you have the data, continue down the notebook to further prepare the data for the ML models. First, we used regex to ensure the wage column, the target variable, is numeric. We the removed the daysjoined column, as it had missing data, the value and potential columns, as they were too closely related to wage and not the player's statistics, as well as the ID and non-numeric columns in order to run the models. Additionally, we decided to filter out players who play goalkeeper as they have a pay curve separate from other players' stats as they are a purely defensive role.

# Preparing the Data for Modeling
We separated the target variable, wage, as Y and the features as X and then separated these new data frames into training and testing data in an 80%-20% split. We also scaled the features to reduce noise from numerically unbalanced scales across columns.

# ML Models used
- Linear Regression
- Random Forest
- KNN (5,2,12)
- SVC (linear,RBF)
- XGBoost
- Neural Network (MAE 50 Epochs)

# Results
![resultsp4](https://github.com/user-attachments/assets/c61f372b-f0dc-416b-acd9-e7ba405b4561)

Of the models tested, Random Forest and XGBoost were the most powerfully predictive. As both these models are decision tree-based, this may be reflective of the way players are valued within the original system on their individual stats, with players reaching certain thresholds of greatness within categories pushing them into higher wage-based values than those who do not. Ultimately, we decided that XGBoost is the best model for this dataset as its parallel decision-making is most in line with the way the various individual stats (dribbling, aggression, volleys, etc.) are divided into broader categories (skill, mentality, attacking, etc.)


# Ethical reasoning
This project is being used, purely, for educational purposes. All code included is original and based on code presented in class. Additional sources of information on coding below.

# Resources
- https://www.kaggle.com/datasets/mukeshmanral/fifa-data-for-eda-and-stats
- https://sofifa.com
- https://scikit-learn.org/dev/modules/generated/sklearn.ensemble.RandomForestRegressor.html
- https://scikit-learn.org/dev/modules/generated/sklearn.linear_model.LinearRegression.html
- https://scikit-learn.org/dev/modules/generated/sklearn.neighbors.KNeighborsRegressor.html#sklearn.neighbors.KNeighborsRegressor
- https://scikit-learn.org/dev/modules/generated/sklearn.neighbors.KNeighborsRegressor.html#sklearn.neighbors.KNeighborsRegressor
- https://xgboost.readthedocs.io/en/stable/
