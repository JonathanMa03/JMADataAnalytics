# Analyse and Share
Summary statistics for both members and casual riders
```R
summary(bikeshare_df_v2$ride_length)
```
```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0     395     704    1296    1283 3356649
```
Comparing measures of spread
```R
aggregate(bikeshare_df_v2$ride_length ~ bikeshare_df_v2$member_casual, FUN = mean)
```
```
##   bikeshare_df_v2$member_casual bikeshare_df_v2$ride_length
## 1                        casual                   1916.7300
## 2                        member                    803.7535
```
```R
aggregate(bikeshare_df_v2$ride_length ~ bikeshare_df_v2$member_casual, FUN = median)
```
```
##   bikeshare_df_v2$member_casual bikeshare_df_v2$ride_length
## 1                        casual                         950
## 2                        member                         564
```
```R
aggregate(bikeshare_df_v2$ride_length ~ bikeshare_df_v2$member_casual, FUN = max)
```
```
##   bikeshare_df_v2$member_casual bikeshare_df_v2$ride_length
## 1                        casual                     3356649
## 2                        member                       93596
```
```R
aggregate(bikeshare_df_v2$ride_length ~ bikeshare_df_v2$member_casual, FUN = min)
```
```
##   bikeshare_df_v2$member_casual bikeshare_df_v2$ride_length
## 1                        casual                           0
## 2                        member                           0
```
We can see that for casuals, the mean and median are both greater than members
## Average Duration by rider type sorted by day of the week
```R
# first order by day of the week
bikeshare_df_v2$day_of_week <- ordered(bikeshare_df_v2$day_of_week, 
                                       levels=c("Sunday", "Monday", "Tuesday", 
                                                "Wednesday", "Thursday", "Friday", 
                                                "Saturday")) 

# calculate average
aggregate(bikeshare_df_v2$ride_length ~ bikeshare_df_v2$member_casual + 
            bikeshare_df_v2$day_of_week, FUN = mean)
```
```
##    bikeshare_df_v2$member_casual bikeshare_df_v2$day_of_week
## 1                         casual                      Sunday
## 2                         member                      Sunday
## 3                         casual                      Monday
## 4                         member                      Monday
## 5                         casual                     Tuesday
## 6                         member                     Tuesday
## 7                         casual                   Wednesday
## 8                         member                   Wednesday
## 9                         casual                    Thursday
## 10                        member                    Thursday
## 11                        casual                      Friday
## 12                        member                      Friday
## 13                        casual                    Saturday
## 14                        member                    Saturday
##    bikeshare_df_v2$ride_length
## 1                    2252.6486
## 2                     922.8562
## 3                    1918.8017
## 4                     781.5540
## 5                    1664.8835
## 6                     753.9494
## 7                    1667.7131
## 8                     756.2363
## 9                    1675.5797
## 10                    753.2709
## 11                   1805.0783
## 12                    787.4889
## 13                   2075.4493
## 14                    902.2964
```
## Sort by rider type, then by day of the week
```R
bikeshare_df_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>%  #creates weekday field using wday()
  group_by(member_casual, weekday) %>%  #groups by user type and weekday
  summarise(number_of_rides = n()                           #calculates the number of rides and average duration 
            ,average_duration = mean(ride_length)) %>%      # calculates the average duration
  arrange(member_casual, weekday)                               # sorts
```
```
## # A tibble: 14 × 4
## # Groups:   member_casual [2]
##    member_casual weekday number_of_rides average_duration
##    <chr>         <ord>             <int>            <dbl>
##  1 casual        Sun              500143            2253.
##  2 casual        Mon              304983            1919.
##  3 casual        Tue              286834            1665.
##  4 casual        Wed              295283            1668.
##  5 casual        Thu              299120            1676.
##  6 casual        Fri              372054            1805.
##  7 casual        Sat              572097            2075.
##  8 member        Sun              406002             923.
##  9 member        Mon              462262             782.
## 10 member        Tue              513632             754.
## 11 member        Wed              522687             756.
## 12 member        Thu              491308             753.
## 13 member        Fri              470988             787.
## 14 member        Sat              454488             902.
```
On average, member riders ride more during the weekdays while casual riders ride more during the weekends
## Visualisations
Casual riders take more rides on the weekends
![GDACSvisual1](https://user-images.githubusercontent.com/124275087/233642618-376eaeeb-7b6e-4da6-a4d2-ea160f6ad1b6.png)
Casual riders also take longer rides on weekends
![GDACSvisual2](https://user-images.githubusercontent.com/124275087/233642812-59dd64e1-ee72-4744-929e-62daeeed611f.png)
## Source and Sink points for casual riders
With few exceptions, the top 10 start and end stations for casual riders are nearly identical. In particular, the top 3 stations (listed below) are the same start and ending points for casual riders. This could be useful for making recommendations for a targeted ad campaign to convert casual riders to member riders.
![GDACSvisual3](https://user-images.githubusercontent.com/124275087/233643402-f912796a-d8b4-4015-8f97-1dc76d2237b9.png)
![GDACSvisual4](https://user-images.githubusercontent.com/124275087/233643445-8f3ffb25-3591-4c34-bf59-57436fee415c.png)
## Source and Sink points for member riders
Similar to the casual riders, the top ten start and end locations for members riders are nearly identical with the exception of Green St & Madison St which appears as the 10th end location for member riders. This could useful for rewarding member riders with special discounts, however that’s outside the realm of the business task.
![GDACSvisual5](https://user-images.githubusercontent.com/124275087/233643629-dab58d24-1d16-454f-a27d-2b2b2bbb4743.png)
![GDACSvisual6](https://user-images.githubusercontent.com/124275087/233643722-660844ba-0159-4f62-bd00-b829a404bd7e.png)
