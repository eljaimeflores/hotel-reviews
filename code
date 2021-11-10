import pandas as pd
import numpy as np
from textblob import TextBlob
from wordcloud import WordCloud
import matplotlib.pyplot as plt
df = pd.read_csv(r"C:\Users\Jaime\Desktop\Datafiniti_Hotel_Reviews.csv")

#Dropping columns that do not have useful information
df = df.drop(columns=['reviews.sourceURLs', 'sourceURLs',"websites","keys"])
states = df["province"].unique()

#Creating a function to seperate the dataset by state 
def getStatedf(state):
    statedf = df.loc[df["province"] == state]
    file_name = state + "_reviews.xlsx"
    statedf.to_excel(file_name)
    print(file_name,"has been created")
    return statedf
    
# With this split list comprehension
# I can make any df that I want by calling the index I want
# Ex: WAdf = split[2]
split = [getStatedf(states[i]) for i in range(len(states))]
#We can also read any of the files made and use those as df, but utilizing this list is more practical

"""With the reviews separated by state it will be easier to analyze the overall sentiment at a more granular scale using the following metrics:<br> 
1.Polarity is float which lies in the range of [-1,1] where 1 means positive statement and -1 means a negative statement.<br>2.Subjective sentences generally refer to personal opinion, emotion or judgment whereas objective refers to factual information. Subjectivity is also a float which lies in the range of [0,1].<br>
However it will be interesting to visualize the overall sentiment across the country"""

#Create subjectivity function
def getSubjectivity(text):
    return TextBlob(text).sentiment.subjectivity

#Create Polarity function
def getPolarity(text):
    return TextBlob(text).sentiment.polarity
    
 # The reviews are of data type object, using ".astype('str')" 
# changes the column into a string, which is the correct data type.
# This will add subjectivity & Polarity columns to the main df 
df["Subjectivity"] = round(df["reviews.text"].astype('str').apply(getSubjectivity),4)
df["Polarity"] = round(df["reviews.text"].astype('str').apply(getPolarity),4) 

#Now subjectivity and polarity can be visualized across the country
import plotly.graph_objects as go
fig = go.Figure(data=go.Scattergeo(
        locationmode = 'USA-states',
        lon = df['longitude'],
        lat = df['latitude'],
        text = df['city']+" "+ "-Polarity is: " + df['Polarity'].astype('str') + ",Subejctivity is: "+ df["Subjectivity"].astype('str'),
        mode = 'markers',
        marker = dict(
            size = 8,
            opacity = 0.8,
            reversescale = False,
            autocolorscale = False,
            symbol = 'circle',
            line = dict(
                width=1,
                color='rgba(102, 102, 102)'
            ),
            colorscale = 'Greens',
            cmin = -1,
            color = df['Polarity'],
            cmax = df['Polarity'].max(),
            colorbar_title="Polarity<br>[-1,1]",
        )))

fig.update_layout(
        title = 'Hotel Reviews<br>Hover for city and score',
        geo = dict(
            scope='usa',
            projection_type='albers usa',
            showland = True,
            landcolor = "rgb(250, 250, 250)",
            subunitcolor = "rgb(217, 217, 217)",
            countrycolor = "rgb(217, 217, 217)",
            countrywidth = 0.5,
            subunitwidth = 0.5
        ),
    )
fig.show() 

"""At a glance it appears as though the coasts have more positive reviews than the hotels in the middle of the country.
From here it would be interesting to see whether or not the positive sentiment is backed up by objectivite reviews. 
There may be negative reviews based on the color of the room or the weather during someone's stay, which is not depictived in this visualization. 
Hence why both polarity and subjectivity are used as an ordered pair."""








