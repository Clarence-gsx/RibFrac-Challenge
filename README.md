# RibFrac-Challenge

Evaluation scripts for [MICCAI 2020 RibFrac Challenge: Rib Fracture Detection and Classification](https://ribfrac.grand-challenge.org/).

# Content

```
RibFrac-Challenge/
    requirements.txt                Required packages for evaluation
    ribfrac/
        evaluation.py               Functions for model evaluation
        nii_dataset.py              The dataset class for .nii reading
```

# Setup

Run the following in command line to install the required packages. First create a specific Anaconda environment and activate it:
```bash
conda create -n ribfrac python=3.7
conda activate ribfrac
```
And then install required packages using pip:
```bash
pip install -r requirements.txt
```

# Usage

## Download the competition data

You can download the competition data by first [Join](https://ribfrac.grand-challenge.org/participants/registration/create/) the challenge then visit the [Dataset](https://ribfrac.grand-challenge.org/dataset/) page.

## Evaluation

We use this script to evaluate your test set submission online. You can evaluate your own prediction locally as well. The evaluation script has very specific requirements on the submission format. Please make sure that these requirements are followed or your submission won't be graded.

To evaluate your prediction locally, you need to prepare the ground-truth and prediction directory. Take validation dataset as an example. After the train/validation data is downloaded, you should unzip it and place all ground-truth label .nii.gz files along with the info .csv file under the same directory as follows:
```
ground_truth_directory/
    RibFrac421-label.nii.gz
    RibFrac422-label.nii.gz
    ...
    RibFrac500-label.nii.gz
    ribfrac-val-info.csv
```

Your prediction should be organized similarly to the ground-truth directory. .nii.gz and .csv should be placed under the same directory as follows:
```
prediction_directory/
    RibFrac421.nii.gz
    RibFrac422.nii.gz
    ...
    RibFrac500.nii.gz
    ribfrac-val-pred.csv
```

Each .nii.gz file should contain a 3D volume with ```n``` fracture regions labelled in integer from ```1``` to ```n```. The order of axes should be ```(x, y, z)```.

The prediction info .csv should have four columns: ```public_id``` (patient ID), ```label_id``` (prediction ID marking the specific connected-region in the .nii volume), ```confidence``` (detection confidence) and ```label_code``` (fracture class), e.g.:

|public_id|label_id|confidence|label_code|
|-|-|-|-|
|RibFrac421|0|0.5|0|
|RibFrac422|1|0.5|3|
|RibFrac423|2|0.5|1|
|...||||
|RibFrac499|0|0.5|0|
|RibFrac500|1|0.5|3|

For each public_id, there should be at least one row representing the background class. Similar to in the ground-truth info .csv, the background record should have ```label_id=0``` and ```label_code=0```. Other than that, each row in the classification prediction .csv represents one predicted fracture area. The public_id should be in the same format as in .nii file names.

After setting all of the above, you can evaluate your prediction through the following command line:
```bash
python ribfrac/evaluation.py --gt_dir <ground_truth_directory> --pred_dir <prediction_directory>
```
