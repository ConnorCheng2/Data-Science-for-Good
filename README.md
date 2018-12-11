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

In ACS data, we need to select target feature that are likely to relate to crime. ACS data is relatively massive and scattered so we are it will be effective and plain if we extract the target features on the same dataFrame. And thus we extract target features from different dataFrame and merge them onto one dataFrame.

![image](https://user-images.githubusercontent.com/42877304/49821583-c76bbf00-fd48-11e8-80d7-c36748415cf0.png)

## Data Aggregation

First data aggregation is to merge police crime records qith police district shapefiles on their common keys, so that we can have a new police crime data with geometry data of police district.
![image](https://user-images.githubusercontent.com/42877304/49822426-20d4ed80-fd4b-11e8-90f6-30110f8ea086.png)

To aggreate the new police crime data with geometry data of police district with census tracts shapefiles, we have to figure out the geometrical relations between police districts and census tracts.
![image](https://user-images.githubusercontent.com/42877304/49823001-a60cd200-fd4c-11e8-9ae1-8abcc505c988.png)

By running the code above, we are able to visualize the relations of between police districts and census tracts, and compute the percentage each census tracts contributes to various polices districts.
![image](https://user-images.githubusercontent.com/42877304/49823060-cccb0880-fd4c-11e8-8c68-a2a1ec5e8160.png)

Then we are able to combine the new police crime data with geometry data of police district with census tracts shapefiles
![image](https://user-images.githubusercontent.com/42877304/49823347-73170e00-fd4d-11e8-9ba4-56abdd7706f5.png)

Hence, with what we aggregated so far, we are able to merge differnent dataframes onto one, so that we can do visualization with it.
![image](https://user-images.githubusercontent.com/42877304/49823546-f59fcd80-fd4d-11e8-9485-1fad0188569f.png)

## Data Visualization

What we can visualize with the new police crime data with police district geometry information(For more plot for other catagory please check out the code "CleanAndVisualizeMA"):
![image](https://user-images.githubusercontent.com/42877304/49823669-3c8dc300-fd4e-11e8-9fcb-63c7d9cbf3d2.png)

What we can visualize with the census data for police district(For more plot for other catagory please check out the code "CleanAndVisualizeMA"):
![image](https://user-images.githubusercontent.com/42877304/49823912-cdfd3500-fd4e-11e8-9cb2-faf614206d9c.png)
## Correlation 

From the visualization results above, we can roughly have a ideal of how crime records related to the target value we choose.
But to specifically and mathematically measure the Equity in policing, we have to use a statistical tool named correlation.

Here, I use the population of different target features(races, genders, ages, poverty level, education, employment condition) from ACS data to correlate with the number of crime recods regarding to different police district. So that's the correlation I found out:

![image](https://user-images.githubusercontent.com/42877304/49823986-f8e78900-fd4e-11e8-9ae7-883c01fc7f84.png)

Notice: the correlation above dosen't necessarily mean that one feature cause the crimes, but it will be a good reference for police department to inspect their policing behavior.
