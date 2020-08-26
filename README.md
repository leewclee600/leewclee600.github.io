# OSIC Plumonary Fibrosis Progression

In a machine learning program, we have to follow the steps below

* Understand the problem

* Plan and collect data

* Explore the data

* Postulate the model

* Fit the model

* Check the model

* Iterate

* Use the model

## Understand the problem
### Background

Pulmonary fibrosis is a lung disease that occurs when lung tissue becomes damaged and scarred. 
This thickened, stiff tissue makes it more difficult for your lungs to work properly. As pulmonary fibrosis worsens, you become progressively shorter of breath.
The scarring associated with pulmonary fibrosis can be caused by a multitude of factors. 
But in most cases, doctors can't pinpoint what's causing the problem. When a cause can't be found, the condition is termed idiopathic pulmonary fibrosis.
The lung damage caused by pulmonary fibrosis can't be repaired, but medications and therapies can sometimes help ease symptoms and improve quality of life. 
For some people, a lung transplant might be appropriate.


Fibrotic lung diseases are difficult to treat even with access to a chest CT scan. 
Patient could suffer from great anxiety because of fibrosis-related symptoms and the diseases’s opaque path of progression.
Open Source Imaging Consortium(OSIC) is a co-operative effort between academia, 
industry and philanthropy designed to enable rapid advances in the fight against Idiopathic Pulmonary Fibrosis(IPF),
fibrosing interstitial diseases(ILDs) and other respiratory diseases, including emphysematous conditions.

### Evaluation 

The result of competition is evaluated in a modified version of the Laplace Log Likelihood.

The formula is below 

![equation](https://latex.codecogs.com/gif.latex?%5Csigma_%7Bclipped%7D%20%3D%20max%28%5Csigma%2C%2070%29),

![equation](https://latex.codecogs.com/gif.latex?%5CDelta%20%3D%20min%20%28%20%7CFVC_%7Btrue%7D%20-%20FVC_%7Bpredicted%7D%7C%2C%201000%20%29%2C),

![equation](https://latex.codecogs.com/gif.latex?metric%20%3D%20-%20%5Cfrac%7B%5Csqrt%7B2%7D%20%5CDelta%7D%7B%5Csigma_%7Bclipped%7D%7D%20-%20%5Cln%20%28%20%5Csqrt%7B2%7D%20%5Csigma_%7Bclipped%7D%20%29.)

The error is thresholded at 1000 ml to avoid large errors adversely penalizing results,
while the confidence values are clipped at 70 ml to reflect the approximate measurement uncertainty in FVC. 
The final score is calculated by averaging the metric across all test set Patient_Weeks (three per patient). 

**Note that metric values will be negative and higher is better.**

### Goal
In this project, we need to predict patents’ severity of decline in lung function based on a CT scan of their lungs. 
This can help patient better understand their prognosis. 
Also, better accurate severity detection could help improvement in treatment trial design and clinical development. 


## plan and collect the data

### Data Description

`Patient`: a unique Id for each patient (also the name of the patient's DICOM folder)

`Weeks`: the relative number of weeks pre/post the baseline CT (may be negative)

`FVC`: the recorded lung capacity in ml

`Percent`: a computed field which approximates the patient's FVC as a percent of the typical FVC for a person of similar characteristics

`Age`

`Sex`

`SmokingStatus`

**train.csv** - the training set, contains full history of clinical information

**test.csv** - the test set, contains only the baseline measurement

**train/** - contains the training patients' baseline CT scan in DICOM format

**test/** - contains the test patients' baseline CT scan in DICOM format

**sample_submission.csv** - demonstrates the submission format

### Understand DICOM file

**Dicom Files**: A DICOM file is an image saved in the Digital Imaging and Communications in Medicine (DICOM) format.


### 

