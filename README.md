# CourseProject

Please fork this repository and paste the github link of your fork on Microsoft CMT. Detailed instructions are on Coursera under Week 1: Course Project Overview/Week 9 Activities.

# Evaluation Measures

| Run ID  | Model               | MAP   | gMAP  | Description                                                       |
| ------- | ------------------- | ----- | ------| ----------------------------------------------------------------- |
| 1a      | bart-large-mnli     | .29   | .28   | 50 papers from each label. 500 total.                             |
| 1b      | scibert             | ?     | ?     | same 500 papers. split into 5 folds for training and validation.  |
| 2a      | bart-large-mnli     | .34   | .32   | same 100 papers as scibert test set.  |
| 2b      | scibert             | .47   | .42   | 400 train, 100 test.                  |


## Average Precision by Topic

| Run ID  | physically_assemble_or_disassemble | protect_from_harm | sense_send_or_process_information | chemically_modify_or_change_energy_state | maintain_structural_integrity | attach | move | process_resources | sustain_ecological_community | change_size_or_color |
| ------- | ---------------------------------- | ----------------- | --------------------------------- | ---------------------------------------- | ----------------------------- | ------ | ---- | ----------------- | ---------------------------- | -------------------- |
| 1a      | .20                                | .39               | .22                               | .24                                      | .29                           | .46    | .34  | .33               | .29                          | .19                  |
| 1b      | .20                                | .39               | .22                               | .24                                      | .29                           | .46    | .34  | .33               | .29                          | .19                  |
| 2a      | .19                                | .47               | .21                               | .49                                      | .22                           | .39    | .50  | .33               | .45                          | .16                  |
| 2b      | .11                                | .22               | .65                               | .41                                      | .54                           | .46    | .61  | .43               | .67                          | .63                  |
