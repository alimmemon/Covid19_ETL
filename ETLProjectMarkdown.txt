
## ETL COVID-19

The Extract Transform and Load project extracted COVID-19 related tweets from Twitter. It included data from January all the way to April. My questions were tweets are related to COVID-19, effects of COVID19, recovery in COVID19, and health in COVID19.

## [](https://github.com/warrenallen/covid202/blob/master/final-report-md-template.md#code-walkthrough)Code Walkthrough

![](https://i.ibb.co/ggCHJ6w/Code1.png)

 - First I imported my dependencies (dependencies are needed for initiating connection to the database and used for data manipulation)
 - Second I initated the remote connection to MongoClient database using IP and other credentials.


![](https://i.ibb.co/DMN8xy0/Code2.png)

 - Defining the function search to return tweet inquiries.


![](https://i.ibb.co/Sc0m6mQ/Code3.png)

 - Definining another function to retrieve tweets sample size of 50000


![](https://i.ibb.co/QjdYpKN/Code4.png)

 - Defining empty lists, and then creating a for loop to append specific retrieved data into them such as ('created_at').


![](https://i.ibb.co/28nYbBr/Code5.png)

 - Creating empty lists to append
 - Then creating a unique for loop to append data into the lists created based on individual hastags such as #covid


![](https://i.ibb.co/2F9mjtS/Code6.png)

 - Creating a data frame using python pandas library to make data appear organinzed and neat. 
 - Then showing first 100 entries
 - Then saving retrieved data locally as comma separated values


![](https://i.ibb.co/3SZCBCt/Code7.png)

 - Creating variables to retrieve specific hastag tweets from df dataframe.
 - Then counting individual hastag.


![](https://i.ibb.co/mcm8JRC/code8.png)

 - Setting the variable equal to specific count outcome values from previous code
 - Creating a bar graph based on count values and hastags
 



![](https://i.ibb.co/znRqKxN/Code9.png)

 - Sorting the database df for tweets information for monday to sunday
 - Creating counts for each day
 - Then setting each day equal to it's specific count 
 - Then creating a data frame using python pandas library to make data appear organinzed and neat. 
 
 ![](https://i.ibb.co/JrWQYPH/Code10.png)
 
 - Defining different variables for plotting a line graph based on days and total count for each day.
 
 



![](https://i.ibb.co/Vqbwqyt/Code11.png) 

 - Sorting the database df for tweets information by months January through April.
 - Creating counts for each month and then inputting the total to its dedicated variable.

![](https://i.ibb.co/JFKQW8q/Code12.png)

 - Defining different variables for plotting a line graph based on months and total count for each month.




## [](https://github.com/warrenallen/covid202/blob/master/final-report-md-template.md#summary-of-results)Summary of Results

![](https://i.ibb.co/25kPndt/Tweets-bargraph.png)
 - Output bar graph based on different tweets
 - Shows that most individual were concerned about their health
#
![](https://i.ibb.co/WvSsr2R/tweet-linegraph.png)
 - Line graph based on saven days of tweets.
 - Graph shows us that on Monday there is an exponential growth of tweets.
 - Tuesday the exponential growth slows down a bit. Wednesday the graph suggest very little growth in tweets.
 - Beginning Thursday there is a significant crash in number of tweets going into Friday.
 - From Friday going into Saturday there is significant increase in number of tweets again.
 - From Saturday to Sunday there is an significant reduction in tweets again.
#
![](https://i.ibb.co/SJ42x9w/tweet-monthgraph.png)

 - Line graph based on January to April
 - From January to February the number of tweets kept on rising almost hitting 15000. 
 - February to March there was a significant increase reaching over 30000 tweets. And the data suggests from March to April there was a significant decrease in number of tweets that month.

## [](https://github.com/warrenallen/covid202/blob/master/final-report-md-template.md#what-i-would-do-next)What I Would do Next

I would improve upon my generated graphs, potentially showing a linear regression on the bar graph specially. I would also further explore data for potential outliers, other possibilities such as an histogram over a loger period of time. I would create more sophisticated functions to generate count data, sorting that is based months, days, timestamps and hastages combined.

## [](https://github.com/warrenallen/covid202/blob/master/final-report-md-template.md#your-full-python-script)Your full python script

"""

import pymongo
from pymongo import MongoClient
import matplotlib.pyplot as plt
import pandas as pd
import scipy.stats as st
import numpy as np
import matplotlib.patheffects as path_effects

#You can leave this alone. It connects you to the database.
client = MongoClient('161.35.53.173',
                     27017,
                     authSource='covid',
                     username='student',
                     password='dogsnotcats')  # See the week 14 video for the password
db = client['covid']
covidtweets = db['tweets']
print (covidtweets)

def search(query={}):
    """Searches Dr. Allen's COVID Tweets DB and returns a PyMongo cursor"""
    
    try:
        results = covidtweets.find(query)
    except:
        print("Could not execute query.")
        sys.exit()
    else:
        return results


def sample(samplesize=1):
    """Returns a random sample of tweets of the size past, or 1."""
    
    results = covidtweets.aggregate([{'$sample': {'size': samplesize}}])
    allowDiskUse:true
    return results

#SEARCHING FOR TWEETS
#Pass a dict to define query or you'll search all 1mil tweets.
#Comment out the line below if you're using the sample method.
data = search()


data = sample(50000)



x1= []
x2= []
x3= []


for x in data:
    x1.append(x['created_at'])
    x2.append(x['id'])
    x3.append(x['full_text'])

    
time_stamp = []
user_id = []
tweet = []

for x in range(len(x3)):
    if "covid" in x3[x]:
        time_stamp.append(x1[x])
        user_id.append(x2[x])
        tweet.append(x3[x])
    elif "effects" in x3[x]:
        time_stamp.append(x1[x])
        user_id.append(x2[x])
        tweet.append(x3[x])
    elif "recovery" in x3[x]:
        time_stamp.append(x1[x])
        user_id.append(x2[x])
        tweet.append(x3[x])
    elif "health" in x3[x]:
        time_stamp.append(x1[x])
        user_id.append(x2[x])
        tweet.append(x3[x])


df= pd.DataFrame({'Time Stamp': x1,
                'User ID': x2,
                'Tweet':x3})


df.head(100)

#initiating the CSV file
df.to_csv(r'alim_coviddata.csv', index = False, header=True)

#Sorting by category
covid19=df[df['Tweet'].str.contains("covid19")]
effects=df[df['Tweet'].str.contains("effects")]
recovery=df[df['Tweet'].str.contains("recovery")]
health=df[df['Tweet'].str.contains("health")]

#Sorting by Count
covid19.count()
effects.count()
recovery.count()
health.count()

covid_count= 400
effect_count=37
recovery_count= 43
health_count= 1103 #1103


name = [ "Effect", "Recovery", "Covid", "Health"]
numbers = [37, 43, 400, 1103]
x_axis = np.arange(len(numbers))
plt.xticks(x_axis, ["#Effect", "#Recovery", "#Covid", "#Health"])
plt.title("Number of Tweets of Based on Hastags")
plt.xlabel("Hastags")
plt.ylabel("Data based on 50000 Tweets")
plt.grid(True)
plt.bar(x_axis, numbers, color='Red', align="center")
plt.savefig("Tweets_bargraph.png")

#Tweets by Days
coviday1=df[df['Time Stamp'].str.contains("Mon")] #Monday
coviday2=df[df['Time Stamp'].str.contains("Tue")] #Tuesday
coviday3=df[df['Time Stamp'].str.contains("Wed")] #Wednesday
coviday4=df[df['Time Stamp'].str.contains("Thu")] #Thursday
coviday5=df[df['Time Stamp'].str.contains("Fri")] #Friday
coviday6=df[df['Time Stamp'].str.contains("Sat")] #Saturday
coviday7=df[df['Time Stamp'].str.contains("Sun")] #Sunday

coviday1.count() #counts for monday
coviday2.count() #counts for tuesday
coviday3.count() #counts for wednesday
coviday4.count() #counts for thursday
coviday5.count() #counts for friday
coviday6.count() #counts for saturday
coviday7.count() #counts for monday

#Counts by days
monday=6105
tuesday=7122
wednesday=7639
thursday=7679
friday=6908
saturday=7543
sunday=7004

dayframe= pd.DataFrame({'Monday': [6105],
                        'Tuesday': [7122],
                       'Wednesday': [7639],
                       'Thursday': [7679],
                       'Friday': [6908],
                       'Saturday': [7543],
                       'Sunday': [7004]})


dayframe

#line plot based on days
days = ["Mon","Tue","Wed","Thur","Fri","Sat","Sun"]
days_numbers = [6105, 7122, 7639, 7679, 6908, 7543, 7004]
plt.title("Number of Tweets of Based on Days")
plt.ylabel("Total Number of Tweets")

plt.xlabel("Linegraph based on 7 days")
plt.grid(True)
plt.plot(days, days_numbers, '-gD',markersize= 10, linewidth=7.0, path_effects=[path_effects.SimpleLineShadow(),
                       path_effects.Normal()], color='r')
plt.savefig("tweet_linegraph.png")

#Counts by Months
covidmon1=df[df['Time Stamp'].str.contains("Jan")] #January
covidmon2=df[df['Time Stamp'].str.contains("Feb")] #February
covidmon3=df[df['Time Stamp'].str.contains("Mar")] #March
covidmon4=df[df['Time Stamp'].str.contains("Apr")] #April

covidmon1.count() #counts for January
covidmon2.count() #counts for February
covidmon3.count() #counts for March
covidmon4.count() #counts for April

january=4265
february=13623
march=30799
april=1313

#Line plot based on months
months = ["January","February","March","April"]
days_numbers = [4265, 13623, 30799, 1313]
plt.title("Number of Tweets of Based on Months")
plt.ylabel("Monthly Tweets in Thousands")
plt.grid(True)
plt.legend("Month")
plt.xlabel("Linegraph based on 4 Months")

plt.plot(months, days_numbers, '-gD', markersize= 10, linewidth=7.0, path_effects=[path_effects.SimpleLineShadow(),
                       path_effects.Normal()], color='green')
                                             
plt.savefig("tweet_monthgraph.png")

"""
