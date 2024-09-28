import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

me_df=pd.read_csv("C:\\Users\\sujoy\\OneDrive\\Desktop\\Python EDA\\GHMC Election 2020\\GHMC Election 2020.csv")

me_df.head()

me_df.tail()

## Checking for Duplicate rows 

me_df[me_df.duplicated()]

## Checking for Null Values (NaN present in Result column can be neglected)

me_df.isnull().sum()

me_df.columns

me_df.info

## Counting the Number of Wards in Hyderabad Muncipality

Count_of_Wards=me_df['Ward No.'].value_counts()

Count_of_Wards

## Counting the Number of Wards Reserved Caste wise in Hyderabad Muncipality

Wards_reserved=me_df['Ward reserved for'].value_counts()

Wards_reserved

## Plotting Line Plot for Wards Reserved Caste wise

sns.lineplot(data=Wards_reserved)

## Total Voters in Each Ward

Total_Voters_in_Each_Ward=me_df.groupby('Ward No.').sum('Total No. of Voters including service voters in the ward')

Total_Voters_in_Each_Ward[:2]

## Total Voters Voted on Election Day

me_df.groupby('Ward No.')[['Total Votes Polled (excluding PBs) in the ward','No. of Postal Ballots Polled in the ward']].sum()

me_df['Total Votes Polled (including PBs)'] = me_df['Total Votes Polled (excluding PBs) in the ward'] + me_df['No. of Postal Ballots Polled in the ward']


me_df

## Total Votes Per Ward 

Total_Votes_Per_Ward= me_df.groupby('Ward No.')['Total Votes Polled (including PBs)'].sum().reset_index().sort_values(by='Total Votes Polled (including PBs)',ascending=False)

Total_Votes_Per_Ward

## Plotting Total Votes Per Ward

sns.barplot(x='Total Votes Polled (including PBs)',y='Ward No.',data=Total_Votes_Per_Ward)
plt.title('Total Votes Per Ward')

## Voter Turn Out Percentage

me_df['Voter_Turnout_Percentage']=(me_df['Total Votes Polled (including PBs)']/me_df['Total No. of Voters including service voters in the ward'])*100

me_df['Voter_Turnout_Percentage']

## Plotting Voter Turnout Percentage Per Ward

sns.barplot(x='Voter_Turnout_Percentage',y='Ward No.',data=me_df)
plt.title('Voter Turnout Percentage Per Ward')

# Ward_with_Higher_Voter_Turnout_Percentage

Ward_with_Higher_Voter_Turnout=me_df.nlargest(5,'Voter_Turnout_Percentage')[['Ward No.','Voter_Turnout_Percentage']]

Ward_with_Higher_Voter_Turnout

sns.barplot(x='Ward No.',y='Voter_Turnout_Percentage',data=Ward_with_Higher_Voter_Turnout)
plt.title('Ward_with_Higher_Voter_Turnout %')

# Ward_with_Lower_Voter_Turnout_Percentage

Ward_with_Lower_Voter_Turnout=me_df.nsmallest(5,'Voter_Turnout_Percentage')[['Ward No.','Voter_Turnout_Percentage']]

Ward_with_Lower_Voter_Turnout

sns.barplot(x='Ward No.',y='Voter_Turnout_Percentage',data=Ward_with_Lower_Voter_Turnout)
plt.title('Ward with Lowest Voter Turnout %')

## Party wise Vote Share 

Party_wise_Votes=me_df.groupby('Party Affiliation if any')['Total Votes Polled (including PBs)'].sum().reset_index().sort_values(by='Total Votes Polled (including PBs)',ascending=False)




Party_wise_Votes

sns.barplot(x='Total Votes Polled (including PBs)',y='Party Affiliation if any',data=Party_wise_Votes)
plt.title('Party Wise Votes Gained')














