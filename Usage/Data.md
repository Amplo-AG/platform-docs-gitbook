# Data Overview

All your data logs are organised by :doc:`../Services` and Incidents. 

.. image:: ../../images/data_overview.PNG
  :width: 600
  :alt: Amplo

## Types of Data
The different Machine Learning Services require slightly different types of data. 
:ref:`ref-automated-diagnostics` requires short logs of 5 seconds. Note that the log 
itself can be longer, in which case the incident interval has to be specified or analysed. 

:ref:`ref-predictive-maintenance` requires longer logs of a few days to months, dependent
on the degradation curve. 

Lastly, :ref:`ref-anomaly-detection` requires does not require any incident data but the more
measurement data from healthy machines. 

## Creating Training Data
Training data be acquired through :ref:`ref-cloud-storage` and by manually uploading logs. 
You can upload logs in the respective folder by dropping the files or clicking on the incident folder
and pressing the upload button there. 