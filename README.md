#         WRANGLING AND CLEANING OF FIFA 2021 DATASET

![image](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/421e5e4f-3384-48a6-87b2-857aeba77717)

Before you can analyse any type of data, it's important to ensure that the data is in good shape (clean) and ready for the intended analysis.

So, what exactly is a clean data? 

A clean data refers to a dataset that has been processed to be free of missing values, inaccuracies, duplicates, errors, misspellings, null entries, outliers, and more.

Having the skill to clean and prepare data is essential for anyone aspiring to be a proficient analyst.

To enhance my expertise in data cleaning, I've decided to take on this exercise. 

There are various tools available for cleaning and preparing data, and in this case, I'll be using Power Query. 

Power Query is a data transformation tool found in both Microsoft Excel and Power BI.

## OBJECTIVES

The objective of this challenge is to prepare the data provided for this exercise and make it available for further analysis by using ETL Approach.

By the end of this exercise, the dataset must be free of the following.

•	Redundant Information

•	Duplicates

•	Missing values

•	Incorrect data types

•	Wrong calculations across rows and columns

•	Errors in spellings and values etc. 


# THE DATASET

The FIFA 2021 dataset downloaded from Kaggle via this [Link]( https://www.kaggle.com/datasets/yagunnersya/fifa-21-messy-raw-dataset-for-cleaning-exploring/download?datasetVersionNumber=2). 

The dataset is a CSV file containing details about football players in 2021. It has about 18,979 rows and 77 columns.

These columns display players information such as special ID, names, ages, where they're from, their positions, how good they are overall, how much they get paid, their contracts, and more. 

Here are the descriptions of some columns in the dataset.  

``ID:`` Unique identification number for each Player

``Name:`` Player’s Name

``Long Name:`` Player’s Full Name

``photo URL:`` URL to the Player’s photo

``player URL:`` URL to the Player’s profile page

``BP:`` Players best position

``OVA:`` Overall rating, a score that represents the Player’s overall ability.

``POT:`` Potential rating, a score that represents the Player’s potential to improve.

``Contract:`` The team the Player is currently playing for and the contract details.

``Positions:`` The various positions on the field that the Player can play.

``W/F:`` Player’s weak foot rating

``SM:`` Player’s skill move rating

``A/W:`` Player’s attacking work rate.

``D/W:`` Player’s defensive work rate

``IR:`` Player’s international reputation rating

``PAC:`` Player’s pace rating

``SHO:`` Player’s Shooting rating (1–5)

``PAS:`` Player’s Passing rating

``DRI:`` Player’s Dribbling rating

``DEF:`` Player’s Defensive rating

``PHY:`` Player’s Physical rating

# DATA CLEANING PROCESS

To bring the data into Power Query Editor, head over to the Data tab in Microsoft Excel.

Click on "Get Data" and then select "From File," followed by "From Text/CSV." 

![Screenshot (440)](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/45e07307-e759-44c4-a609-cad5e8533b54)

Pick the location of the file to load it into the Query Editor.

![Loaded data](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/653aed6d-ef77-4a49-8707-53ab040e87a0)


The cleaning process starts by removing some columns that are unnecessary or won't contribute to our analysis.

These columns include Photo URL and Player URL. To eliminate these columns, choose the two columns.

Right-click on the selected columns, and from the drop-down list, click on "Remove Columns," as shown in the image below.

![Screenshot (441)](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/150b8469-ed60-4c18-846f-6279fc4ee883)


The ID serves as a unique identifier for each player in the dataset. Upon inspecting the column profile, we observe a difference between the unique count and the distinct count, indicating the presence of duplicates. Specifically, player ID 251698 appears more than once.

![Screenshot (442)](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/d571c1e7-f2d0-4a14-8c24-92aad5c31d0e)

To fix that, select the ID column. Right click and choose “remove duplicates” from the drop-down list. 

![duplicate](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/9f7eaee3-5634-4537-9adb-19ce894d3818)

Checking for misspelling in the “Name” column, all names are correct except one. 
This name has a special character which needs to be replaced. 
The "Name" column is a combination of the first initial of the player's first name and their full last name. 

To fix this, replace the special character with (S) which is the first initial of the player's first name as shown in the image below. 

![special](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/cb9af477-f817-4177-b592-9b25a837215b)


Dealing with the "Team and Contract" column involves some steps. There is the need to separate the club names from the contract years.

First, split the column using the delimiter(~).

To do this, click on the "Split Column" option in the Home or Transform tab. 

Choose "By delimiter" from the list, specify the delimiter and click okay. 

![deliter fex](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/b6bb589c-af5a-4530-9327-166b74c5cbf5)


Next action is to extract the year at the end of each club name in Column "Team and Contract.1". 

Select the column (Team and Contract.1), click on “Split Column” option in the Home or Transform tab. 

Choose “By number of Characters”. 

In the pop-up window, specify the number of characters (4), select “Once as far right as possible” and click Okay. 

![position](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/defaadf9-ef97-477f-b7c5-79645bd70795)

Rename the column "Team & Contract.1.1" to "Team."

Upon reviewing the "Team" column, certain issues need addressing.

For players marked as Free, the club name includes "Free" and a date.

For players on loan, the club name includes the loan date and "On Loan."

To remove the appended "Free" and date, or "On Loan" for loaned players, the following M-codes were employed:

```m
= Table.ReplaceValue(
    #"Renamed Columns2",
    each [Team],
    each if Text.EndsWith([Team], "Fr") then Text.Start([Team], Text.Length([Team]) - 2) else [Team],
    Replacer.ReplaceValue,
    {"Team"}
)
```

![free](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/a3f794a4-0fe9-4530-a3bd-1b5af0db9afd)


```m
= Table.ReplaceValue(
    #"Remove Fr",
    each [Team],
    each if Text.EndsWith([Team], "Lo") then Text.Start([Team], Text.Length([Team]) - 18) else [Team],
    Replacer.ReplaceValue,
    {"Team"}
)
```
![loan](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/e5460326-8540-4e3e-9871-41f4d4f6214f)


![onloand](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/4bb7eff8-c325-4dd9-9429-23e569bb328d)


Rename Column “Team & Contract.1.2” and “Team & Contract.2” to “Contract StartYear” and “Contract EndYear” respectively. 

In the Contract StartYear Column, replace “an” with “On Loan” and “ee” with “Free”.  This will help in the creation of a new column “Contract Agreement”. 

![an](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/dd8964a4-2003-4030-9cfa-87e04ae1a2a6)

The "Contract Agreement" column indicates the type of deal the player signed. 

This information will be useful for categorizing the players into three groups: Free, Contract, and Loan.

To create this column, navigate to the "Add Column" tab, 

click on "Conditional Column" and set up the column as illustrated in the image below.

![cnAg](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/34a8c478-79af-43cd-a59a-26554e39eb36)

Now, modify the data type for the "Contract StartYear" column to a whole number and replace any errors with zero (0). This adjustment will facilitate calculating the contract duration by finding the difference between the Contract StartYear and Contract EndYear.
Add “Contract duration” column by subtracting the Contract StartYear from the Contract EndYear as shown in the image below. 
Combine the "Contract StartYear" and "Contract EndYear" columns using the delimiter (-), and label the new column as "Contract" and replace “0- “with “null”. 
The "Height" column is recorded in feet and inches. To convert it to centimeters (CM), replace the double quotes (") with nothing (“”) and replace the single quotes (') with periods (.). Then, split the column using the delimiter (.) to separate it into two columns—one with foot values and the other with values in inches.
Convert the column (Height.1) with foot values to centimeters by multiplying each value by 30.48. Similarly, convert the column (Height.2) with inches values to centimeters by multiplying each value by 2.54.
Generate a new column (Height (cm)) by adding both columns "Height.1" and "Height.2" to obtain the overall height in centimeters.
Remove Height.1 and Height.2 columns. 
The "Weight" column is in pounds, with "lbs" attached to its values. To resolve this, replace "lbs" with nothing ("") and change the data type to a whole number, as illustrated in the image below.
Change the column name from “Weight” to “Weight (Ibs)”.
The column "BP" contains the abbreviated positions of players. It would be laborious to replace each abbreviation with the full name individually. To address this, I define the position in a record, as shown below.
You can replace the abbreviated positions with their full names using the following M-code:

```m
= Table.TransformColumns(#"Renamed Columns",{{"BP", each Record.FieldOrDefault(BestPosition,_,_)}})
```

This code assumes that you have a record named "BestPosition" where the keys are the abbreviated positions, and the values are the corresponding full names. 
To fix the formatting of the Wage, Value, and Release Clause columns, follow these steps:

1. **Wage Column:**
   - Extract the number between "€" and "K."
   - Multiply the extracted number by 1000.

2. **Value and Release Clause Columns:**
   - Extract the number between "€" and "M."
   - Multiply the extracted number by 1000000.

3. **Instructions:**
   - Select all three columns: Wage, Value, and Release Clause.
   - In the Transform tab, click on "Extract" and choose the "Text between delimiter" option.
   - Specify the start delimiter as "€" and the end delimiter as "M" for Value and Release Clause.
   - Click OK to perform the extraction.

4. **Modify M-Code for Wage Column:**
   - Change the end delimiter for the Wage Column to "K" in the M-code.

Change the data type of all three columns (Wage, Value, and Release Clause) to currency and rename them by adding (€). 
For the "W/F," "S/M," and "IR" columns, which represent a player's weak foot rating, skill move rating, and international rating, respectively, and are rated on a scale from 1 to 5, you can eliminate the star rating and change the data type to whole numbers using the "Replace Value" feature.
To remove the star ratings from the "W/F," "S/M," and "IR" columns, follow these steps:
1. Select all three columns: "W/F," "S/M," and "IR."
2. Go to the Transform tab.
3. Click on "Replace Values."
4. Replace the star ("★") with nothing ("").
5. Confirm the replacement.

The Loan date column has most of its value as N/A which is the date value for the players which are not on loan. We replace the N/A with null values and change the data type to date. 
To replace "N/A" values with null and change the data type to date in the "Loan Date" column, you can use the following M-code:

```m
= Table.TransformColumns(
    #"PreviousStep",
    {
        {"Loan Date", each if _ = "N/A" then null else DateTime.FromText(_, "en-US"), type nullable date}
    }
)
```

This code replaces "N/A" with null and changes the data type to a nullable date.

To address the inconsistencies in the "Hits" column where some values have "K" attached, signifying a thousand, you can remove the "K" and multiply those values by 1000. Here's the M-code for this transformation:

```m
= Table.TransformColumns(
    #"Replaced Value9",
    {
        {"Hits", each if Text.EndsWith(_, "K") then Number.FromText(Text.Start(_, Text.Length(_) - 1)) * 1000 else Number.FromText(_)}
    }
)
```

This code checks if a value in the "Hits" column ends with "K." If it does, it removes the "K" and multiplies the remaining number by 1000. If not, it converts the value to a number directly. Adjust the column names as needed.
Some columns need to be in percentage format. These columns include PAC, SHO, PAS, DRI, DEF, PHY, POT, and ↓OVA. To convert these columns to percentages, I divided each value by 100 and changed their data type to percentage. You can find the code for this transformation below. 
