## Summary
In 2016, Cyclistic launched a bike-sharing program in Chicago, which has grown to a fleet of 5,824 bicycles across 692 stations. The program offers pricing plans for casual riders (single-ride or full-day passes) and annual memberships for Cyclistic members. The finance team has determined that annual members are more profitable and the marketing team wants to convert more casual riders into members. They plan to analyze historical bike trip data to identify trends and understand why casual riders would buy a membership, and how digital media could affect their marketing tactics.
# Data Description
| Data | Type | Description |
| ---- | ---- | ----------- |
| ride_id | Categorical | Unique number assigned for a ride trip |
| rideable_type | Categorical | Type of bike used during trip |
| started_at | Time | Start date and time for a trip |
| ended_at | Time | End date and time for a trip |
| start_station_name | Categorical | Name of station where trip started |
| start_station_id | Categorical | Unique identification code assigned to the start station |
| end_station_name | Categorical | Name of the station where the trip ended |
| end_station_id | Categorical | Unique identification code assigned to the end station |
| start_lat | Numeric | Latitude position of where the trip started |
| start_lng | Numeric | Longitude position of where the trip started |
| end_lat | Numeric | Latitude position of where the trip ended |
| end_lng | Numeric | Longitude position of where the trip ended |
| member_casual | Categorical | Customer type; “member” = annual member, “casual” = casual rider |
