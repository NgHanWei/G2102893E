# CE7455 Project

Step by step instruction for how to run RASA NLU and Paraphraser to obtain results.

## Overview

Pretrained models can be found in the '''models''' folder.

The results of the various pretrained models are found in '''results''' folder

## Instructions

### Install the dependencies
It is recommended to create a virtual environment with python version 3.7 and activate it before running the following:
```sh
pip install -r requirements.txt
```
Activate VirtualEnvironment in the folder named NLUParaphrasing

### Splitting Training and Test Data

1. split the training data into 90/10
	$ rasa data split nlu --nlu data/nlu/appended_original_nlu.yml --training-fraction 9

2. rename the 10% test file to non_paraphrased_test_data_from_appended_original_nlu.yml

3. rename the 90% train file to non_paraphrased_training_data_from_appended_original_nlu.yml

### Training baseline RASA NLU Model

4. Train nlu model with non_paraphrased_training_data_from_appended_original_nlu.yml
	$ rasa train --nlu --data data/nlu/non_paraphrased_training_data_from_appended_original_nlu.yml

### Performing Paraphrasing on training set

5. Paraphrase non_paraphrased_training_data_from_appended_original_nlu.yml by shifting the file to Pharaphraser folder, and convert it into XLSX format.
	a. Convert Yaml to XLSX by using: https://www.convertcsv.com/yaml-to-csv.htm 
	b. Update nlu_test.xlsx by importing the converted data.
	c. Run split.py to generate a new expanded.tsv file:
$ python split.py nlu_test.xlsx
e. Add "- " to nlu_examples_expanded by ="- "&<cell>

6. Run the paraphrasing python script.
	$ python run paraphraser.py

7. Rename the resulting yaml file to paraphrased_training_data_from_non_paraphrased_training_data.yml
	
### Training Paraphrased Training Model
	
8. Train nlu model with paraphrased_training_data_from_non_paraphrased_training_data.yml
	$ rasa train --nlu --data data/nlu/paraphrased_test_data_from_paraphrased_appended_original_nlu.yml

### Paraphrasing on entire dataset
	
9. Paraphrase appended_original_nlu.yml by shifting the file to Paraphraser folder, and converting it into XLSX format.
	a. Convert Yaml to XLSX by using: https://www.convertcsv.com/yaml-to-csv.htm 
	b. Update nlu_test.xlsx by importing the converted data.
	c. Run split.py to generate a new expanded.tsv file:
$ python split.py nlu_test.xlsx
e. Add "- " to nlu_examples_expanded by ="- "&<cell>

10. Run the paraphrasing python script.
	$ python run paraphraser.py

11. Rename the resulting yaml file to paraphrased_appended_original_nlu.yml

12. split the training data into 90/10
	$ rasa data split nlu --nlu data/nlu/appended_original_nlu.yml --training-fraction 9

13. renamed the 10% test file to paraphrased_test_data_from_paraphrased_appended_original_nlu.yml
	
### Training and Testing remaining model permutations

14. Test the model created in step 4. with non_paraphrased_test_data_from_appended_original_nlu.yml
	$ rasa test nlu --nlu data/nlu/non_paraphrased_test_data_from_appended_original_nlu.yml --model model/<auto-generated-model-name-from-step-4.tar>

15. Test the model created in step 4. with paraphrased_test_data_from_paraphrased_appended_original_nlu.yml
	$ rasa test nlu --nlu data/nlu/paraphrased_test_data_from_paraphrased_appended_original_nlu.yml --model model/<auto-generated-model-name-from-step-4.tar>

16. Test the model created in step 8. with non_paraphrased_test_data_from_appended_original_nlu.yml
	$ rasa test nlu --nlu data/nlu/non_paraphrased_test_data_from_appended_original_nlu.yml --model model/<auto-generated-model-name-from-step-8.tar>
17. Test the model created in step 8. with paraphrased_test_data_from_paraphrased_appended_original_nlu.yml
	$ rasa test nlu --nlu data/nlu/paraphrased_test_data_from_paraphrased_appended_original_nlu.yml --model model/<auto-generated-model-name-from-step-8.tar>

