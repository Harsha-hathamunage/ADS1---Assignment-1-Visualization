# ADS1---Assignment-1-Visualization
This repo contains the python code of the Applied Data Science 01, Assignment 1 (Visualization)

**********************************************************************************************************
***********************************************Python Code************************************************
**********************************************************************************************************
"""
Created on Thu Mar  2 00:57:23 2023

@author: Harsha Sandaruwan Hathamunage
"""

import pandas as pd
import matplotlib.pyplot as plt

# Create function to plot line graph


def plot_line_graph(df):
    """
    Plots a line graph showing the population growth over time for each country in the given dataframe.

    Args:
    - df (Pandas dataframe): A dataframe containing the population growth data for different countries and years.
        It must have the following columns:
            - 'Country Name': the name of the country
            - 'Time': the year
            - 'Population growth (annual %) [SP.POP.GROW]': the population growth rate for that year and country
    """
    fig, ax = plt.subplots()

    for country in df['Country Name'].unique():
        data = df[df['Country Name'] == country]
        ax.plot(
            data['Time'], data['Population growth (annual %) [SP.POP.GROW]'], label=country)

    ax.legend()
    ax.set_xlabel('Year')
    ax.set_ylabel('Population Growth (Annual %)')
    ax.set_title('Population Growth Change Over the Years in South Asia')
    plt.show()

# Create function to plot pie chart


def plot_pie_chart(df, year):
    """
    Function to plot a pie chart of population growth for different countries for a given year.

    Args:
    - df (pandas.DataFrame): The DataFrame containing the data to plot.
    - year (int): The year for which to plot the data.

    """

    # Filter the dataframe to get data for the given year
    df_year = df[df['Time'] == year]

    # Group the data by country and calculate the total population growth for each country
    grouped_df = df_year.groupby('Country Name')[
        'Population growth (annual %) [SP.POP.GROW]'].sum().reset_index()

    # Create a pie chart
    plt.pie(grouped_df['Population growth (annual %) [SP.POP.GROW]'],
            labels=grouped_df['Country Name'], autopct='%1.1f%%')
    plt.title(f'Population growth in {year}')
    plt.show()

# Create function to plot bar chart


def plot_bar_plot(df, country1, country2):
    """
    Plots a bar chart showing the population growth of two countries over different years.

    Args:
    - df : pandas DataFrame
        DataFrame containing the population growth data for different countries and years.
    - country1 : str
        The name of the first country to be compared.
    - country2 : str
        The name of the second country to be compared.
    """

    # Filter the data for the two countries
    df_country1 = df[df['Country Name'] == country1]
    df_country2 = df[df['Country Name'] == country2]

    # Get the population growth data for the two countries
    data_country1 = df_country1['Population growth (annual %) [SP.POP.GROW]']
    data_country2 = df_country2['Population growth (annual %) [SP.POP.GROW]']

    # Set the years and the bar width
    years = ['1990', '1995', '2000', '2005', '2010', '2015', '2018']
    bar_width = 0.35

    # Set the x positions of the bars
    r1 = range(len(years))
    r2 = [x + bar_width for x in r1]

    # Create the bar chart
    plt.bar(r1, data_country1, color='blue', width=bar_width,
            edgecolor='grey', label=country1)
    plt.bar(r2, data_country2, color='green',
            width=bar_width, edgecolor='grey', label=country2)

    # Add xticks on the middle of the group bars
    plt.xlabel('Year')
    plt.xticks([r + bar_width for r in range(len(years))], years)

    # Set the y-axis label
    plt.ylabel('Population Growth (Annual %)')

    # Set the plot title
    plt.title(f'Population Growth Comparison ({country1} vs {country2})')

    # Show the legend and the plot
    plt.legend()
    plt.show()


# create sample dataframe using CSV file
df_population_GW_S_A = pd.read_csv("South_Asia_Population_Growth_Data.csv")
print(df_population_GW_S_A)

# drop unnecessary rows
df_population_GW_S_A = df_population_GW_S_A.drop([59, 60])

# drop rows with missing values
df_population_GW_S_A = df_population_GW_S_A.dropna()

# drop unnecessary columns
df_population_GW_S_A = df_population_GW_S_A.drop("Time Code", axis=1)

# print cleaned data frame
print(df_population_GW_S_A)

# plot line graph
plot_line_graph(df_population_GW_S_A)

# plot pie chart
plot_pie_chart(df_population_GW_S_A, 2018)

# plot bar plot
plot_bar_plot(df_population_GW_S_A, 'India', 'Maldives')
