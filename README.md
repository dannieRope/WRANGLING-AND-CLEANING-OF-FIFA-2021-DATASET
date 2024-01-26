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

To import the data into Power Query Editor, I navigated to the Data tab in Microsoft Excel.

Click on "Get Data" and then select "From File," followed by "From Text/CSV." 

![Screenshot (440)](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/45e07307-e759-44c4-a609-cad5e8533b54)

Specify the file's location to load it into the Query Editor.

![Loaded data](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/653aed6d-ef77-4a49-8707-53ab040e87a0)


I started the cleaning process by removing some columns that are unnecessary or won't contribute to our analysis.

These columns include Photo URL and Player URL. To eliminate these columns, choose the two columns.

Right-click on the selected columns, and from the drop-down list, click on "Remove Columns", as shown in the image below.

![Screenshot (441)](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/150b8469-ed60-4c18-846f-6279fc4ee883)


The ID serves as a unique identifier for each player in the dataset. After examining the column profile, I noticed a difference between the unique count and the distinct count, suggesting the existence of duplicates. I addressed this by eliminating duplicates within the ID column.

![Screenshot (442)](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/d571c1e7-f2d0-4a14-8c24-92aad5c31d0e)

To fix that, select the ID column. Right click and choose “remove duplicates” from the drop-down list. 

![duplicate](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/9f7eaee3-5634-4537-9adb-19ce894d3818)

While examining the "Name" column for misspellings, all names were accurate except one. 
This particular name contained a special character that required correction.
The "Name" column comprises the first initial of the player's first name and their full last name. To rectify this, I replaced the special character with "(S)", representing the first initial of the player's first name, as illustrated in the image below.

![special](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/cb9af477-f817-4177-b592-9b25a837215b)


To clean the "Team and Contract" column, several steps are involved. There is the need to separate the club names from the contract years.

Initially, the column was split using the tilde as the delimiter. 
This was achieved by selecting the "Split Column" option in either the Home or Transform tab.
The process involved choosing "By delimiter" from the list, specifying the tilde (~) as the delimiter, and confirming the action by clicking okay.

![deliter fex](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/b6bb589c-af5a-4530-9327-166b74c5cbf5)


The subsequent step is to extract the year at the end of each club name in the "Team and Contract.1" column.

Begin by selecting the column (Team and Contract.1), then click on the “Split Column” option available in the Home or Transform tab.

Opt for “By number of Characters” from the provided options. In the pop-up window, specify the number of characters (4), select “Once as far right as possible,” and confirm the action by clicking Okay.

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

Now, modify the data type for the "Contract StartYear" column to a whole number and replace any errors with zero (0). 

This adjustment will facilitate calculating the contract duration by finding the difference between the Contract StartYear and Contract EndYear.

To change the data type of the Contract StartYear, Select the column, and in the data type drop-down, choose the whole number. 

After Changing the data type, replace the errors by select the column, right click and select replace errors. 

![replace error ](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/d40b9e5c-93ff-4a23-8b16-38673e55a3b0)


Add “Contract duration” column by subtracting the Contract StartYear from the Contract EndYear as shown in the image below. 

![contract duration](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/d114d684-e79a-4ddc-9ee2-ad73563259f8)



Combine the "Contract StartYear" and "Contract EndYear" columns using the delimiter (-), and label the new column as "Contract" and replace “0- “with “null”. 

![merge](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/a76b9ab9-712b-4242-8acb-638b15ab2596)


The "Height" column is expressed in feet and inches. To convert it to centimeters, modify the "Height" column by converting the feet portion to centimeters (multiplied by 30.48) and the inches portion to centimeters (multiplied by 2.54). The transformation can be accomplished using the following M-code.

```m
= Table.TransformColumns(
    #"Replaced Value4",
    {
        {"Height", each Number.FromText(Text.BeforeDelimiter(_, "'")) * 30.48 + Number.FromText(Text.Start(Text.AfterDelimiter(_, "'"), 1)) * 2.54}
    }
)
```

![Height](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/dbc92bfc-f87f-480e-933c-a6d30d28c3cd)

The "Weight" column is in pounds, with "lbs" attached to its values. To resolve this, replace "lbs" with nothing ("") and change the data type to a whole number, as illustrated in the image below. 

The provided M-code achieves this transformation:
```m
= Table.TransformColumns(
    #"Extracted Text After Delimiter",
    {
        {"Weight", each Number.FromText(Text.BeforeDelimiter(_, "lbs"))}
    }
)
```
This code snippet effectively removes the "lbs" suffix and converts the "Weight" column to whole numbers.


![weight](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/ac03d32f-d90e-4bea-af67-3b23ee2aeec7)

Change the Height and Weight column name to "Height(cm) and “Weight (Ibs)” respectively. 

The column "BP" contains the abbreviated positions of players. It would be laborious to replace each abbreviation with the full name individually. To address this, I define the positions in a record, as shown below.
```
BestPosition =
[CAM = "Center Attack Midfield",
 CB = "Center Back",
 CDM = "Center Defending Midfield",
 CF = "Center Forward",
 RW = "Right Wing",
 CM = "Center Midfield",
 LW = "Left Wing",
 GK = "Goal Keeper",
 LB = "Left Back",
 LM = "Left MidField",
 LWB = "Left Wing Back",
 RB = "Right Back",
 RWB = "Right Back",
 RM = "Right Midfield",
 ST = "Striker"]

```

![best position](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/93c889ee-a582-487c-805f-8daa61584df8)


You can replace the abbreviated positions with their full names using the following M-code:

```m
= Table.TransformColumns(#"Renamed Columns",{{"BP", each Record.FieldOrDefault(BestPosition,_,_)}})
```

This code assumes that you have a record named "BestPosition" where the keys are the abbreviated positions, and the values are the corresponding full names. 

![best position](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/ea094594-28ff-4193-b905-187cc0c24b6a)


To fix the formatting of the Wage, Value, and Release Clause columns, follow these steps:

1. **Wage Column:**
   - Extract the number between "€" and "K."
   - Multiply the extracted number by 1000.

2. **Value and Release Clause Columns:**
   - Extract the number between "€" and "M" and number between "€" and "K"
   - Multiply the extracted number by 1000000.

3. **Instructions:**
   - Select all three columns: Wage, Value, and Release Clause.
   - In the Transform tab, click on "Extract" and choose the "Text between delimiter" option.
   - Specify the start delimiter as "€" and the end delimiter as "M" for Value and Release Clause.
   - Click OK to perform the extraction.

4. **Modify M-Code for Wage Column:**
   - Change the end delimiter for the Wage Column to "K" in the M-code.
     
Below is the  m-code for the above transformation steps 

```m
= Table.TransformColumns(Lookup, {{"Value", each try Number.FromText(Text.BetweenDelimiters(_, "€", "M"))*1000000 otherwise Number.FromText(Text.BetweenDelimiters(_, "€", "K"))*1000 }, 
         {"Release Clause", each try Number.FromText(Text.BetweenDelimiters(_, "€", "M"))*1000000 otherwise Number.FromText(Text.BetweenDelimiters(_, "€", "K"))*1000 },
         {"Wage", each Number.FromText(Text.BetweenDelimiters(_, "€", "K"))*1000}})
```

![wage](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/d6ee9203-24eb-4a84-b908-0d496d1a062b)

Change the data type of all three columns (Wage, Value, and Release Clause) to currency and rename them by adding (€). 

For the "W/F," "S/M," and "IR" columns, which represent a player's weak foot rating, skill move rating, and international rating, respectively, are rated on a scale from 1 to 5, you can eliminate the star rating and change the data type to whole numbers using the "Replace Value" feature.

To remove the star ratings from the "W/F," "S/M," and "IR" columns, follow these steps:

1. Select all three columns: "W/F," "S/M," and "IR."
   
3. Go to the Transform tab.
   
5. Click on "Replace Values."
   
7. Replace the star ("★") with nothing ("").
   
9. Confirm the replacement.

![star fixed](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/e80d6b2c-10e5-4f53-b49e-49b21cbb3628)


The Loan date column has most of its value as N/A which is the date value for the players which are not on loan. We replace the N/A with null values and change the data type to date. 
To replace "N/A" values with null and change the data type to date in the "Loan Date" column, you can use the following M-code:

```m
= Table.TransformColumns(
    #"PreviousStep",
    {
        {"Loan Date", each if _ = "N/A" then "Not on Loan" else DateTime.FromText(_, "en-US"), type nullable date}
    }
)
```

This code replaces "N/A" with null and changes the data type to a nullable date.

![loanfixed](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/77ab6c62-32f7-4cba-b139-402ec1a1077d)


To address the inconsistencies in the "Hits" column where some values have "K" attached, signifying a thousand, you can remove the "K" and multiply those values by 1000. Here's the M-code for this transformation:

```m
= Table.TransformColumns(
    #"Replaced Value9",
    {
        {"Hits", each if Text.EndsWith(_, "K") then Number.FromText(Text.Start(_, Text.Length(_) - 1)) * 1000 else Number.FromText(_)}
    }
)
```

This code checks if a value in the "Hits" column ends with "K." If it does, it removes the "K" and multiplies the remaining number by 1000. If not, it converts the value to a number directly. 

![Hitfixed](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/9859cc8f-a9fe-4c3f-9422-6a1a7ff2a316)

Some columns need to be in percentage format. These columns include PAC, SHO, PAS, DRI, DEF, PHY, POT, and ↓OVA. To convert these columns to percentages, I divided each value by 100 and changed their data type to percentage. You can find the code for this transformation below. 

```m
= Table.TransformColumns(Hits, {{"PAC", each _ / 100,Percentage.Type},{"SHO", each _ / 100, Percentage.Type},{"PAS", each _ / 100,Percentage.Type},{"DRI", each _ / 100, Percentage.Type},{"DEF", each _ / 100, Percentage.Type},{"PHY", each _ / 100, Percentage.Type},{"POT", each _ / 100,Percentage.Type},{"↓OVA", each _ / 100,Percentage.Type}})
```
![%final](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/6376001e-560a-4afb-ae34-0bde5535dbf0)


Finally, I rename all the abbreviated columns to their full names so they are easier to recognize and understand. The code and the outcome are provided below. 

```m
= Table.RenameColumns(#"Divided Column",{{"↓OVA", "Overall Rating"},{"POT", " Potential Rating"},{"W/F", "Weak Foot Rating"},{"SM", "Skill Move Rating"},{"A/W", "Attacking Work Rate"},{"D/W", "Defensive Work Rate"},{"IR", "International Reputation Rating"},{"PAC", "Pace Rating"},{"SHO", "Shooting Rating"},{"PAS", "Passing Rating"},{"DRI", "Dribbling Rating"},{"DEF", "Defensive Rating"},{"PHY", "Physical Rating"},{"BP", "Best Position"},{"BOV", "Overall Rating in Best Position"}})
```

 ![Columnsfinal](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/8d175381-fdd9-4224-a75c-6a05ef061a03)
 

 Attacking’, ‘Crossing’, ‘Finishing’, ‘Heading_accuracy’, ‘Short_passing’, ‘Volleys’, ‘Skill’, ‘Dribbling’, ‘Curve’, ‘FK_accuracy’, ‘Long_passing’, ‘Ball_control’, ‘Movement’, ‘Acceleration’, ‘Spring_speed’, ‘Agility’, ‘Reactions’, ‘Balance’, ‘Power’, ‘Shot_power’, ‘Jumping’, ‘Stamina’, ‘Strength’, ‘Long_shot’, ‘Mentality’, ‘Aggression’, ‘Interception’, ‘Positioning’, ‘Vision’, ‘Penalties’, ‘Composure’, ‘Defending’, ‘Marking’, ‘Standing_tackle’, ‘Sliding_tackle’, ‘Goalkeeping’, ‘GK_handling’, ‘GK_diving’, ‘Gk_kicking’, ‘Gk_positioning’, ‘Gk_reflexes’, ‘Total_stats’, ‘Base_stats’, ‘Pace’, ‘Shooting’, ‘Passing’, ‘DRI’, ‘DEF’, ‘Physical’, ‘Best_position’, ‘Preferred_foot’, ‘Attacking_workrate’, and ‘Defensive_workrate’ : were checked, and no issues found within the data. The data types were checked, and changes were made for some that had wrong data types and column names corrected.

The image below is the preview of the clean data ready for further analysis. 

![Clean](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/90737891-a74e-4094-a75e-5e2d4abacb2f)

You can also access the clean data [here](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/blob/main/Fifa21_Clean.xlsx)


 ## CONCLUSION 
 
Cleaning the data wasn't easy. I had to search for ways to clean certain columns, and in the process, I discovered M-code.

Understanding how to use M-code for data transformation simplifies the process and reduces the number of steps needed for cleaning.

It is important to remember that in order to clean the FIFA 2021 dataset, one must possess domain knowledge of the data category.

For this reason, more research is advised. Cleaning this data exposed me to the world of football by helping me comprehend terms used in the game.

![image](https://github.com/dannieRope/WRANGLING-AND-CLEANING-OF-FIFA-2021-DATASET/assets/132214828/877d4650-7df7-4603-95b2-759dda25080b)




