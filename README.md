Curriculum-Industry Skill Feature Store Using Feast


Student Details
Name: K.Jaswanth
Register Number: 231FA04C57
Section: 03
---
Problem Statement
The curriculum-industry skill-gap problem is to identify the difference between the skills covered by the curriculum and the skills demanded by industry.
This project converts a curriculum-industry skill-gap dataset into a Feast-based feature store. Feature engineering is performed to calculate skill-gap related features. These features are stored and managed using Feast and are retrieved for machine-learning training and online prediction.
The project demonstrates the complete feature-store workflow:
1. Dataset creation
2. Feature engineering
3. Feast installation
4. Entity creation
5. Data source creation
6. FeatureView creation
7. Feature registration using `feast apply`
8. Historical feature retrieval
9. Machine-learning model training
10. Feature materialization
11. Online feature retrieval
12. Final prediction
13. Documentation using README
14. GitHub repository submission
---
Dataset
Dataset Size
Total entries: 1000
Original dataset: `skill\_gap\_dataset\_1000.csv`
Feature dataset: `skill\_features.parquet`
The original dataset contains curriculum coverage and industry demand information for different technical skills.
---
Original Dataset Columns
Column	Description
`record\_id`	Unique identifier for each record
`skill\_id`	Identifier of the skill
`skill`	Name of the technical skill
`category`	Skill category
`observation`	Observation associated with the skill
`curriculum\_score`	Curriculum coverage score
`industry\_demand`	Industry demand score
---
Feature Dataset Columns
After feature engineering, the following additional features are created:
Column	Description
`record\_id`	Unique record identifier
`skill\_id`	Skill identifier
`skill`	Technical skill
`category`	Skill category
`observation`	Skill observation
`curriculum\_score`	Curriculum coverage score
`industry\_demand`	Industry demand score
`skill\_gap`	Difference between industry demand and curriculum score
`gap\_percentage`	Skill gap represented as a percentage
`priority`	Priority category based on the skill gap
`event\_timestamp`	Timestamp used by Feast for feature data
---
Target
The machine-learning target used in this project is:
`priority`
The priority is determined from the calculated skill gap.
Skill Gap	Priority
`skill\_gap >= 30`	High
`skill\_gap >= 10` and `< 30`	Medium
`skill\_gap < 10`	Low
Therefore, the project treats `priority` as the target for the machine-learning model.
---
How the Dataset Entries Were Created
The dataset contains curriculum coverage scores and industry demand scores for different technical skills.
The values are used to represent the relationship between what is taught in the curriculum and what is required by industry.
Feature engineering is then performed using Python to calculate:
`skill\_gap`
`gap\_percentage`
`priority`
`event\_timestamp`
The resulting feature data is stored in Parquet format for use by Feast.
---
Feature Engineering
Feature engineering converts the original curriculum-industry data into features that can be consumed by the Feast feature store.
Feast Features
Feature	Meaning
`curriculum\_score`	Level of curriculum coverage for a skill
`industry\_demand`	Level of industry demand for a skill
`skill\_gap`	Difference between industry demand and curriculum coverage
`gap\_percentage`	Skill gap expressed as a percentage
---
Skill Gap Calculation
The skill gap is calculated using:
```text
skill\_gap = industry\_demand - curriculum\_score

````
Example
```
industry\_demand = 80
curriculum\_score = 54

skill\_gap = 80 - 54
          = 26

```
Therefore, the skill gap is:
```
26

```
---
Gap Percentage Calculation
The gap percentage is calculated using:
```
gap\_percentage = (skill\_gap / curriculum\_score) \* 100

```
Example
```
skill\_gap = 26
curriculum\_score = 54

gap\_percentage = (26 / 54) \* 100
                 = 48.15%

```
Therefore, the gap percentage is approximately:
```
48.15%

```
---
Priority Calculation
The priority is derived from the skill gap.
```
if skill\_gap >= 30:
    priority = High

elif skill\_gap >= 10:
    priority = Medium

else:
    priority = Low

```
This converts the numerical skill gap into a categorical target that can be used by the machine-learning model.
---
Feast Architecture
The overall architecture of the project is:
```
Original Dataset
      |
      v
Feature Engineering
      |
      v
Parquet Offline Data
      |
      v
Feast FeatureView
      |
      +--------------------------+
      |                          |
      v                          v
Historical Features         Materialization
      |                          |
      v                          v
Model Training             Online Store
                                 |
                                 v
                          Online Retrieval
                                 |
                                 v
                             Prediction

```
---
Feast Implementation
1. Entity
The Feast entity represents the key used to identify the feature records.
In this project, the entity is based on:
```
skill\_id

```
Each skill is identified using its unique `skill\_id`.
The entity allows Feast to associate feature values with a particular skill.
---
2. Data Source
The engineered feature data is stored in a Parquet file:
```
skill\_features.parquet

```
This Parquet file acts as the offline data source for Feast.
The data source contains the engineered features together with the timestamp information required by Feast.
---
3. FeatureView
A Feast FeatureView defines the features that Feast manages and makes available for retrieval.
The FeatureView contains the skill-related features required by this project, including:
```
curriculum\_score
industry\_demand
skill\_gap
gap\_percentage

```
The FeatureView connects the entity, data source, and feature definitions.
---
4. Feast Apply
The Feast configuration is registered using:
```
feast apply

```
The purpose of `feast apply` is to apply the Feast configuration and register the entities, data sources, and FeatureViews in the feature store.
This makes the feature definitions available for feature retrieval and materialization.
---
5. Historical Feature Retrieval
Historical feature retrieval is used to obtain feature values corresponding to historical records.
Feast provides:
```
get\_historical\_features()

```
Historical retrieval is useful for machine-learning training because it provides feature values associated with the appropriate historical timestamps.
The retrieved historical features are then used for model training.
---
6. Machine-Learning Model
The engineered Feast features are used as inputs to a machine-learning model.
The target variable is:
```
priority

```
The model learns the relationship between the skill-related features and the priority category.
The general workflow is:
```
Feast Historical Features
          |
          v
Feature Matrix
          |
          v
Machine-Learning Model
          |
          v
Priority Prediction

```
---
7. Materialization
After the features are registered and historical data is available, Feast materialization is performed.
Materialization transfers feature values from the offline data source into the online store.
This allows the latest feature values to be retrieved efficiently for online prediction.
The workflow is:
```
Offline Data
     |
     v
Materialization
     |
     v
Online Store

```
---
8. Online Feature Retrieval
Feast provides:
```
get\_online\_features()

```
for retrieving feature values from the online store.
Online retrieval is used during prediction to obtain the required feature values for a skill.
The workflow is:
```
Skill ID
   |
   v
Online Feature Retrieval
   |
   v
Feature Values
   |
   v
Machine-Learning Model
   |
   v
Prediction

```
---
Results
Historical Feature Output
Historical feature retrieval was successfully performed using Feast.
The retrieved historical feature data contains the engineered features required for machine-learning training, including:
```
curriculum\_score
industry\_demand
skill\_gap
gap\_percentage

```
The historical feature retrieval demonstrates that Feast can retrieve feature values from the offline data source for model training.
---
Model Accuracy
The machine-learning model was trained using the historical features retrieved from Feast.
Model accuracy:
```
REPLACE THIS WITH THE EXACT ACCURACY FROM YOUR COLAB OUTPUT

```
For example, if your notebook prints:
```
Accuracy: 0.85

```
then write:
```
Model Accuracy: 85%

```
The accuracy shown here must match the actual result produced by the notebook.
---
Online Feature Output
Online feature retrieval was successfully performed using Feast.
The online feature values were retrieved from the Feast online store after materialization.
The retrieved features are used as input to the machine-learning model for online prediction.
Example feature structure:
```
curriculum\_score
industry\_demand
skill\_gap
gap\_percentage

```
The exact values should correspond to the output produced by the Colab notebook.
---
Final Prediction
The final online prediction produced by the project is:
```
Final Prediction: Medium

```
This demonstrates the complete flow from feature retrieval to machine-learning prediction.
---
Required Analysis
1. What is the entity in your Feast implementation?
The entity in this Feast implementation is:
```
skill\_id

```
It identifies the individual skill for which feature values are stored and retrieved.
---
2. List the features stored in your FeatureView.
The main features stored in the FeatureView are:
```
curriculum\_score
industry\_demand
skill\_gap
gap\_percentage

```
These features represent curriculum coverage, industry demand, and the calculated skill gap.
---
3. Explain how one feature was calculated.
The `skill\_gap` feature is calculated as:
```
skill\_gap = industry\_demand - curriculum\_score

```
For example:
```
industry\_demand = 80
curriculum\_score = 54

skill\_gap = 80 - 54
          = 26

```
Therefore, the calculated skill gap is `26`.
---
4. What is the difference between your original dataset and the feature dataset?
The original dataset contains the basic curriculum and industry information.
The feature dataset contains the original information plus engineered features.
Original dataset:
```
record\_id
skill\_id
skill
category
observation
curriculum\_score
industry\_demand

```
Feature dataset additionally contains:
```
skill\_gap
gap\_percentage
priority
event\_timestamp

```
Therefore, the feature dataset is prepared specifically for feature-store usage and machine-learning workflows.
---
5. What is the purpose of the offline store?
The offline store contains historical feature data.
It is useful for:
Historical feature retrieval
Creating training datasets
Machine-learning model training
Maintaining historical feature values
In this project, the engineered feature data is stored in Parquet format and is used as the offline source.
---
6. What is the purpose of the online store?
The online store contains feature values that can be retrieved for online prediction.
It is designed for low-latency access to feature values needed by machine-learning models during prediction.
In this project, the online store is used after materialization.
---
7. What is the purpose of `feast apply`?
`feast apply` applies the Feast configuration and registers the feature-store definitions.
It registers and updates objects such as:
Entities
Data sources
FeatureViews
This makes the feature definitions available to Feast.
---
8. What does materialization do?
Materialization transfers feature values from the offline data source into the online store.
The process can be represented as:
```
Offline Data
     |
     v
Materialization
     |
     v
Online Store

```
After materialization, the features can be retrieved for online prediction.
---
9. What is the advantage of retrieving features through Feast instead of manually calculating them separately during training and prediction?
Using Feast provides a centralized feature-management process.
Instead of independently calculating features during training and prediction, the same registered features can be retrieved through the feature store.
Advantages include:
Consistent feature definitions
Reusable features
Historical feature retrieval
Online feature retrieval
Reduced duplication of feature-engineering logic
Better separation between feature engineering and model serving
This helps maintain consistency between training and prediction workflows.
---
10. State two limitations of your current dataset.
Limitation 1
The dataset is limited to the available curriculum coverage and industry-demand values. It may not represent all real-world skills and industry requirements.
Limitation 2
The dataset contains a fixed set of observations and may not capture rapidly changing industry skill requirements over time.
---
11. State two ways your feature store could be improved when more curriculum and industry evidence becomes available.
Improvement 1
More curriculum and industry data can be added regularly so that the feature store represents current skill requirements more accurately.
Improvement 2
Additional features can be introduced, such as more detailed industry evidence, skill trends, job-market information, and additional curriculum indicators when reliable data becomes available.
---
Project Files
The project contains the following important files:
```
skillgap\_feast/
│
├── data/
│   ├── online\_store.db
│   ├── registry.db
│   ├── skill\_features.parquet
│   └── skill\_gap\_dataset\_1000.csv
│
├── feature\_store/
│
├── model/
│
├── README.md
├── feature\_store.py
├── feature\_store.yaml
├── skill\_features.parquet
└── skill\_gap\_dataset\_1000.csv

```
The exact directory structure may depend on the local Feast project configuration.
---
Complete Workflow
The complete project workflow is:
```
1\\. Create Skill-Gap Dataset
            |
            v
2\\. Perform Feature Engineering
            |
            v
3\\. Create Parquet Feature Dataset
            |
            v
4\\. Install Feast
            |
            v
5\\. Configure feature\_store.yaml
            |
            v
6\\. Create Feast Entity
            |
            v
7\\. Create Data Source
            |
            v
8\\. Create FeatureView
            |
            v
9\\. Run feast apply
            |
            v
10\\. Retrieve Historical Features
            |
            v
11\\. Train Machine-Learning Model
            |
            v
12\\. Materialize Features
            |
            v
13\\. Retrieve Online Features
            |
            v
14\\. Make Final Prediction
            |
            v
15\\. Document Project in README
            |
            v
16\\. Upload Project to GitHub

```
---
Conclusion
This project demonstrates how a curriculum-industry skill-gap dataset can be transformed into a Feast-based feature store.
The project covers feature engineering, entity creation, data-source configuration, FeatureView creation, historical feature retrieval, machine-learning training, materialization, online feature retrieval, and final prediction.
The final prediction produced by the current implementation is:
```
Medium

```
The project is ready for GitHub submission after verifying the actual model accuracy and final output values against the Colab notebook.
---
