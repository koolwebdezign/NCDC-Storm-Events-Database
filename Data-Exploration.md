# Data-Exploration
J. Welch  
October 17, 2016  

I am an MBA student at Bridgewater State University, enrolled in MGMT582 - Business Intelligence/Analytics.  This data exploration project is intended to be my final project and in-class presentation.  The goal of this project is to gain practical working experience with R Studio and to apply some data analysis techniques in which we have been introduced within this course.

I have chosen to work with the NCDC Storm Events Database available at DATA.gov.  For more details related to this raw data set, see <https://catalog.data.gov/dataset/ncdc-storm-events-database>.

My first step in preparing my analysis will be to write a script to get the data at Data.gov.  After following instructions about the storm events database, I found that the raw data sits in zipped files located on an FTP server at <ftp://ftp.ncdc.noaa.gov/pub/data/swdi/stormevents/csvfiles/>.  I am going to attempt to write a script that will automatically get this data from its FTP location, unzip each file, and then append it to create a larger file that can be analyzed here with R Studio.


```r
if (!file.exists("./stormdata.csv")) {
    # Load the R.utils library
    library("R.utils")

    for (year in 2010:2013) {
        # Identify the URL of the download file
        URL <- paste("ftp://ftp.ncdc.noaa.gov/pub/data/swdi/stormevents/csvfiles/StormEvents_details-ftp_v1.0_d", year, "_c20160223.csv.gz", sep="")
        #print(URL)

        # Download the file into the current directory
        download.file(URL, destfile = "./temp.csv.gz")

        # Unzip the file
        gunzip("./temp.csv.gz")

        # Read the file into memory
        tempdata <- read.csv("./temp.csv")
    
        if (year==2010) {
            stormdata <- tempdata
        } else {
            stormdata <- rbind(stormdata, tempdata)
        }

        # Delete the temporary file
        unlink("./temp.csv")
        
        # Write the loaded data to local CSV file
        write.csv(stormdata, "./stormdata.csv")
    }
    
} else if (!exists("stormdata")) {
    stormdata <- read.csv("./stormdata.csv")
}
```

```
## Warning: package 'R.utils' was built under R version 3.2.5
```

```
## Loading required package: R.oo
```

```
## Warning: package 'R.oo' was built under R version 3.2.5
```

```
## Loading required package: R.methodsS3
```

```
## Warning: package 'R.methodsS3' was built under R version 3.2.5
```

```
## R.methodsS3 v1.7.1 (2016-02-15) successfully loaded. See ?R.methodsS3 for help.
## R.oo v1.20.0 (2016-02-17) successfully loaded. See ?R.oo for help.
## 
## Attaching package: 'R.oo'
## 
## The following objects are masked from 'package:methods':
## 
##     getClasses, getMethods
## 
## The following objects are masked from 'package:base':
## 
##     attach, detach, gc, load, save
## 
## R.utils v2.4.0 (2016-09-13) successfully loaded. See ?R.utils for help.
## 
## Attaching package: 'R.utils'
## 
## The following object is masked from 'package:utils':
## 
##     timestamp
## 
## The following objects are masked from 'package:base':
## 
##     cat, commandArgs, getOption, inherits, isOpen, parse, warnings
```

```r
# Output the first 10 rows of the file
head(stormdata, n=10)
```

```
##    BEGIN_YEARMONTH BEGIN_DAY BEGIN_TIME END_YEARMONTH END_DAY END_TIME
## 1           201007         7       1251        201007       7     1630
## 2           201001        17       2300        201001      18     1500
## 3           201010         1        830        201010       1     1000
## 4           201007         6        951        201007       6     1830
## 5           201012        26       1700        201012      27     1800
## 6           201002        25       2305        201002      26       38
## 7           201002        16       1200        201002      17        0
## 8           201003        14       1345        201003      14     1415
## 9           201001        21        300        201001      21      600
## 10          201003        11       1816        201003      16      830
##    EPISODE_ID EVENT_ID         STATE STATE_FIPS YEAR MONTH_NAME
## 1       43850   254780 NEW HAMPSHIRE         33 2010       July
## 2       36500   211550 NEW HAMPSHIRE         33 2010    January
## 3       44854   260014 NEW HAMPSHIRE         33 2010    October
## 4       43850   254779 NEW HAMPSHIRE         33 2010       July
## 5       46989   273769 NEW HAMPSHIRE         33 2010   December
## 6       37004   215305 NEW HAMPSHIRE         33 2010   February
## 7       36944   214918 NEW HAMPSHIRE         33 2010   February
## 8       37397   218778 NEW HAMPSHIRE         33 2010      March
## 9       36775   213816       FLORIDA         12 2010    January
## 10      36906   214762          IOWA         19 2010      March
##      EVENT_TYPE CZ_TYPE CZ_FIPS              CZ_NAME WFO
## 1          Heat       Z      12 EASTERN HILLSBOROUGH BOX
## 2    Heavy Snow       Z      12 EASTERN HILLSBOROUGH BOX
## 3   Strong Wind       Z      12 EASTERN HILLSBOROUGH BOX
## 4          Heat       Z      12 EASTERN HILLSBOROUGH BOX
## 5  Winter Storm       Z      12 EASTERN HILLSBOROUGH BOX
## 6     High Wind       Z      12 EASTERN HILLSBOROUGH BOX
## 7    Heavy Snow       Z      12 EASTERN HILLSBOROUGH BOX
## 8   Strong Wind       Z      12 EASTERN HILLSBOROUGH BOX
## 9   Flash Flood       C      33             ESCAMBIA MOB
## 10        Flood       C      23               BUTLER DMX
##       BEGIN_DATE_TIME CZ_TIMEZONE      END_DATE_TIME INJURIES_DIRECT
## 1  07-JUL-10 12:51:00       EST-5 07-JUL-10 16:30:00               0
## 2  17-JAN-10 23:00:00       EST-5 18-JAN-10 15:00:00               0
## 3  01-OCT-10 08:30:00       EST-5 01-OCT-10 10:00:00               0
## 4  06-JUL-10 09:51:00       EST-5 06-JUL-10 18:30:00               0
## 5  26-DEC-10 17:00:00       EST-5 27-DEC-10 18:00:00               0
## 6  25-FEB-10 23:05:00       EST-5 26-FEB-10 00:38:00               0
## 7  16-FEB-10 12:00:00       EST-5 17-FEB-10 00:00:00               0
## 8  14-MAR-10 13:45:00       EST-5 14-MAR-10 14:15:00               2
## 9  21-JAN-10 03:00:00       CST-6 21-JAN-10 06:00:00               0
## 10 11-MAR-10 18:16:00       CST-6 16-MAR-10 08:30:00               0
##    INJURIES_INDIRECT DEATHS_DIRECT DEATHS_INDIRECT DAMAGE_PROPERTY
## 1                  0             0               0           0.00K
## 2                  0             0               0           0.00K
## 3                  0             0               0          50.00K
## 4                  0             0               0           0.00K
## 5                  0             0               0           0.00K
## 6                  0             0               0           2.50M
## 7                  0             0               0           0.00K
## 8                  0             1               0          10.00K
## 9                  0             0               0           0.00K
## 10                 0             0               0          50.00K
##    DAMAGE_CROPS                    SOURCE MAGNITUDE MAGNITUDE_TYPE
## 1         0.00K                      AWOS        NA               
## 2         0.00K                  CoCoRaHS        NA               
## 3         0.00K                 Newspaper        45             EG
## 4         0.00K                      ASOS        NA               
## 5         0.00K           Trained Spotter        NA               
## 6         0.00K                      ASOS        55             MG
## 7         0.00K           Trained Spotter        NA               
## 8         0.00K                 Newspaper        35             EG
## 9         0.00K           County Official        NA               
## 10        0.00K Official NWS Observations        NA               
##               FLOOD_CAUSE CATEGORY TOR_F_SCALE TOR_LENGTH TOR_WIDTH
## 1                               NA                     NA        NA
## 2                               NA                     NA        NA
## 3                               NA                     NA        NA
## 4                               NA                     NA        NA
## 5                               NA                     NA        NA
## 6                               NA                     NA        NA
## 7                               NA                     NA        NA
## 8                               NA                     NA        NA
## 9              Heavy Rain       NA                     NA        NA
## 10 Heavy Rain / Snow Melt       NA                     NA        NA
##    TOR_OTHER_WFO TOR_OTHER_CZ_STATE TOR_OTHER_CZ_FIPS TOR_OTHER_CZ_NAME
## 1                                                  NA                  
## 2                                                  NA                  
## 3                                                  NA                  
## 4                                                  NA                  
## 5                                                  NA                  
## 6                                                  NA                  
## 7                                                  NA                  
## 8                                                  NA                  
## 9                                                  NA                  
## 10                                                 NA                  
##    BEGIN_RANGE BEGIN_AZIMUTH BEGIN_LOCATION END_RANGE END_AZIMUTH
## 1           NA                                     NA            
## 2           NA                                     NA            
## 3           NA                                     NA            
## 4           NA                                     NA            
## 5           NA                                     NA            
## 6           NA                                     NA            
## 7           NA                                     NA            
## 8           NA                                     NA            
## 9            2           ENE SOUTH FLOMATON         2         ENE
## 10           3           ESE   NEW HARTFORD         2          NE
##      END_LOCATION BEGIN_LAT BEGIN_LON END_LAT  END_LON
## 1                        NA        NA      NA       NA
## 2                        NA        NA      NA       NA
## 3                        NA        NA      NA       NA
## 4                        NA        NA      NA       NA
## 5                        NA        NA      NA       NA
## 6                        NA        NA      NA       NA
## 7                        NA        NA      NA       NA
## 8                        NA        NA      NA       NA
## 9  SOUTH FLOMATON   30.9958  -87.2388 30.9901 -87.2318
## 10    PARKERSBURG   42.5589  -92.5583 42.5886 -92.7608
##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             EPISODE_NARRATIVE
## 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          A strong ridge built into Southern New England resulting in temperatures nearing 100 with high humidity. Heat index values ranged from 100 to 106 for most of Southern New England on the 6th and again on the 7th in a more limited area, generally the Connecticut River Valley.
## 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            A coastal storm passing southern New England just southeast of Nantucket resulted in a period of snow across northern Massachusetts and southwest New Hampshire.
## 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Several waves of low pressure moved across Southern New England bringing unseasonably warm air into the area. This allowed for good mixing which brought strong winds from a strong low level jet to the surface.
## 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          A strong ridge built into Southern New England resulting in temperatures nearing 100 with high humidity. Heat index values ranged from 100 to 106 for most of Southern New England on the 6th and again on the 7th in a more limited area, generally the Connecticut River Valley.
## 5                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     A strengthening winter storm passed southeast of Nantucket and brought heavy snow and strong winds to southwest New Hampshire, resulting in near blizzard conditions at times.  More than 2000 flights were cancelled along the east coast due to the storm, despite this, Manchester Airport remained open through the storm.  Snowfall totals of 8 to 14 inches were widely observed.
## 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        Low pressure centered over Martha's Vineyard brought strong northeast winds onshore into southern New Hampshire. Sustained wind speeds were around 45 mph with gusts between 55 mph and 65 mph. Trees, utility poles, and wires were downed by these winds with these sometimes damaging homes, cars, and other property on the ground.  This also resulted in numerous power outages across southern New Hampshire.
## 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Low pressure moved northeastward from the New Jersey coast, southeast of Nantucket, and into the Gulf of Maine. This spread three to six inches of snow across much of Southern New England.
## 8                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  A stacked low pressure system (surface low and upper level low on top of each other) moved southeast of Nantucket, spreading rain across Southern New England. This resulted in widespread rainfall totals of three to six inches. In eastern Massachusetts, a strong southeasterly low level jet pumped ample moisture into the area, resulting in rainfall totals on the order of six to ten inches. This resulted in major flooding across eastern Massachusetts and Rhode Island, including small stream, urban, and poor drainage flooding. In addition, the Concord River at Lowell, the Shawsheen River at Wilimington, and the Pawtuxet River at Cranston reached record flood stages within two to four days of the rain. ||Strong winds associated with the low pressure system and the low level jet affected both the east and south coasts, resulting in numerous downed trees and wires and some minor structural damage to a few buildings.
## 9                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    On the evening of January 20th, a warm front was moving slowly northeast off the Gulf of Mexico with a moist unstable air-mass moving slowly inland across the central Gulf Coast region. A series of slow moving thunderstorms repeatedly moved across the deeply saturated grounds of the western Florida Panhandle, producing numerous reports of flooding and flash flooding across the region. Prior to this event, northwest Florida was experiencing one of it's wettest winter season on record.
## 10 Iowa enjoyed a welcomed period of dry weather from February 21 through March 5.   Unfortunately, temperatures remained too low to allow substantial melting of the considerable Iowa snowpack during this dry spell.   Rain, mostly in the light to moderate category, was frequent and widespread from the 6th through the 13th.   The rain was accompanied by much warmer temperatures and resulted in rapid snowmelt.   Snow cover, which ranged from 1 to 7 inches along the Missouri border to as much as 30 inches in northwest Iowa at the beginning of the month, was gone by March 17 (with the exception of some drifts).   The combination of snow melt and rainfall pushed nearly all Iowa rivers above flood stage with major flooding reported at some northern Iowa locations.   Despite much drier weather during the second one-half of March, flooding continued at month???s end along the Des Moines River, the Mississippi River below Muscatine and over parts of northwest Iowa.    Rainfall amounts for the month were generally above normal over southern Iowa and well below normal over the northeast.   Rain totals varied from 0.58 inches at Waukon to 4.09 inches at Beaconsfield.   There was only one snow event of consequence during the month.   Snow fell over all but far northern Iowa on the 19th-20th with greatest amounts in the south where 6 or more inches fell at Murray, Lacona, Indianola, Newton, and Des Moines.   The statewide average snowfall was 2.0 inches for the month, or 2.8 inches less than normal.   This ranks as the 23rd lowest March total among 123 years of record.   However, the meager March snowfall was enough to push the statewide average seasonal snowfall total to 47.7 inches or seventh highest of record (greatest total since 1978-1979 winter).  With the month starting out with an extensive snow and water content in the snow pack of 2 to as much as 10 inches, flooding was a virtual certainty across the CWA.  Due to the slow melt, most rivers crested on the high end of moderate intensity, with a few reaching major flood stage.  Initially, ice jamming was a significant problem.  Some of the worst occurred on the Raccoon west of Des Moines, resulting in flash flooding.  Flash flooding also occurred in the the Fort Dodge area with considerable flooding in the town of Fort Dodge.  Scattered ice jams also occurred in north central and northwest Iowa in the Mason City and Estherville areas.  Much of the flooding was to agricultural lands for much of the period, however cities along the rivers were inundated by the flood waters.  It was fortunate that heavy rainfall did not occur during the melting process.  Much of Iowa avoided what could have been a major disaster.
##                                                                                                                                                                                                                                                                                                                                                      EVENT_NARRATIVE
## 1                                                                                                                                                                                                                                                     Heat index values at the Nashua Boire Field (KASH) Automated Weather Observing System were 100 to 104 degrees.
## 2                                                                                                                                                                                                                                                                                                      Four to eight inches fell across eastern Hillsborough County.
## 3                                                                                                                                       In Manchester, firefighters responded to about 50 calls of downed trees and limbs that brought down power lines.  A tree fell onto a horse on a farm on Speare Road in Hudson.  A tree was downed onto a house in Merrimack.
## 4                                                                                                                                                                                Heat index values at the Manchester Airport (KMHT) Automated Surface Observing System and the Nashua Boire Field (KASH) Automated Weather Observing System were 100 to 104 degrees.
## 5                                                                                                                                                                                                                                                                                    Snowfall totals of 6 to 10 inches were observed in eastern Hillsborough County.
## 6  The Automated Surface Observing System at Manchester Airport (KMHT) recorded sustained wind speeds of 43 mph and gusts to 63 mph.  The wind downed numerous trees and wires across Bedford.  In addition, a scout camp near Manchester (Camp Carpenter) reported around 200 downed trees on its property.  They also sustained damage to many of their buildings.
## 7                                                                                                                               Five to eight inches of snow fell across eastern Hillsborough County.  Snow began in earnest around the afternoon rush hour.  This resulted in roads that were difficult to plow and thus slippery, resulting in numerous accidents.
## 8            A 42 year old man travelling on Interstate 93 in Manchester was killed when his car was struck by a 70 foot pine tree.  Two passengers, the man's 36 year old wife and 2 year old daughter, were injured and were transported to the hospital.  Saturated soil and strong winds combined, resulting in many downed trees across southern New Hampshire.
## 9                                                                                                                                                                                                                                                                                     Carnley Road near Century, Florida closed due to high water covering the road.
## 10                                                                                                                                                                                                                                                                                                                                                                  
##    DATA_SOURCE
## 1          CSV
## 2          CSV
## 3          CSV
## 4          CSV
## 5          CSV
## 6          CSV
## 7          CSV
## 8          CSV
## 9          CSV
## 10         CSV
```

```r
# Output the names of the columns
names(stormdata)
```

```
##  [1] "BEGIN_YEARMONTH"    "BEGIN_DAY"          "BEGIN_TIME"        
##  [4] "END_YEARMONTH"      "END_DAY"            "END_TIME"          
##  [7] "EPISODE_ID"         "EVENT_ID"           "STATE"             
## [10] "STATE_FIPS"         "YEAR"               "MONTH_NAME"        
## [13] "EVENT_TYPE"         "CZ_TYPE"            "CZ_FIPS"           
## [16] "CZ_NAME"            "WFO"                "BEGIN_DATE_TIME"   
## [19] "CZ_TIMEZONE"        "END_DATE_TIME"      "INJURIES_DIRECT"   
## [22] "INJURIES_INDIRECT"  "DEATHS_DIRECT"      "DEATHS_INDIRECT"   
## [25] "DAMAGE_PROPERTY"    "DAMAGE_CROPS"       "SOURCE"            
## [28] "MAGNITUDE"          "MAGNITUDE_TYPE"     "FLOOD_CAUSE"       
## [31] "CATEGORY"           "TOR_F_SCALE"        "TOR_LENGTH"        
## [34] "TOR_WIDTH"          "TOR_OTHER_WFO"      "TOR_OTHER_CZ_STATE"
## [37] "TOR_OTHER_CZ_FIPS"  "TOR_OTHER_CZ_NAME"  "BEGIN_RANGE"       
## [40] "BEGIN_AZIMUTH"      "BEGIN_LOCATION"     "END_RANGE"         
## [43] "END_AZIMUTH"        "END_LOCATION"       "BEGIN_LAT"         
## [46] "BEGIN_LON"          "END_LAT"            "END_LON"           
## [49] "EPISODE_NARRATIVE"  "EVENT_NARRATIVE"    "DATA_SOURCE"
```

```r
# List the structure of the file
str(stormdata)
```

```
## 'data.frame':	266381 obs. of  51 variables:
##  $ BEGIN_YEARMONTH   : int  201007 201001 201010 201007 201012 201002 201002 201003 201001 201003 ...
##  $ BEGIN_DAY         : int  7 17 1 6 26 25 16 14 21 11 ...
##  $ BEGIN_TIME        : int  1251 2300 830 951 1700 2305 1200 1345 300 1816 ...
##  $ END_YEARMONTH     : int  201007 201001 201010 201007 201012 201002 201002 201003 201001 201003 ...
##  $ END_DAY           : int  7 18 1 6 27 26 17 14 21 16 ...
##  $ END_TIME          : int  1630 1500 1000 1830 1800 38 0 1415 600 830 ...
##  $ EPISODE_ID        : int  43850 36500 44854 43850 46989 37004 36944 37397 36775 36906 ...
##  $ EVENT_ID          : int  254780 211550 260014 254779 273769 215305 214918 218778 213816 214762 ...
##  $ STATE             : Factor w/ 68 levels "ALABAMA","ALASKA",..: 44 44 44 44 44 44 44 44 14 24 ...
##  $ STATE_FIPS        : int  33 33 33 33 33 33 33 33 12 19 ...
##  $ YEAR              : int  2010 2010 2010 2010 2010 2010 2010 2010 2010 2010 ...
##  $ MONTH_NAME        : Factor w/ 12 levels "April","August",..: 6 5 11 6 3 4 4 8 5 8 ...
##  $ EVENT_TYPE        : Factor w/ 52 levels "Astronomical Low Tide",..: 19 21 38 19 45 23 21 38 13 14 ...
##  $ CZ_TYPE           : Factor w/ 2 levels "C","Z": 2 2 2 2 2 2 2 2 1 1 ...
##  $ CZ_FIPS           : int  12 12 12 12 12 12 12 12 33 23 ...
##  $ CZ_NAME           : Factor w/ 3236 levels "5NM E OF FAIRPORT MI TO ROCK ISLAND PASSAGE",..: 768 768 768 768 768 768 768 768 842 276 ...
##  $ WFO               : Factor w/ 123 levels "ABQ","ABR","AFC",..: 17 17 17 17 17 17 17 17 84 31 ...
##  $ BEGIN_DATE_TIME   : Factor w/ 131148 levels "01-APR-10 00:00:00",..: 5984 15618 902 5138 26395 25111 14658 12966 19826 9590 ...
##  $ CZ_TIMEZONE       : Factor w/ 9 levels "AKST-9","AST-4",..: 4 4 4 4 4 4 4 4 3 3 ...
##  $ END_DATE_TIME     : Factor w/ 129414 levels "01-APR-10 06:52:00",..: 5805 16370 830 5037 27258 25890 15240 12647 19363 14708 ...
##  $ INJURIES_DIRECT   : int  0 0 0 0 0 0 0 2 0 0 ...
##  $ INJURIES_INDIRECT : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ DEATHS_DIRECT     : int  0 0 0 0 0 0 0 1 0 0 ...
##  $ DEATHS_INDIRECT   : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ DAMAGE_PROPERTY   : Factor w/ 881 levels "","0.00K","0.01K",..: 2 2 286 2 2 129 2 50 2 286 ...
##  $ DAMAGE_CROPS      : Factor w/ 307 levels "","0.00K","0.01K",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ SOURCE            : Factor w/ 41 levels "Airplane Pilot",..: 4 10 20 3 34 3 34 20 12 23 ...
##  $ MAGNITUDE         : num  NA NA 45 NA NA 55 NA 35 NA NA ...
##  $ MAGNITUDE_TYPE    : Factor w/ 5 levels "","EG","ES","MG",..: 1 1 2 1 1 4 1 2 1 1 ...
##  $ FLOOD_CAUSE       : Factor w/ 8 levels "","Dam / Levee Break",..: 1 1 1 1 1 1 1 1 3 5 ...
##  $ CATEGORY          : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ TOR_F_SCALE       : Factor w/ 7 levels "","EF0","EF1",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ TOR_LENGTH        : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ TOR_WIDTH         : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ TOR_OTHER_WFO     : Factor w/ 78 levels "","ABR","AKQ",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ TOR_OTHER_CZ_STATE: Factor w/ 36 levels "","AL","AR","FL",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ TOR_OTHER_CZ_FIPS : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ TOR_OTHER_CZ_NAME : Factor w/ 422 levels "","ALLEN","ATOKA",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ BEGIN_RANGE       : int  NA NA NA NA NA NA NA NA 2 3 ...
##  $ BEGIN_AZIMUTH     : Factor w/ 17 levels "","E","ENE","ESE",..: 1 1 1 1 1 1 1 1 3 4 ...
##  $ BEGIN_LOCATION    : Factor w/ 30745 levels "","(0E4)PAYSON ARPT",..: 1 1 1 1 1 1 1 1 12382 9365 ...
##  $ END_RANGE         : int  NA NA NA NA NA NA NA NA 2 2 ...
##  $ END_AZIMUTH       : Factor w/ 17 levels "","E","ENE","ESE",..: 1 1 1 1 1 1 1 1 3 6 ...
##  $ END_LOCATION      : Factor w/ 31174 levels "","(0E4)PAYSON ARPT",..: 1 1 1 1 1 1 1 1 12552 10352 ...
##  $ BEGIN_LAT         : num  NA NA NA NA NA ...
##  $ BEGIN_LON         : num  NA NA NA NA NA ...
##  $ END_LAT           : num  NA NA NA NA NA ...
##  $ END_LON           : num  NA NA NA NA NA ...
##  $ EPISODE_NARRATIVE : Factor w/ 39302 levels "A 1-year-old girl died after being left in a hot car in Kingsville for about 45 minutes.  The temperature outside the vehicle w"| __truncated__,..: 2892 229 7546 2892 2582 6522 6575 2455 6980 6172 ...
##  $ EVENT_NARRATIVE   : Factor w/ 164523 levels "","A 1.5-foot diameter healthy tree was snapped.",..: 13304 11959 15378 13301 27321 29279 11267 173 9743 1 ...
##  $ DATA_SOURCE       : Factor w/ 1 level "CSV": 1 1 1 1 1 1 1 1 1 1 ...
```




