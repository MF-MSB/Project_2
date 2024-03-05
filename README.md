# Project_2

## Explain project

Develop CarGenie a tool for predicting used car prices using public data. The "Vehicle Sales and Market Trends Dataset" provides a comprehensive collection of information pertaining to the sales transactions of various vehicles. This dataset encompasses details such as the year, make, model, trim, body type, transmission type, VIN (Vehicle Identification Number), state of registration, condition rating, odometer reading, exterior and interior colors, seller information, MMR (Manheim Market Report) values, selling price, and sale date.

https://www.kaggle.com/datasets/syedanwarafridi/vehicle-sales-data/data


## Project_Diary

    #02/22/24 Data exploration. Loaded csv into Jupyter notebook
    - Initial thoughts
            1. It's a large data matrix 558,837 rows and 16 columns. 
            2. 'make' has some heterogeneity and conflicts in vehicle naming. Multiple reasons for this including first letter capitalization and truck references.
            3. 'transmission' has a disprotional number of missing values. There are errors in the 'tranmission values' pointing to a column shift. I'm calling this the 2015 Jetta problem.
            4. 'make' has 10301 missing values. All of the rows containing no make data also contain no model, no trim and no body data, triple NaN problem. These can be dropped.
            5. 'interior' contains an unusal symbol for some values in place of NaN, the symbol is —, it's a large -.
            6. 'seller' is highly dispirate and can't see a way to reduce this. Vendor specific financing? Lease vs not lease? Mine with keywords?
            7.  Another thought on 'seller'. Only inlcude data from 'seller' with more than some value, n = 5?
            8. 'seller' alberta looks like it contains errors.
            9. 'seller' nissan-infiniti and nissan infiniti need to be renamed
            10.'seller' ford motor credit company llc and ford motor credit company llc pd  need to be renamed
            11. 'seller' is dealership financing worthwhile?
            12. Tempted to let 'seller' ride pending minor, obvious fixes and see what comes out downstream.
            13. Unique model is tricky to look at and is best viewed by 'make'. Will fix 'make' and then view model data.
            14. Do trucks warrant a separate make value?
            15. A valid VIN number is 17 characters should not consist of the following letters: I (i), O (o), and Q (q). The standard is to use a scored-through 0 to differentiate it from the O letter.
            16. I don't think VIN will help the model but it could serve as a cool look-up tool so I'm leaving it in.
            17. 'state' is contaminated with VINS but the 2015 Jetta problem explains this.
            18. 'condition' tricky. Has min value of 1 max value of 49. Distribution is really weird. Some missing values.
            19. 'odometer' is a little odd. The distribution looks fine but some of the values are very low. correlation between 'odometer' vs 'year'
            20. Inspect low 'odometer'?
            21. color and interior contains a lot of large dash values. This needs thought. Color clearly impacts price of car.
            22. mmr some of the values are very low. I'll make a dataframe that contains all sub 1000 vehicles and then review it.
            23. mmr some of the values are very low. I'll make a dataframe that contains all sub 1000 vehicles and then review it.
            24. saledate. Hmmmmmm. How to review this for problems, datatime. Or a regular expression?
            25. Can I make a new column 'saletime'? Address this as last step of data cleaning. Time is given it 24hr clock format.
            26. Happy with progress Trim is going to be tough.
    
    #02/23/24
            1. Fixed the tranmission frame shift identified by the presence of the words sedan/Sedan in the 'transmission' column. Notes on this:
                a) I made a new DataFrame containing only the rows with sedan or Sedan in the 'transmission' column. This has 26 rows.
                b) The 26 rows are all from the same year, make, model and trim of car. 2015, Volkwagen, Jetta, SE PZEV w/Connectivity.
                c) Identified the problem. The trim should be SE PZEV w/Connectivity, Navigation. Navigation was parsed into 'body' in the source data.
                d) Added , Navigation to the trim and shifted the remaining columns over. This created NaN in saledate.
                e) There were NaN values in the VIN column (actually the tranmission column). but I converted them all to automatic as all other entries for this vehcile are automatic.
                f) update the DataFrame.
            2. Addressed vehicle naming. Brute force approach. Lot's of issues related to capitalization.

**important note to self. For final model need to consider number of total values in each variable**

            3. Dropped rows with no make. See 2/22/24 4)
            4. Addressed body naming. Brute force approach. Car and trucks done separately to presever sanity.
            3. Addressed color. 
                a) noted 'off-white' and large dash '—'. 
                b) Renamed 'off-white' to cream. Dropped '—' rows.
            4. Addressed color.
                a) noted 'off-white' and large dash '—'. 
                b) Renamed 'off-white' to cream. Dropped '—' rows.
            5. Addressed mmr.
                a) The distribution of values is interesting. Histogram of mmr values has two peaks. I don't know why. A graph of year vs mmr is as expected. It would be good to look at this by make and by model. 
                b) There are 11 missing values. All are from a single seller and represent that sellers complete contribution to the dataset. They also have no sellingprice and no saledate and the cars sold are well represented in the source data. Previous claim not substiated. I dropped these runs.
            6. Why is the number of saledate nulls not 26 or greater based on the 2015 jetta problem?
                a) Jetta 2015 saledates are None. Not NaN. None is new source of null and needs thought.
            7. Addressed odomoeter. 50 missing values but no obvious over representation of makes or off distributions of numerical values. Dropped these.
            8. Model. 76 missing values and it looks like BMWs and Audis are the problem. Model and trim is a problem. Leaving alone but not ignoring.

    #02/24/24 Continuation of data exploration/cleaning
            1. Count the characters in VIN all rows have 17 characters. Need to convert all lower case letters in VIN to Caps.
            2. There are 13,226 sellers. Seller is not trivial. Do I wait to see contribution of seller to model and then cycle back to cleaning?
            2. Tried to min/max on odometer and got an error. Odometer dtype has changed?!
            3. Will I create three DataFrames to move forwards, they are: 1) Drop transmission null, 2) Fill transmission null with Automatic 3) Create a function to fill the transmission nulls with automatic and manual. This needs thought.
            4. Tried to min/max on odometer and got an error. Odometer dtype has changed?!
            5. I dont see any issues with odometer.
            6. Let's look at state. 

**Discovered my fix to the 2015 Jetta problem did not apply correctly** 

            7. In the DataFrame new_sedan_df the state column contains NaN and seller contains NaN. There is data for condition but that is entirely subjective. All the cars are 2015. These data have no predicitve poweroutside of 2015. I looked at whole data set for case independent Volkswagen Jetta and these 26 are the only ones. 
            8. New thought. I need qualifying criteria for each column if a seller has only sold one car they are more to confound the model then strengthen it. Likewise if data only exist for a single year that will be of no use prediciting prices for multiple years. 
            9. I'm dropping the Jettas and I'm going to look at the makes and how many models have multiple year data. 
            10. Addressing saledate. What year is the model, how old is the car? Need year from saledate a calculation of car age.
            11. Extracted the year from saledate and plotted it against condition. No relationship. older cars do have lower condition values.
            12. All saleyears are 2014/2015 year and saleyear are interchangable. 
            13. I plotted a box plot for the condition values and I think it's worth keeping the values.
            14. Lets look at data for individual makes. 

    #03/04/24 Remedy for poor project diary updates.
            1. I was apparently not saving my edits to the diary. 
            2. I had really good meetings with Seth (2/28/24) and Nurgul (2/29/24). Super productive really valuable and massively impacted project direction.
            3. The files inlcuded for this project, and they order they should be viewed, are below. A brief description is provided for each:
                1. data_exploration.ipynb
                    Initial deep dive into the dataset. Lot's of human learning here. This was all pre-Seth/Nurgul.
                2. data_cleaning.ipynb
                    Post-Seth/Nurgul. Really trying to remove human learning and reinforcing machine learning.
                3. data_cleaning_no_mmr.ipynb
                    As above with the mmr data removed. 
                4. Linear_Regression_Analysis.ipynb
                    Coin toss to see what I would see and a good place to pick up from class. The conclusion is this dataset needs more advanced analysis methods.
                5. Multi_Regression_Analysis.ipynb
                    Picking up from class and workign with the cleaned car sales data set. Very good fun but dangerously addicitive.
                6. Multi_Regression_Analysis_Body.ipynb.
                    As above with analysis based on vehicle body type.
                7. Multi_Regression_Analysis_Body_No_Outliers.ipynb.
                    As above with analysis based on vehcile body with outliers removed based on Interquartile Range (IQR) method. 
                8. mulitcolinearity_Analysis.ipynb
                    I don't understand this outcome. The large number display the greatest co-linearity. I need to review this.
                9. Random_Forest_Analysis.ipynb
                    Picking up from class using Random Forest model.
                10. Random_Forest_Analysis_Body.ipynb
                    Picking up from class using Random Forest model and using vehicle body type.
                11. Lasso_Regression.ipynb
                    Picking from class using this model. This was very fast to run. Interesting.
                12. Ridge_Regression.ipynb
                    Picking from class using this model. This was also very fast to run. Interesting.
                    



            
            
        
