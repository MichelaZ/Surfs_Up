# Surf’s Up
The client is looking for investors in a surf and shake shop. One potential investor is interested in weather due to prior investment.  I used SQLAlchemy to examine Hawaiian weather from an existing database.

<details><summary><h2>Methods:</h2></summary>

1. I imported the dependencies for the project.
```
%matplotlib inline
from matplotlib import style
style.use('fivethirtyeight')
import matplotlib.pyplot as plt

import numpy as np
import pandas as pd
import datetime as dt

import sqlalchemy
from sqlalchemy.ext.automap import automap_base
from sqlalchemy.orm import Session
from sqlalchemy import create_engine, func
```
2. I prepared the engine and set the tables to a variable.
```
engine = create_engine("sqlite:///hawaii.sqlite")
Base = automap_base()
Base.prepare(engine, reflect=True)

Measurement = Base.classes.measurement
Station = Base.classes.station
session = Session(engine)
from sqlalchemy import extract
```
3. I queried the tobs and dates for June, set a list of the temperatures to a variable, created a DataFrame from that list, and used .describe() to get the June temperature summary statistics.
```
june_temps = session.query(Measurement.date, Measurement.tobs).filter(extract('month', Measurement.date)==6).all()
june_temps_list = [x.tobs for x in june_temps]
june_temp_df = pd.DataFrame(june_temps_list,columns=['June Temps'])
june_temp_df.describe()

```
![June temps . describe](https://github.com/MichelaZ/Surfs_Up/blob/main/Resources/June_temps.png)

4. I did the same thing for the December temps.
```
dec_temps = session.query(Measurement.date, Measurement.tobs).filter(extract('month', Measurement.date)==12).all()

dec_temps_list = [x.tobs for x in dec_temps]
dec_temp_df = pd.DataFrame(dec_temps_list,columns=['December Temps'])
dec_temp_df.describe()
```
![Dec temps . describe](https://github.com/MichelaZ/Surfs_Up/blob/main/Practice/dec_temps.png)

5. I queried the precipitation and dates for June, set a list of the precipitation to a variable, created a DataFrame from that list, and used . describe to get the June precipitation summary statistics.
```
june_prcp = session.query(Measurement.prcp).filter(extract('month', Measurement.date) == 6).all()
june_prcp = [x.prcp for x in june_prcp]
june_prcp_df = pd.DataFrame(june_prcp)
june_prcp_df.describe()
```
![June precipitation .describe](https://github.com/MichelaZ/Surfs_Up/blob/main/Resources/June_Prcp.png)

6. I did the same thing for the December precipitation.
```
dec_prcp = session.query(Measurement.prcp).filter(extract('month', Measurement.date) == 12).all()
dec_prcp = [x.prcp for x in dec_prcp]
dec_prcp_df = pd.DataFrame(dec_prcp)
dec_prcp_df.describe()
```
![December precipitation .describe](https://github.com/MichelaZ/Surfs_Up/blob/main/Resources/Dec_Prcp.png)

</details>

## Results
I ran additional queries to find the average precipitation for December and June in Looking at the high temps temperatures and precipitation we can glean how many days will probably be good days to grab an ice cream at the beach. The average temperatures for June (75) and December (71) as well as max temps (June: 85 December: 83) are very similar. The min temperatures (June: December: 56) and precipitation (Mean – June: .1 December: .2) differ quite a bit more from June to December. It would be good to take a look at the times of day it rains and the wind direction/speed; but there is no data on this in the database. 
## Recommendations
Weather is only one factor when it comes to whether a location is good for surfing. To really show potential investors you’ve chosen a good location for your surf/Ice Cream shop you’d want to look into surf report data such as swell size, period, and direction; offshore/Onshore wind conditions; wind speed and direction; and tide. One benefit of the surf and shake shop is that even if conditions aren’t ideal for surfing they might still be good for people to stop in for a shake.
