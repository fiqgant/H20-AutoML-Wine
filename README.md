# H20-AutoML-Wine
Learn how to use AutoML to build and tune machine learning models in Python using the H2O.ai library and the wine dataset. This tutorial provides code examples and plots to help you understand how to streamline your machine learning workflow with AutoML.

# [Article](https://www.fiqlab.dev/blog/automl_h2o)

## Step 1: Install the H2O.ai library

The first step is to install the H2O.ai library. You can do this by running the following command:

```bash
pip install h2o
```

## Step 2: Import the library and start an H2O cluster

Next, you need to import the H2O.ai library and start an H2O cluster. You can do this by running the following code:

```Python
import h2o
h2o.init()
```

## Step 3: Load the wine dataset

Next, you need to load the wine dataset into an H2O frame. You can do this using the import_file function, like so:

```Python
# Load the wine dataset into an H2O frame
data = h2o.import_file("data/wine.csv")
```

## Step 4: Split the data into training and testing sets

Next, you need to split the data into training and testing sets. This is typically done using a 80/20 or 70/30 split, with the larger portion being used for training and the smaller portion being used for testing. You can do this using the split_frame function, like so:

```Python
# Split the data into 70% training and 30% testing
train, test = data.split_frame(ratios=[0.7, 0.3])
```

## Step 5: Build and tune a machine learning model using AutoML

Now it's time to build and tune a machine learning model using AutoML. You can do this using the H2OAutoML function, like so:

```Python
# Build and tune a machine learning model using AutoML
# Specify the maximum runtime as 30 seconds and the number of models to build as 5
aml = h2o.automl.H2OAutoML(max_runtime_secs=30, nfolds=5, seed=1)
aml.train(y='target_column', training_frame=train)
```

```bash:Output
Model Details
=============
H2OStackedEnsembleEstimator : Stacked Ensemble
Model Key: StackedEnsemble_BestOfFamily_4_AutoML_1_20221222_171530

No summary for this model
ModelMetricsRegression: stackedensemble
** Reported on train data. **

MSE: 0.06451856497863058
RMSE: 0.2540050491203484
MAE: 0.0677302226971585
RMSLE: 0.10289310387988594
Mean Residual Deviance: 0.06451856497863058
ModelMetricsRegression: stackedensemble
** Reported on cross-validation data. **

MSE: 0.018692892195425935
RMSE: 0.1367219521343443
MAE: 0.040627865678815066
RMSLE: 0.04633239961799816
Mean Residual Deviance: 0.018692892195425935
Cross-Validation Metrics Summary:
mean	sd	cv_1_valid	cv_2_valid	cv_3_valid	cv_4_valid	cv_5_valid
mae	0.0355645	0.0304106	0.0198031	0.0721367	0.0209310	0.0631356	0.0018161
mean_residual_deviance	0.0160914	0.0163070	0.0072111	0.0344280	0.0058413	0.0329687	0.0000079
mse	0.0160914	0.0163070	0.0072111	0.0344280	0.0058413	0.0329687	0.0000079
r2	0.969596	0.0315619	0.9845737	0.9310247	0.9919707	0.9404468	0.9999643
residual_deviance	0.0160914	0.0163070	0.0072111	0.0344280	0.0058413	0.0329687	0.0000079
rmse	0.1062566	0.0774672	0.0849184	0.1855478	0.0764283	0.1815727	0.0028157
rmsle	0.0339555	0.0273807	0.0264773	0.0716957	0.0199961	0.0502053	0.0014033

[tips]
Use `model.explain()` to inspect the model.
--
Use `h2o.display.toggle_user_tips()` to switch on/off this section.
```

## Step 6: Plot the top 10 best performing algorithms

You can use the leaderboard attribute of the H2OAutoML object to view the performance of each model, and then plot the top 10 best performing algorithms using a library like matplotlib.

First, you will need to install matplotlib by running the following command:

```bash
pip install matplotlib
```

Then, you can use the following code to plot the top 10 best performing algorithms:

```Python
import matplotlib.pyplot as plt

# View the leaderboard
lb = aml.leaderboard

# Extract the top 10 models
top_models = lb.as_data_frame().iloc[:10, :]

# Extract the model names and mean residual deviances
model_names = top_models['model_id'].tolist()
mean_residual_deviances = top_models['mean_residual_deviance'].tolist()

# Set the figure size
plt.figure(figsize=(10, 5))

# Plot the results
plt.bar(model_names, mean_residual_deviances)
plt.ylabel('Mean Residual Deviance')
plt.title('Performance of the Top 10 Algorithms')

# Adjust the x-axis tick labels
plt.xticks(rotation=90)

plt.show()
```

This code will extract the top 10 models from the leaderboard, extract the model names and mean residual deviances, and then plot a bar chart showing the performance of the top 10 algorithms. It will also set the figure size to be larger, and rotate the x-axis tick labels by 90 degrees to make them easier to read.

<div className="grid place-items-center">
  <img
    className="inline rounded-lg"
    src="https://raw.githubusercontent.com/fiqgant/fiqlab/main/public/static/images/blog/top10.png"
    alt="h2o"
  />
</div>

## Step 7: Make predictions on the test set using the best model

Finally, you can make predictions on the test set using the best model. You can do this using the predict function, like so:

```Python
# Make predictions on the test set using the best model
predictions = aml.predict(test)
predictions.head()
```

And that's it! You have

```bash:Output
predict
1.00465
1.00465
1.00465
1.00465
1.00465
1.00465
1.00465
1.00465
1.00465
1.00465
```

Here is an example of how you can plot the predictions made by the best model on the test set:

```Python
import matplotlib.pyplot as plt

# Extract the true labels and predictions
y_true = test['target_column'].as_data_frame().values
y_pred = predictions['predict'].as_data_frame().values

# Plot the results
plt.plot(y_true, label='True labels')
plt.plot(y_pred, label='Predictions')
plt.ylabel('Target Column')
plt.xlabel('Sample Index')
plt.title('Predictions Made by the Best Model')
plt.legend()
plt.show()
```

This code will extract the true labels and predictions from the test set, and then plot the results using a line plot. The x-axis will be the sample index, and the y-axis will be the target column.

<div className="grid place-items-center">
  <img
    className="inline rounded-lg"
    src="https://raw.githubusercontent.com/fiqgant/fiqlab/main/public/static/images/blog/prediction_automl.png"
    alt="h2o"
  />
</div>

I hope this helps! Let me know if you have any other questions.
