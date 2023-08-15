## Brief Summary:

# Classifying Steel Defects Using Deep Learnign
==============================

_The identification and classification of surface defects in rolled steel is an essential quality control measure in the steel industry to ensure a surface finish that meets the customers' visual and performance expecations. Traditionally, a high-speed camera is installed in the rolling mill that captures images of strip steel that passes through. A human is usually responsible to identify and class these defects, however accuracy is usually compromised this way. In this project, a CNN was built to classify these surface defects._


## Data

Data was obtained from a page on [Kaggle](https://www.kaggle.com/datasets/fantacher/neu-metal-surface-defects-data)
which was sourced from a research paper mentioned in the final report under /report

The data set included 6 defect types, each having 300 images. In total there were 1800 images. The 6 defect types were:
*	Crazing: Formation of microcracks in the surface when exposed to high stress
*	Inclusion: Unwanted formation of oxides or other non-metallic particles in the substrate
*	Scratches: Abrasions on the surface
*	Rolled in scale: Oxidated scale that has been rolled into the steel
*	Patches: Visual irregularities in the steel surface
*	Pitted: Small areas of corrosion

These defects are caused by material quality issues, manufacturing and/or human error.

The images were stored as .bmp files each in a folder according to their defect type. The filenames were read and a column was added that stated each image's defect class.

![](/reports/figures/data_example.png)


## EDA

All were grayscale 200x200 pixel images. Examples of each defect type is shows below:

![](/reports/figures/images_example.png)


## Preprocessing

The 1800 grayscale images were read the DataFrame comprised of the image paths and were stored in a numpy array. The shape of the data was (1800,200,200). This array served as the feature set. The images were also standardized so that each pixel value was between 0 and 1. The defect type column as mentioned previously was one-hot encoded as served as the target variable.


## Model Explanation

During the initial investigation, a ResNet-50 model using weights from Image Net were used. The data was transformed to a (1800,200,200,3) array since this model only accepts RGB images. The original top layer of the ResNet model had 1000 neurons corresponding to the 1000 classes in the ImageNet dataset. This layer was replaced by a dense layer with 6 neurons for the 6 defect types. The model architecture is shown below:

![](/reports/figures/resnet_map.png)

The second model under investigation was a CNN. The model was chosen to be as simple as possible to accomadate for limited compute. The architecture is shown below:

![](/reports/figures/

## Modelling

## Discussion and Conclusion


LGBM contributes 60% to the ensemble model, placing a balanced importance on the influential  elements. It relies on temperature first and foremost but still performs well. Extra Trees has a limited contribution, possibly due to overfitting. Default CatBoost outperforms the Voting Regressor, but the latter is still chosen as the final model since it will be more generalizable to new data.

For this business use case, both MAE and RMSE are used to judge the model's performance. Metallurgists need only a rough estimate of steel performance. The Voting Regressor ensemble model socred an MAE of ~14 MPa, RMSE of ~28 MPa, and an R2 of 0.96 when cross-validating. Considering the mean Yield strength of the set is 361 MPa, its performance is excellent for this use case.

An evaluation was done on a subset of the data at a temperature of 27˚C (around room temperature). Firstly, all observations recorded at 27˚C were indexed. Using this index, new X and Y datasets were created. These new sets were also cleared of any training data. To reiterate, the resulting dataset (25 observations) was comprised exclusively of test and validation data recorded at 27˚C. The results are shown below:


[](/reports/figures/metrics_27.png)






 

When scored on the new test and validation data, the model still performed quite well during cross-validation. Its MAE was ~27 MPa, RMSE was ~39 MPa and R2 was 0.91 which is lower than the results when the model was fitted to all the data but still quite usable.

Due to the reasons specified above, the final model was chosen to be the ensemble Voting Regressor.


Project Organization
------------

    ├── LICENSE
    ├── README.md          
    ├── data
    │   ├── external       <- Data from third party sources.
    │   ├── interim        <- Intermediate data that has been transformed.
    │   └── raw            <- The original, immutable data dump.
    │
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
    │          
    │
    ├── reports            <- Generated analysis as DOCX, PDF
    │   └── figures        <- Generated graphics and figures to be used in reporting

--------

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>
# Classifying Steel Defects Using Deep Learning

