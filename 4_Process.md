# Process
Data processing and analyzing will primary occur in RStudio using the R programming language with supplement visualizations done via Tableau. R Libraries are utilised in this process. 

## Combining Data
The data is separated by month; one CSV file per month. In order to order analyze all 12-months worth of data it’s necessary to combine the files as follows. NOTE: The directory path will vary depending on where the original files were downloaded/saved.
``` R
# setting up files 
file_names <- dir(path = "Desktop/Courses/GDAStuff/CaseStudy/Case_Study_01_Cyclistic/Datasets", 
                  pattern = NULL, all.files = FALSE, full.names = FALSE, 
                  recursive = FALSE, ignore.case = FALSE, include.dirs = FALSE, 
                  no.. = FALSE)

# creating dataframe for last 12 months of data
cyclistic_df <- do.call(rbind, lapply(file_names, read.csv))

# export df to CSV file
write.csv(cyclistic_df, "cyclistic_202103-202203.csv", row.names = TRUE) # completed 2022-04-21
```
## Create Data Frame
Once we’ve combined the necessary data into a single CSV file, we can read it and store in a data frame. This will allow for ease of use during the cleaning process and the analysis process.
``` R
bikeshare_df <- read_csv("Datasets/cyclistic_202103-202203.csv")
```
```
## New names:
## Rows: 5952028 Columns: 14
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (7): ride_id, rideable_type, start_station_name, start_station_id, end_... dbl
## (5): ...1, start_lat, start_lng, end_lat, end_lng dttm (2): started_at,
## ended_at
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...1`
```
## Inspecting Data Frame
```R
head(bikeshare_df)
```
```
## # A tibble: 6 × 14
##    ...1 ride_id          rideable_type started_at          ended_at           
##   <dbl> <chr>            <chr>         <dttm>              <dttm>             
## 1     1 CFA86D4455AA1030 classic_bike  2021-03-16 08:32:30 2021-03-16 08:36:34
## 2     2 30D9DC61227D1AF3 classic_bike  2021-03-28 01:26:28 2021-03-28 01:36:55
## 3     3 846D87A15682A284 classic_bike  2021-03-11 21:17:29 2021-03-11 21:33:53
## 4     4 994D05AA75A168F2 classic_bike  2021-03-11 13:26:42 2021-03-11 13:55:41
## 5     5 DF7464FBE92D8308 classic_bike  2021-03-21 09:09:37 2021-03-21 09:27:33
## 6     6 CEBA8516FD17F8D8 classic_bike  2021-03-20 11:08:47 2021-03-20 11:29:39
## # … with 9 more variables: start_station_name <chr>, start_station_id <chr>,
## #   end_station_name <chr>, end_station_id <chr>, start_lat <dbl>,
## #   start_lng <dbl>, end_lat <dbl>, end_lng <dbl>, member_casual <chr>
```
```R
summary(bikeshare_df)
```
```
##       ...1           ride_id          rideable_type     
##  Min.   :      1   Length:5952028     Length:5952028    
##  1st Qu.:1488008   Class :character   Class :character  
##  Median :2976014   Mode  :character   Mode  :character  
##  Mean   :2976014                                        
##  3rd Qu.:4464021                                        
##  Max.   :5952028                                        
##                                                         
##    started_at                     ended_at                   start_station_name
##  Min.   :2021-03-01 00:01:09   Min.   :2021-03-01 00:06:28   Length:5952028    
##  1st Qu.:2021-06-15 21:08:34   1st Qu.:2021-06-15 21:35:02   Class :character  
##  Median :2021-08-13 20:17:38   Median :2021-08-13 20:38:45   Mode  :character  
##  Mean   :2021-08-20 17:11:57   Mean   :2021-08-20 17:33:33                     
##  3rd Qu.:2021-10-12 08:05:40   3rd Qu.:2021-10-12 08:19:54                     
##  Max.   :2022-03-31 23:59:47   Max.   :2022-04-01 22:10:12                     
##                                                                                
##  start_station_id   end_station_name   end_station_id       start_lat    
##  Length:5952028     Length:5952028     Length:5952028     Min.   :41.64  
##  Class :character   Class :character   Class :character   1st Qu.:41.88  
##  Mode  :character   Mode  :character   Mode  :character   Median :41.90  
##                                                           Mean   :41.90  
##                                                           3rd Qu.:41.93  
##                                                           Max.   :45.64  
##                                                                          
##    start_lng         end_lat         end_lng       member_casual     
##  Min.   :-87.84   Min.   :41.39   Min.   :-88.97   Length:5952028    
##  1st Qu.:-87.66   1st Qu.:41.88   1st Qu.:-87.66   Class :character  
##  Median :-87.64   Median :41.90   Median :-87.64   Mode  :character  
##  Mean   :-87.65   Mean   :41.90   Mean   :-87.65                     
##  3rd Qu.:-87.63   3rd Qu.:41.93   3rd Qu.:-87.63                     
##  Max.   :-73.80   Max.   :42.17   Max.   :-87.49                     
##                   NA's   :4883    NA's   :4883
```
Inspecting column names
```
##  [1] "...1"               "ride_id"            "rideable_type"     
##  [4] "started_at"         "ended_at"           "start_station_name"
##  [7] "start_station_id"   "end_station_name"   "end_station_id"    
## [10] "start_lat"          "start_lng"          "end_lat"           
## [13] "end_lng"            "member_casual"
```
After inspection, there are is an extra column “… 1” which is not needed since it only contains a list of numbers which correspond to the row number - this was created when the the 12-months worth of data was created. This will need to be removed in the cleaning process.
Check the total number of rows is 5,952,028 after combining the 12-month data:
```
## [1] 5952028
```
## Cleaning and Formatting Data
Add Columns for Date, Month, Year, Day of the Week, Ride Length
```R
bikeshare_df$date <- as.Date(bikeshare_df$started_at)
bikeshare_df$month <- format(as.Date(bikeshare_df$date), "%m")
bikeshare_df$day <- format(as.Date(bikeshare_df$date), "%d")
bikeshare_df$year <- format(as.Date(bikeshare_df$date), "%Y")
bikeshare_df$day_of_week <- format(as.Date(bikeshare_df$date), "%A")
bikeshare_df$ride_length <- difftime(bikeshare_df$ended_at,bikeshare_df$started_at)
is.factor(bikeshare_df$ride_length) #Convert from factor to numerice
bikeshare_df$ride_length <- as.numeric(as.character(bikeshare_df$ride_length))
is.numeric(bikeshare_df$ride_length)
```
## Remove bad data
The data frame includes a few hundred entries when bikes were taken out of docks and checked for quality by Divvy or ride_length was negative. We will create a new version of the data frame (v2) since data is being removed.
```R
bikeshare_df_v2 <- bikeshare_df[!(bikeshare_df$ride_length<0),] # removes neg values
bikeshare_df_v2 <- mutate(bikeshare_df_v2, ...1 = NULL) # removes extra col
```
## Inspect new data frame
```R
head(bikeshare_df_v2)
```
```
## # A tibble: 6 × 19
##   ride_id rideable_type started_at          ended_at            start_station_n…
##   <chr>   <chr>         <dttm>              <dttm>              <chr>           
## 1 CFA86D… classic_bike  2021-03-16 08:32:30 2021-03-16 08:36:34 Humboldt Blvd &…
## 2 30D9DC… classic_bike  2021-03-28 01:26:28 2021-03-28 01:36:55 Humboldt Blvd &…
## 3 846D87… classic_bike  2021-03-11 21:17:29 2021-03-11 21:33:53 Shields Ave & 2…
## 4 994D05… classic_bike  2021-03-11 13:26:42 2021-03-11 13:55:41 Winthrop Ave & …
## 5 DF7464… classic_bike  2021-03-21 09:09:37 2021-03-21 09:27:33 Glenwood Ave & …
## 6 CEBA85… classic_bike  2021-03-20 11:08:47 2021-03-20 11:29:39 Glenwood Ave & …
## # … with 14 more variables: start_station_id <chr>, end_station_name <chr>,
## #   end_station_id <chr>, start_lat <dbl>, start_lng <dbl>, end_lat <dbl>,
## #   end_lng <dbl>, member_casual <chr>, date <date>, month <chr>, day <chr>,
## #   year <chr>, day_of_week <chr>, ride_length <dbl>
```
```R
colnames(bikeshare_df_v2)
```
```
##  [1] "ride_id"            "rideable_type"      "started_at"        
##  [4] "ended_at"           "start_station_name" "start_station_id"  
##  [7] "end_station_name"   "end_station_id"     "start_lat"         
## [10] "start_lng"          "end_lat"            "end_lng"           
## [13] "member_casual"      "date"               "month"             
## [16] "day"                "year"               "day_of_week"       
## [19] "ride_length"
```
```R
View(bikeshare_df_v2)
nrow(bikeshare_df_v2) # check num of rows
```
```
## [1] 5951881
```
```R
any(bikeshare_df_v2$start_station_name == "HQ QR") # doesn't exist in data set
```
```
## [1] NA
```
```R
any(bikeshare_df_v2$ride_length < 0) # checking for negative values
```
```
## [1] FALSE
```
## Prepare for Export
After cleaning, a CSV file will be exported to preserve the clean data and another file will be created for use in Tableau.
```R
write_csv(bikeshare_df_v2, "2022-04-26_cyclistic_clean_data.csv")
```
Since the data frame containing the clean data is too large to upload to Tableau Public (file limit of 1 GB) a subset of the data frame will be created and exported.
```R
# selection of desired columns to keep for export
myvars <- c("ride_id", "rideable_type", "member_casual", "date", "month", 
            "day", "year", "day_of_week", "ride_length", "start_station_name", 
            "end_station_name")

# store selected columns in a data frame
bikeshare_subset <- bikeshare_df_v2[myvars]

# write subset data frame to CSV file
write_csv(bikeshare_subset, "2022-04-30_cyclistic_subset.csv")
```
