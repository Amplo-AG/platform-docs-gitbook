# Workflow

Before we start using the platform, we want to explain the general workflow. 
This consist of:

## Training Data
To get started with the machine learning services, training data has to be organised. 
Connecting blob storages can help to quickly get a lot of unlabelled data on the platform, which 
then can easily be labelled on the platform. Organising good training data sets is critical for
successful machine learning projects. Amplo is thereby on the forefronts of [Data Centric AI](https://www.forbes.com/sites/gilpress/2021/06/16/andrew-ng-launches-a-campaign-for-data-centric-ai/).
The [Training Data Overview](Data.md) always shows your organised training data, and allows you to 
easily change it.

## Training Models
After (re)organising your training data, [Amplo's AutoML](https://github.com/Amplo-GmbH/AutoML) takes care 
of the model development. With one click, you can start the AutoML, and training results will await shortly. 
Read here more about [Model Training](Training.md).

## Labelling Predictions
Deployment of trained models happens automatically. Therefore, you can directly use the models in production. 
This creates either diagnoses or alerts. It is important to ensure the model is made aware whenever it is 
making incorrect predictions. Therefore, labelling alerts & diagnoses is important for the continuous improvement 
of the models. [Labelling Predictions](Labelling.md) tells you more about this. 

## Monitoring
Amplo comes with a built-in [Model Monitoring](Monitoring.md) solution. Feature distributions, 
prediction times, outliers & missing values are all monitored. Therefore, drifting data 
distributions or concept drift is detected early on. Amplo simply notifies you when and where
you have to add data to avoid the real world from differing too much from your training data.

## Integration
All the information on your data, models & predictions are available over an OpenAPI, and are therefore easily
integrated with any CRM, ERP or Ticket System. Read more about this in our [OpenAPI Documentation](https://platform.amplo.ch/openapi).
