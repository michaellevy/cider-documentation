# Overview of features

Cider provides tooling for predicting poverty with machine learning and remote data such as phone metadata and satellite imagery. It also provides tooling to evaluate the performance of these estimations. Cider is divided into seven modules, which include the creation of poverty indices from underlying survey data as a ground-truth with which to train machine learning models, feature engineering from mobile phone metadata and supervised learning, and comparisons to counterfactual targeting methods and bias audits for the finished predictions. Each of the six modules is described below; for more information, visit Cider's documentation or [Github repo](https://github.com/emilylaiken/cider/).

## Features

### Poverty index construction
Creating poverty indices or a proxy-means test is a standard practice in international development. While consumption surveys are considered the most comprehensive approach to understanding a household’s wealth and are used in economics research, they often take hours to complete. Their length often makes them infeasible to use in emergency response or poverty alleviation programs. Therefore, economists and aid practitioners create poverty indices that select a few components from a longer survey (such as a consumption survey) that are most useful for summarizing overall poverty. The current statistical process for creating indices is manual and time-consuming. This package quickly generates an index that can be used in a survey to determine poverty more rapidly. The results of this survey can be used to collect training data for the machine learning poverty prediction rapidly. They can also be used as a standalone tool to create poverty short indices to assess a household’s poverty (without any use of machine learning).

Suppose the survey's components haven't been picked yet. In that case, this module can also select the N components of a survey - an asset index, proxy-means test, etc. - that are most predictive of poverty, building the set of questions that should be asked when collecting ground truth data. A dataset of survey responses linked to unique IDs is then uploaded, constructing each observation’s poverty description based on underlying components. 

### Feature engineering from mobile phone data
Mobile phone data typically contain records of each transaction (call, text, balance recharge, mobile money transaction, etc.) completed on a cellular network. This module automates the calculation of subscriber-level features from the mobile phone metadata, using the uploaded mobile phone metadata datasets and features of interest specification. 

### Machine learning for poverty prediction
This module trains a machine learning model to predict poverty from mobile phone features and uses that model to produce poverty predictions for all subscribers that have consented to the use of their data. 

First, upload a ground-truth poverty measure for a set of subscribers (like the poverty measure generated by the poverty index module or a hand-crafted poverty score). Next, upload features (like those produced by the feature engineering module) for all subscribers that have consented. 

The module will match ground-truth poverty data to features, train the machine learning model to predict poverty from the phone features, and report the model’s accuracy metrics on a test set. Multiple machine learning models will be trained during the training stage, and the best model, based on a pre-specified metric, will be selected and used to predict poverty for the entire user base. 

### Targeting comparisons
It is critical to compare how accurate mobile phone data targeting is compared to other targeting methods (at the very least, in comparison to geographic targeting methods). Upload a set of ground-truth poverty observations (which can be any set, including the data produced by the poverty index module and used in the machine learning module) along with a set of targeting options. The targeting module will compare the targeting options based on inclusion and exclusion errors, helping the program manager make a more informed choice of targeting mechanism


### Bias audits
When considering an algorithmic targeting method, it is essential to check whether the algorithm is biased for or against particular subgroups.  Upload a set of ground-truth poverty observations (which can be any set, including the data produced by the poverty index module and used in the machine learning module) along with observations on demographics, such as gender, age, ethnicity, religion, or other characteristics sensitive to the project's context. Also, upload the poverty proxy that will be used to target (such as the phone-based wealth predictions produced by the machine learning module or any other individual-level proxy poverty measure). This module will check for bias in the targeting method against each subgroup identified in the ground truth data. 

### Home location inference
Poverty is frequently concentrated in geographic clusters so that aid may be targeted only to subscribers living in certain areas. Mobile phone data can be used to estimate where subscribers live, based on the location of the cell towers through which they place transactions. Upload a dataset of mobile phone transactions. This module will infer the most likely location of each subscriber at the cell tower level (or an alternatively administrative level, if a shapefile is provided). If ground truth location data is available for a subset of subscribers, this module will also evaluate the accuracy of the mobile phone-based location inference. 

### Opt-in and opt-out workflows
In order to make the preservation of people’s privacy and the respect of their consent as easy as possible, this module implements opt-in and opt-out features that can be integrated in all processes that use sensitive data (raw CDR data, processed features etc.). The user of the toolkit can set the required capability at start time in the configuration file, with opt-in as default resulting in no mobile phone subscribers being initially included in the analysis, and vice versa for opt-out. Afterwards, they can flexibly provide lists of users that have opted-in or out, respectively, to include or exclude them from all the subsequent analyses. 
