# Data_Cleaning

# My GitHub Account

Welcome to my GitHub account! This repository contains various projects and code snippets that I have worked on. Below is an overview of the code snippet included in this repository.

## Code Snippet

The following code snippet demonstrates a data preprocessing workflow using pandas to clean and manipulate an Excel file.

```python
#Finding out the directory
import os

current_dir = os.getcwd()
print(current_dir)

#importing Excel File.
import pandas as pd

file_path = "C:/Users/bindu/Downloads/CustomerCallList.xlsx"
df = pd.read_excel(file_path)
print(df.head(10))

#dropping the duplicates
df = df.drop_duplicates()

#dropping unwanted columns
df.drop(columns='Not_Useful_Column')

#stripping out the unwanted parameters in the lastname
df["Last_Name"] = df["Last_Name"].str.strip("123._/")

#removing non-alphanumeric characters and formatting the phone number
df['Phone_Number'] = df['Phone_Number'].str.replace('[^a-zA-Z0-9]','')
df['Phone_Number'] = df['Phone_Number'].apply(lambda x: str(x))
df['Phone_Number'] = df['Phone_Number'].apply(lambda x: x[:3] + '-' + x[3:6] + '-' + x[6:10])

#removing null values
df['Phone_Number'] = df['Phone_Number'].replace("nan--","")
df['Phone_Number'] = df['Phone_Number'].replace("Na--","")
df = df.fillna('')

#splitting the 'Address' column into 'Street_Address', 'State', and 'Zipcode'
df[['Street_Address', 'State', 'Zipcode']] = df['Address'].str.split(',', 2, expand=True)

#updating 'Do_Not_Contact' column values
df['Do_Not_Contact'] = df['Do_Not_Contact'].replace("Yes", 'Y')
df['Do_Not_Contact'] = df['Do_Not_Contact'].replace("No", 'N')
df['Do_Not_Contact'] = df['Do_Not_Contact'].replace("Y", 'Yes')
df['Do_Not_Contact'] = df['Do_Not_Contact'].replace("N", 'No')

#updating 'Paying Customer' column values
df["Paying Customer"] = df["Paying Customer"].replace('Yes', 'Y')
df["Paying Customer"] = df["Paying Customer"].replace('No', 'N')
df["Paying Customer"] = df["Paying Customer"].replace('N', 'No')
df["Paying Customer"] = df["Paying Customer"].replace('Y', 'Yes')
df = df.replace('N/a', "")

#removing rows with empty phone numbers
for x in df.index:
    if df.loc[x, 'Phone_Number'] == '':
        df.drop(x, inplace=True)

#dropping rows with null phone numbers
df.dropna(subset=["Phone_Number"], inplace=True)
