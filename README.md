<p align="left"><img src="https://cdn-images-1.medium.com/max/184/1*2GDcaeYIx_bQAZLxWM4PsQ@2x.png"></p>

# 💎 Kaggle Competition - Predict Diamond Prices 💎

[Link to competition](https://www.kaggle.com/competitions/dataptmad1121/overview)

## **🎯 Objectives**

The goal of this project is **the prediction of the diamond's price (USD)** based on the following characteristics:

- **Carat**: weight of the diamond 
- **Dimensions** in mm (length:x, width:y, depth:z)
- **Table**: width of top of diamond relative to widest point
- **Depth**: total depth percentage = z / mean(x, y) = 2 * z / (x + y) 
- **Color**: diamond colour, from J (worst) to D (best)
- **Cut**: quality of the cut (Fair, Good, Very Good, Premium, Ideal)
- **Clarity**: a lack of internal defects called inclusions (I1 (worst), SI2, SI1, VS2, VS1, VVS2, VVS1, IF (best))
- **City**: place where the diamond where sold

The prediction is done using:

- **Regression Supervised Learning** (as there is an expected output: the diamond's price).
- **Batch Learning** technique since the data is not live-fed from a source.
- **Prediction's accuracy** is meassured using **RMSE** (Root Mean Square Error).

## **👩🏻‍💻 Resources**

- Python 3.7 and libraries ([Pandas](https://pandas.pydata.org/pandas-docs/stable/reference/index.html), [NumPy](https://numpy.org/doc/stable/user/index.html),[sqlalchemy](https://www.sqlalchemy.org/), [matplotlib](https://matplotlib.org/stable/users/index),[seaborn](https://seaborn.pydata.org/index.html),[sklearn](https://scikit-learn.org/stable/user_guide.html) and [xgboost](https://xgboost.readthedocs.io/en/stable/)).
- Data sets: diamonds_test.csv,diamonds_train.db and sample_submission.csv, downloaded from [kaggle competition](https://www.kaggle.com/competitions/dataptmad1121/overview).

## **🗄 Folder structure**
```
 └── project
    ├── .gitignore
    ├── requirements.txt
    ├── README.md
    ├── code
    │   ├── 0_EDA.ipynb
    │   └── 1_Model_Training_cat_coding_target_coding_mean_XGB   
    └── data
        ├── data_extractor.ipynb
        ├── diamonds_train.db
        ├── diamonds_train.csv
        ├── diamonds_test.csv
        ├── sample_submission.csv      
        └── diamonds_predictions_1.csv
 ```
 
 - **code**: this folder contains the jupyter notebooks that perform the caldulation of diamond's price predition:
    - 0_EDA.ipynb
    - 1_Model_Training_Catcodes_targetcodes_XGB.ipynb
    - 1_Model_Training_Catcodes_targetcodes_XGB.ipynb 
 - **data**: in this folder all data files are stored:
    - Input files (train and test data) needed for the analysis.
    - data_extractor.ipynb needed for extracting the train set from diamonds_train.db in csv.
    - sample_submission.csv as an example of proper format for submit diamond's price preditions in kaggle.
    - diamonds_predictions files for storing diamond's price preditions.
    

## **🔍 Exploratory data analysis**     
*0_EDA.ipynb*

Conclussions:
 
  - The data volume is enough for performing a ML predicion (40455 rows).
  - Both datasets (train and test) are clean (no nulls and a few zero values) so is not needed to do data cleaning.
  - There are some outliers (8% for train and 1% for test), as outliers are included also in the train set it's possible that won't be interesting to exclude them.
  - Train and test distribution is very similar for the different features (data balancing won't be needed).
  - Carat is the feature with more correlation with the price (table and depht have a week correlation)    

 
## **🙌 Winner solution**   

*1_Model_Training_cat_coding_target_coding_mean_XGB*

- Model choosen is **XGBRegressor**
- **All features are considered** in the analysis (XGB has a good perform even there are dependent features or with week correlation in the dataset).
- Data preparation is not needed as:
    - There are not null values.
    - There are few zero values (they are present in train/test and XGB can predict with 0 values)
    - There are few **outliers**, as we can not exclude them in the train, it's better to **keep** them in the train so the model can learn about them.
- **Feature scalling** does not provide better results (data range are not very big)
- **Target encoding using price's mean** seems to reduce error variance.
- **Target encoding using price's std** does not seems to provide better results.
- **Cross target encoding** not seems to provide better results (there is more overfitting).
- **Category encoding** performs better that **one hot encoding** when using tree decission models.
- **Stopping the model from growth** seems to avoid overfitting.

 
 
 