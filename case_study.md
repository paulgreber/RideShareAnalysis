# Case Study - Cyclistic Ride Share Data

Cyclistic is a bike-share service operating a fleet of 5842 bicycles across 692 stations across Chicago.

Cyclistic categorized their riders in one of two categories:

* Casual riders who use single-ride passes or full-day passes
* Riders with annual memberships.

Cyclistic's financial analysts have concluded that annual members are more profitable than casual riders.

Cyclistic's marketing team want to create a marketing campaign designed to convert casual riders into annual members.

Before the marketing campaign can be started trends need to be identified in Cyclistic's historical bike trip data.

## Ask

Cyclistic intends to create a marketing campaign designed to convert casual riders into annual members. To be able to do this effectively, historical bike trip data needs to be analyzed to determine any difference in how casual riders differ from annual members.

## Prepare

### Data Source

Cyclistic is not a real company, however the data for this exercise is real and comes from company called Divvy, which is a real bike ride-share company that operates in Chicago.

The past twelve months of data (2022-04 to 2023-03) has been acquired from Divvy's severs located [here](https://divvy-tripdata.s3.amazonaws.com/index.html). This data is made freely available under the following [license](https://ride.divvybikes.com/data-license-agreement). Each month of data is contained in it's own CSV file which will need to be merged together before the data can be analyzed.

It is expected that the data is free of bias and is credible, this is because the data comes from Divvy itself and not a third party.

### Privacy Issues

There should be no privacy issues in the data, as no names, credit card numbers or user ids are present. The data does contain a `ride_id` which is a unique random 16-character alpha-numeric string which acts as an identifier of the ride, however this does not relate to who actually took the ride.

What the data does contain is information related to:

* When a ride took place
* How long was the ride
* Where a ride took place
* What kind of bike was used
* If the user was a casual rider or an annual member

Based on this it appears that there will be many elements of the data that can be examined to determine what differences exist between casual riders and annual members.

## Process

### Tools Used

Python will be used to process the data for analysis. Python was chosen because it contains a wide range of powerful tools (such as Pandas) that can easily clean and validate a large set of data.

### Data Integrity

The first way that the integrity of the data was checked for null values in the dataset. Doing this discovered that there were null values in much of the values, however these null values occurred only in the locational information. The choice was made to keep these entries in our dataset and only remove them when doing analysis related to the location of the rides.

The next way that the integrity of the data was analyzed was by looking for errors in the `started_at` and `ended_at` columns. These columns contained the dates and times that a ride took place. Any rows where a `ended_at` was earlier than a `started_at` were removed as this indicated an error occurred when recording the entry.

### Data Transformations

#### Ride Date and Time

A large portion of our analysis was going to be spent looking at when and for how long rides were taken. To do this several operations had to be performed.

First, the ride duration had to be calculated. This was done by subtracting the `started_at` from the `ended_at` and then returning the result as the number of elapsed minutes. These values were added to a newly created column called `ride_duration`.

Next, the day of the week and month were extracted from the `started_at` entry and used to create a `day_of_week` and `month` entries.

#### Ride Location

While the physical location of most of the rides were present, they are not terribly meaningful values when trying to determine trends in rider behavior. Therefor, the latitude and longitude of each ride (when available) was translated to it's corresponding neighborhood so that trends could be identified to determine which areas.

This translation was done by acquiring a [Shapefile](https://en.wikipedia.org/wiki/Shapefile) the neighborhoods of Chicago from the [Chicago Data Portal](https://data.cityofchicago.org/Facilities-Geographic-Boundaries/Boundaries-Neighborhoods/bbvz-uum9). The file was opened with Geopandas (a Pandas extension library for working with geospatial data) which allowed the content of the shapefile to be worked with as a Pandas dataframe and also allows the shapefile to be drawn.

The data from the shapefile was then joined with the data from our main dataset, resulting in our main dataset containing the neighborhoods that each starting latitude and longitude existed in.

## Analyze

Analysis will attempt to answer the following questions:

* When are users riding?
* What are users riding?
* Where are users riding?

But before we begin, let's look at how many riders there are:

![Count of Annual Members vs Casual Riders](/analysis/count_annual_members_vs_casual_riders.png)
![Percent of Annual Members vs Casual Riders](/analysis/percent_member_vs_casual.png)

### When are Users Riding?

![Rider Count by Month](/analysis/rider_each_month.png)
The number of riders is lowest during the colder months and higher during the warm months. However during cold months, riders with an annual membership is still much higher than casual riders. But during the warm month (summer in particular) the number of casual riders increases to be almost equal with that the riders with annual memberships.

![Rider Count by Day of Week](/analysis/rider_day_of_week.png)
Riders with annual memberships is highest during the week, while casual riders is highest during the weekends.

![Rider Count by Time of Day](/analysis/rider_time_of_day.png)
All riders tend to ride most often in the afternoon and least often at night.

Casual riders tend to ride more in the evening than they do in the morning, while riders with annual memberships tend to ride as often in the mornings as they do in the evenings.

![Ride Duration](/analysis/ride_duration.png)
*Note that this plot is made with all outlying data removed*

On average, casual riders take slightly longer rides than riders with annual memberships.

### What are Users Riding?

![Bike Type by Membership](/analysis/bike_type_by_membership.png)
In general, casual riders tend to ride electric bikes more often then they do classical peddle bikes. Whereas annual members ride either bike at almost equal numbers.

Casual riders are also the only riders who use docked bikes.

![Bike Type by Month](/analysis/bike_type_by_month.png)
Looking at the bike types by month we see something strange. The trend seen above seems to hold true, however for the month of May and June usage of classic bikes increased dramatically, followed by a sharp increase in electric bikes in July, August, September and October.

More investigation is needed to determine if this was an actual change in rider behavior or if there was some other explanation, such as a shortage of electric bikes.

### Where are Users Riding?

