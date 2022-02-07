# Training Models

At Amplo, we use a state of the art `AutoML Pipeline <https://github.com/Amplo-GmbH/AutoML>`_, 
specifically designed for small, unbalanced time series problems.
The power of this Machine Learning is fully integrated into the Smart Maintenance Platform and 
available to you with just a few clicks. 

.. image:: ../../images/Training.png
    :width: 600
    :alt: Training.png


## Starting a Training
To start a training, click `Training` in the menu and click on the `Start Training` button. 
It's as simple as selecting the model you'd like to start training and then sitting back. 
Our AutoML pipeline takes approximately 24 hours to complete the full optimization on our 
compute cluster. Once it's finished, the model is automatically deployed if certain accuracy
metrics are accomplished. 

## When to start a training
There's pretty much two reasons why you would start a training session:

- When you're new to the platform, just uploaded a bunch of incident data or organised data storages. 
- When the deployed model gets a outdated, or you have added/labelled ~5+ logs of incident data.
