# Data-Science-for-Good: How to measure juestic?

The Center for Policing Equity (CPE) host a Kaggle compettition for community trainers working together to build more fair and just systems. The main problems of this competition consist of two. How do you measure justice? And how do you solve the problem of racism in policing? In this project, I will discover factors that drive racial disparities in policing by analyzing census and police department deployment data.

## 1.Dataset

To begin with, lets look into the datasets that we are provided.

In Cpe-data folder, they are catagorized by different region of different state.
![image](https://user-images.githubusercontent.com/42877304/49816131-40641a00-fd3b-11e8-9062-6e449758296e.png)

Take Dept_11-00091 as example, it's the dataset folder from Massachusetts, and the three main dataset are police shapefiles(which are police distirct geometry data), ASC data(which are census data containing gender, race, proverty, education level, employment condition, etc), and police crime records(which are provided by police departments across the United States, it's the first and largest collection of standardized police behavioral data)
![image](https://user-images.githubusercontent.com/42877304/49816261-80c39800-fd3b-11e8-8d77-b74e7cf89669.png)

### prerequisite knowledge

Note that ACS data is census data regarding to census tract, which is an area roughly equivalent to a neighborhood established by the Bureau of Census for analyzing populations. Census tracts generally encompass a population between 2,500 to 8,000 people. Bureau of Census describes them as "relatively permanent", but they do change over time.
![image](https://user-images.githubusercontent.com/42877304/49816320-9d5fd000-fd3b-11e8-813a-a277b41b99dc.png)

Then, we have to consider how to combine census data regarding to census tracts when we are provided geometry data of police districts, because census tracts and police distircts are different geometry concept. To settle this problem, we need to pull down supplementary data from US census Bureau.

Luckily, we can easily download shapefiles for census tracts from US census Bureau's website:https://www.census.gov/geo/maps-data/data/cbf/cbf_tracts.html
![image](https://user-images.githubusercontent.com/42877304/49818342-76f06380-fd40-11e8-8f08-c7d04e836343.png)

The next thing might be new to us is what we call shapefiles. Without getting into too much details, Shapefiles contain geospatial information (e.g. the shape of the boundaries of a U.S. County within the context of some coordinate system stored in a .shp file) and attributes about the geographical entity. Shapefiles can be read using GeoPandas.

Each Shapfiles possess its own Coordinate Reference Systems(CRS). In order to Map differnet shapefiles on the same coordinate, we have to standalize their CRS before mapping.

## Clean and Select Data

In the police crime record data, there are unknown data or empty data in our target features, for example, races. We have to repalce them with NaN values.
![image](https://user-images.githubusercontent.com/42877304/49819814-4d393b80-fd44-11e8-861f-b18c98b64b0e.png)


