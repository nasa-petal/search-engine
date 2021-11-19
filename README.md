# CourseProject

Please fork this repository and paste the github link of your fork on Microsoft CMT. Detailed instructions are on Coursera under Week 1: Course Project Overview/Week 9 Activities.

# Evaluation Measures

| Run ID  | Model                          | MAP   | gMAP  | Description                                                                                                                           |
| ------- | ------------------------------ | ----- | ------| ------------------------------------------------------------------------------------------------------------------------------------- |
| 1a      | facebook/bart-large-mnli       | .29   | .28   | (pretrained zero-shot) 50 papers from each label. 500 total.                                                                          |
| 1b      | allenai/scibert_scivocab_cased | .64   | .64   | same 500 papers. split into 5 folds for training and validation (80/20 split). 2 epochs, train_batch_size=8, validation_batch_size=4  |
| 1c      | bert_base_cased                | .48   | .47   | same 500 papers. split into 5 folds for training and validation (80/20 split). 2 epochs, train_batch_size=8, validation_batch_size=4  |

## Average Precision by Topic

| Run ID  | physically_assemble_or_disassemble | protect_from_harm | sense_send_or_process_information | chemically_modify_or_change_energy_state | maintain_structural_integrity | attach | move | process_resources | sustain_ecological_community | change_size_or_color |
| ------- | ---------------------------------- | ----------------- | --------------------------------- | ---------------------------------------- | ----------------------------- | ------ | ---- | ----------------- | ---------------------------- | -------------------- |
| 1a      | .20                                | .39               | .22                               | .24                                      | .29                           | .46    | .34  | .33               | .29                          | .19                  |
| 1b      | .46                                | .60               | .78                               | .54                                      | .65                           | .65    | .71  | .71               | .70                          | .65                  |
| 1c      | .37                                | .59               | .68                               | .33                                      | .50                           | .32    | .54  | .57               | .50                          | .42                  |
