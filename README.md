# Mapping
#Texas


sales_2016 <- read.csv("Texas Sales 2016 by city.csv")
sales_2017 <- read.csv("Texas Sales 2017 by city.csv")

sales_2016$NetSales <- sales_2016$NET_SALES16
sales_2017$NetSales <- sales_2017$NET_SALES17
#stack two tables

sales_2017 <- sales_2017[, -4]
head(sales_2017)
sales_2016 <- sales_2016[,-4]
sales_2016
head(sales_2016)

sales<-rbind(sales_2016, sales_2017)
sales
sales$order_dt_Chr <- as.character(sales$ORDER_DT_KEY)
head(sales$order_dt_Chr)
sales$Year <- as.numeric(substr(sales$order_dt_Chr,1,4))
sales$Year

head(sales)
sales <- sales[,-5]
head(sales)

str(sales)
table(sales$CITY_NAME)

#read in storm data
install.packages("tidyverse")
install.packages("plyr")
library(tidyverse)

# Creates storms data set from NOAA Atlantic Hurricane data, which is provided
# in an unorthodox format: a csv that alternates between header/identifier rows
# and data rows.

# Read in data set so each line is a character string
storm_strings <- read_lines("http://www.nhc.noaa.gov/data/hurdat/hurdat2-2000-2017-083017.txt")

install.packages("maps")
library(maps)
map.cities(us.cities, country = "Texas")

sales_city<-as.data.frame(table(sales$CITY_NAME))
head(sales_city)

install.packages("ggmap")
library(ggmap)
sales_city$Cities <- as.character(sales_city$Var1)
lonlat <- geocode(unique(sales_city$Cities))

map<-map.cities(x=us.cities, country = "TX", orientation = c(latitude, longitude, rotaiton))
data(us.cities)
head(us.cities)
cities1 <- subset(us.cities, country.etc = "TX")
table(us.cities$country.etc)
cities

library(dplyr)
gorup<- sales %>%
  group_by(CITY_NAME)
  summarize(sum(NetSales))
  
  
install.packages("sqldf")
library(sqldf)

importance<-sqldf("select sum(NetSales) as sales, count(distinct order_dt_key), CITY_NAME from
        sales group by CITY_NAME
        order by sales")

map <- get_map(location = "texas", zoom =6, source = "google")
map_houston <- get_map("houston, texas")
ggmap(map_houston, extent="normal")
ggmap(map, extent = "normal")

#https://stackoverflow.com/questions/20936840/r-get-longitude-latitude-data-for-cities-and-add-it-to-my-dataframe
