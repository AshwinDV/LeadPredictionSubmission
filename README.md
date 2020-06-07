ML Ops (Machine Learning Operations)
ML project life cycle includes various stages like business understanding, data acquisition, data analysis, data cleaning, model class selection, model training, hyperparameter tuning, model evaluation, model validation, model serving, performance monitoring. A lot of progress has been made in the stages from data analysis till model validation with availability of large datasets, on demand compute options, access to state of the art ML algorithms in the form of libraries. The two steps which is still not optimised are as below
1.	Translating business problem statement to ML problem statement
2.	Taking the model to production
ML Ops is a way is an approach to solve second problem by unifying ML system development (Dev) and ML system operation (Ops). In other words ML Ops is adopting DevOps principles to ML projects taking into consideration that ML is not just code, it’s code plus data and models. ML Ops is a set of practices that combines Machine Learning, DevOps and Data Engineering, which aims to deploy and maintain ML systems in production reliably and efficiently.
In layman's terms ML Ops can be defined as automating the ML project workflow and implementing monitoring in every stage to trigger appropriate action. This involves using various tools and processes like git for source code version, github for shared repository, github actions for automating CI/CD, MLflow for model version control & tracking, kubeflow for organising ML workflow, leverage distributed compute infrastructure for hyperparameter tuning, enable scalable deployment in an infrastructure agnoistic manner and so on.
Summary of the approach and justification
1.	Business understanding: The goal is to identify which leads will be converted as customers. This will help better sales resources and increase conversion rate.
2.	Data understanding:
i.	The leads data has id, landing page, origin, first contact date of list of potential customers.
ii.	Closed data provides details of customers who actually became customers and thus helps derive target label for this problem statement.
iii.	The analysis of data showed that conversion rate is not seasonal.
iv.	~10% of the leads was converted and data has class imbalance
v.	Landing page and origin are both categorical fields with huge number of categories. This cannot be logically grouped due to lack of information as that which ids correspond to which website, which origins are related etc.
3.	Data cleaning:
i.	Missing values in origin field was stored as 'unknown' which was an existing category in data
ii.	Categorical values were label encoded
iii.	Contact data was dropped as conversion rate was found to be non-seasonal and thus this field will not give any valuable information in predicting lead conversion.
iv.	Data was split into training and validation sets with 30% data held out for validation.
v.	No data was kept for testing/hyperparameter tuning as cross validation approach will be used.
vi.	SMOTE was used for minority class oversampling before training
4.	Model class selection:
i.	As this is a classification problem with limited date we rule out deep learning models.
ii.	As there are two categorical features with lots of categories converting them to one hot encoding would lead to too many features and decrease performance.
a.	If one hot encoding is used as is then it will most likely lead to overfitting
b.	If we use dimensionality reduction then there will be some loss of information
iii.	With all the above points in mind trees is the best option we have
iv.	As dataset is small randomforest would be perfect fit in this case as compared to simple decision tree
5.	Model training: Randomforest was trained with default parameters on minority class oversampled data and metrics were captured to get a sense of model performance.
6.	Hyperparameter tuning:
i.	min_samples_split and n_estimators were tuned
ii.	Recall was chosen as metric on which hyper parameters were tuned as we would not want to miss out on any potential customers
7.	Model evaluation: Model was evaluated using 3-fold cross validation and metrics was captured.
8.	Model validation:
i.	Model was tested on unseen data and metrics was noted
ii.	Model performance dropped by ~20%
iii.	With further analysis it was found out that out of ~1000 landing page, origin combination ~600 pairs occur only once which caused performance degradation.
9.	Model training: Model was trained on full data with minority class oversampled on chosen value for hyper parameter.
10.	Model serving: 1. Flask app was built to host the model locally. 2. Local deployment was tested using random test data sent via python requests library. 3. Finally model was deployed on Heroku which provides platform as a service.
11.	Monitoring 1. Concept drift: Detect fundamental changes leading to performance degradation
i.	PSI (Population Stability Index) score would be calculated against training data and real time test data to identify if there is any major change in distribution and an email would be sent as alert if there is a drift.
ii.	Detect degradation in metrics compared to validation data and alert via email
2. Data drift: Change in data encoding
iii.	Detect if any new landing id is received during serving and alert via email
iv.	Detect if any new origin is received during serving and alert via email
12.	Unit testing 1. Basic unit tests were performed using pytest 2. Flake8 tests were done to identify syntax, styling errors
13.	Source code version control was done using git
14.	Model and metrics was version controlled using MLflow
15.	Continuous Integration
i.	CI was done using Github actions
ii.	As part of CI a ubuntu image was built with required installs and unit & flake8 tests were conducted on each push
16.	Continuous Deployment
i.	CD was done using Github actions
ii.	On push and successful pass of tests as part of CI process web app will be deployed in Heroku

