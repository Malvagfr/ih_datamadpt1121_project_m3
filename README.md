<p align="left"><img src="https://cdn-images-1.medium.com/max/184/1*2GDcaeYIx_bQAZLxWM4PsQ@2x.png"></p>

# 💎 Kaggle Competition - Predict Diamond Prices 💎

[Link to competition](https://www.kaggle.com/competitions/dataptmad1121/overview)

## **🎯 Objectives**

The goal of this project is **the prediction of the diamond's price (USD)** based on the following characteristics:

- **Carat**: weight of the diamond 
- **Dimensions** in mm (length:x, width:y, depth:z)
- **Table**: width of top of diamond relative to widest point
- **Depth**: total depth percentage = z / mean(x, y) = 2 * z / (x + y) 
- **Color**: diamond colour, from (J (worst), I, H, G, F, E, D (best))
- **Cut**: quality of the cut (Fair, Good, Very Good, Premium, Ideal)
- **Clarity**: lack of internal defects called inclusions (I1 (worst), SI2, SI1, VS2, VS1, VVS2, VVS1, IF (best))
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
    │   └── 2_Model_Training_cat_coding_target_coding_mean_XGB
    └── data
        ├── data_extractor.ipynb
        ├── diamonds_train.db
        ├── diamonds_train.csv
        ├── diamonds_test.csv
        ├── sample_submission.csv 
        ├── diamonds_predictions_1.csv 
        └── diamonds_predictions_2.csv
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
[*0_EDA.ipynb*](https://github.com/Malvagfr/ih_datamadpt1121_project_m3/blob/main/code/0_EDA.ipynb)

Conclussions:
 
  - The data volume is enough for performing a ML predicion (40455 rows).
  - Both datasets (train and test) are clean (no nulls and few zero values) so is not needed to do data cleaning.
  - There are some outliers (8% for train and 1% for test), as outliers are included also in the train set it's possible that won't be interesting to exclude them.
  - Train and test distribution are very similar for the different features (data balancing won't be needed).
  - Carat (diamond's weight) has the strongest correlation with the price (table and depht have a week correlation). 
  
  
## **🔧 Options Performance**    


| Outliers | Scaling | One hot encod | Cat encod | Target encod Mean  |Targ encod Std  | Cross Targ encod | Model | RMSE Train | RMSE Val | RMSE Test |
|---------|----------|-------|------------|----------|-------|------------|----------|----------|----------|----------|
| Yes  | No | No | Yes | No | No| No |RandomForestRegressor | 491 | 581 | 583 |
| Yes  | No | No | Yes | No | No| No |XGBRegressor | 390 | 538 | 543 |
| **Yes**  | No | **No** | **Yes** | **Yes** | **No**| **No** | **XGBRegressor** | **447** | **529** | **528** |
| Yes  | No | No | Yes | Yes | No| Yes |XGBRegressor | 395 | 529 | 532 |
| **Yes**  | No | **No** | **Yes** | **Yes** | **Yes**| **No** |**XGBRegressor** | **406** | **529** | **529** |
| Yes  |  Yes | No | Yes | Yes | Yes | No |XGBRegressor | 447 | 529 | 532 |
 

## **🙌 Winner solution: conclusions**   

[*1_Model_Training_cat_coding_target_coding_mean_XGB*](https://github.com/Malvagfr/ih_datamadpt1121_project_m3/blob/main/code/1_Model_Training_cat_coding_target_coding_mean_XGB.ipynb)

- Model choosen is **XGBRegressor**
- **All features are considered** in the analysis (XGB has a good perform even when there are correlated features or with weak correlation with the target).
- Data preparation is not needed as:
    - There are not null values.
    - There are few zero values (they are present in train/test and XGB can predict with 0 values)
    - There are few **outliers**, as is not possible to exclude them from the test, it's better to **keep** them so the model can learn.
- **Feature scalling** does not provide better results (data range is slight).
- **Target encoding using price's mean** seems to reduce error variance.
- **Target encoding using price's std** does not seems to provide better results.
- **Cross target encoding** not seems to provide better results (it cause overfitting).
- **Category encoding** performs better that **one hot encoding** when using tree decission models.
- **Stopping the model from growth** customizing model's hyperparameters seems to avoid overfitting.

 
 
 