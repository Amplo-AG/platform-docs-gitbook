# Model Monitoring

Monitoring is always key to identify performance issues and understanding behavior. This is no exception to the machine learning services available on the platform.

![](<../.gitbook/assets/image (1).png>)

### Identifying problems

When you go to `Models` > `Monitoring`, you see various plots. It detects and visualizes data drift and data quality issues directly, so that if your training data differs wildly from your production (real-world) data, it is observed. Senior service engineers are notified, so that they can identify where the differences are and adapt the training data to minimize the differences.

#### Data Quality

The first four graphs show you the prediction distribution & times, as well as the outliers & missing values in the data. The missing values & outliers should be a flat line. It is a bad sign when those numbers are increasing, as it means you have increasingly more missing data and extremer values. Ideally both numbers are low, close to or actually zero.

#### Data Drift

In the feature distribution figures, you see two lines (orange & blue) per feature. These are probability density functions. The X-axis shows the values that are collected of this feature, both in production (blue) and training (orange). The Y-axis indicates the probability of a certain value. So the higher the graph at a certain value, the more this value has been measured.

Whenever the training data is different from the real-world data, it becomes very difficult for a model to make correct predictions. Imagine a model that predicts ice cream sales in winter which is trained only on summer data. That is probably not going to go very well. But determining whether you are doing this is rather difficult when your machine has 1.000+ signals and your models use 30+. And that is why Amplo developed the monitoring solution.
