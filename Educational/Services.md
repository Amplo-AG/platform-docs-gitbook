# Machine Learning Services

This section describes in more detail the requirements for data, setup & integration.

## Automated Diagnostics
Automated Diagnostics is a service dedicated to reduce Time To Diagnose & Time To Resolve. Instead of hours of data
analyses and field inspections, Machine Learning models can accurately determine the root cause of incidents and
directly instruct the optimal resolution.
This service is often the first to be implemented as it can be done manually, and therefore without intrusive
infrastructural setup. However, the manual approach is more time-consuming than the automatic setup, as will become
obvious from the explanation below.

### Training Data

To detect the root cause of a malfunctioning machine, machine learning models are trained on snapshots of data. These
snapshots are ideally short (~5 seconds) during which the failure occurred or its consequences are most noticeable.
If you are unable to extract the exact time interval of the occurred incident, try and limit the window of the snapshot
to 1 day. For longer logs, we internally analyse where in the snapshot the failure patterns are most present and filter
the rest.
The training data may contain as many sensor measurements and flags / status' as is available. See
[Custom Flags](../Advanced/Permissions.md#<Flags & Serial Number>) to find out how to extract additional information such as Serial Numbers.
If possible, having a timestamp to every data point is extremely useful. This allows for additional resampling
strategies.
Though it slightly depends on the complexity of the incident, a rule of thumb is that we need a minimum of 20-30
faulty snapshots per incident and 100+ snapshots of healthy data.

### Automatic Setup

Amplo can integrate with various cloud data storages, such as Google Cloud Storage, AWS S3 and Azure Blob Storage and
service ticket systems such as Salesforce and Freshdesk (see more at :ref:`ref-data-integration` and
:ref:`ref-ticket-integration`). If you chose to connect your data storage and ticket system, Amplo can extract all
necessary information to get an overview of frequently occurring incidents and collect the corresponding incident data.
Amplo uses all this information to create a training data set which is automatically used to train an Automated
Diagnostics model for every incident with more than 30 occurrences.

### Manual Setup

Even without structured data storage or ticket system you can do smarter maintenance!
Collect a list of incidents & occurrences with the corresponding measurement data. 
Upload it manually in the Data Overview and see the power of our AutoML! 

### Integration

The automated diagnostics service is fully available over APIs. This means that you can adapt your ticket system to send
a measurement log file to Amplo upon ticket creation. Within a few seconds, the API responds with our diagnosis. With
some customization you can visually show this diagnosis in your ticket system. For more information, see :doc:`APIs`.

.. note::
    This requires custom APIs and UI in your ticket system. Get in contact with your ticket system provider to see if
    this is possible. If you are considering ticket systems, Freshdesk is a modern ticket system that allows all
    required functionality.

.. _ref-anomaly-detection:

## Anomaly Detection

### Training Data

To create an accurate Anomaly Detection model, it is important to include as much variety as possible with measurement
data from healthy machines. That includes data from as many machines as possible, preferably for 6 - 12 months.
In total, we resample the data and make a operating map of approximately 1 million data points.

> **_NOTE:_** 
    Dependent on the complexity of your machine, it may be possible to start Anomaly Detection with less data.

### Setup

Aside the training data which need to be prepared, Anomaly Detection requires a continuous data connection to your
machines, which we call Data Streams (see :ref:`ref-data-integration`). The frequency of this data connection
is dependent on your use case. The trade-off here is that with a higher frequency, anomalies are spotted quicker, but it
also becomes more costly to process the additional bandwidth.

.. _ref-condition-monitoring:

### Output
Whenever Anomaly Detection detects an issue, all your Automated Diagnostics models are automatically run on the anomaly. 
Hopefully, this will directly provide you with the resolution. If it's however a new type of incident and diagnostics
don't detect an issue, the Anomaly will be left for Service Engineers to be labelled. 

## Condition Monitoring

### Setup

Condition Monitoring is a rule based approach. Therefore, this setup does not require any collection of training data.
Instead, your Service Engineers can create a set of rules.

```
1. IF ( [Oil Pressure] < [800] AND [Oil Age] < [365 days] FOR [600 seconds]  ) { Oil Leak }
2. IF ( VARIANCE [Vibration] > [300] AND [Gearbox Temperature] > [150] FOR [15 seconds] ) { Bearing Failure }
3. IF ( [Error4] = 186 AND [DC Voltage] < 700 ) { Insulation Failure }
4. ...
```

You can define the thresholds and statements yourself, which may depend on the measured data and / or machine states
& flags.

Additionally, similar to Anomaly Detection, Condition Monitoring requires connected Data Streams.

.. _ref-predictive-maintenance:

## Predictive Maintenance

### Training Data
Similar to Automated Diagnostics, Predictive Maintenance directly predicts the root cause of an incident and the
appropriate resolution. Contrary to Automated Diagnostics, Predictive Maintenance predicts incidents days or weeks
before they occur. To detect patterns this early on, it's important to have longer and more snapshots for the training
data. Though dependent on the complexity and degradation curve of the incident, we can train models with 30-50 logs
containing data of a few days to a few months. It is important that the log contains at least the full degradation
curve, and that this degradation curve is properly labelled.

### Labelling
As Predictive Maintenance predicts not only the probability of an upcoming incident, but also the duration until the
incident is most-likely to occur, it is important that the degradation curve is labelled accordingly. This degradation
curve has a starting and end point and ranges from 0 - 100 %.
Every snapshot that is collected in the training data of a Predictive Maintenance model, it is important to label this
degradation curve properly, whether it's an linear, exponential, or step function.

### Setup
Similar to Anomaly Detection and Condition Monitoring, Predictive Maintenance requires connected Data Streams.