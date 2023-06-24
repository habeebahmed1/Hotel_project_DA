# Hotel Booking Data Analysis project

This project is based on Hotel Booking for this project I took data from kaggle [Link](https://www.kaggle.com/datasets/mojtaba142/hotel-booking). In this project we have to find the solution to increasing number to reservation cancellation by analyzing the data. what are the steps we can take to increase the number of reservation and lower the number of cancellations.

## 1)Packages Used in this Project

'''
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
import datetime as dt '''

## 2)load data using python

'''
df = pd.read_csv('hotel_booking.csv')
'''

## 3) View data

'''
df.head()
![head](/screenshots/head.jpg)

'''

## 4) Check how many number to rows and columns

'''
df.shape()
'''
![shape](/screenshots/rowsandcolumns.JPG)

## 5) Check the names of the columns

'''
df.columns

![clm.name](/screenshots/column%20names.JPG)

## 6) get the info of the data

'''
df.info()
'''
![info](/screenshots/info.JPG)

## 7) Change the data type of reservation_status_date to datetime

'''
df['reservation_status_date'] = pd.to_datetime(df['reservation_status_date'])

'''

## 8) describe data include objects

'''
for col in df.describe(include = 'object').columns:
print(col)
print(df[col].unique())
print('-'\*50)

![describe](/screenshots/describe.JPG)
'''

## 9) check the missing value

'''
df.isnull().sum()

![missingvalue](/screenshots/missingvalue.JPG)
'''

## 10) Droping two value'company','agent'

'''
df.drop(['company','agent'], axis = 1, inplace = True)
df.dropna(inplace = True)

'''

## 11) Check how many percent is cancelation done

'''
cancelled_percent = df['is_canceled'].value_counts(normalize = True)
print(cancelled_percent)
'''
![cancellation percentage](/screenshots/cancelation%20percentage.JPG)

## 12) Check in figure

'''
plt.figure(figsize = (5,4))
plt.title('Reservation status count')
plt.bar(['Not canceled', 'Canceled'],df['is_canceled'].value_counts(), edgecolor = 'k', width = 0.7)
plt.show()
'''
![cancellation fig](/screenshots/cancel%20fig.JPG)

## 13) comparing using seaborn(sns) count method

'''
plt.figure(figsize = (8,4))
ax1 = sns.countplot(x = 'hotel', hue = 'is*canceled', data = df, palette = 'Blues')
legend_labels,* = ax1. get_legend_handles_labels()
ax1.legend(bbox_to_anchor = (1,1))
plt.title('Reservation statue in different hotels', size = 20)
plt.xlabel('hotel')
plt.ylabel('number of reservations')
plt.legend(['not canceled', 'canceled'])
plt.show()
'''
![comparing](/screenshots/comparing.JPG)

## 14) chack the percentage of cancelation of resort hotels

'''
resort_hotel = df[df['hotel'] == 'Resort Hotel']
resort_hotel['is_canceled'].value_counts(normalize = True)
'''
![comparing](/screenshots/cancelation%20percentage%20of%20resort%20hotel.JPG)

## 15) chack the percentage of cancelation of city hotels

'''
City_hotel = df[df['hotel'] == 'City Hotel']
City_hotel['is_canceled'].value_counts(normalize = True)
'''

![comparing](/screenshots/cancelation%20percentage%20of%20city%20hotel.JPG)

## 16) Get the mean of both the hotels

'''
resort_hotel = resort_hotel.groupby('reservation_status_date')[['adr']].mean()
City_hotel = City_hotel.groupby('reservation_status_date')[['adr']].mean()

'''

## 17) Average Daily Rate in City and Resort Hotel

'''
plt.figure(figsize = (20,8))
plt.title('Average Daily Rate in City and Resort Hotel', fontsize = 30)
plt.plot(resort_hotel.index, resort_hotel['adr'], label = 'Resort Hotel')
plt.plot(City_hotel.index, City_hotel['adr'], label = 'City Hotel')
plt.legend(fontsize = 20)
plt.show()
'''

![avrg](/screenshots/Average%20daily%20rate%20in%20city%20and%20resort%20Hotel.JPG)

## 18) Reservation Status per month

'''
df['month'] = df['reservation_status_date'].dt.month
plt.figure(figsize = (16,8))
ax1 = sns.countplot(x = 'month', hue = 'is*canceled', data = df, palette = 'bright')
legend_labels,* = ax1. get_legend_handles_labels()
ax1.legend(bbox_to_anchor=(1,1))
plt.title('Reservation Status per month', size = 20)
plt.xlabel('month')
plt.ylabel('number of reservations')
plt.legend(['not canceled', 'canceled'])
plt.show()
'''

![avrg ](/screenshots/Reservation%20status%20per%20month.JPG)

## 19) ADR permonth

'''
plt.figure(figsize = (15,8))
plt.title('ADR per month', fontsize = 30)
sns.barplot('month', 'adr', data = df[df['is_canceled'] == 1].groupby('month')[['adr']].sum().reset_index())
plt.show()
'''

![adr](/screenshots/adrpermonth.JPG)

## 20) Top 10 countries with reservation canceled

'''
cancelled_data = df[df['is_canceled'] ==1]
top_10_country = cancelled_data['country'].value_counts()[:10]
plt.figure(figsize = (8,8))
plt.title('Top 10 countries with reservation canceled')
plt.pie(top_10_country, autopct = '%.2f', labels = top_10_country.index)
plt.show()

'''
![countries](/screenshots/top%2010%20countries.JPG)

## 21) View market_segment

'''
df['market_segment'].value_counts(normalize = True)
'''
![market segment](/screenshots/Market%20segment.JPG)

## 22) cancelled data

'''
cancelled_data['market_segment'].value_counts(normalize = True)

'''
![Cancelled data](/screenshots/cancelled%20data.JPG)

## 23) Average Daily Rate 2016 and 2017 data

'''

cancelled_df_adr = cancelled_df_adr[(cancelled_df_adr['reservation_status_date']>'2016') & (cancelled_df_adr['reservation_status_date']<'2017-09')]
not_cancelled_df_adr = not_cancelled_df_adr[(not_cancelled_df_adr['reservation_status_date']>'2016') & (not_cancelled_df_adr['reservation_status_date']<'2017-09')]

plt.figure(figsize = (20,6))
plt.title('Average Daily Rate')
plt.plot(not_cancelled_df_adr['reservation_status_date'],not_cancelled_df_adr['adr'], label = 'not cancelled')
plt.plot(cancelled_df_adr['reservation_status_date'],cancelled_df_adr['adr'], label = 'cancelled')
plt.legend()

'''
![Cancelled data](/screenshots/Average%20Daily%20rate.JPG)
