# Exploring

This assignment uses data from the UC Irvine Machine Learning Repository, a popular repository for machine learning datasets. In particular, we will be using the “Individual household electric power consumption Data Set” which I have made available on the course web site:

Dataset: Electric power consumption [20Mb]
Description: Measurements of electric power consumption in one household with a one-minute sampling rate over a period of almost 4 years. Different electrical quantities and some sub-metering values are available.


          # First plot

          library("data.table")

        setwd("~/Desktop/datasciencecoursera/4_Exploratory_Data_Analysis/project/data")

        powerDT <- data.table::fread(input = "household_power_consumption.txt" #Reads in data from file then subsets data for specified dates
                                     , na.strings="?"
                                     )

        powerDT[, Global_active_power := lapply(.SD, as.numeric), .SDcols = c("Global_active_power")] # Prevents histogram from printing in scientific notation


        powerDT[, Date := lapply(.SD, as.Date, "%d/%m/%Y"), .SDcols = c("Date")]  # Change Date Column to Date Type


        powerDT <- powerDT[(Date >= "2007-02-01") & (Date <= "2007-02-02")] # Filter Dates 
        png("plot1.png", width=480, height=480)


        hist(powerDT[, Global_active_power], main="Global Active Power", 
             xlab="Global Active Power (kilowatts)", ylab="Frequency", col="Red") # Plot 1

        dev.off()
        
        
        
          # Plot 2       
          library("data.table")

          setwd("~/Desktop/datasciencecoursera/4_Exploratory_Data_Analysis/project/data")


          powerDT <- data.table::fread(input = "household_power_consumption.txt"          #Reads the data 
                                       , na.strings="?"
          )

          powerDT[, Global_active_power := lapply(.SD, as.numeric), .SDcols = c("Global_active_power")]


          powerDT[, dateTime := as.POSIXct(paste(Date, Time), format = "%d/%m/%Y %H:%M:%S")]  # Making the filtered and graphed by time of day


          powerDT <- powerDT[(dateTime >= "2007-02-01") & (dateTime < "2007-02-03")]  # Filter Dates 

          png("plot2.png", width=480, height=480)


          plot(x = powerDT[, dateTime]
               , y = powerDT[, Global_active_power]
               , type="l", xlab="", ylab="Global Active Power (kilowatts)")

          dev.off()
