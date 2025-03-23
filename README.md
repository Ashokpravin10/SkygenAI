📊 Client & Subscription Analysis Using Python
This project analyzes financial and client data using Python to answer four key business questions:

📌 Project Agenda
The project processes and analyzes four datasets to provide insights into:

1. Number of Finance Lending and Blockchain Clients.

2. Industry with the Highest Subscription Renewal Rate.

3. Average Inflation Rate During Subscription Renewals.

4. Median Amount Paid Per Year for All Payment Methods.

📂 Dataset Details
The following datasets were used:

1. finanical_information.csv
start_date – Period start date.

end_date – Period end date.

inflation_rate – Inflation rate during the period.

gdp_growth_rate – GDP growth rate during the period.

2. industry_client_details.csv
client_id – Unique identifier for each client.

company_size – Size of the company (Small, Medium, Large).

industry – Industry of the client (Finance Lending, Block Chain, etc.).

location – Geographical location of the client.

3. payment_information.csv
client_id – Unique identifier for the client.

payment_date – Date of payment.

amount_paid – Amount paid by the client.

payment_method – Payment method used (Bank Transfer, Check, Credit Card).

4. subscription_information.csv
client_id – Unique identifier for the client.

subscription_type – Type of subscription (Monthly/Yearly).

start_date – Start date of the subscription.

end_date – End date of the subscription.

renewed – Whether the subscription was renewed (TRUE/FALSE).

📚 Step-by-Step Analysis and Data Transformation
⚡️ Step 1: Import Required Libraries and Load Data
python
Copy
Edit
# Import necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load CSV files
financial_df = pd.read_csv("/mnt/data/finanical_information.csv")
client_df = pd.read_csv("/mnt/data/industry_client_details.csv")
payment_df = pd.read_csv("/mnt/data/payment_information.csv")
subscription_df = pd.read_csv("/mnt/data/subscription_information.csv")

✅ What Happens:

Libraries are imported.

All CSVs are loaded into Pandas DataFrames.

⚡️ Step 2: Convert Date Columns to Datetime Format
python
Copy
Edit
# Convert financial and subscription date columns to datetime
financial_df['start_date'] = pd.to_datetime(financial_df['start_date'])
financial_df['end_date'] = pd.to_datetime(financial_df['end_date'])
subscription_df['start_date'] = pd.to_datetime(subscription_df['start_date'])
subscription_df['end_date'] = pd.to_datetime(subscription_df['end_date'])
payment_df['payment_date'] = pd.to_datetime(payment_df['payment_date'], errors='coerce')

✅ What Happens:

Date columns are converted to datetime for easier manipulation.

Invalid dates in payment_date are handled with errors='coerce'.

⚡️ Step 3: Count Finance Lending and Blockchain Clients
python
Copy
Edit
# Count clients for Finance Lending and Block Chain
finance_clients = client_df[client_df['industry'] == 'Finance Lending'].shape[0]
blockchain_clients = client_df[client_df['industry'] == 'Block Chain'].shape[0]

print(f"Number of Finance Lending clients: {finance_clients}")
print(f"Number of Blockchain clients: {blockchain_clients}")

✅ What Happens:

Counts clients by filtering the industry column.

Displays the count of Finance Lending and Block Chain clients.

⚡️ Step 4: Industry with the Highest Subscription Renewal Rate
python
Copy
Edit
# Filter renewed subscriptions
renewed_df = subscription_df[subscription_df['renewed'] == True]

# Merge with client data to get industry information
renewed_clients_df = pd.merge(renewed_df, client_df, on='client_id', how='inner')

# Calculate renewal rate per industry
renewal_rate = renewed_clients_df['industry'].value_counts(normalize=True) * 100

# Plot renewal rate by industry
plt.figure(figsize=(10, 6))
sns.barplot(x=renewal_rate.index, y=renewal_rate.values, palette='viridis')
plt.title('Renewal Rate by Industry')
plt.xlabel('Industry')
plt.ylabel('Renewal Rate (%)')
plt.xticks(rotation=45)
plt.show()

# Industry with highest renewal rate
top_renewed_industry = renewal_rate.idxmax()
print(f"The industry with the highest renewal rate is: {top_renewed_industry}")

✅ What Happens:

Renewed subscriptions are filtered.

Merged with client data to get industry info.

Renewal rate calculated and plotted.

Displays industry with the highest renewal rate.

⚡️ Step 5: Average Inflation Rate During Renewals
python
Copy
Edit
# Filter only renewed subscriptions
renewed_sub_df = subscription_df.loc[subscription_df['renewed'] == True].copy()

# Function to get inflation rate during subscription renewal period
def get_inflation_rate(end_date):
    for _, row in financial_df.iterrows():
        if row['start_date'] <= end_date <= row['end_date']:
            return row['inflation_r']
    return np.nan

# Apply function to get inflation rate during renewal
renewed_sub_df['inflation_rate'] = renewed_sub_df['end_date'].apply(get_inflation_rate)

# Calculate the average inflation rate
avg_inflation_rate = renewed_sub_df['inflation_rate'].mean()
print(f"The average inflation rate during subscription renewals is: {avg_inflation_rate:.2f}%")

✅ What Happens:

Renewed subscriptions are filtered.

Inflation rate fetched by checking which period matches the subscription end_date.

Average inflation rate calculated and displayed.

⚡️ Step 6: Median Amount Paid Per Year for All Payment Methods
python
Copy
Edit
# Convert 'payment_date' to datetime
payment_df['payment_date'] = pd.to_datetime(payment_df['payment_date'], errors='coerce')

# Extract year from 'payment_date'
payment_df['year'] = payment_df['payment_date'].dt.year

# Group by year and calculate median payment amount
median_payment_per_year = payment_df.groupby('year')['amount_paid'].median().reset_index()

# Plot median amount paid per year
plt.figure(figsize=(10, 6))
sns.lineplot(data=median_payment_per_year, x='year', y='amount_paid', marker='o', color='orange')
plt.title('Median Amount Paid Per Year for All Payment Methods')
plt.xlabel('Year')
plt.ylabel('Median Amount Paid')
plt.grid()
plt.show()

# Display median payments per year
print(median_payment_per_year)

✅ What Happens:

Payment data is converted to datetime.

Year is extracted from payment_date.

Median amount paid per year is calculated and plotted.

📊 Final Visualizations
Renewal Rate by Industry

Barplot showing renewal rate across different industries.

Median Amount Paid Per Year

Line plot showing the trend of median payment per year.
