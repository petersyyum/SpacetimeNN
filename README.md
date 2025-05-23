
![fMRI music genre data contrast for one specific run](https://github.com/petersyyum/SpacetimeNN/blob/main/figures/fMRI_music_contrast-dis.svg)

---------------------------------------------------------------------------
# **10/1 Update: Contains some information on the data processing for the fMRI music genre project.**

<p align="center">
  <img src="fmri_data.png" alt="A single run in the 410`*`1.5s timesteps"/>
  <h1 align="center">A single run in the 410*1.5s timesteps</h1>
</p>


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

> AAL_atlas.ipynb: How to transform brain atlas to extract voxels in the fMRI space. python colab to do registration.

> Proj_CNN_training_AAL.ipynb: Preliminary result using CNN+LSTM/ LSTM to classify use extraacted voxels on fMRIpreped data.

> ROI_ana.Rmd: ROI based analysis in R and gymnastics. Rmarkdown to do registration.

> Try_run_all.ipynb: The fMRIprep script you can run on google drive (you may need extra space).

> fMRIpreped pdf file pdf: https://drive.google.com/drive/folders/1-ewJ7c2kcI0oIpoWjTgRSPVRT5Sh-3Mh

> Transformer_scratch_hubert.ipynb
Music raw data(X_array.npy): https://drive.google.com/file/d/1EMuGTeNsF70Aw-3aW1x3Dhy24CmLvdWK/view?usp=sharing ; Labels(Y.npy): https://drive.google.com/file/d/1ONK_bjMniMD3pmZ5nNUSraRXhWViCiAd/view?usp=sharing


# **TODO:**
- Test other anatomical region see if we can improve the result(robustness-five-fold and accuracy wise).

- Try different neural network architectures.

- See if we can average the signal in different ROI regions and get better prediction from ensemble based methods.

- Explore and interpret the neural networks by visualize the intermediate layers to understand how music genre is actually
  classified in ANN to derive insights on the physiology.

# Other architectures and paper links:

[Is Space-Time Attention All You Need for Video Understanding?](https://arxiv.org/pdf/2102.05095)

[Attend and Decode: 4D fMRI Task State Decoding Using Attention Models](https://proceedings.mlr.press/v136/nguyen20a/nguyen20a.pdf)
> [Github](https://github.com/LLNL/BAnD/blob/main/band/models/resnet3d.py)

## Related biological insights on the brain imaging

[Mass general hospital Auditory Cognition Lab](https://aclab.martinos.org/research-2/)
> [Spectrotemporal content of human auditory working memory represented in functional connectivity patterns](https://www.nature.com/articles/s42003-023-04675-8)

### Cortical Parcellation
[A quantified comparison of cortical atlases on the basis of trait morphometricity](https://www.sciencedirect.com/science/article/pii/S0010945222003008)

[Desikan Atlas: An automated labeling system for subdividing the human cerebral cortex on MRI scans into gyral based regions of interest](https://pubmed.ncbi.nlm.nih.gov/16530430/)


