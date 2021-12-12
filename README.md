# Text Retrieval for biomimicry function identification in a corpus of biology papers

## Overview

This repository contains code that performs multi-label text classification on a corpus of biology papers using a variety of classifiers. Each classifier generates a MAP score for comparison.

## File Structure

`golden.json` original dataset of biology papers

`Data-Analaysis-of-the-PeTaL-Labeled-Dataset.ipynb` shows some metrics of the data (Modification of original work by: David Smith)

`data-exploration-and-cleaning.ipynb` additional data exploration and cleaning. This generates the files in the `data` folder

`data/cleaned.csv` data cleaned and prepared for classification. Columns are present for each label. A 1 means the paper was assigned the lable, 0 means it was not. Column 'y' contains a list of the binary indicators for each labels, and is just a different format from the label columns, used in linear-svm.ipynb.

`transformers-zs-nli-and-scibert.ipynb` compares the classifiers, bart-large-mnli, and SciBERT.

`linear-svm.ipynb` classifies using a LinearSVC classifier.

`scibert-ml.ipynb` faster SciBERT classifier, takes less time to train and generate predictions than `transformers-zs-nli-and-scibert.ipynb` most likely due to smaller max_token_length and no k-fold cross validation. Adapted from original work by Venelin Valkov: https://curiousily.com/posts/multi-label-text-classification-with-bert-and-pytorch-lightning/

`LOTClass` folder contains the [github repo](https://github.com/yumeng5/LOTClass) for the LOTClass implementation.  Adapted from original work by Meng, Yu and Zhang, Yunyi and Huang, Jiaxin and Xiong, Chenyan and Ji, Heng and Zhang, Chao and Han, Jiawei. The `petal.sh` file inside the folder will build the model and produce the test output. Creating the dataset viewing output can be done using the `LOTClass.ipynb` file. This model requires a GPU and is still in development phase.

## How to Use

`transformers-zs-nli-and-scibert.ipynb` can be loaded in Colab. Setting the runtime to GPU will enable faster model training and predictions. All the cells can be run in order. After running the first cell, you may see a button in the cell output to restart the runtime. This should be done. Final scores are computed at the bottom of the notebook. To just run one of the two models, for example SciBERT, don't run the cells with comment labels at the top of the cell containing 'bart-large-mnli'. SciBERT training and predictions can be made faster by reducing the number of k-fold splits. The code line where this can be done can be found by searching the notebook for the comment labelled 'CONFIG'. To replace SciBERT with BERT, uncomment the two lines marked with 'CONFIG' comments.

* bart-large-mnli takes about 2.5 hours to generate 500 predictions
* SciBERT takes about 2 hours to train and generate 500 predictions

`linear-svm.ipynb` can be loaded in Colab. This model does not use a GPU. All the cells can be run in order. After running the first cell, you may see a button in the cell output to restart the runtime. This should be done. The final MAP score is computed at the bottom of the notebook.

`scibert-ml.ipynb` can be loaded in Colab. This model requires a GPU. All the cells can be run in order. Training only takes ~20 minutes. The final MAP score is computed at the bottom of the notebook.

`LOTClass.ipynb` can be loaded locally to build the datasets for the LOTClass folder. This model requires a GPU. All the cells can be run in order. 

## Team Member Contributions

Brandon Ruffridge – `data-exploration-and-cleaning.ipynb`, `transformers-zs-nli-and-scibert.ipynb`, `linear-svm.ipynb`, `scibert-ml.ipynb` adaptations of original work by Venelin Valkov

Christian Ortiz – `LOTClass.ipynb`

Jay Kim – 

Brendan Lynch – 


## Results

| Run ID | Model                            | MAP   | gMAP  | Description                                                                                                                                                                                    |
| ------ | -------------------------------- | ----- | ------| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1a     | facebook/bart-large-mnli         | .29   | .28   | transformers-zs-nli-and-scibert.ipynb. (pretrained zero-shot) 50 papers from each label. 500 total.                                                                                            |
| 1b     | facebook/bart-large-mnli         | .31   | .30   | transformers-zs-nli-and-scibert.ipynb. (pretrained zero-shot) 50 papers from each label. 500 total. Reworded the class names slightly.                                                         |
| 2      | allenai/scibert_scivocab_cased   | .64   | .64   | transformers-zs-nli-and-scibert.ipynb. same 500 papers. split into 5 folds for training and validation (80/20 split). 2 epochs, train_batch_size=8, validation_batch_size=4, max_token_length=512                    |
| 3a     | allenai/scibert_scivocab_uncased | .71   | .70   | transformers-zs-nli-and-scibert.ipynb. same 500 papers. split into 5 folds for training and validation (80/20 split). 2 epochs, train_batch_size=8, validation_batch_size=4, max_token_length=512                    |
| 3b     | allenai/scibert_scivocab_uncased | .67   | .66   | scibert-ml.ipynb. All ~1000 papers in data/cleaned.csv. split into training and validation (90/10 split) using multi-label stratification to preserve class distributions in both splits. 8 epochs, train_batch_size=12, validation_batch_size=12, max_token_length=256                    |
| 4      | bert_base_cased                  | .48   | .47   | transformers-zs-nli-and-scibert.ipynb. same 500 papers. split into 5 folds for training and validation (80/20 split). 2 epochs, train_batch_size=8, validation_batch_size=4, max_token_length=512                    |
| 5      | bert_base_uncased                | .53   | .51   | transformers-zs-nli-and-scibert.ipynb. same 500 papers. split into 5 folds for training and validation (80/20 split). 2 epochs, train_batch_size=8, validation_batch_size=4, max_token_length=512                    |
| 6      | OneVsRest/LinearSVC              | .63   |       | linear-svm.ipynb. All ~1000 papers in data/cleaned.csv. 4-fold nested cross validation with hyperparameter tuning (75/25).                                                                             |
| 7      | Roberta-large-mnli               | .29   | .25   | transformers-zs-nli-and-scibert.ipynb. (pretrained zero-shot) 50 papers from each label. 500 total.                                                                                            |

### Average Precision by Topic

| Run ID | physically_assemble_or_disassemble | protect_from_harm | sense_send_or_process_information | chemically_modify_or_change_energy_state | maintain_structural_integrity | attach | move | process_resources | sustain_ecological_community | change_size_or_color |
| ------ | ---------------------------------- | ----------------- | --------------------------------- | ---------------------------------------- | ----------------------------- | ------ | ---- | ----------------- | ---------------------------- | -------------------- |
| 1a     | .20                                | .39               | .22                               | .24                                      | .29                           | .46    | .34  | .33               | .29                          | .19                  |
| 1b     | .21                                | .43               | .30                               | .30                                      | .33                           | .23    | .27  | .40               | .30                          | .28                  |
| 2      | .46                                | .60               | .78                               | .54                                      | .65                           | .65    | .71  | .71               | .70                          | .65                  |
| 3a     | .51                                | .67               | .84                               | .69                                      | .73                           | .75    | .76  | .70               | .72                          | .69                  |
| 3b     | .47                                | .58               | .76                               | .68                                      | .61                           | .76    | .78  | .72               | .65                          | .67                  |
| 4      | .37                                | .59               | .68                               | .33                                      | .50                           | .32    | .54  | .57               | .50                          | .42                  |
| 5      | .33                                | .59               | .78                               | .45                                      | .56                           | .36    | .70  | .46               | .54                          | .50                  |
| 7      | .17                                | .47               | .30                               | .70                                      | .33                           | .14    | .11  | .20               | .18                          | .27                  |

## Evaluation

bart-large-mnli performed the worst. Even with little training data BERT and SciBERT were better. SciBERT outperformed BERT, most likely due to SciBERT being trained on scientific literature which is what our corpus is. A shallow model, LinearSVC, performed surprisingly well. It is unclear why the second SciBERT classifier shown in Run 3b, underperformed the first version, but it could be due to reducing max_token_length to 256. This was necessary because setting it to 512 in the second version consumed all of the available GPU memory in Colab which crashed the runtime.

## Future Work

Try out some of these approaches:

Giannis Karamanolakis, Subhabrata Mukherjee, Guoqing Zheng, and Ahmed Hassan Awadallah.
2021. Self-training with weak supervision.

Dheeraj Mekala, Xinyang Zhang, and Jingbo Shang. 2020. META: Metadata-empowered weak
supervision for text classification.

Timo Schick and Hinrich Sch¨utze. 2021. Exploiting cloze-questions for few-shot text classification
and natural language inference.

Zihan Wang, Dheeraj Mekala, and Jingbo Shang. 2021. X-class: Text classification with extremely
weak supervision.

Qizhe Xie, Zihang Dai, Eduard Hovy, Thang Luong, and Quoc Le. 2020. Unsupervised data augmentation
for consistency training.

## Rubric for final submissions

### Source Code

Make sure copied code is cited.

For all other topics, the rubrics are:    
No source code submitted -- 0 pts.   
Source code submitted with some missing files -- 5 pts.   
Complete source code submitted -- 10 pts.   

### Documentation

7. Software code submission with documentation (65%, due end of Week 15, Dec 09 2021)

Each team must submit the software code produced for the project along with a written documentation. The documentation should consist of the following elements: 1) An overview of the function of the code (i.e., what it does and what it can be used for). 2) Documentation of how the software is implemented with sufficient detail so that others can have a basic understanding of your code for future extension or any further improvement. 3) Documentation of the usage of the software including either documentation of usages of APIs or detailed instructions on how to install and run a software, whichever is applicable. 4) Brief description of contribution of each team member in case of a multi-person team. Note that if you are in a team, it is your responsibility to figure out how to contribute to your group project, so you will need to act proactively and in a timely manner if your group coordinator has not assigned a task to you. There will be no opportunity to make up for any task that you failed to accomplish. In general, all the members of a team will get the same grade for the project unless the documentation submission indicates that some member(s) only superficially participated in the project without doing much actual work; in that case, we will discount the grade. Everyone is expected to spend at least 20 hours to seriously work on your course project as a minimum, not including the time spent for preparing the documentation.

The 65% of the grade would be distributed as follows: 45% for source code submission; 20% for documentation submission. The 20% for the documentation submission includes 5% for overview of functions, 10% for implementation documentation, 5% for usage documentation. There is no strict length requirement for the documentation.

At the final stage of your project, you need to deliver the following:    
Your documented source code and main results.    
Self-evaluation. Have you completed what you have planned? Have you got the expected outcome? If not, discuss why.    
A demo that shows your code can actually run and generate the desired results. If there is a training process involved, you don’t need to show that process during the demo. If your code takes too long to run, try to optimize it, or write some intermediate results (e.g. inverted index, trained model parameters, etc.) to disk beforehand.

11. [FINAL] Documentation: score (0-2).   
This part is graded based on completion.    
No documentation submitted (0 pts).   
Documentation submitted with some missing details, either setup or other questions (1 pt).   
Documentation submitted with all details (2 pts).   

### Software Usage Tutorial Video

8. Software usage tutorial presentation (20%, due end of Week 15, Dec 09 2021)

At the end of the semester, every project team will be asked to submit a short tutorial presentation (e.g., a voiced ppt presentation) to explain how the developed software is to be used. The presentation must include (1) sufficient instructions on how to install the software if applicable, (2) sufficient instructions on how to use the software, and (3) at least one example of use case so as to allow a grader to use the provided use case to test the software. There is no strict length requirement for this video submission, but you should target at 5~10 minutes. A presentation shorter than 5 minutes is unlikely detailed enough to help users understand how to use the software, whereas a longer video than 10 minutes might be too long for impatient users. However, feel free to produce a longer presentation if needed.

The tutorial presentation would be graded based on

1) completion of the presentation (10%); and

2) result of testing the software by graders (10%).

If the software passes the test (i.e., is working as expected), full points will be given; otherwise, points will be deducted from the 10% allocated to the “result of testing the software by graders.”

12. [FINAL] Please provide feedback on the Software usage tutorial. Is it submitted?    
Please describe the results of testing the code. What were your inputs/outputs? Was the testing successful? If not, did you set up a live demo session, and was it successful?

13. [FINAL] Software usage tutorial presentation: score (0-2).   
Presentation not submitted AND Testing was unsuccessful (0 pts).   
Only presentation submitted OR only testing was successful (1 pt).   
Presentation submitted AND successful testing (2 pts).   
