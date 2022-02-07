# Workflow

Before we start using the platform, we want to explain the general workflow. 
This consist of:

- :doc:`Data`
- :doc:`Labelling`
- :doc:`Training`

Data is at the heart of machine learning, and therefore at the heart of this Smart Maintenance Platform. 
Even when you made the necessary :ref:`ref-data-integration`, the :doc:`Data` keeps track of your training 
data. You can inspect your training data here, keep it organised by incident and see all made changes. 
The moment of failure or failure degradation is noted with data labelling. The :doc:`Labelling` helps you with this. 
It allows you to mark intervals of incident occurrence, mark remaining useful lifetimes and the useful features. 
It also closes provides a feedback loop so newly occurring incidents are used to enrich the training data. 
With a labelled and organised training data set, you can start training models in the :doc:`Training`. 
We made this as easy as possible, but it will a bit of time to run! 
Once completed, you can see the results and all other information in the :doc:`Models`. You can directly use the different 
services on the platform and over APIs!

Additionally, to this workflow, there are some more advanced topics which you may want to use:

- :doc:`../Advanced/Preprocessor.rst`
- :doc:`../Advanced/Permissions.rst`
- :doc:`../Advanced/APIs.rst`