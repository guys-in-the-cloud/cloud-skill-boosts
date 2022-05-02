# Explore Machine Learning Models With Explainable

## Follow instructions carefully
- create bucket
![image](https://user-images.githubusercontent.com/104570014/166257545-dab56135-e17f-4cdd-9d29-8ad8011326be.png)

- select AI-Platform
![image](https://user-images.githubusercontent.com/104570014/166253504-7cfa9914-7cb3-407b-9afd-95ddb63810a5.png)
-  click View notebook instances. 
![image](https://user-images.githubusercontent.com/104570014/166253650-d16b4561-1123-4722-b6d3-2788577ab5c0.png)
- create notebook 
![image](https://user-images.githubusercontent.com/104570014/166254009-72296df2-eeaf-472a-a6d0-8c6b1737dfdd.png)<br>
![image](https://user-images.githubusercontent.com/104570014/166254138-bc599b36-703f-4373-87f2-54caeab5e6db.png)
- all option will be default <br>
![image](https://user-images.githubusercontent.com/104570014/166254184-5e27a757-927d-4e06-9043-5c99a05d4a14.png)
- this will take time to setup jupyter notebook (open jupyternotebook)
![image](https://user-images.githubusercontent.com/104570014/166255032-18de6d6d-0783-42aa-9c8a-c561592b197a.png)

- After go to Terminal
 ![image](https://user-images.githubusercontent.com/104570014/166255479-28c5b257-07e9-4014-beb1-926cab19f09b.png)


## Download the Challenge Notebook
```
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
```
- go to <b>training-data-analyst/quests/dei</b>
- Open the notebook file what-if-tool-challenge.ipynb.
- Download and import the dataset hmda_2017_ny_all-records_labels.
- execute all the code <b>(press leftshift+Enter to execute command)</b>
## Train the first model

```
model = Sequential()
model.add(layers.Dense(200, input_shape=(input_size,), activation='relu'))
model.add(layers.Dense(50, activation='relu'))
model.add(layers.Dense(20, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))
model.compile(loss='mean_squared_error', optimizer='adam', metrics=['accuracy'])
model.fit(train_data, train_labels, epochs=10, batch_size=2048, validation_split=0.1)

```
- (paste code like this image given below)
![image](https://user-images.githubusercontent.com/104570014/166251423-fe670734-6d7e-4d6e-bfc9-0014de7200d6.png)

## Train your second model

```
limited_model = Sequential()
limited_model.add(layers.Dense(200, input_shape=(input_size,), activation='relu'))
limited_model.add(layers.Dense(50, activation='relu'))
limited_model.add(layers.Dense(20, activation='relu'))
limited_model.add(layers.Dense(1, activation='sigmoid'))
limited_model.compile(loss='mean_squared_error', optimizer='adam', metrics=['accuracy'])
limited_model.fit(limited_train_data, limited_train_labels, epochs=10, batch_size=2048, validation_split=0.1)

```
- (paste code like this image given below)


## Add project_id in Deploy Your models to the AI Platform


![image](https://user-images.githubusercontent.com/104570014/166252052-8c3a8141-37ce-4baa-be79-e4684fc0079c.png)

## Using the What-if Tool to interpret your model

```
config_builder = (WitConfigBuilder(
    examples_for_wit[:num_datapoints],feature_names=column_names)
    .set_custom_predict_fn(limited_custom_predict)
    .set_target_feature('loan_granted')
    .set_label_vocab(['denied', 'accepted'])
    .set_compare_custom_predict_fn(custom_predict)
    .set_model_name('limited')
    .set_compare_model_name('complete'))
WitWidget(config_builder, height=800)

```
![image](https://user-images.githubusercontent.com/104570014/166252948-179d06dc-e40a-445e-8d5e-9846d29fcc61.png)

## Create the Complete AI Platform model
- <b>Here in your side the name will be changed so carefully fill the details</b>
- go to AI Platform ---> left hand side Models ( click create model)

