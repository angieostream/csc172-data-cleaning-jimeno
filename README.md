# Data Cleaning with AI Support

## Student Information
- Name: ANGELYN M. JIMENO
- Course Year: BSCS 4
- Date: 2025-09-29

## Dataset
- Source: https://www.kaggle.com/competitions/titanic/data
- Name: Titanic (train.csv)

## Issues found
- Missing values:
  - Age: 177 missing (filled with median)
  - Cabin: 687 missing (~77%) — dropped
  - Embarked: 2 missing (filled with mode)
- Duplicates: 0
- Inconsistencies: extra whitespace in string fields
- Outliers: high Fare values (IQR clipping applied)

## Cleaning steps
1. Normalized missing markers ('?' and empty strings → NaN)
2. Filled `Age` with median
3. Filled `Embarked` with mode
4. Dropped `Cabin` (mostly missing)
5. Checked duplicates (none found)
6. Trimmed whitespace and standardized `Sex`/`Embarked`
7. Applied IQR clipping to `Fare`
8. Saved cleaned dataset to `data/cleaned_dataset.csv`

## AI prompts used

**Prompt 1:**  
I loaded the Titanic DataFrame `df`. Give pandas code to:  
- replace '?' and empty strings with NaN  
- fill `Age` with median  
- fill `Embarked` with the mode  
- drop `Cabin` column  
- drop exact duplicates  
- print missing counts before and after each step  
Include short comments.

**Generated code:**

```python
import pandas as pd
import numpy as np

df.replace(['?', ''], np.nan, inplace=True)

df['Age'].fillna(df['Age'].median(), inplace=True)

df['Embarked'].fillna(df['Embarked'].mode()[0], inplace=True)

df.drop(columns=['Cabin'], inplace=True)

df.drop_duplicates(inplace=True)

for c in df.select_dtypes(include=['object']).columns:
    df[c] = df[c].astype(str).str.strip()
df['Sex'] = df['Sex'].str.lower()
df['Embarked'] = df['Embarked'].str.upper()

num_cols = df.select_dtypes(include=[np.number]).columns.tolist()
for col in num_cols:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    df[col] = df[col].clip(lower=lower, upper=upper)

df.to_csv("../data/cleaned_dataset.csv", index=False) 
```


## Results
- Rows before: 891
- Rows after: 891
- Columns before: 12
- Columns after: 11 (Cabin dropped)
- Missing values after cleaning: 0
- Duplicates removed: 0
- Outliers clipped in numeric columns

Video: https://drive.google.com/drive/folders/1dRKKWJ27ZVaR4y1tc3rFhqAkibHdbPR1?usp=sharing 