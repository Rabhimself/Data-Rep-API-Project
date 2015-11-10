# Data Representation Project - 2015
**Robert Bryant**

##National Roads Weather Station Data
###Supplied by Transport Infrastructure Ireland

##Introduction
The purpose of this project is to be a design for an API utilizing the National Roads Weather Station Dataset 
found at [Data.Gov.Ie](https://data.gov.ie/dataset/national-roads-weather-station-data) to provide realtime road conditions.

###About The Dataset
The Dataset is provided as an XML document, and is updated every five minutes using data from 80+ weather stations across Ireland. 
These stations provide weather information in relation to road safety, information such as: air temperature, precipitation, wind speed & direction and Road Surface temperature.

The XML document is around 3300 lines long, with the header providing payload publication information, and each weather station's measurements listed as individual nodes. It is important to note that these nodes are nested alongside the header and publication information. A typical node looks like this:

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

Each measurement is indexed, and each measurement has multiple values associated with it. For example; Wind measurements include the maximum wind speed recorded, direction bearing, compass direction, and speed.

###Querying the Dataset
Individual station measurements can be requested by station ID's, and are requested from:
> www.irelandweathernow.ie/station/

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

GPS coordinates can be used to find the closest weather station.
> www.irelandweathernow.ie/station/find?loc:[latitude]+[Longitude]




###Parameters

More specific requests can be made regarding station measurements by using parameters.

 - wind
 - precip
 - temp
 - road


For example:
  - www.irelandweathernow.ie/station/NRA2284?cat=wind
