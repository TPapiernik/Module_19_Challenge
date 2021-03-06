# Module 19 Challenge - Charity Analysis - Neural Network

## Overview

Project Origination Date: 2021-10-18

#### Disclaimer

No part of this analysis should be considered Financial Advise. It is to be used for Mathematical
and Computer Programming Demonstration Purposes Only. Conduct Your Own Due Diligence.


### Purpose

The purpose of this analysis is to create a Binary Classifier
that is capable of predicting whether applicants will be successful
if funded by Alphabet Soup.


### Deliverables

1. Preprocessing Data for a Neural Network Model
2. Compile, Train, and Evaluate the Model
3. Optimize the Model
4. A Written Report on the Neural Network Model (this README.md)


### Resources

- Software:
	- Jupyter notebook server 6.3.0, running Python 3.7.10 64-bit
		- Dependencies:
			- matplotlib
			- os
			- pandas
			- sklearn.model_selection [train_test_split]
			- sklearn.preprocessing [OneHotEncoder, StandardScaler]
			- tensorflow
			- tensorflow.keras.callbacks [ModelCheckpoint]

- Data:
	- `charity_data.csv`
		- Client-Provided dataset containing more than 34,000 organizations
		that have received funding from Alphabet Soup over the years.
	- `AlphabetSoupCharity_starter_code.ipynb`
		- Client-Provided Starter Code Jupyter Notebook File
		- `AlphabetSoupCharity.ipynb`
		- User-Modified version of `AlphabetSoupCharity_starter_code.ipynb` used to complete Deliverables 1-2
	- `AlphabetSoupCharity.h5`
		- User-Saved Model Results HDF5 File
	- `AlphabetSoupCharity_Optimization.ipynb`
		- User-Modified version of `AlphabetSoupCharity.ipynb` used to optimize Neural Network to complete Deliverable 3
	- `AlphabetSoupCharity_Optimization.h5`
		- User-Saved Optimized Model Results HDF5 File


Additional information about `crypto_data_symbol_added_edited.csv` is outlined below in Tables 1 & 2.

**Table 1: Source Data Description**
| File Name                               | Brief Description of Contents
|-----------------------------------------|------------------------------
| `charity_data.csv`                      | Non-Quoted Comma Delimited ASCII Text File. 3.5 Mb; 34,300 lines. 1 Header Line; 34,299 unique Data/Metadata Lines. 1 Line Corresponds to 1 Organization Record. 12 Fields, no leading or trailing whitespace. Added 'CRLF' at EOF for correct line-count parsing. No blank or NULL values. Shortest Line: 66 characters (including delimiters), Longest Line: 159 characters (including delimiters).

**Table 2: `charity_data.csv` Fields**
| Field Name                              | Client Description of Contents                | Additional Description of Contents
|-----------------------------------------|-----------------------------------------------|-----------------------------------
| `EIN`                                   | Identification Column                         | 34,299 unique entries. Integer Employee Identification Numbers, 8-9 digits Arabic Numerals. Min: 10520599; Max: 996086871
| `NAME`                                  | Identification Column                         | 19,568 unique entries. Upper-Case Free ASCII Text containing Letters, Numbers, and Spaces. 1-34 characters.
| `APPLICATION_TYPE`                      | Alphabet Soup Application Type                | 17 unique entries. Alphanumeric Application Type Code consisting of a Capital 'T' followed by 1-2 digits. [2, 3, 4, 5, 6, 7, 8, 9, 10, 12, 13, 14, 15, 17, 19, 25, 29]
| `AFFILIATION`                           | Affiliated Sector of Industry                 | 6 unique entries. Upper- and Lower-Case ASCII Text, with one category containing a forward slash '/'. 5-16 characters.
| `CLASSIFICATION`                        | Government Organization Classification        | 71 unique entries. Alphanumeric Classification Type Code consisting of a Capital 'C' followed by 1 or 4 digits. Min: 0; Min-Non-Zero: 1000; Max: 8210
| `USE_CASE`                              | Use Case for Funding                          | 5 unique entries. Upper- and Lower-Case ASCII Text, 5-13 characters.
| `ORGANIZATION`                          | Organization Type                             | 4 unique entries. Upper- and Lower-Case ASCII Text, with one category containing hyphen-minus '-', 5-12 characters.
| `STATUS`                                | Active Status                                 | 2 unique entries. Boolean Field, '1', '0'.
| `INCOME_AMT`                            | Income Classification                         | 9 unique entries. Pseudo Numeric String Field Containing Incomes and Ranges, no thousands separator. Examples: '0', '25000-99999', '1M-5M', '50M+'
| `SPECIAL_CONSIDERATIONS`                | Special Consideration for Application         | 2 unique entries, 'Y', 'N'
| `ASK_AMT`                               | Funding Amount Requested                      | 8,747 unique entries. Integer amounts with Arabic Numerals, no thousands separators, decimal points, or symbols. 4-10 digits. Min: $5,000; Max: $8,597,806,340
| `IS_SUCCESSFUL`                         | Was the Money Used Effectively?               | 2 unique entries. Boolean Field, '1', '0'.

	  
#### Data Quality                           

Overall, data quality is high, at least at the surface level of metadata, without considering veracity.
'CRLF' was added to EOF for correct line-count parsing. Beyond this, the dataset remains unmodified
from the original version furnished.

No fields contained errant leading or trailing whitespace, and all fields contain their expected types of data and values within reasonable ranges (if $8.6 Billion is a reasonable amount to ask for in funding).

## Deliverables

### Deliverable 1

See `AlphabetSoupCharity.ipynb`

### Deliverable 2

See `AlphabetSoupCharity.ipynb` and `AlphabetSoupCharity.h5`

### Deliverable 3

See `AlphabetSoupCharity_Optimization.ipynb` and `AlphabetSoupCharity_Optimization.h5`

### Deliverable 4

This 'README.md'


## Results

### Data Preprocessing

- What variable(s) are considered the target(s) for your model?
	- The target for this model is the `IS_SUCCESSFUL` column. The qualification metric is unknown, but this Boolean
	metric shows whether the Funding was used Effectively or Not.
- What variable(s) are considered to be the features for your model?
	- The features originally to complete Deliverable 2 is `APPLICATION_TYPE`, `AFFILIATION`, `CLASSIFICATION`, `USE_CASE`, `ORGANIZATION`,
	 `STATUS`, `INCOME_AMT`, `SPECIAL_CONSIDERATIONS`, and `ASK_AMT`
	- During Model Optimization in Deliverable 3, `NAME` was added as a feature.
- What variable(s) are neither targets nor features, and should be removed from the input data?
	- Originally to complete Deliverables 1-2, `EIN` and `NAME` were dropped.
	- During Model Optimization in Deliverable 3, `NAME` was re-incorporated as a feature.

### Compiling, Training, and Evaluating the Model

- How many neurons, layers, and activation functions did you select for your neural network model, and why?
	- Originally, to complete Deliverable 2, two hidden layers were added. The first with 90 neurons, and the second with 45 neurons.
	The original number of inputs was 43, so 90 neurons were chosen as approximately 2x+ the number of inputs. For the second hidden layer,
	the number of neurons was the number of neurons for the first hidden layer divided by two.
	- To complete Deliverable 3, a third hidden layer was added with 10 nodes, and the number of nodes for the first hidden layer
	was increased to 100 to improve mode performance. The number of neurons for the third hidden layer is approximately the number of neurons
	for the second hidden layer divided by 2^2 (4).
- Were you able to achieve the target model performance?
	- In Deliverable 2, the model performance achieved was 72.6%, which was below the target of 75%.
	- In Deliverable 3, optimizations were applied, and model performance was increased to 78.9%, above the target of 75%.
- What steps did you take to try and increase model performance?
	1. Increased the number of nodes in Hidden Layer 1 to 100
	2. Added a third hidden layer with 10 nodes
	3. Changed the activation function for Hidden Layers 2 and 3 to "sigmoid"
	4. Reincorporated `NAME` as a feature input for the model. To reduce the number of inputs, only 'NAMES' appearing 5 or more times were considered. Names appearing less than 5 times were categorized as 'Other'.


## Summary

Prior to optimization, the model correctly predicted good use of funds roughly 70% of the time. Following optimization, model accuracy was boosted to nearly 80%.

A different model that could be used to solve this classification problem is a Random Forest, which are good in cases of classification.


-- END --
