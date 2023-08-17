## Brief Summary:

# Classifying Steel Defects Using Deep Learnign
==============================

_The identification and classification of surface defects in rolled steel is an essential quality control measure in the steel industry to ensure a surface finish that meets the customers' visual and performance expecations. Traditionally, a high-speed camera is installed in the rolling mill that captures images of strip steel that passes through. A human is usually responsible to identify and class these defects, however accuracy is usually compromised this way. Recently, advances in technology have introduced models that can detect and classify defects, the model built in this project being one of them._


## Data

Data was obtained from a repository on [Kaggle](https://www.kaggle.com/datasets/fantacher/neu-metal-surface-defects-data)
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

The 1800 grayscale images were read the DataFrame comprised of the image paths and were stored in a numpy array. The shape of the data was (1800,200,200). This array served as the feature set. The images were also Min-Max Noramalized so that each pixel value was between 0 and 1. The defect type column as mentioned previously was one-hot encoded as served as the target variable.


## Model Explanation

During the initial investigation, a ResNet-50 model using weights from ImageNet were used. The data was transformed to a (1800,200,200,3) array since this model only accepts RGB images. The original top layer of the ResNet model had 1000 neurons corresponding to the 1000 classes in the ImageNet dataset. This layer was replaced by a dense layer with 6 neurons for the 6 defect types. The model architecture is shown below:

![](/reports/figures/resnet_map.png)

The second model under investigation was a CNN. The model was chosen to be as simple as possible to accomodate for limited compute. The architecture is shown below:

![](/reports/figures/cnn_map.png)


## Modelling

A Dummy Classifier was built yielding a macro accuracy of 0.17 on the validation set.

### ResNet
The f1-score macro average was chosen as the evaluation metric since all defect classes are equally important. A ResNet model with a GAP layer was first explored and yielded a maximum validation accuracy of 0.4181. The GAP layer was replaced with a Flatten layer. Its accruacy increased significantly to 0.73 on the validation set. It did struggle differentiating between the "Pitted" and "Rolled" classes. Using the Flatten layer instead of a GAP layer increased accuracy probably since less information is lost.

### CNN
The CNN(optimizer=Adam, learning_rate=0.001) was explored and displayed robust accuracy and minimal loss. The model did not overfit significantly. Its learning curve is shown below:

![](/reports/figures/train_val_graph_cnn.png)

The model significantly outperformed the ResNet model with an accuracy of 0.93.

### Hyperparameter Tuning

Since the CNN performed so well, the only hyperparameter tuned was the learning rate. A RandomSearch object was instantiated with a learning rate range between 0.0001, and 0.0151. 12 trials were performed and the graph below summarizes the results:

![](/reports/figures/acc_plot.png)

The model with a learning rate of 0.0016 peformed the best on the validation set. A local maximum can be seen around this range.

### z-Score Standardizing Images

The model performed superbly, however z-score Standardizing the images could further increase its performance. Therefore, the images (feature set) were adjusted so that the mean of the image array was 0 and its standard deviation was 1. The model was then trained using these adjusted images. The results on the validation set are shown below:

![](/reports/figures/classification_report/class_norm_cnn.png)
![](/reports/figures/confusion_matrix/conmat_norm_cnn.png)

## Evaluation on test set

![](/reports/figures/classification_report/class_test.png)
![](/reports/figures/confusion_matrix/conmat_test.png)


## Discussion and Conclusion

The CNN model built performed superbly. As it can be seen, the model did not overfit on the training or validation data and was therefore chosen as the final model. As it can be seen it yielded a 0.97 F1 score macro average. It should be noted that this model is overall excellent at classification but the utility of this model depends heavily on the type of defect that occurs most in production. For example, the precision and recall for “Inclusions” and “Pitted” is notable but is still lower than that of the other defects. A mill that encounters more “Inclusions” and/or “Pitted” defects than most might find this model to be less useful. 

This model will save time, money and manpower when applied to the automated camera system in a rolling mill. To implement this model in the production line, an engineer would have to build a pipeline to capture, access, evaluate and store the images and their predictions. As mentioned earlier, a model would have to be built that can further analyze these defects for other features such as size, shape etc.


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

