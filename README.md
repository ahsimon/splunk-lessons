# Splunk Tutorial

## Prerequisites

Create a splunk account 

# Installation

1.  Download Splunk [Enterprise](https://www.splunk.com/en_us/download/splunk-enterprise.html) trial version
2.  
```sh 
wget -O splunk-9.1.1.tgz "https://download.splunk.com/products/splunk/releases/9.1.1/linux/splunk-9.1.1-64e843ea36b1-Linux-x86_64.tgz"
```

2. Install Splunk
```sh
tar xvf  splunk-9.1.1.tgz -C /opt
```

3.  Start Splunk and set user name / admin password
```sh 
cd /opt/splunk/bin
sudo ./splunk start --accept-license 
```

4. Download Splunk [tutorial data](https://docs.splunk.com/Documentation/Splunk/9.1.1/SearchTutorial/Systemrequirements) 
      
* [tutorialdata.zip](https://docs.splunk.com/images/Tutorial/tutorialdata.zip) *do not uncompress file
* [prices.csv.zip](https://docs.splunk.com/images/d/db/Prices.csv.zip) 


5. Import tutorialdata.zip in your local Splunk 
    
   
# SLP Queries

```sh
index=main
sourcetype=access_combined_wcookie
status != 200
| timechart span=5m count by categoryId
```

```sh
index=main
sourcetype=access_combined_wcookie
bytes > 3000 AND bytes < 3600
```

```sh
index=main
sourcetype=access_combined_wcookie
|  stats max(bytes) AS "Biggest Request",
         min(bytes) AS "Smallest Request"
         median(bytes) AS "Median Requ
         est",
         count AS "Total Request"
```

* Output from the left side is input for the right side

Logical expressions
```
index=main
sourcetype=access_combined_wcookie
|  stats count(eval(status=200 OR status = "4**")) AS "Internal Server Errors"
```   

```
index=main
sourcetype=access_combined_wcookie
| fieldsummary maxvals=10
```
**fieldsummary**: Generates statistical values for each field, and maxvals aggregates the maximum of different values for each field

```
index=main
sourcetype=access_combined_wcookie
| timechart span=1h avg(bytes) as "Response Size"
|  eventstats avg("Response Size")
```

**eventstats** Adds summary statistics to all search results

```
index=_internal log_level=WARN OR log_level=ERROR
| stats count by component
| sort count
| streamstats sum(count)
```

**streamstats** Adds summary statistics to all search results in a streaming manner
