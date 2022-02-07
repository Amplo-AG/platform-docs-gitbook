Though all your measurement data is organised by :doc:`../Services` and Incident, 
some log files require some further attention before you can start :doc:`Training` models:

- Long :ref:`ref-automated-diagnostics` logs (>5s)
- :ref:`ref-predictive-maintenance` logs for degradation curves

.. image:: ../../images/Labelling.png
  :width: 600
  :alt: Labelling.png

## Labelling Automated Diagnostics Logs
When Automated Diagnostics logs are above 5 seconds, it's often helpfull to mark the interval where the failure 
patterns are most obvious in the data. When you mark the failure, all unmarked data will be disregarded when 
you train the corresponding model. 
You can open the label interface shown in the picture below from the :doc:`Data` or from a :ref:`ref-labeljobs`. 

- Use the select bar above the graph to (de)select the columns shown in the graph. 
- Use the brush to scroll through the timeline of the log. 
- Double-click on the graph to show the entire timeline. 
- Hold shift and drag from left to right to mark a faulty interval. 
- With the top left 

Always ensure to capture the incident interval where the incident is most obvious from the data!

.. image:: ../../images/Failure.png
  :width: 600
  :alt: Failure.png


## Labelling Predictive Maintenance Logs
Predictive Maintenance predicts not only the probability of a specific incident, also the timing of when such incident is most-likely to occur. 
In order to do so, we need to train the models how far away the incident is at any given point in time. 
Therefore, it's important to label the deterioration from start to end. Instead of marking the incident interval, 
you can drag the deterioration from 0% to 100% over the timeline. 

.. _ref-labeljobs:

## Label Jobs
At the point a ML service makes a prediction, the true state of your machine is generally unknown. 
To ensure optimal accuracies, it's very important to provide feedback to the ML models about their predictions. 
That's why it's possible to label the predictions, which happens in a two step approach. 

1. The ticket owner, who sends out the work order and resolves the incident, labels the prediction once the incident is confirmed. 
  Upon labelling the prediction, a `Label Job` is created. 
2. A data labeller (often a Senior Service Engineer), gets assigned to verify and confirm the Label Job. Here, he/she can mark 
  the interval for Automated Diagnostics Tickets or the Incident Deteriation for Predictive Maintenance Tickets. Once the labeller
  approves the Label Job, the measurement data is added into the training data. 

> **_NOTE:_** 
  Only after the Label Job is approved, will the measurement data enter the training data!