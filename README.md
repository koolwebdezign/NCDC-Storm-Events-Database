# NCDC Storm Events Database Analysis

## Get and Clean Data from DATA.gov

I am an MBA student at Bridgewater State University, enrolled in MGMT582 - Business Intelligence/Analytics.  This data exploration project is intended to be my final project and in-class presentation.  The goal of this project is to gain practical working experience with R Studio and to apply some data analysis techniques in which we have been introduced within this course.

I have chosen to work with the NCDC Storm Events Database available at DATA.gov.  For more details related to this raw data set, see <https://catalog.data.gov/dataset/ncdc-storm-events-database>.

My first step in preparing my analysis will be to write a script to get the data at Data.gov.  After following instructions about the storm events database, I found that the raw data sits in zipped files located on an FTP server at <ftp://ftp.ncdc.noaa.gov/pub/data/swdi/stormevents/csvfiles/>.  I am going to attempt to write a script that will automatically get this data from its FTP location, unzip each file, and then append it to create a larger file that can be analyzed here with R Studio.

## Initial Table Exploration and Observation

The script that gets and cleans the data successfully assembles 10+ years of data from the NCDC Storm Events Database from calendar year 2006 through summer of 2016.  We have eliminated the long text fields related to the description of the storm events.  We did this in order to produce a table size which is manageable on our desktop computers.  Elimination of these text fields dropped the file size to nearly 25% of its original size.  Our final CSV file is appx 193MB in size and it contains 656K observations of 50 variables.

With this compiled data set, we can conduct some exploration techniques as outlined on RDatamining.com.  We will follow the examples put forth on the following page in order to begin our exploration of this significant data file.

<http://www.rdatamining.com/examples/exploration>

## Table Sub-Setting

The exercises in the *Initial Table Exploration and Observation* section allowed us to conduct some basic observations about the data set as a whole.  This is helpful yet when you browse the data and to gain a general understanding of the table content.  Now you may want to extract data into a subset (ie. to extract data by eliminating additional columns or to even extract information based on specific observations.)  Sub-setting is similar to strategies whereby one might execute a SQL SELECT statement from a database.  The difference here now with R is that our main data frame is sitting in memory.  We can now take pieces of this data set and conduct separate analysis.

