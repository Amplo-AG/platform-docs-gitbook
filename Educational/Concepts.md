In case you're not familiar with Machine Learning, this page concisely describes the
main concepts and terminology you should understand.

## Machine Learning
The different [Machine Learning Services](Services.md) all rely on the same principles. 
Conceptually they rely on the following aspects:

### Model
A Machine Learning Model is a computer model that (once trained) mimics the relation between certain inputs and
outputs. In our case, we train it so that based on sensor and log data of your machines, we can predict for example
when a specific failure is going to happen. A model represents what is learned from your data. For Automated
Diagnostics and Predictive maintenance, there is always *one model per incident* (see [Platform Setup](../Getting_Started/Setup.md)).

### Training Data
Machine Learning models are able to predict specific incidents by learning from examples. Therefore, to train a
model which is able to predict whether a specific incident is going to happen two weeks from now, we need
approximately 20-30 examples to learn from. These examples may contain sensor measurements such as temperatures,
voltages and pressures, but also machine states, flags, errors or other event logs. In [Machine Learning Services](Services.md)
we explain the requirements for these examples more in-depth. These examples together is what we call training data.

### Labelling
A very important aspect of training data is that it's labelled. That means that for each example in the training
data, the model knows exactly which incident occurred. Only then can a model try and see what makes those examples different from
the rest, which is then used in the future to detect and predict these incidents. To make sure that models remain
predicting well, labelling is a continuous process. Every new prediction that is labelled goes into the training data,
so that you can iteratively update the models with all those new examples!

### Automated Machine Learning
Automated Machine Learning, or AutoML, is an optimization pipeline that automatically designs models for a specific
training data set. It uses structured data cleaning and feature engineering steps, tries various types of models
and optimizes the hyperparameters (model settings) over a predefined search space. We have dedicated a whole section
about the details of how we tailored our AutoML for perfect models.

## Services
See [Machine Learning Services](Services.md) for a more detailed overview of the data & setup requirements and use cases.
This section provides a concise introduction into the various predictive maintenance services the platform
offers.

### Automated Diagnostics
Many Service Engineers and Field Technicians spend hours if not days to identify the root cause of a malfunctioning
machine. To avoid this, Automated Diagnostics analyses a snapshot of measurement data of a malfunctioning machine
to determine the root cause or resolution within seconds.

### Anomaly Detection
Most machine failures start with undetected anomalies. Detecting anomalies early on allows us to place special attention to
those machines that show unexpected behavior and resolve problems earlier. Anomaly Detection constantly monitors and
flags data that falls outside of normal operating points. When anomalies are detected, we use Automated Diagnostics
models to try and identify the anomaly.

### Condition Monitoring
Service Engineers often have expert knowledge of their machines. For the cases which are expressible with
if/else rules, Condition Monitoring is the go-to strategy. Here we don't use machine learning, but rely fully on
the Service Engineers' knowledge. We also use these rules to make the labelling process easier.

### Predictive Maintenance
To avoid machine downtime and to give Service Engineers maximum time to organise maintenance, Predictive Maintenance
constantly looks for failure patterns of specific incidents in your machine data. Often, these can be found weeks
before the machine becomes critical.


