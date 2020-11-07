Modules that Generate Heat Map for Traffic Density in Year x Month

```
        # heat map: Year x Month
        trafficHeatMap = function(data){
          years = sort(unique(data$S2_YEAR))
          months = sort(unique(data$S2_MONTH))
          heat.data <- matrix(rep(NA,length(years)*length(months)),nrow = length(years), ncol = length(months))

           for(i in 1:length(years)){
            for(j in 1:length(months)){
              if(length(subset(data, data$S2_YEAR==years[i] & data$S2_MONTH==months[j])$COUNTS)==0){
                heat.data[i,j] = 0  
              } else {

                heat.data[i,j] <- subset(data, S2_YEAR==years[i] & S2_MONTH==months[j])$COUNTS

              }

            }
          }

          image.plot(as.numeric(years), as.numeric(months), heat.data, 
                     axes=FALSE, xlab = "Year", ylab = "Month") /
          points(0, 0) /
          axis(side=1, at=as.numeric(years), labels = years, las=2, cex.axis=1.2) /
          axis(side=2, at=as.numeric(months), labels = months, las=1, cex.axis=1.2) /

          for(i in 1:length(years)){
            for(j in 1:length(months)){
              text(as.numeric(years)[i], as.numeric(months)[j], heat.data[i,j], col="white", cex=1.5)
            }
          } /
          title(paste("Traffic Heat Map by Year and Month in ", data$Airfield[1]))



        }


        # get heat data counts
        get_traffic_S2YEAR_S2MONTH_Airfield = function(data){

          fn$sqldf("
                SELECT
                Airfield,
                S2_YEAR,
                S2_MONTH,
                COUNT(*) COUNTS
                FROM 
                data
                WHERE Airfield = \"$airfield\" and S2_YEAR != \"2026\" and S2_YEAR != \"2030\"
                GROUP BY 
                Airfield, S2_YEAR, S2_MONTH
                ")


        }

        # generate heat map
        airfield = unique(unique_comb$Airfield)[1]
        traffic_S2YEAR_S2MONTH_AUC = get_traffic_S2YEAR_S2MONTH_Airfield(unique_comb)
        trafficHeatMap(traffic_S2YEAR_S2MONTH_AUC)
```
