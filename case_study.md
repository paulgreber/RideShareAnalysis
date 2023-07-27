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

Cyclistic is not a real company, however the data for this exercise is real and comes from company called Divvy, which is a real bike ride-share company that operates in Chicago.

The past twelve months of data (2022-04 to 2023-03) has been acquired from Divvy's severs located [here](https://divvy-tripdata.s3.amazonaws.com/index.html). This data is made freely available under the following [license](https://ride.divvybikes.com/data-license-agreement). Each month of data is contained in it's own CSV file which will need to be merged together before the data can be analyzed.

It is expected that the data is free of bias and is credible, this is because the data comes from Divvy itself and not a third party.

There should be no privacy issues in the data, as no names, credit card numbers or user ids are present. The data does contain a `ride_id` which is a unique random 16-character alpha-numeric string which acts as an identifier of the ride, however this does not relate to who actually took the ride.

What the data does contain is information related to:

* When a ride took place
* How long was the ride
* Where a ride took place
* What kind of bike was used
* If the user was a casual rider or an annual member

Based on this it appears that there will be many elements of the data that can be examined to determine what differences exist between casual riders and annual members.

## Process

Python will be used to process the data for analysis. Python was chosen because it contains a wide range of powerful tools (such as Pandas) that can easily clean and validate a large set of data.

The first way that the integrity of the data was checked for null values in the dataset. Doing this discovered that there were null values in much of the values, however these null values occurred only in the locational information. The choice was made to keep these entries in our dataset and only remove them when doing analysis related to the location of the rides.

The next way that the integrity of the data was analyzed was by looking for errors in the `started_at` and `ended_at` columns. These columns contained the dates and times that a ride took place. Any rows where a `ended_at` was earlier than a `started_at` were removed as this indicated an error occurred when recording the entry.

A large portion of our analysis was going to be spent looking at when and for how long rides were taken. To do this several operations had to be performed.

First, the ride duration had to be calculated. This was done by subtracting the `started_at` from the `ended_at` and then returning the result as the number of elapsed minutes. These values were added to a newly created column called `ride_duration`

Next, the day of the week and month were extracted from the 