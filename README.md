# Project_2

## Explain project



## Project_Diary

#02/22/24 Data exploration. Loaded csv into Jupyter notebook
    - Initial thoughts
            1. It's a large data matrix 558,837 rows and 16 columns. 
            2. 'make' has some heterogeneity and conflicts in vehicle naming. Multiple reasons for this including first letter capitalization and truck references.
            3. 'transmission' has a disprotional number of missing values. There are errors in the 'tranmission values' pointing to a column shift. I'm calling this the 2015 Jetta problem.
            4. 'make' has 10301 missing values. All of the rows containing no make data also contain no model, no trim and no body data. These can be dropped.
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
