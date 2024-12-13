# Data Analytics Project

## Overview
This project performs data analysis on datasets including cooking sessions, order details, and user information. It includes steps for data cleaning, merging, and visualization of insights.

## Project Structure
- `upliance Assignment.ipynb`: The main Python script that performs data analysis.
- `CleanedData.csv`: The cleaned and analyzed dataset, saved after performing various transformations.
- `DataAnalyticsAssignment.xlsx`: The raw Excel file containing the data.
  
## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/vijayasklm/uplineDataAnalyticsProject.git

2.Install required libraries:
pip install pandas matplotlib seaborn

## Usage
To run the analysis, execute the analysis_script.py:
python upliance Assignment.ipynb

## Output
The cleaned and analyzed dataset will be saved as CleanedData.csv.


## The code is:
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load datasets
user_details = pd.read_excel(r"C:\Users\Balamanikanta\Downloads\Assignment.xlsx")
user_details

# Load datasets from Excel file
file_path = r"C:\Users\Balamanikanta\Downloads\Assignment.xlsx"
user_details = pd.read_excel(file_path, sheet_name="UserDetails.csv")
cooking_sessions = pd.read_excel(file_path, sheet_name="CookingSessions.csv")
order_details = pd.read_excel(file_path, sheet_name="OrderDetails.csv")
user_details,cooking_sessions,order_details

# Data Cleaning
# Check for missing values
print(user_details.isnull().sum())
print(cooking_sessions.isnull().sum())
print(order_details.isnull().sum())

# Fill or drop missing values as appropriate
order_details["Rating"] = order_details["Rating"].fillna(0)  # Replace NaN ratings with 0

# Check for duplicates
print(user_details.duplicated().sum())
print(cooking_sessions.duplicated().sum())
print(order_details.duplicated().sum())

# Merging datasets
merged_data = pd.merge(order_details, cooking_sessions, on=["Session ID", "User ID"], how="left")
merged_data = pd.merge(merged_data, user_details, on="User ID", how="left")
merged_data

column_names = merged_data.columns
print(column_names)

# Analysis
# Relationship between cooking session ratings and order completion
completed_orders = merged_data[merged_data["Order Status"] == "Completed"]
sns.scatterplot(data=completed_orders, x="Session Rating", y="Rating")
plt.title("Cooking Session Ratings vs Order Ratings")
plt.show()

# Popular dishes by frequency of orders
dish_popularity = merged_data["Dish Name_x"].value_counts().head(5)
print("Most popular dishes:")
print(dish_popularity)

# Visualize dish popularity
dish_popularity.plot(kind="bar", color="skyblue")
plt.title("Top 5 Popular Dishes")
plt.xlabel("Dish Name")
plt.ylabel("Number of Orders")
plt.show()

# Demographic influences on behavior
# Spending by age group
merged_data["Age Group"] = pd.cut(merged_data["Age"], bins=[20, 30, 40, 50], labels=["20-30", "30-40", "40-50"])
age_spending = merged_data.groupby("Age Group", observed=False)["Amount (USD)"].sum()

# Visualize spending by age group
age_spending.plot(kind="bar", color="green")
plt.title("Spending by Age Group")
plt.xlabel("Age Group")
plt.ylabel("Total Spending (USD)")
plt.show()

# Business Recommendations
# Trend of orders over time
order_trend = merged_data.groupby("Order Date").size()
order_trend.plot(kind="line", marker="o")
plt.title("Trend of Total Orders Over Time")
plt.xlabel("Order Date")
plt.ylabel("Number of Orders")
plt.xticks(rotation=45)
plt.show()

# Heatmap of correlations between numeric variables
correlation_matrix = merged_data[["Session Rating", "Rating", "Duration (mins)", "Amount (USD)"]].corr()
sns.heatmap(correlation_matrix, annot=True, cmap="coolwarm")
plt.title("Correlation Heatmap")
plt.show()

# Save cleaned data and insights
merged_data.to_csv("CleanedData.csv", index=False)

# Final Note
print("Analysis complete. Key insights saved to CleanedData.csv.")

from IPython.display import FileLink

# Create a downloadable link for the CSV file
FileLink(r'CleanedData.csv')



