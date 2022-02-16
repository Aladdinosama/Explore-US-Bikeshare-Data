# Explore-US-Bikeshare-Data
We are going to analyze US Bikeshare Data using Python

import time
import pandas as pd
import numpy as np

CITY_DATA = { 'ch': 'chicago.csv',
              'ny': 'new_york_city.csv',
              'wa': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    while True:
        city = input ("\nplease enter first two letter for the wanted city: " ).lower()
        if city in CITY_DATA:
            break
        else :
            print ("\ninvalid input")
            
    # TO DO: get user input for month (all, january, february, ... , june)
    months = ["jan","feb","mar","apr","may","jun","all"]
    while True:
        month = input ("please enter first three letters for the wanted month (jan,feb,mar,apr,may,jun) or (all) for no month filter: ").lower()
        if month in months :
            break
        else :
            print ("\n invalid input\n")

    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    days=["sat","sun","mon","tue","wed","thu","fri","all"]
    while True :
        day = input("please enter first three letters for the wanted day (sat,sun,mon,tue,wed,thu,fri) or (all) for no day filter: ").lower()
        if day in days:
            break
        else :
            print("invalid input")

    print('-'*40)
    
    return city,month,day

def load_data(city,month,day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    df = pd.read_csv(CITY_DATA[city])
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    df['month'] = df['Start Time'].dt.month_name()
    df['day'] = df['Start Time'].dt.day_name()
    if month != 'all':
        df = df[df['month'].str.startswith(month.title())]
    if day != 'all':
        df = df[df['day'].str.startswith(day.title())]

    return df

def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    common_month = df['month'].mode()[0]
    print ("most frequent month: {}".format(common_month))


    # TO DO: display the most common day of week
    common_day = df['day'].mode()[0]
    print ("most frequent day: {}".format(common_day))


    # TO DO: display the most common start hour
    df['hour'] = df['Start Time'].dt.hour
    common_hour = df['hour'].mode()[0]
    print("most frequent hour: {}".format(common_hour))


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    common_start_station = df['Start Station'].mode()[0]
    print("most common start station: {}\n".format(common_start_station))


    # TO DO: display most commonly used end station
    end_station = df['End Station'].mode()[0]
    print ("most common end station: {}\n".format(end_station))


    # TO DO: display most frequent combination of start station and end station trip
    df['trip'] = df['Start Station'] + ' - ' + df['End Station']
    trip = df['trip'].mode()[0]
    print ("most common trip: {}".format(trip))


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    total_time = df['Trip Duration'].sum()
    print ("Total travel time: {} s".format(total_time))


    # TO DO: display mean travel time
    mean_time = df['Trip Duration'].mean()
    print ("Mean travel time: {} s".format(mean_time))


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)
    
def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    types = df['User Type'].value_counts()
    print ('counts of user types :\n{}'.format(types.to_string()))


    # TO DO: Display counts of gender
    if 'Gender' in df :
        gender = df['Gender'].value_counts()
        print('\ncounts of gender :\n{}'.format(gender.to_string()))
    else :
        print ("\nthis city hasn't gender data for users")


    # TO DO: Display earliest, most recent, and most common year of birth
    if 'Birth Year' in df :
        earliest = df['Birth Year'].min()
        print('\nearliest year of birth : {}\n'.format(earliest.astype(int)))
        recent = df['Birth Year'].max()
        print('most recent year of birth : {}\n'.format(recent.astype(int)))
        common_year = df['Birth Year'].mode()[0]
        print('most common year of birth : {}\n'.format(common_year.astype(int)))
    else :
        print("\nthis city hasn't birth data for users")


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def raw(df):
    loc = 0
    while True:
        answer = input("check the raw data as 5 raws? (y/n)\n").lower()
        if answer not in ["y","n"]:
            print("invalid answer")
        elif answer == "y":
            print(df.iloc[loc:loc+5])
            loc += 5
        elif answer == "n":
            break

def main():
    while True:
        city,month,day = get_filters()
        df = load_data(city,month,day)
        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)
        raw(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
