---------------------------------------------------------------------------
# **10/1 Update: Contains some information on the data processing for the fMRI music genre project.**

# Poject Goal
> Use machine learning to classify music genre and perhaps guide the interpretation of brain decoding of music genre information.

> Paper: https://pubmed.ncbi.nlm.nih.gov/33164348/

## Data Sources:
OpenNeuro: https://openneuro.org/datasets/ds003720/versions/1.0.0

Basic Dataset structure in OpenNeuro:

```
  |--sub-001 (subject/patient number 1)
    |--------anat (Anatomical image, for better spatial resolution of the brain structure)
              |-----------sub-001_t1w.json (Contains structural meta-information about the structural MRI image of patient)
              |-----------sub-001_T1w.nii (The structural information for patient number 1.)
    |--------func (Functional image, less spatial resolution, but contains better temporal resolution, fMRI)
    
              |-----------sub-001_task-Test_run-01_bold.json （Contains the json meta data of the fMRI collection）
              |-----------sub-001_task-Test_run-01_bold.tsv （Contains the different music genre that the patient is exposed to in the 610s. We extract each row as training instance）
              |-----------sub-001_task-Test_run-01_bold.nii (The 4D volume for the 610s runs)
```

## Preprocessed Data

- We used fMRIprep to preprocess the data to remove the noise and artifacts.

- Link to the preprocessed data: https://drive.google.com/drive/folders/1-81WyfipFcik9S71jDwQu8djQGEhZCR0?usp=sharing

- Folder location: sub-001/func/(*t1w or MNI152NLin2009cAsym space)

## Brain atlases:

- Using the whole brain to do a full 4dCNN does not generate meaningful baseline results. The current baseline ~20%
uses fmriprepped data and Right herschl gyrus region (~611 voxels extracted from the 95,117,95 voxels).

- You can find the brain atlases transformation and the atlass annotations (AAL, Desikan) here: https://github.com/neurodata/neuroparc 

- The code files has a short Rmarkdown that demonstrates how to do structural brain alignment in R/python. The general workflow
  is to convert
  >**Averaged Brain space (MNI152)**--->>**Structural Brain space(sMRI) specific to the patient**--->>**fMRI space**
  where each ----->> is connected by a (non)linear transformation.

# Files in the current folder

> fMRIpreped pdf file pdf.
> Rmarkdown/python colab to do registration.
> fMRIprep codes

# **TODO:**
- Test other anatomical region see if we can improve the result(robustness-five-fold and accuracy wise).

- See if we can average the signal in different ROI regions and get better prediction from ensemble based methods.

