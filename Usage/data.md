When you go to `Data` > `Training Data`, you have a nice overview of all your organised 
training data. This goes by four categories, `Diagnostics`, `Predictive`, `Healthy` and `Unlabelled`.

![](.gitbook/assets/Data.png)

## Types of Data
The different Machine Learning Services require slightly different types of data.
Note that [Condition Monitoring](../Educational/Services.md#condition-monitoring) does not
require any training data, and that [Anomaly Detection](../Educational/Services.md#anomaly-detection)
requires solely [Healthy](#healthy) or [Unlabelled](#unlabelled) data. 

### Diagnostics
[Automated Diagnostics](../Educational/Services.md#automated-diagnostics) requires short logs of a few 100ms 
to maximum 30 seconds. The purpose of Automated Diagnostics is to determine the state of the machine, 
and identify faulty subsystems. Therefore, when labelling data, you only have to mark the interval 
where the issue is present.

### Predictive
[Predictive Maintenance](../Educational/Services.md#predictive-maintenance) requires longer logs. 
If you want to predict an incident three months before occurring, your logs need to contain at least
3 months of data. 
When labelling this data, you mark the degradation of the incident. This gives the state of health
as well as time to failure to the models.  

> **_NOTE:_** 
> Not all incidents are detectable three months in advance. Some incidents escalate very quickly, 
> and are therefore only recognisable shortly before the moment of failure. 

### Healthy
To get a complete overview of all your machine's operating data, healthy data is necessary to be
included as well. This can be arbitrarily much, as generally healthy data is easier to acquire. 
The healthy data is used to in Automated Diagnostics, Predictive Maintenance and for [Anomaly Detection](../Educational/Services.md#anomaly-detection).
Your Healthy & Unlabelled data is especially important for Anomaly Detection. This data is used
to build a map of what is ordinary operating data for your machine, and everything extraordinary
is flagged as an anomaly. Better to include a bit too much than a bit too less and deal with false positives!

### Unlabelled
Often, it can be a tedious task to collect enough training data. To help you in this process, 
you can also use large data dumps which are not labelled yet. With Amplo's data visualizations
and Interactive Labeller, you can efficiently find the interesting data that needs labelling. 
This is often orders of magnitude faster than searching in your own ERP or Data Lake. 
Additionally, unlabelled data is also used for Anomaly Detection.

## Creating Training Data
Currently, there are three ways to connect your data to the platform. 
1. Manual file upload
2. Connecting [blob storages](../Getting_Started/Setup.md#storage-connections) (data lakes)
3. Connecting [data streams](../Getting_Started/Setup.md#data-stream-connections) (live data)