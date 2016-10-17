# NCDC-Storm-Events-Database

I am an MBA student at Bridgewater State University, enrolled in MGMT582 - Business Intelligence/Analytics.  This data exploration project is intended to be my final project and in-class presentation.  The goal of this project is to gain practical working experience with R Studio and to apply some data analysis techniques in which we have been introduced within this course.

I have chosen to work with the NCDC Storm Events Database available at DATA.gov.  For more details related to this raw data set, see <https://catalog.data.gov/dataset/ncdc-storm-events-database>.

My first step in preparing my analysis will be to write a script to get the data at Data.gov.  After following instructions about the storm events database, I found that the raw data sits in zipped files located on an FTP server at <ftp://ftp.ncdc.noaa.gov/pub/data/swdi/stormevents/csvfiles/>.  I am going to attempt to write a script that will automatically get this data from its FTP location, unzip each file, and then append it to create a larger file that can be analyzed here with R Studio.

