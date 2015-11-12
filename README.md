# Data Representation Project - 2015
**Robert Bryant**

##Introduction
The purpose of this project is to be a design for an API utilizing the National Roads Weather Station Dataset 
found at [Data.Gov.Ie](https://data.gov.ie/dataset/national-roads-weather-station-data) to provide realtime road conditions.

###About The Dataset
####National Roads Weather Station Data

The Dataset is provided as an XML document, and is updated every five minutes using data from 80+ weather stations across Ireland. 
These stations provide weather information in relation to road safety, information such as: air temperature, precipitation, wind speed & direction and Road Surface temperature.

The XML document is around 3300 lines long, with the header providing payload publication information, and each weather station's measurements listed as individual nodes. It is important to note that these nodes are nested alongside the header and publication information. A typical node in the dataset looks like this:

```
<siteMeasurements>
   <measurementSiteReference>NRA1688</measurementSiteReference>
   <measurementTimeDefault>2015-10-13T12:00:00+01:00</measurementTimeDefault>
   <measuredValue index="1">
    <basicDataValue xsi:type="TemperatureInformation">
     <period>3600</period>
     <temperature>
      <airTemperature>12</airTemperature>
      <dewPointTemperature>6.2</dewPointTemperature>
     </temperature>
    </basicDataValue>
   </measuredValue>
   <measuredValue index="2">
    <basicDataValue xsi:type="PrecipitationInformation">
     <period>3600</period>
     <precipitationDetail>
     </precipitationDetail>
    </basicDataValue>
   </measuredValue>
   <measuredValue index="3">
    <basicDataValue xsi:type="WindInformation">
     <period>3600</period>
     <wind>
      <maximumWindSpeed>2.3</maximumWindSpeed>
      <windDirectionBearing>306</windDirectionBearing>
      <windDirectionCompass>northWest</windDirectionCompass>
      <windSpeed>0.1</windSpeed>
     </wind>
    </basicDataValue>
   </measuredValue>
   <measuredValue index="4">
    <basicDataValue xsi:type="RoadSurfaceConditionInformation">
     <period>3600</period>
     <roadSurfaceConditionMeasurements>
      <protectionTemperature>0</protectionTemperature>
      <roadSurfaceTemperature>19.9</roadSurfaceTemperature>
     </roadSurfaceConditionMeasurements>
    </basicDataValue>
   </measuredValue>
  </siteMeasurements>

```
Each station node's fields can be broken down to the following relevant fields.

| Field     | Description |
--------------------------|------------------------------
| measurementSiteReference | The station's identifier |
| measuredValue | Index for each measurement category |
| airTemperature | Ambient air temperature recorded |
| dewPointTemperature | The temperature at which dew will form  |
| precipitationType | Type of precipitation recorded |
| maximumWindSpeed | Highest wind speed recorded for that period |
| windDirectionBearing | Wind direction bearing |
| windDirectionCompass | Compass based wind direction |
| windSpeed | Average wind speed |
| roadSurfaceTemperature | Road surface temperature |

Each measurement type is indexed, and each measurement has multiple values associated with it. For example; wind measurements include the maximum wind speed recorded, direction bearing, compass direction, and speed.

###Querying the API

Individual station measurements can be requested by station IDs, and are requested from:
> www.irelandweathernow.ie/station/

Each station's measurements are stored as JSON objects. These objects are identified by the station's ID (as supplied by the National Roads Authority). An example of one such object is:
```
   {
      "id":"NRA1681",
      "temp":{ "air":16, 
               "dewPoint": 6.5
            },
      "precip":"rain",
      "wind":{ "maximumWindSpeed":2.3,
               "bearing":306,
               "compass":"northWest",
               "speed":0.1
            },
      "road":{
               "protectionTemp":0,
               "roadSurfaceTemp":19.9
            }
   }
```

A response containing measurements from all stations is requested from:
> www.irelandweathernow.ie/station/all

The API will return an array of stations, each with their own last recorded measurements, in JSON format. This will allow a developer to easily import and use the collection using a standard JSON parse.

A truncated example of a response is:
```
[
   {
      "id":"NRA1681",
      "temp":{ "air":16, 
               "dewPoint": 6.5
            },
      "precip":"rain",
      "wind":{ "maximumWindSpeed":2.3,
               "bearing":306,
               "compass":"northWest",
               "speed":0.1
            },
      "road":{
               "protectionTemp":0,
               "roadSurfaceTemp":19.9
            }
   },
      {
      "id":"NRA1764",
      "temp":{ "air":10.3, 
               "dewPoint": 7.4
            },
      "precip": null,
      "wind":{ "maximumWindSpeed":2.2,
               "bearing":133,
               "compass":"southEast",
               "speed":0.6
            },
      "road":{
               "protectionTemp":0,
               "roadSurfaceTemp":20.5
            }
   }
]
```

GPS coordinates can be used to find the closest weather station using the following URL:
> www.irelandweathernow.ie/station/find?loc:[latitude]+[Longitude]

This will return a single station object in the same format had a specific station been requested. The GPS lookup will be done by the backend.

A list of all stations by ID and their locations in a user friendly format can be retrieved from:

> www.irelandweathernow.ie/station/list

Here's an example of the object array returned:
```
[
   {
      "id":"NRA4207",
      "location":"Clonmacken Link Road"
   },
   {
      "id":"NRA4842",
      "location":"Crusheen"
   },
   {
      "id":"NRA5130",
      "location":"Dublin Tunnel"
   }
   ...
]
```
###Parameters

More specific requests can be made regarding individual station measurements by using parameters.
 - wind
 - precip
 - temp
 - road
 

For example:
  - www.irelandweathernow.ie/station/NRA2284?m=wind
 
      Returns station NRA2284's last wind measurements.


  - www.irelandweathernow.ie/station/loc:53.277628+-9.009711?m=temp&road
  
      Returns the temperature and road conditions for the station nearest the coords: 53.277628,-9.009711.


