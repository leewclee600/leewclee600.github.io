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

## Explore the data

### Understand DICOM file

**Dicom Files**: A DICOM file is an image saved in the Digital Imaging and Communications in Medicine (DICOM) format.

we can use `pydicom.dcmread()` to extract dicom data.

Here is a sample DICOM data: 

<img width="416" alt="sample_dicom" src="https://user-images.githubusercontent.com/34506100/91347619-a2cfa900-e7b0-11ea-9ed2-2791c8d092bd.png">

Since a patient has 12 - 80+ CT scans, we can observe the "inhale". 

![ezgif com-video-to-gif](https://user-images.githubusercontent.com/34506100/91349638-96991b00-e7b3-11ea-89cd-8adf70e8684c.gif)

### Explore csv file

First preview train.csv and test.csv

![head train and test data](https://user-images.githubusercontent.com/34506100/91361393-2fd12d00-e7c6-11ea-9208-87dc0b32e85e.png)

Also, by checking the dataset using 

`train.isnull().values.any()`

`len(train["Patient"].unique())`

We notice that there are **no missing values** and **176 unique partients**

Most of the patients have 8-10 entries from frequency plot below 

![num of entry pat](https://user-images.githubusercontent.com/34506100/91361818-1c729180-e7c7-11ea-8f30-bf49b746aeb2.png)

### Patients detail 

The **Ages** of the patients are between 50 and 90 years old. Most of them are between 60 and 70 years old.

The **Gender** of the patents are mostly Males. 

The **Smoking Status** of most patients are Ex-smokers. Less then 50% never smoked. Less then 10% still smoking. 


![patient plot](https://user-images.githubusercontent.com/34506100/91362078-a3c00500-e7c7-11ea-8662-fe5ee03576f0.png)

