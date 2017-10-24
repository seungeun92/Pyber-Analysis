

```python
# Dependencies
from citipy import citipy
from matplotlib import pyplot as plt
from scipy import stats
import numpy as np
import pandas as pd
import seaborn as sns
import requests
```


```python
#Read worldcities.csv from citipy
api_key= "518f982824caeb562219656130eaf212"
worldcities = "citipy/worldcities.csv"
cities_df = pd.read_csv(worldcities)
```


```python
#sample random from cities_df
selected_cities = cities_df.sample(n=500)
five_cities= selected_cities.reset_index()
five_cities.drop(["index"], axis=1, inplace = True)
```


```python
city_weather = five_cities
```


```python
# Create blank columns for necessary fields
city_weather["Temperature (F)"] = ""
city_weather["Humidity (%)"] = ""
city_weather["Cloudiness (%)"] = ""
city_weather["Wind Speed (mph)"] = ""
```


```python
# Counter
row_count = 1

# Loop through and grab the lat/lng using Google maps
for index, row in city_weather.iterrows():
    
    # Create endpoint URL
    target_url = "http://api.openweathermap.org/data/2.5/weather?lat=%s&lon=%s&APPID=%s&units=imperial" % (city_weather.iloc[index]["Latitude"], city_weather.iloc[index]["Longitude"], api_key)
    # Print log to ensure loop is working correctly
    print("Processing Record " + str(row_count) + " of 500 | " + city_weather.iloc[index]["City"])
    print(target_url)
    row_count += 1
    
    # Run requests to grab the JSON at the requested URL
    weather_location = requests.get(target_url).json()
    
    # Append the max temp, humidity, cloud %, and wind speed to the appropriate columns
    # Use try / except to skip any cities with errors
    try: 
        weather_temp = weather_location["main"]["temp_max"]
        weather_humid = weather_location["main"]["humidity"]
        weather_cloud = weather_location["clouds"]["all"]
        weather_wind = weather_location["wind"]["speed"]
        
        city_weather.set_value(index, "Temperature (F)", weather_temp)
        city_weather.set_value(index, "Humidity (%)", weather_humid)
        city_weather.set_value(index, "Cloudiness (%)", weather_cloud)
        city_weather.set_value(index, "Wind Speed (mph)", weather_wind)
        
    except:
        print("Error with city data. Skipping")
        continue
        
# Visualize
city_weather.head()
```

    Processing Record 1 of 500 | paullo
    http://api.openweathermap.org/data/2.5/weather?lat=45.416667&lon=9.4&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 2 of 500 | poysdorf
    http://api.openweathermap.org/data/2.5/weather?lat=48.666667&lon=16.633333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 3 of 500 | jhajjar
    http://api.openweathermap.org/data/2.5/weather?lat=28.616667&lon=76.65&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 4 of 500 | kamenz
    http://api.openweathermap.org/data/2.5/weather?lat=51.266667&lon=14.1&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 5 of 500 | malihabad
    http://api.openweathermap.org/data/2.5/weather?lat=26.916667&lon=80.716667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 6 of 500 | el aguila
    http://api.openweathermap.org/data/2.5/weather?lat=4.91345&lon=-76.040042&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 7 of 500 | lins
    http://api.openweathermap.org/data/2.5/weather?lat=-21.666667&lon=-49.75&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 8 of 500 | oshakati
    http://api.openweathermap.org/data/2.5/weather?lat=-17.7833333&lon=15.6833333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 9 of 500 | vad
    http://api.openweathermap.org/data/2.5/weather?lat=47.2&lon=23.75&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 10 of 500 | isahaya
    http://api.openweathermap.org/data/2.5/weather?lat=32.841111&lon=130.043056&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 11 of 500 | asmar
    http://api.openweathermap.org/data/2.5/weather?lat=35.033328&lon=71.358087&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 12 of 500 | jvari
    http://api.openweathermap.org/data/2.5/weather?lat=42.7138889&lon=42.0536111&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 13 of 500 | jaltepec
    http://api.openweathermap.org/data/2.5/weather?lat=19.733333&lon=-98.633333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 14 of 500 | forchheim
    http://api.openweathermap.org/data/2.5/weather?lat=49.723056&lon=11.066944&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 15 of 500 | santa rosa de lima
    http://api.openweathermap.org/data/2.5/weather?lat=13.6247222&lon=-87.8936111&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 16 of 500 | ghosi
    http://api.openweathermap.org/data/2.5/weather?lat=26.108333&lon=83.543611&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 17 of 500 | santo tomas
    http://api.openweathermap.org/data/2.5/weather?lat=8.185833&lon=125.803889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 18 of 500 | sokol
    http://api.openweathermap.org/data/2.5/weather?lat=59.461667&lon=40.120556&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 19 of 500 | alcanena
    http://api.openweathermap.org/data/2.5/weather?lat=39.459005&lon=-8.668924&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 20 of 500 | nyirbogat
    http://api.openweathermap.org/data/2.5/weather?lat=47.803395&lon=22.065606&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 21 of 500 | boyolangu
    http://api.openweathermap.org/data/2.5/weather?lat=-8.1181&lon=111.8935&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 22 of 500 | monte san pietro
    http://api.openweathermap.org/data/2.5/weather?lat=44.433333&lon=11.133333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 23 of 500 | kristiansand
    http://api.openweathermap.org/data/2.5/weather?lat=58.166667&lon=8.0&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 24 of 500 | yangjiang
    http://api.openweathermap.org/data/2.5/weather?lat=21.85&lon=111.966667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 25 of 500 | teavaro
    http://api.openweathermap.org/data/2.5/weather?lat=-17.5&lon=-149.7666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 26 of 500 | mtsensk
    http://api.openweathermap.org/data/2.5/weather?lat=53.276567&lon=36.573337&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 27 of 500 | gehrden
    http://api.openweathermap.org/data/2.5/weather?lat=52.316667&lon=9.6&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 28 of 500 | krasnozavodsk
    http://api.openweathermap.org/data/2.5/weather?lat=56.45&lon=38.216667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 29 of 500 | parintins
    http://api.openweathermap.org/data/2.5/weather?lat=-2.6&lon=-56.733333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 30 of 500 | piraju
    http://api.openweathermap.org/data/2.5/weather?lat=-23.2&lon=-49.383333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 31 of 500 | musaler
    http://api.openweathermap.org/data/2.5/weather?lat=40.1544444&lon=44.3777778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 32 of 500 | lavos
    http://api.openweathermap.org/data/2.5/weather?lat=40.093628&lon=-8.828263&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 33 of 500 | retalhuleu
    http://api.openweathermap.org/data/2.5/weather?lat=14.533333&lon=-91.683333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 34 of 500 | demir kapija
    http://api.openweathermap.org/data/2.5/weather?lat=41.405&lon=22.2488889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 35 of 500 | huaihua
    http://api.openweathermap.org/data/2.5/weather?lat=27.549444&lon=109.959167&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 36 of 500 | ambarawa
    http://api.openweathermap.org/data/2.5/weather?lat=-7.263333&lon=110.3975&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 37 of 500 | borovskoy
    http://api.openweathermap.org/data/2.5/weather?lat=53.8&lon=64.15&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 38 of 500 | llanera
    http://api.openweathermap.org/data/2.5/weather?lat=43.433333&lon=-5.883333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 39 of 500 | mayskiy
    http://api.openweathermap.org/data/2.5/weather?lat=50.519881&lon=36.458777&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 40 of 500 | chiautla
    http://api.openweathermap.org/data/2.5/weather?lat=19.533333&lon=-98.883333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 41 of 500 | sibulao
    http://api.openweathermap.org/data/2.5/weather?lat=7.324545&lon=122.241799&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 42 of 500 | vasai
    http://api.openweathermap.org/data/2.5/weather?lat=19.35&lon=72.8&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 43 of 500 | rubi
    http://api.openweathermap.org/data/2.5/weather?lat=41.492256&lon=2.033047&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 44 of 500 | schitu
    http://api.openweathermap.org/data/2.5/weather?lat=44.35&lon=24.566667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 45 of 500 | tehachapi
    http://api.openweathermap.org/data/2.5/weather?lat=35.1322222&lon=-118.4480556&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 46 of 500 | cawayan
    http://api.openweathermap.org/data/2.5/weather?lat=13.3672&lon=122.5094&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 47 of 500 | gambat
    http://api.openweathermap.org/data/2.5/weather?lat=27.351453&lon=68.52051&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 48 of 500 | garoafa
    http://api.openweathermap.org/data/2.5/weather?lat=45.783333&lon=27.2&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 49 of 500 | ville-marie
    http://api.openweathermap.org/data/2.5/weather?lat=47.333333&lon=-79.433333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 50 of 500 | plan-les-ouates
    http://api.openweathermap.org/data/2.5/weather?lat=46.167615&lon=6.119125&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 51 of 500 | petropavlivka
    http://api.openweathermap.org/data/2.5/weather?lat=48.456429&lon=36.436699&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 52 of 500 | kolga
    http://api.openweathermap.org/data/2.5/weather?lat=59.4866667&lon=25.6191667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 53 of 500 | baryshevo
    http://api.openweathermap.org/data/2.5/weather?lat=54.9564&lon=83.1822&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 54 of 500 | wembley
    http://api.openweathermap.org/data/2.5/weather?lat=55.15&lon=-119.15&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 55 of 500 | alexandria
    http://api.openweathermap.org/data/2.5/weather?lat=43.983333&lon=25.333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 56 of 500 | findlay
    http://api.openweathermap.org/data/2.5/weather?lat=41.0441667&lon=-83.65&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 57 of 500 | portoroz
    http://api.openweathermap.org/data/2.5/weather?lat=45.5161111&lon=13.5883333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 58 of 500 | trencianske teplice
    http://api.openweathermap.org/data/2.5/weather?lat=48.9166667&lon=18.1666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 59 of 500 | izvoarele
    http://api.openweathermap.org/data/2.5/weather?lat=44.033889&lon=25.774722&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 60 of 500 | volodarsk
    http://api.openweathermap.org/data/2.5/weather?lat=56.231051&lon=43.187671&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 61 of 500 | enghien-les-bains
    http://api.openweathermap.org/data/2.5/weather?lat=48.966667&lon=2.316667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 62 of 500 | bay shore
    http://api.openweathermap.org/data/2.5/weather?lat=40.725&lon=-73.2458333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 63 of 500 | actopan
    http://api.openweathermap.org/data/2.5/weather?lat=20.266667&lon=-98.933333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 64 of 500 | fossombrone
    http://api.openweathermap.org/data/2.5/weather?lat=43.683333&lon=12.8&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 65 of 500 | libertad
    http://api.openweathermap.org/data/2.5/weather?lat=8.2755&lon=123.8382&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 66 of 500 | putnam
    http://api.openweathermap.org/data/2.5/weather?lat=41.915&lon=-71.9094444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 67 of 500 | novoaltaysk
    http://api.openweathermap.org/data/2.5/weather?lat=53.3917&lon=83.9363&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 68 of 500 | olmos
    http://api.openweathermap.org/data/2.5/weather?lat=-5.9847222&lon=-79.7452778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 69 of 500 | arteche
    http://api.openweathermap.org/data/2.5/weather?lat=12.2645&lon=125.4048&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 70 of 500 | arandas
    http://api.openweathermap.org/data/2.5/weather?lat=20.7&lon=-102.35&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 71 of 500 | diavatos
    http://api.openweathermap.org/data/2.5/weather?lat=40.5455556&lon=22.2669444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 72 of 500 | balky
    http://api.openweathermap.org/data/2.5/weather?lat=47.383365&lon=34.94396&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 73 of 500 | kintore
    http://api.openweathermap.org/data/2.5/weather?lat=57.216667&lon=-2.35&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 74 of 500 | arrecife
    http://api.openweathermap.org/data/2.5/weather?lat=28.965706&lon=-13.551378&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 75 of 500 | sengiley
    http://api.openweathermap.org/data/2.5/weather?lat=53.962222&lon=48.794444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 76 of 500 | tournefeuille
    http://api.openweathermap.org/data/2.5/weather?lat=43.583905&lon=1.34685&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 77 of 500 | samarskoye
    http://api.openweathermap.org/data/2.5/weather?lat=46.937&lon=39.6881&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 78 of 500 | artigas
    http://api.openweathermap.org/data/2.5/weather?lat=-30.4&lon=-56.4666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 79 of 500 | champerico
    http://api.openweathermap.org/data/2.5/weather?lat=14.3&lon=-91.916667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 80 of 500 | miramar
    http://api.openweathermap.org/data/2.5/weather?lat=22.266667&lon=-97.8&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 81 of 500 | krk
    http://api.openweathermap.org/data/2.5/weather?lat=45.0258333&lon=14.5730556&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 82 of 500 | bolvasnita
    http://api.openweathermap.org/data/2.5/weather?lat=45.35&lon=22.3&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 83 of 500 | laguindingan
    http://api.openweathermap.org/data/2.5/weather?lat=8.574722&lon=124.442222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 84 of 500 | olpe
    http://api.openweathermap.org/data/2.5/weather?lat=51.033333&lon=7.85&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 85 of 500 | elatia
    http://api.openweathermap.org/data/2.5/weather?lat=38.6333333&lon=22.7666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 86 of 500 | shahganj
    http://api.openweathermap.org/data/2.5/weather?lat=26.05&lon=82.683333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 87 of 500 | san jeronimo de tunan
    http://api.openweathermap.org/data/2.5/weather?lat=-11.95&lon=-75.25&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 88 of 500 | harihar
    http://api.openweathermap.org/data/2.5/weather?lat=14.516667&lon=75.8&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 89 of 500 | oreye
    http://api.openweathermap.org/data/2.5/weather?lat=50.733333&lon=5.366667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 90 of 500 | mikonos
    http://api.openweathermap.org/data/2.5/weather?lat=37.45&lon=25.3333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 91 of 500 | csanadapaca
    http://api.openweathermap.org/data/2.5/weather?lat=46.55&lon=20.883333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 92 of 500 | hasselt
    http://api.openweathermap.org/data/2.5/weather?lat=50.933333&lon=5.333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 93 of 500 | anadyr
    http://api.openweathermap.org/data/2.5/weather?lat=64.75&lon=177.483333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 94 of 500 | sanford
    http://api.openweathermap.org/data/2.5/weather?lat=28.8002778&lon=-81.2733333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 95 of 500 | arroio dos ratos
    http://api.openweathermap.org/data/2.5/weather?lat=-30.083333&lon=-51.716667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 96 of 500 | san francisco del valle
    http://api.openweathermap.org/data/2.5/weather?lat=14.4333333&lon=-88.95&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 97 of 500 | catina
    http://api.openweathermap.org/data/2.5/weather?lat=46.85&lon=24.183333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 98 of 500 | nueva florida
    http://api.openweathermap.org/data/2.5/weather?lat=15.4666667&lon=-87.5&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 99 of 500 | san juan de opoa
    http://api.openweathermap.org/data/2.5/weather?lat=14.7833333&lon=-88.7&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 100 of 500 | palestina
    http://api.openweathermap.org/data/2.5/weather?lat=-1.9333333&lon=-79.7333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 101 of 500 | lencois paulista
    http://api.openweathermap.org/data/2.5/weather?lat=-22.6&lon=-48.783333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 102 of 500 | yakuplu
    http://api.openweathermap.org/data/2.5/weather?lat=40.983333&lon=28.65&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 103 of 500 | barbatesti
    http://api.openweathermap.org/data/2.5/weather?lat=44.866667&lon=23.5&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 104 of 500 | komarichi
    http://api.openweathermap.org/data/2.5/weather?lat=52.4151&lon=34.7905&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 105 of 500 | tequila
    http://api.openweathermap.org/data/2.5/weather?lat=20.883333&lon=-103.833333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 106 of 500 | socol
    http://api.openweathermap.org/data/2.5/weather?lat=44.860833&lon=21.370278&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 107 of 500 | khlong luang
    http://api.openweathermap.org/data/2.5/weather?lat=14.064667&lon=100.645778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 108 of 500 | ukwa
    http://api.openweathermap.org/data/2.5/weather?lat=21.966667&lon=80.466667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 109 of 500 | schiffweiler
    http://api.openweathermap.org/data/2.5/weather?lat=49.366667&lon=7.133333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 110 of 500 | alexandroupoli
    http://api.openweathermap.org/data/2.5/weather?lat=40.8475&lon=25.8744444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 111 of 500 | aydun
    http://api.openweathermap.org/data/2.5/weather?lat=32.507222&lon=35.8575&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 112 of 500 | shahdadkot
    http://api.openweathermap.org/data/2.5/weather?lat=27.847709&lon=67.905497&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 113 of 500 | peshawar
    http://api.openweathermap.org/data/2.5/weather?lat=34.008&lon=71.578488&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 114 of 500 | comerio
    http://api.openweathermap.org/data/2.5/weather?lat=18.22&lon=-66.2263889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 115 of 500 | vicente guerrero
    http://api.openweathermap.org/data/2.5/weather?lat=23.75&lon=-103.983333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 116 of 500 | alangilanan
    http://api.openweathermap.org/data/2.5/weather?lat=9.642&lon=123.1059&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 117 of 500 | lanciano
    http://api.openweathermap.org/data/2.5/weather?lat=42.233333&lon=14.383333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 118 of 500 | buzovna
    http://api.openweathermap.org/data/2.5/weather?lat=40.517888&lon=50.113901&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 119 of 500 | schenefeld
    http://api.openweathermap.org/data/2.5/weather?lat=53.6&lon=9.816667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 120 of 500 | kamena vourla
    http://api.openweathermap.org/data/2.5/weather?lat=38.7833333&lon=22.7833333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 121 of 500 | huaiyin
    http://api.openweathermap.org/data/2.5/weather?lat=33.588611&lon=119.019167&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 122 of 500 | guarapuava
    http://api.openweathermap.org/data/2.5/weather?lat=-25.383333&lon=-51.45&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 123 of 500 | mehran
    http://api.openweathermap.org/data/2.5/weather?lat=33.1222&lon=46.1646&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 124 of 500 | sao francisco
    http://api.openweathermap.org/data/2.5/weather?lat=-15.95&lon=-44.866667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 125 of 500 | drensteinfurt
    http://api.openweathermap.org/data/2.5/weather?lat=51.8&lon=7.75&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 126 of 500 | yamachiche
    http://api.openweathermap.org/data/2.5/weather?lat=46.283333&lon=-72.833333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 127 of 500 | peski
    http://api.openweathermap.org/data/2.5/weather?lat=51.2531&lon=42.4693&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 128 of 500 | novomikhaylovskiy
    http://api.openweathermap.org/data/2.5/weather?lat=44.2626&lon=38.8585&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 129 of 500 | conchagua
    http://api.openweathermap.org/data/2.5/weather?lat=13.3077778&lon=-87.8647222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 130 of 500 | san antonio palopo
    http://api.openweathermap.org/data/2.5/weather?lat=14.7&lon=-91.116667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 131 of 500 | andros
    http://api.openweathermap.org/data/2.5/weather?lat=37.8333333&lon=24.9333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 132 of 500 | brest
    http://api.openweathermap.org/data/2.5/weather?lat=48.390756&lon=-4.486165&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 133 of 500 | cagliari
    http://api.openweathermap.org/data/2.5/weather?lat=39.216667&lon=9.116667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 134 of 500 | waregem
    http://api.openweathermap.org/data/2.5/weather?lat=50.883333&lon=3.416667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 135 of 500 | elwood
    http://api.openweathermap.org/data/2.5/weather?lat=40.2769444&lon=-85.8419444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 136 of 500 | breves
    http://api.openweathermap.org/data/2.5/weather?lat=-1.666667&lon=-50.483333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 137 of 500 | tapel
    http://api.openweathermap.org/data/2.5/weather?lat=18.28907&lon=122.029153&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 138 of 500 | tenancingo
    http://api.openweathermap.org/data/2.5/weather?lat=19.147222&lon=-98.180556&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 139 of 500 | bania
    http://api.openweathermap.org/data/2.5/weather?lat=44.895833&lon=22.044722&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 140 of 500 | rasht
    http://api.openweathermap.org/data/2.5/weather?lat=37.280769&lon=49.58319&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 141 of 500 | soller
    http://api.openweathermap.org/data/2.5/weather?lat=39.768206&lon=2.716361&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 142 of 500 | damilag
    http://api.openweathermap.org/data/2.5/weather?lat=8.354722&lon=124.812222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 143 of 500 | rheinfelden
    http://api.openweathermap.org/data/2.5/weather?lat=47.562979&lon=7.780108&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 144 of 500 | ottaviano
    http://api.openweathermap.org/data/2.5/weather?lat=40.85&lon=14.466667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 145 of 500 | madhogarh
    http://api.openweathermap.org/data/2.5/weather?lat=26.276&lon=79.1869&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 146 of 500 | shahada
    http://api.openweathermap.org/data/2.5/weather?lat=21.466667&lon=74.3&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 147 of 500 | chilibre
    http://api.openweathermap.org/data/2.5/weather?lat=9.15&lon=-79.6166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 148 of 500 | alem paraiba
    http://api.openweathermap.org/data/2.5/weather?lat=-21.866667&lon=-42.683333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 149 of 500 | taggia
    http://api.openweathermap.org/data/2.5/weather?lat=43.866667&lon=7.85&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 150 of 500 | aksu
    http://api.openweathermap.org/data/2.5/weather?lat=41.123056&lon=80.264444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 151 of 500 | apopka
    http://api.openweathermap.org/data/2.5/weather?lat=28.6802778&lon=-81.5097222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 152 of 500 | krasnyy oktyabr
    http://api.openweathermap.org/data/2.5/weather?lat=48.25&lon=38.2&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 153 of 500 | hooglede
    http://api.openweathermap.org/data/2.5/weather?lat=50.983333&lon=3.083333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 154 of 500 | kongwa
    http://api.openweathermap.org/data/2.5/weather?lat=-6.2&lon=36.4166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 155 of 500 | buturlinovka
    http://api.openweathermap.org/data/2.5/weather?lat=50.823889&lon=40.609167&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 156 of 500 | exeter
    http://api.openweathermap.org/data/2.5/weather?lat=43.35&lon=-81.483333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 157 of 500 | janja
    http://api.openweathermap.org/data/2.5/weather?lat=44.6652778&lon=19.2477778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 158 of 500 | milingimbi
    http://api.openweathermap.org/data/2.5/weather?lat=-12.10188&lon=134.919006&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 159 of 500 | shirokaya rechka
    http://api.openweathermap.org/data/2.5/weather?lat=56.6694&lon=60.4726&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 160 of 500 | poanas
    http://api.openweathermap.org/data/2.5/weather?lat=25.766667&lon=-103.616667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 161 of 500 | ambulu
    http://api.openweathermap.org/data/2.5/weather?lat=-8.3504&lon=113.6069&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 162 of 500 | norsup
    http://api.openweathermap.org/data/2.5/weather?lat=-16.0666667&lon=167.3833333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 163 of 500 | cristinesti
    http://api.openweathermap.org/data/2.5/weather?lat=48.1&lon=26.383333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 164 of 500 | qostanay
    http://api.openweathermap.org/data/2.5/weather?lat=53.166667&lon=63.583333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 165 of 500 | ningbo
    http://api.openweathermap.org/data/2.5/weather?lat=29.878186&lon=121.549453&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 166 of 500 | livadi
    http://api.openweathermap.org/data/2.5/weather?lat=40.125&lon=22.1516667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 167 of 500 | forst
    http://api.openweathermap.org/data/2.5/weather?lat=51.733333&lon=14.633333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 168 of 500 | north creek
    http://api.openweathermap.org/data/2.5/weather?lat=47.8197222&lon=-122.175&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 169 of 500 | lagonglong
    http://api.openweathermap.org/data/2.5/weather?lat=8.807222&lon=124.790278&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 170 of 500 | butte
    http://api.openweathermap.org/data/2.5/weather?lat=46.0038889&lon=-112.5338889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 171 of 500 | manzanillo
    http://api.openweathermap.org/data/2.5/weather?lat=19.05&lon=-104.333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 172 of 500 | dracut
    http://api.openweathermap.org/data/2.5/weather?lat=42.6702778&lon=-71.3025&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 173 of 500 | sungai siput utara
    http://api.openweathermap.org/data/2.5/weather?lat=4.8151&lon=101.0786&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 174 of 500 | pilar
    http://api.openweathermap.org/data/2.5/weather?lat=11.487&lon=122.9963&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 175 of 500 | daphne
    http://api.openweathermap.org/data/2.5/weather?lat=30.6033333&lon=-87.9036111&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 176 of 500 | aulnoye-aymeries
    http://api.openweathermap.org/data/2.5/weather?lat=50.201412&lon=3.838439&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 177 of 500 | vulcan
    http://api.openweathermap.org/data/2.5/weather?lat=50.4&lon=-113.25&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 178 of 500 | vietri sul mare
    http://api.openweathermap.org/data/2.5/weather?lat=40.666667&lon=14.733333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 179 of 500 | paharpur
    http://api.openweathermap.org/data/2.5/weather?lat=32.103844&lon=70.97235&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 180 of 500 | maraa
    http://api.openweathermap.org/data/2.5/weather?lat=-1.833333&lon=-65.366667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 181 of 500 | blackfoot
    http://api.openweathermap.org/data/2.5/weather?lat=43.1905556&lon=-112.3441667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 182 of 500 | manzalesti
    http://api.openweathermap.org/data/2.5/weather?lat=45.5&lon=26.65&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 183 of 500 | shevchenkove
    http://api.openweathermap.org/data/2.5/weather?lat=49.695853&lon=37.173484&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 184 of 500 | san andres itzapa
    http://api.openweathermap.org/data/2.5/weather?lat=14.62&lon=-90.844167&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 185 of 500 | moldoveni
    http://api.openweathermap.org/data/2.5/weather?lat=46.833333&lon=26.766667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 186 of 500 | san roque
    http://api.openweathermap.org/data/2.5/weather?lat=10.6043&lon=123.7441&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 187 of 500 | san miguelito
    http://api.openweathermap.org/data/2.5/weather?lat=11.4&lon=-84.9&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 188 of 500 | ischia
    http://api.openweathermap.org/data/2.5/weather?lat=40.733333&lon=13.95&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 189 of 500 | anaimalai
    http://api.openweathermap.org/data/2.5/weather?lat=10.583333&lon=76.933333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 190 of 500 | calarasi
    http://api.openweathermap.org/data/2.5/weather?lat=47.616667&lon=27.266667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 191 of 500 | gardendale
    http://api.openweathermap.org/data/2.5/weather?lat=33.66&lon=-86.8127778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 192 of 500 | san miguel acatan
    http://api.openweathermap.org/data/2.5/weather?lat=15.7&lon=-91.616667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 193 of 500 | farum
    http://api.openweathermap.org/data/2.5/weather?lat=55.809873&lon=12.362286&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 194 of 500 | olari
    http://api.openweathermap.org/data/2.5/weather?lat=46.383333&lon=21.55&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 195 of 500 | nuseni
    http://api.openweathermap.org/data/2.5/weather?lat=47.1&lon=24.2&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 196 of 500 | cavaillon
    http://api.openweathermap.org/data/2.5/weather?lat=43.836033&lon=5.040492&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 197 of 500 | bentiu
    http://api.openweathermap.org/data/2.5/weather?lat=9.2333333&lon=29.8333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 198 of 500 | zhovtneve
    http://api.openweathermap.org/data/2.5/weather?lat=49.987839&lon=29.531292&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 199 of 500 | berehove
    http://api.openweathermap.org/data/2.5/weather?lat=44.901477&lon=33.621587&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 200 of 500 | chieri
    http://api.openweathermap.org/data/2.5/weather?lat=45.016667&lon=7.816667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 201 of 500 | reitoca
    http://api.openweathermap.org/data/2.5/weather?lat=13.8258333&lon=-87.4652778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 202 of 500 | tarbagatay
    http://api.openweathermap.org/data/2.5/weather?lat=51.1747&lon=109.0931&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 203 of 500 | valparaiso
    http://api.openweathermap.org/data/2.5/weather?lat=41.4730556&lon=-87.0611111&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 204 of 500 | sinteu
    http://api.openweathermap.org/data/2.5/weather?lat=47.15&lon=22.483333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 205 of 500 | montalto uffugo
    http://api.openweathermap.org/data/2.5/weather?lat=39.4&lon=16.15&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 206 of 500 | lianran
    http://api.openweathermap.org/data/2.5/weather?lat=24.922707&lon=102.484959&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 207 of 500 | rehburg-loccum
    http://api.openweathermap.org/data/2.5/weather?lat=52.466667&lon=9.2&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 208 of 500 | kristallopiyi
    http://api.openweathermap.org/data/2.5/weather?lat=40.6380556&lon=21.0852778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 209 of 500 | mascote
    http://api.openweathermap.org/data/2.5/weather?lat=-15.55&lon=-39.283333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 210 of 500 | pocos de caldas
    http://api.openweathermap.org/data/2.5/weather?lat=-21.8&lon=-46.566667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 211 of 500 | jacqueville
    http://api.openweathermap.org/data/2.5/weather?lat=5.205148&lon=-4.414604&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 212 of 500 | taldom
    http://api.openweathermap.org/data/2.5/weather?lat=56.733333&lon=37.533333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 213 of 500 | coslada
    http://api.openweathermap.org/data/2.5/weather?lat=40.423776&lon=-3.561287&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 214 of 500 | voljevac
    http://api.openweathermap.org/data/2.5/weather?lat=43.8786111&lon=17.6591667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 215 of 500 | troy
    http://api.openweathermap.org/data/2.5/weather?lat=42.7283333&lon=-73.6922222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 216 of 500 | hojai
    http://api.openweathermap.org/data/2.5/weather?lat=26.0&lon=92.866667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 217 of 500 | dusi
    http://api.openweathermap.org/data/2.5/weather?lat=12.766667&lon=79.683333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 218 of 500 | brownsburg
    http://api.openweathermap.org/data/2.5/weather?lat=39.8433333&lon=-86.3977778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 219 of 500 | lunca de sus
    http://api.openweathermap.org/data/2.5/weather?lat=46.533333&lon=25.966667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 220 of 500 | narnaund
    http://api.openweathermap.org/data/2.5/weather?lat=29.216667&lon=76.15&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 221 of 500 | santa catalina
    http://api.openweathermap.org/data/2.5/weather?lat=10.603607&lon=-75.288235&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 222 of 500 | khujner
    http://api.openweathermap.org/data/2.5/weather?lat=23.783333&lon=76.6&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 223 of 500 | xiaolan
    http://api.openweathermap.org/data/2.5/weather?lat=22.673811&lon=113.231874&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 224 of 500 | dumabato
    http://api.openweathermap.org/data/2.5/weather?lat=16.316667&lon=121.7&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 225 of 500 | dawukou
    http://api.openweathermap.org/data/2.5/weather?lat=39.041944&lon=106.395833&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 226 of 500 | salto
    http://api.openweathermap.org/data/2.5/weather?lat=-31.3833333&lon=-57.9666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 227 of 500 | nicoya
    http://api.openweathermap.org/data/2.5/weather?lat=10.148275&lon=-85.452013&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 228 of 500 | willmar
    http://api.openweathermap.org/data/2.5/weather?lat=45.1219444&lon=-95.0430556&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 229 of 500 | jaumave
    http://api.openweathermap.org/data/2.5/weather?lat=23.416667&lon=-99.383333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 230 of 500 | balesti
    http://api.openweathermap.org/data/2.5/weather?lat=45.433333&lon=27.233333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 231 of 500 | cupseni
    http://api.openweathermap.org/data/2.5/weather?lat=47.55&lon=23.933333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 232 of 500 | del carmen
    http://api.openweathermap.org/data/2.5/weather?lat=15.0&lon=120.533333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 233 of 500 | putlod
    http://api.openweathermap.org/data/2.5/weather?lat=15.3706&lon=120.8675&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 234 of 500 | liancheng
    http://api.openweathermap.org/data/2.5/weather?lat=21.646734&lon=110.28172&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 235 of 500 | conselheiro lafaiete
    http://api.openweathermap.org/data/2.5/weather?lat=-20.666667&lon=-43.8&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 236 of 500 | mikulcice
    http://api.openweathermap.org/data/2.5/weather?lat=48.817654&lon=17.05&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 237 of 500 | sebastian
    http://api.openweathermap.org/data/2.5/weather?lat=27.8161111&lon=-80.4708333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 238 of 500 | bolingbrook
    http://api.openweathermap.org/data/2.5/weather?lat=41.6986111&lon=-88.0683333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 239 of 500 | kunovice
    http://api.openweathermap.org/data/2.5/weather?lat=49.040529&lon=17.466141&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 240 of 500 | tarcea
    http://api.openweathermap.org/data/2.5/weather?lat=47.45&lon=22.183333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 241 of 500 | chynadiyovo
    http://api.openweathermap.org/data/2.5/weather?lat=48.481788&lon=22.821701&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 242 of 500 | rantepao
    http://api.openweathermap.org/data/2.5/weather?lat=-2.9701&lon=119.8978&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 243 of 500 | roeselare
    http://api.openweathermap.org/data/2.5/weather?lat=50.95&lon=3.133333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 244 of 500 | wolfeboro
    http://api.openweathermap.org/data/2.5/weather?lat=43.5838889&lon=-71.2077778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 245 of 500 | lyubashivka
    http://api.openweathermap.org/data/2.5/weather?lat=47.837157&lon=30.259763&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 246 of 500 | lingyuan
    http://api.openweathermap.org/data/2.5/weather?lat=41.24&lon=119.401111&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 247 of 500 | iwakuni
    http://api.openweathermap.org/data/2.5/weather?lat=34.15&lon=132.183333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 248 of 500 | nyzhnya krynka
    http://api.openweathermap.org/data/2.5/weather?lat=48.113497&lon=38.160636&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 249 of 500 | belyy gorodok
    http://api.openweathermap.org/data/2.5/weather?lat=56.965&lon=37.5125&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 250 of 500 | ulles
    http://api.openweathermap.org/data/2.5/weather?lat=46.336114&lon=19.844544&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 251 of 500 | san roque
    http://api.openweathermap.org/data/2.5/weather?lat=15.261463&lon=120.651831&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 252 of 500 | fuyang
    http://api.openweathermap.org/data/2.5/weather?lat=32.9&lon=115.816667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 253 of 500 | sepatan
    http://api.openweathermap.org/data/2.5/weather?lat=-6.118889&lon=106.575&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 254 of 500 | lifford
    http://api.openweathermap.org/data/2.5/weather?lat=54.8319444&lon=-7.4836111&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 255 of 500 | dallas
    http://api.openweathermap.org/data/2.5/weather?lat=33.9236111&lon=-84.8408333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 256 of 500 | devon
    http://api.openweathermap.org/data/2.5/weather?lat=53.366667&lon=-113.733333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 257 of 500 | naral
    http://api.openweathermap.org/data/2.5/weather?lat=23.1666667&lon=89.5&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 258 of 500 | fremont
    http://api.openweathermap.org/data/2.5/weather?lat=41.4333333&lon=-96.4977778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 259 of 500 | movila miresei
    http://api.openweathermap.org/data/2.5/weather?lat=45.216667&lon=27.6&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 260 of 500 | usti
    http://api.openweathermap.org/data/2.5/weather?lat=50.666667&lon=14.033333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 261 of 500 | remigio
    http://api.openweathermap.org/data/2.5/weather?lat=-6.933333&lon=-35.783333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 262 of 500 | billund
    http://api.openweathermap.org/data/2.5/weather?lat=55.731157&lon=9.113309&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 263 of 500 | giengen
    http://api.openweathermap.org/data/2.5/weather?lat=48.616667&lon=10.25&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 264 of 500 | dambulla
    http://api.openweathermap.org/data/2.5/weather?lat=7.86&lon=80.6516667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 265 of 500 | lasalgaon
    http://api.openweathermap.org/data/2.5/weather?lat=20.15&lon=74.233333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 266 of 500 | ibtin
    http://api.openweathermap.org/data/2.5/weather?lat=32.761667&lon=35.100833&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 267 of 500 | visp
    http://api.openweathermap.org/data/2.5/weather?lat=46.294524&lon=7.884736&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 268 of 500 | luckenwalde
    http://api.openweathermap.org/data/2.5/weather?lat=52.083333&lon=13.166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 269 of 500 | nicolet
    http://api.openweathermap.org/data/2.5/weather?lat=46.216667&lon=-72.6&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 270 of 500 | troyes
    http://api.openweathermap.org/data/2.5/weather?lat=48.300727&lon=4.085244&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 271 of 500 | obuasi
    http://api.openweathermap.org/data/2.5/weather?lat=6.2&lon=-1.6666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 272 of 500 | ixtahuacan
    http://api.openweathermap.org/data/2.5/weather?lat=15.416667&lon=-91.766667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 273 of 500 | fantan
    http://api.openweathermap.org/data/2.5/weather?lat=40.3941667&lon=44.685&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 274 of 500 | sriperumbudur
    http://api.openweathermap.org/data/2.5/weather?lat=12.968889&lon=79.948889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 275 of 500 | sonneberg
    http://api.openweathermap.org/data/2.5/weather?lat=50.35&lon=11.166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 276 of 500 | vilia
    http://api.openweathermap.org/data/2.5/weather?lat=38.1666667&lon=23.3333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 277 of 500 | wampusirpi
    http://api.openweathermap.org/data/2.5/weather?lat=15.1833333&lon=-84.6166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 278 of 500 | daxian
    http://api.openweathermap.org/data/2.5/weather?lat=31.215921&lon=107.500922&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 279 of 500 | znojmo
    http://api.openweathermap.org/data/2.5/weather?lat=48.856116&lon=16.056412&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 280 of 500 | sindi
    http://api.openweathermap.org/data/2.5/weather?lat=20.8&lon=78.866667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 281 of 500 | rusca montana
    http://api.openweathermap.org/data/2.5/weather?lat=45.566667&lon=22.45&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 282 of 500 | apaseo el grande
    http://api.openweathermap.org/data/2.5/weather?lat=20.566667&lon=-100.7&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 283 of 500 | hervey bay
    http://api.openweathermap.org/data/2.5/weather?lat=-25.285&lon=152.873&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 284 of 500 | naftah
    http://api.openweathermap.org/data/2.5/weather?lat=33.873094&lon=7.877652&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 285 of 500 | san joaquin
    http://api.openweathermap.org/data/2.5/weather?lat=-24.95&lon=-56.116667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 286 of 500 | chapulhuacan
    http://api.openweathermap.org/data/2.5/weather?lat=21.166667&lon=-98.9&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 287 of 500 | plaridel
    http://api.openweathermap.org/data/2.5/weather?lat=15.65&lon=121.0&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 288 of 500 | san juan de arama
    http://api.openweathermap.org/data/2.5/weather?lat=3.346389&lon=-73.889444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 289 of 500 | santa ana
    http://api.openweathermap.org/data/2.5/weather?lat=9.192778&lon=125.564444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 290 of 500 | cascais
    http://api.openweathermap.org/data/2.5/weather?lat=38.69745&lon=-9.423141&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 291 of 500 | minot
    http://api.openweathermap.org/data/2.5/weather?lat=48.2325&lon=-101.2958333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 292 of 500 | port washington
    http://api.openweathermap.org/data/2.5/weather?lat=40.8255556&lon=-73.6986111&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 293 of 500 | waltershausen
    http://api.openweathermap.org/data/2.5/weather?lat=50.9&lon=10.566667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 294 of 500 | mazabuka
    http://api.openweathermap.org/data/2.5/weather?lat=-15.8666667&lon=27.7666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 295 of 500 | dagatan
    http://api.openweathermap.org/data/2.5/weather?lat=13.7389&lon=121.199&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 296 of 500 | sebaco
    http://api.openweathermap.org/data/2.5/weather?lat=12.8511111&lon=-86.0994444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 297 of 500 | kadubivtsi
    http://api.openweathermap.org/data/2.5/weather?lat=48.583372&lon=25.76871&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 298 of 500 | alausi
    http://api.openweathermap.org/data/2.5/weather?lat=-2.2&lon=-78.8333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 299 of 500 | canora
    http://api.openweathermap.org/data/2.5/weather?lat=51.633333&lon=-102.433333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 300 of 500 | kaniama
    http://api.openweathermap.org/data/2.5/weather?lat=-7.516667&lon=24.183333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 301 of 500 | nazira
    http://api.openweathermap.org/data/2.5/weather?lat=26.916667&lon=94.733333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 302 of 500 | sonegaon
    http://api.openweathermap.org/data/2.5/weather?lat=20.65&lon=78.683333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 303 of 500 | poya
    http://api.openweathermap.org/data/2.5/weather?lat=-21.35&lon=165.15&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 304 of 500 | tangua
    http://api.openweathermap.org/data/2.5/weather?lat=1.094727&lon=-77.394821&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 305 of 500 | asopos
    http://api.openweathermap.org/data/2.5/weather?lat=36.7333333&lon=22.8666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 306 of 500 | livezi
    http://api.openweathermap.org/data/2.5/weather?lat=46.4&lon=26.733333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 307 of 500 | sundargarh
    http://api.openweathermap.org/data/2.5/weather?lat=22.116667&lon=84.033333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 308 of 500 | voznesenskoye
    http://api.openweathermap.org/data/2.5/weather?lat=54.89&lon=42.756944&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 309 of 500 | santa eulalia
    http://api.openweathermap.org/data/2.5/weather?lat=-11.85&lon=-76.6833333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 310 of 500 | nemcice nad hanou
    http://api.openweathermap.org/data/2.5/weather?lat=49.342743&lon=17.209161&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 311 of 500 | buenavista
    http://api.openweathermap.org/data/2.5/weather?lat=12.176389&lon=123.790556&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 312 of 500 | tugaya
    http://api.openweathermap.org/data/2.5/weather?lat=7.882773&lon=124.174003&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 313 of 500 | romanovka
    http://api.openweathermap.org/data/2.5/weather?lat=60.044301&lon=30.713997&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 314 of 500 | oak ridge
    http://api.openweathermap.org/data/2.5/weather?lat=28.4708333&lon=-81.4247222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 315 of 500 | huelva
    http://api.openweathermap.org/data/2.5/weather?lat=37.271447&lon=-6.949464&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 316 of 500 | seini
    http://api.openweathermap.org/data/2.5/weather?lat=47.75&lon=23.283333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 317 of 500 | domodossola
    http://api.openweathermap.org/data/2.5/weather?lat=46.116667&lon=8.283333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 318 of 500 | sansai
    http://api.openweathermap.org/data/2.5/weather?lat=18.83075&lon=99.003806&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 319 of 500 | magoula
    http://api.openweathermap.org/data/2.5/weather?lat=37.0833333&lon=22.4166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 320 of 500 | girardot
    http://api.openweathermap.org/data/2.5/weather?lat=4.298659&lon=-74.804681&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 321 of 500 | boma
    http://api.openweathermap.org/data/2.5/weather?lat=-5.85&lon=13.05&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 322 of 500 | banbury
    http://api.openweathermap.org/data/2.5/weather?lat=52.05&lon=-1.333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 323 of 500 | akhtyrskiy
    http://api.openweathermap.org/data/2.5/weather?lat=44.8546&lon=38.3031&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 324 of 500 | levashovo
    http://api.openweathermap.org/data/2.5/weather?lat=60.103688&lon=30.20683&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 325 of 500 | dourbali
    http://api.openweathermap.org/data/2.5/weather?lat=11.8094444&lon=15.8611111&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 326 of 500 | zeliezovce
    http://api.openweathermap.org/data/2.5/weather?lat=48.05&lon=18.6666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 327 of 500 | santa cruz
    http://api.openweathermap.org/data/2.5/weather?lat=13.2580556&lon=-87.3483333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 328 of 500 | durango
    http://api.openweathermap.org/data/2.5/weather?lat=43.171235&lon=-2.633795&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 329 of 500 | umm lajj
    http://api.openweathermap.org/data/2.5/weather?lat=25.021264&lon=37.268502&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 330 of 500 | campo alegre
    http://api.openweathermap.org/data/2.5/weather?lat=-9.8&lon=-36.35&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 331 of 500 | francesti
    http://api.openweathermap.org/data/2.5/weather?lat=45.0&lon=24.1&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 332 of 500 | det udom
    http://api.openweathermap.org/data/2.5/weather?lat=14.905978&lon=105.078364&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 333 of 500 | prichard
    http://api.openweathermap.org/data/2.5/weather?lat=30.7386111&lon=-88.0788889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 334 of 500 | areia branca
    http://api.openweathermap.org/data/2.5/weather?lat=-10.766667&lon=-37.316667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 335 of 500 | bokoro
    http://api.openweathermap.org/data/2.5/weather?lat=12.3766667&lon=17.0580556&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 336 of 500 | lonoy
    http://api.openweathermap.org/data/2.5/weather?lat=11.513611&lon=122.729722&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 337 of 500 | fantanele
    http://api.openweathermap.org/data/2.5/weather?lat=46.416667&lon=24.75&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 338 of 500 | chulym
    http://api.openweathermap.org/data/2.5/weather?lat=55.099722&lon=80.957222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 339 of 500 | carranglan
    http://api.openweathermap.org/data/2.5/weather?lat=15.9613&lon=121.0641&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 340 of 500 | karsun
    http://api.openweathermap.org/data/2.5/weather?lat=54.199409&lon=46.982697&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 341 of 500 | calaba
    http://api.openweathermap.org/data/2.5/weather?lat=15.2953&lon=120.8701&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 342 of 500 | waldkirchen
    http://api.openweathermap.org/data/2.5/weather?lat=48.733333&lon=13.6&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 343 of 500 | cabras
    http://api.openweathermap.org/data/2.5/weather?lat=39.93&lon=8.531389&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 344 of 500 | kostinbrod
    http://api.openweathermap.org/data/2.5/weather?lat=42.8166667&lon=23.2166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 345 of 500 | elati
    http://api.openweathermap.org/data/2.5/weather?lat=39.5038889&lon=21.5347222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 346 of 500 | yeadon
    http://api.openweathermap.org/data/2.5/weather?lat=39.9388889&lon=-75.2558333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 347 of 500 | kigoma
    http://api.openweathermap.org/data/2.5/weather?lat=-4.8769444&lon=29.6266667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 348 of 500 | susanville
    http://api.openweathermap.org/data/2.5/weather?lat=40.4163889&lon=-120.6519444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 349 of 500 | priiskovyy
    http://api.openweathermap.org/data/2.5/weather?lat=54.656111&lon=88.687222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 350 of 500 | insuratei
    http://api.openweathermap.org/data/2.5/weather?lat=44.916667&lon=27.6&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 351 of 500 | batangas
    http://api.openweathermap.org/data/2.5/weather?lat=13.7567&lon=121.0584&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 352 of 500 | cambe
    http://api.openweathermap.org/data/2.5/weather?lat=-23.233333&lon=-51.25&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 353 of 500 | santa teresa
    http://api.openweathermap.org/data/2.5/weather?lat=11.7333333&lon=-86.2166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 354 of 500 | santa maria de jesus
    http://api.openweathermap.org/data/2.5/weather?lat=14.518333&lon=-90.726667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 355 of 500 | puerto guzman
    http://api.openweathermap.org/data/2.5/weather?lat=0.964538&lon=-76.407953&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 356 of 500 | woburn
    http://api.openweathermap.org/data/2.5/weather?lat=42.4791667&lon=-71.1527778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 357 of 500 | spoleto
    http://api.openweathermap.org/data/2.5/weather?lat=42.733333&lon=12.733333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 358 of 500 | thongwa
    http://api.openweathermap.org/data/2.5/weather?lat=16.7619444&lon=96.5277778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 359 of 500 | augustow
    http://api.openweathermap.org/data/2.5/weather?lat=53.844711&lon=22.980133&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 360 of 500 | phuntsholing
    http://api.openweathermap.org/data/2.5/weather?lat=26.85&lon=89.3833333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 361 of 500 | tankhoy
    http://api.openweathermap.org/data/2.5/weather?lat=51.552778&lon=105.130556&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 362 of 500 | staraya
    http://api.openweathermap.org/data/2.5/weather?lat=59.927488&lon=30.627645&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 363 of 500 | togul
    http://api.openweathermap.org/data/2.5/weather?lat=53.465&lon=85.9128&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 364 of 500 | auray
    http://api.openweathermap.org/data/2.5/weather?lat=47.670252&lon=-2.991828&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 365 of 500 | changuinola
    http://api.openweathermap.org/data/2.5/weather?lat=9.4333333&lon=-82.5166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 366 of 500 | zakharo
    http://api.openweathermap.org/data/2.5/weather?lat=37.4833333&lon=21.65&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 367 of 500 | cicero dantas
    http://api.openweathermap.org/data/2.5/weather?lat=-10.6&lon=-38.383333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 368 of 500 | bucecea
    http://api.openweathermap.org/data/2.5/weather?lat=47.766667&lon=26.433333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 369 of 500 | faleula
    http://api.openweathermap.org/data/2.5/weather?lat=-13.8&lon=-171.8&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 370 of 500 | huallanca
    http://api.openweathermap.org/data/2.5/weather?lat=-9.85&lon=-76.9333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 371 of 500 | yichang
    http://api.openweathermap.org/data/2.5/weather?lat=30.77135&lon=111.321458&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 372 of 500 | arenys de mar
    http://api.openweathermap.org/data/2.5/weather?lat=41.581905&lon=2.549358&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 373 of 500 | boadilla del monte
    http://api.openweathermap.org/data/2.5/weather?lat=40.402784&lon=-3.875623&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 374 of 500 | hemel hempstead
    http://api.openweathermap.org/data/2.5/weather?lat=51.75&lon=-0.466667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 375 of 500 | domzale
    http://api.openweathermap.org/data/2.5/weather?lat=46.1380556&lon=14.5977778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 376 of 500 | bordeaux
    http://api.openweathermap.org/data/2.5/weather?lat=44.840439&lon=-0.5805&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 377 of 500 | belen
    http://api.openweathermap.org/data/2.5/weather?lat=-23.466111&lon=-57.261944&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 378 of 500 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?lat=58.816667&lon=-122.533333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 379 of 500 | bytosh
    http://api.openweathermap.org/data/2.5/weather?lat=53.901389&lon=34.123889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 380 of 500 | garissa
    http://api.openweathermap.org/data/2.5/weather?lat=-0.4569444&lon=39.6583333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 381 of 500 | ceyhan
    http://api.openweathermap.org/data/2.5/weather?lat=37.024722&lon=35.8175&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 382 of 500 | mgachi
    http://api.openweathermap.org/data/2.5/weather?lat=51.05&lon=142.266667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 383 of 500 | cernusco sul naviglio
    http://api.openweathermap.org/data/2.5/weather?lat=45.516667&lon=9.316667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 384 of 500 | cornella
    http://api.openweathermap.org/data/2.5/weather?lat=41.352784&lon=2.070769&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 385 of 500 | quirinopolis
    http://api.openweathermap.org/data/2.5/weather?lat=-18.533333&lon=-50.5&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 386 of 500 | itapicuru
    http://api.openweathermap.org/data/2.5/weather?lat=-11.316667&lon=-38.25&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 387 of 500 | buza
    http://api.openweathermap.org/data/2.5/weather?lat=46.9&lon=24.15&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 388 of 500 | one hundred mile house
    http://api.openweathermap.org/data/2.5/weather?lat=51.65&lon=-121.283333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 389 of 500 | ryazanskaya
    http://api.openweathermap.org/data/2.5/weather?lat=44.955279&lon=39.588938&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 390 of 500 | korneuburg
    http://api.openweathermap.org/data/2.5/weather?lat=48.35&lon=16.333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 391 of 500 | kambileyevskoye
    http://api.openweathermap.org/data/2.5/weather?lat=43.078578&lon=44.752165&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 392 of 500 | navoloki
    http://api.openweathermap.org/data/2.5/weather?lat=57.465719&lon=41.963443&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 393 of 500 | passa quatro
    http://api.openweathermap.org/data/2.5/weather?lat=-22.383333&lon=-44.966667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 394 of 500 | rheinfelden
    http://api.openweathermap.org/data/2.5/weather?lat=47.566667&lon=7.8&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 395 of 500 | tyarlevo
    http://api.openweathermap.org/data/2.5/weather?lat=59.700556&lon=30.456389&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 396 of 500 | ragusa
    http://api.openweathermap.org/data/2.5/weather?lat=36.916667&lon=14.733333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 397 of 500 | cleveland
    http://api.openweathermap.org/data/2.5/weather?lat=33.7438889&lon=-90.7247222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 398 of 500 | bethausen
    http://api.openweathermap.org/data/2.5/weather?lat=45.833056&lon=21.952778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 399 of 500 | bokaa
    http://api.openweathermap.org/data/2.5/weather?lat=-24.45&lon=26.0166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 400 of 500 | ladispoli
    http://api.openweathermap.org/data/2.5/weather?lat=41.933333&lon=12.083333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 401 of 500 | ciocile
    http://api.openweathermap.org/data/2.5/weather?lat=44.816667&lon=27.233333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 402 of 500 | brancovenesti
    http://api.openweathermap.org/data/2.5/weather?lat=46.866667&lon=24.766667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 403 of 500 | nea malgara
    http://api.openweathermap.org/data/2.5/weather?lat=40.6094444&lon=22.6816667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 404 of 500 | el escano de tepale
    http://api.openweathermap.org/data/2.5/weather?lat=14.75&lon=-87.0666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 405 of 500 | point pedro
    http://api.openweathermap.org/data/2.5/weather?lat=9.8166667&lon=80.2333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 406 of 500 | healesville
    http://api.openweathermap.org/data/2.5/weather?lat=-37.65395&lon=145.517181&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 407 of 500 | gigaquit
    http://api.openweathermap.org/data/2.5/weather?lat=9.5951&lon=125.6968&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 408 of 500 | canatlan
    http://api.openweathermap.org/data/2.5/weather?lat=24.516667&lon=-104.783333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 409 of 500 | holt
    http://api.openweathermap.org/data/2.5/weather?lat=42.6405556&lon=-84.5152778&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 410 of 500 | kangasala
    http://api.openweathermap.org/data/2.5/weather?lat=61.466667&lon=24.083333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 411 of 500 | lewisville
    http://api.openweathermap.org/data/2.5/weather?lat=33.0461111&lon=-96.9938889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 412 of 500 | floresta
    http://api.openweathermap.org/data/2.5/weather?lat=5.859027&lon=-72.918818&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 413 of 500 | baie-comeau
    http://api.openweathermap.org/data/2.5/weather?lat=49.216667&lon=-68.15&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 414 of 500 | sao domingos do prata
    http://api.openweathermap.org/data/2.5/weather?lat=-19.866667&lon=-42.966667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 415 of 500 | bicaz chei
    http://api.openweathermap.org/data/2.5/weather?lat=46.816667&lon=25.883333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 416 of 500 | godella
    http://api.openweathermap.org/data/2.5/weather?lat=39.533333&lon=-0.416667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 417 of 500 | la ermita
    http://api.openweathermap.org/data/2.5/weather?lat=14.4166667&lon=-87.1&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 418 of 500 | pulau sebang
    http://api.openweathermap.org/data/2.5/weather?lat=2.455&lon=102.2329&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 419 of 500 | radomiresti
    http://api.openweathermap.org/data/2.5/weather?lat=44.116667&lon=24.683333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 420 of 500 | namtsy
    http://api.openweathermap.org/data/2.5/weather?lat=62.716111&lon=129.665833&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 421 of 500 | dersca
    http://api.openweathermap.org/data/2.5/weather?lat=47.983333&lon=26.2&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 422 of 500 | skotterud
    http://api.openweathermap.org/data/2.5/weather?lat=59.983333&lon=12.116667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 423 of 500 | centerville
    http://api.openweathermap.org/data/2.5/weather?lat=40.9180556&lon=-111.8713889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 424 of 500 | matipo
    http://api.openweathermap.org/data/2.5/weather?lat=-20.283333&lon=-42.35&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 425 of 500 | homer
    http://api.openweathermap.org/data/2.5/weather?lat=59.6425&lon=-151.5483333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 426 of 500 | nagsabaran
    http://api.openweathermap.org/data/2.5/weather?lat=16.666667&lon=120.333333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 427 of 500 | haifa
    http://api.openweathermap.org/data/2.5/weather?lat=32.815556&lon=34.989167&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 428 of 500 | crucea
    http://api.openweathermap.org/data/2.5/weather?lat=47.35&lon=25.616667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 429 of 500 | ramana
    http://api.openweathermap.org/data/2.5/weather?lat=40.442222&lon=49.980556&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 430 of 500 | terme
    http://api.openweathermap.org/data/2.5/weather?lat=41.209167&lon=36.973889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 431 of 500 | zafra
    http://api.openweathermap.org/data/2.5/weather?lat=38.416667&lon=-6.416667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 432 of 500 | cotija
    http://api.openweathermap.org/data/2.5/weather?lat=19.816667&lon=-102.683333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 433 of 500 | dumbraveni
    http://api.openweathermap.org/data/2.5/weather?lat=45.533333&lon=27.116667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 434 of 500 | kenora
    http://api.openweathermap.org/data/2.5/weather?lat=49.766667&lon=-94.466667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 435 of 500 | soluno-dmitriyevskoye
    http://api.openweathermap.org/data/2.5/weather?lat=44.4074&lon=42.724&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 436 of 500 | karmaskaly
    http://api.openweathermap.org/data/2.5/weather?lat=54.3709&lon=56.1837&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 437 of 500 | westminster
    http://api.openweathermap.org/data/2.5/weather?lat=33.7591667&lon=-118.0058333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 438 of 500 | nagyszenas
    http://api.openweathermap.org/data/2.5/weather?lat=46.683333&lon=20.666667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 439 of 500 | vyksa
    http://api.openweathermap.org/data/2.5/weather?lat=55.3175&lon=42.174444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 440 of 500 | prochnookopskaya
    http://api.openweathermap.org/data/2.5/weather?lat=45.0666&lon=41.1175&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 441 of 500 | ayase
    http://api.openweathermap.org/data/2.5/weather?lat=35.386462&lon=139.417373&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 442 of 500 | paltin
    http://api.openweathermap.org/data/2.5/weather?lat=45.783333&lon=26.716667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 443 of 500 | viking
    http://api.openweathermap.org/data/2.5/weather?lat=53.083333&lon=-111.783333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 444 of 500 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?lat=71.433333&lon=136.066667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 445 of 500 | managpi
    http://api.openweathermap.org/data/2.5/weather?lat=13.3108&lon=121.204&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 446 of 500 | qui nhon
    http://api.openweathermap.org/data/2.5/weather?lat=13.766667&lon=109.233333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 447 of 500 | bhopal
    http://api.openweathermap.org/data/2.5/weather?lat=23.266667&lon=77.4&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 448 of 500 | schouweiler
    http://api.openweathermap.org/data/2.5/weather?lat=49.5825&lon=5.9563889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 449 of 500 | perama
    http://api.openweathermap.org/data/2.5/weather?lat=35.3697222&lon=24.6972222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 450 of 500 | pinagsibaan
    http://api.openweathermap.org/data/2.5/weather?lat=13.838056&lon=121.318889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 451 of 500 | psyzh
    http://api.openweathermap.org/data/2.5/weather?lat=44.233056&lon=42.018333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 452 of 500 | san jose
    http://api.openweathermap.org/data/2.5/weather?lat=12.5296&lon=124.4875&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 453 of 500 | nadlac
    http://api.openweathermap.org/data/2.5/weather?lat=46.166667&lon=20.75&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 454 of 500 | cocora
    http://api.openweathermap.org/data/2.5/weather?lat=44.733333&lon=27.05&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 455 of 500 | ryabovo
    http://api.openweathermap.org/data/2.5/weather?lat=59.4&lon=31.133333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 456 of 500 | inisa
    http://api.openweathermap.org/data/2.5/weather?lat=7.8&lon=4.283333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 457 of 500 | mukhtolovo
    http://api.openweathermap.org/data/2.5/weather?lat=55.467507&lon=43.199734&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 458 of 500 | el chichicaste
    http://api.openweathermap.org/data/2.5/weather?lat=14.0666667&lon=-86.3&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 459 of 500 | santa cruz
    http://api.openweathermap.org/data/2.5/weather?lat=8.603611&lon=124.911944&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 460 of 500 | zarasai
    http://api.openweathermap.org/data/2.5/weather?lat=55.7333333&lon=26.25&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 461 of 500 | pangpang
    http://api.openweathermap.org/data/2.5/weather?lat=15.9395&lon=120.3099&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 462 of 500 | fort walton beach
    http://api.openweathermap.org/data/2.5/weather?lat=30.4055556&lon=-86.6188889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 463 of 500 | kostakioi
    http://api.openweathermap.org/data/2.5/weather?lat=39.1375&lon=20.9588889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 464 of 500 | grodzisk mazowiecki
    http://api.openweathermap.org/data/2.5/weather?lat=52.106384&lon=20.623141&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 465 of 500 | kanadey
    http://api.openweathermap.org/data/2.5/weather?lat=53.167858&lon=47.526436&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 466 of 500 | tatawin
    http://api.openweathermap.org/data/2.5/weather?lat=32.929674&lon=10.451767&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 467 of 500 | nabari
    http://api.openweathermap.org/data/2.5/weather?lat=34.616667&lon=136.083333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 468 of 500 | biysk
    http://api.openweathermap.org/data/2.5/weather?lat=52.536389&lon=85.207222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 469 of 500 | merida
    http://api.openweathermap.org/data/2.5/weather?lat=20.966667&lon=-89.616667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 470 of 500 | villahermosa
    http://api.openweathermap.org/data/2.5/weather?lat=17.983333&lon=-92.916667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 471 of 500 | chepo
    http://api.openweathermap.org/data/2.5/weather?lat=9.1666667&lon=-79.1&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 472 of 500 | denderleeuw
    http://api.openweathermap.org/data/2.5/weather?lat=50.883333&lon=4.066667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 473 of 500 | hochstadt
    http://api.openweathermap.org/data/2.5/weather?lat=49.7&lon=10.8&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 474 of 500 | burayevo
    http://api.openweathermap.org/data/2.5/weather?lat=55.840693&lon=55.408336&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 475 of 500 | gegeny
    http://api.openweathermap.org/data/2.5/weather?lat=48.15&lon=21.95&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 476 of 500 | sur
    http://api.openweathermap.org/data/2.5/weather?lat=22.5666667&lon=59.5288889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 477 of 500 | pidsandawan
    http://api.openweathermap.org/data/2.5/weather?lat=6.932222&lon=124.590833&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 478 of 500 | bensonville
    http://api.openweathermap.org/data/2.5/weather?lat=6.4461111&lon=-10.6125&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 479 of 500 | kabanga
    http://api.openweathermap.org/data/2.5/weather?lat=-2.625&lon=30.4819444&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 480 of 500 | kurobe
    http://api.openweathermap.org/data/2.5/weather?lat=36.865783&lon=137.43636&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 481 of 500 | parita
    http://api.openweathermap.org/data/2.5/weather?lat=8.0&lon=-80.5166667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 482 of 500 | highland park
    http://api.openweathermap.org/data/2.5/weather?lat=40.4958333&lon=-74.4247222&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 483 of 500 | la fria
    http://api.openweathermap.org/data/2.5/weather?lat=8.2188889&lon=-72.2483333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 484 of 500 | chongoyape
    http://api.openweathermap.org/data/2.5/weather?lat=-6.6405556&lon=-79.3891667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 485 of 500 | polikastron
    http://api.openweathermap.org/data/2.5/weather?lat=40.9936111&lon=22.5658333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 486 of 500 | mogocha
    http://api.openweathermap.org/data/2.5/weather?lat=53.733333&lon=119.766667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 487 of 500 | san jose
    http://api.openweathermap.org/data/2.5/weather?lat=15.7923&lon=120.9894&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 488 of 500 | xewkija
    http://api.openweathermap.org/data/2.5/weather?lat=36.032778&lon=14.258056&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 489 of 500 | el higo
    http://api.openweathermap.org/data/2.5/weather?lat=21.766667&lon=-98.466667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 490 of 500 | kotar
    http://api.openweathermap.org/data/2.5/weather?lat=24.716667&lon=80.983333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 491 of 500 | bodaybo
    http://api.openweathermap.org/data/2.5/weather?lat=57.850556&lon=114.193333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 492 of 500 | marantao
    http://api.openweathermap.org/data/2.5/weather?lat=7.95&lon=124.233333&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 493 of 500 | leuna
    http://api.openweathermap.org/data/2.5/weather?lat=51.316667&lon=12.016667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 494 of 500 | sabana grande de boya
    http://api.openweathermap.org/data/2.5/weather?lat=18.95&lon=-69.8&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 495 of 500 | agotnes
    http://api.openweathermap.org/data/2.5/weather?lat=60.406111&lon=5.018889&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 496 of 500 | coron
    http://api.openweathermap.org/data/2.5/weather?lat=11.9986&lon=120.2043&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 497 of 500 | dabat
    http://api.openweathermap.org/data/2.5/weather?lat=12.984167&lon=37.765&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 498 of 500 | cape canaveral
    http://api.openweathermap.org/data/2.5/weather?lat=28.4055556&lon=-80.605&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 499 of 500 | schoneiche
    http://api.openweathermap.org/data/2.5/weather?lat=52.216667&lon=13.516667&APPID=518f982824caeb562219656130eaf212&units=imperial
    Processing Record 500 of 500 | west freehold
    http://api.openweathermap.org/data/2.5/weather?lat=40.2419444&lon=-74.3016667&APPID=518f982824caeb562219656130eaf212&units=imperial
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>City</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>it</td>
      <td>paullo</td>
      <td>45.416667</td>
      <td>9.400000</td>
      <td>51.8</td>
      <td>66</td>
      <td>0</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>at</td>
      <td>poysdorf</td>
      <td>48.666667</td>
      <td>16.633333</td>
      <td>50</td>
      <td>76</td>
      <td>0</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>in</td>
      <td>jhajjar</td>
      <td>28.616667</td>
      <td>76.650000</td>
      <td>71.6</td>
      <td>53</td>
      <td>0</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>de</td>
      <td>kamenz</td>
      <td>51.266667</td>
      <td>14.100000</td>
      <td>48.2</td>
      <td>100</td>
      <td>90</td>
      <td>4.7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>in</td>
      <td>malihabad</td>
      <td>26.916667</td>
      <td>80.716667</td>
      <td>71.6</td>
      <td>83</td>
      <td>0</td>
      <td>4.38</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_weather.to_csv("WeatherPy_csv_SL", sep='\t')
```


```python
#Temperature vs Latitude
# Set style of scatterplot
sns.set()

# Create scatterplot of dataframe
sns.lmplot('Latitude', 'Temperature (F)', data=city_weather, aspect = 1.5, fit_reg=False) 

# Set title and labels
plt.title('City Latitude vs. Max Temperature (10/23/17)')
plt.xlabel('Latitude')
plt.ylabel('Temperature (F)')

plt.ylim(-50,150)
plt.xlim(-60,100)

tempvslat = plt.gcf()
plt.savefig('tempvslat.png')
plt.show()
```


![png](output_7_0.png)



```python
#Humidity vs Latitude
# Set style of scatterplot
sns.set()

# Create scatterplot of dataframe
sns.lmplot("Latitude", "Humidity (%)", aspect = 1.5, data=city_weather, fit_reg=False) 

# Set title and labels
plt.title('City Latitude vs. Humidity (10/23/17)')
plt.xlabel('Latitude')
plt.ylabel('Humidity (%)')

plt.ylim(0,120)
plt.xlim(-60,100)

humivslat = plt.gcf()
plt.savefig('humidityvslat.png')
plt.show()
```


![png](output_8_0.png)



```python
#Cloudiness vs Latitude
# Set style of scatterplot
sns.set()

# Create scatterplot of dataframe
sns.lmplot("Latitude", "Cloudiness (%)", aspect = 1.5, data=city_weather, fit_reg=False) 

# Set title and labels
plt.title('City Latitude vs. Cloudiness (10/23/17)')
plt.xlabel('Latitude')
plt.ylabel('Cloudiness (%)')

plt.ylim(-20,120)
plt.xlim(-60,100)

cloudvslat = plt.gcf()
plt.savefig('cloudinessvslat.png')
plt.show()
```


![png](output_9_0.png)



```python
#Wind Speed vs Latitude
# Set style of scatterplot
sns.set()

# Create scatterplot of dataframe
sns.lmplot("Latitude", "Wind Speed (mph)", aspect = 1.5, data=city_weather, fit_reg=False) 

# Set title and labels
plt.title('City Latitude vs. Wind Speed (10/23/17)')
plt.xlabel('Latitude')
plt.ylabel('Wind Speed (mph)')

plt.ylim(-5,30)
plt.xlim(-60,100)

windvslat = plt.gcf()
plt.savefig('windvslat.png')
plt.show()
```


![png](output_10_0.png)


Three observable trends for month of October:
1. The higher the city's latitude, the lower the maximum temperature.
2. The higher the city's latitude, the more likely that the humidity will be higher.
3. The higher the city's latitude, the more likely that the wind speed will be higher.
