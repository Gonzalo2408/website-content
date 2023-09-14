title: Automated analysis of intracoronary OCT images for patients with acute myocardial infarction 
groups: ai-for-health
finished: false
type: student
picture: vacancies/cardiac-oct-segmentation.png
template: project-single
people: Gonzalo Rodriguez Esteban, Jos Thannhauser, Rick Volleberg, Silvan Quax,  Niels van Royen, Bram van Ginneken
description: Development of a model for the automatic analysis of intracoronary optical coherence tomography (OCT) images obtained during cardiac catheterization in patients with acute myocardial infarction 

 **Start date: 28-11-2022** <br>
 **End date: 28-09-2022**
 
## Clinical Problem
Acute myocardial infarction (MI), also known as a “heart attack”, is a life threatening condition that occurs when blood supply to the heart is decreased or stopped, usually as a result of narrowing / blockage of a coronary artery due to atherosclerotic disease. In patients with MI, coronary angiography (CAG) is usually performed to localize the occluded artery, known as the culprit lesion, and to treat this artery by placing a stent (Dotter procedure). In some cases, more than one lesion is found during CAG. As of yet, it remains largely unknown whether it is beneficial to treat these non-culprit lesions in patients with so-called multi-vessel disease. To date, we cannot accurately predict which atherosclerotic plaques are vulnerable and have a high risk of causing an MI in the future, and which plaques are relatively safe.

Optical coherence tomography (OCT) is a relatively new imaging modality within interventional cardiology and cardiovascular research. OCT uses near-infrared light and is used to provide a high-resolution intravascular image of the coronary artery, allowing for assessment of plaque morphology and characteristics. Previous research has demonstrated its potential on several key aspects in the management of patients with atherosclerotic coronary artery disease, leading to beneficial patient outcome. Hence, OCT is a potential game changes in the field of interventional cardiology, as it may serve as a future tool to distinguish ‘high risk’ from ‘safe’ plaques in patients with acute MI and multi vessel disease.

Currently, however, the use of OCT in daily clinical practice is limited due to a lack of training for assessment and analysis of OCT-acquired pullbacks. In the current project, we aim at easing the use of OCT by means of artificial intelligence (AI), thereby increasing the availability of AI-based OCT algorithms for clinical decision making, and eventually improving patients’ health.

![image]({{ IMGURL }}/images/projects/cardiac-oct.png)

## Solution

A semantic segmentation algorithm based on the no-new U-Net (nnU-Net) architecture was developed that segments 12 different targets (lumen, guidewire, intima, lipid, calcium, media, catheter, sidebranch, red and white thrombus, white thrombus, dissection and plaque rupture)  in intracoronary OCT scans. A total of 4 models were trained: one of them was trained on 2D OCT frames, and the other three were trained on pseudo 3D input, in which k frames before and after the frame with the ground truth annotation are also included as input. Thus, we trained these three models using k = 1, k = 2 and k = 3 frames before and after. As preprocessing steps, a circular mask and resizing interpolator are employed to create a curated dataset suitable for training. Figure 1 shows the preprocessing and training framework.

<p>
    <img src="imgaes/projects/nnunet_framework_cardiac_oct" alt>
    <span style="font-style: normal;">
        <strong>Figure 1.</strong> Preprocessing and training frameworks, for the 2D case and the k = 3 case for the pseudo 3D model.
    </span>
</p>


After the model training, a post-processing framework based on automatic lipid and calcium measurements is designed, based on the predicted segmentations. This automated analysis measures the Fibrous Cap Thickness (FCT) and lipid arc for lipid, assesing for Thin-Cap Fibroatheroma (TCFA) detection, and the calcium arc, thickness and depth for calcium. A threshold for lipid and calcium size is estimated by computing the ROC curves and find the mimumum nº of pixels that a lipid or calcium region must contain in order to consider it as such. A final analysis addresses for the "black-box" problem that many DL models suffer. This is done by retrieving the reliability curves and calcuting the total Expected Calibration Error (ECE), including the ECE for lipid and calcium, for the test set. Moreover, further analysis on the final probability maps are performed in order to validate these maps as a measure for uncertainty, focusing into lipid and calcium regions. Below, Figure 2 shows the post-processing framework.



Our group already has developed a prototype algorithm for the segmentation of intracoronary OCT images (Figure 1). Within the current project, we aim to improve this prototype in terms of segmentation of frequently occurring structures (e.g. lumen, vascular layers, lipid, calcium), to improve detection of rarer structures, and to identify markers of plaque vulnerability. Main scientific challenges lie in the development of efficient annotation strategies for individual frames (per-frame analysis) as well as for full pullbacks (multi-frame analysis). Furthermore, AI related challenges include the development of methods to reject unreliable results (outlier classes) and to perform under low-data regime. 

To extend current ability of OCT-integrated AI systems to detect luminal border, calcium and the external elastic membrane, we aim to develop a semantic segmentation deep learning algorithm for automated image annotation and target automated segmentation of the lumen, intima, calcium, lipids, media and measures of plaque vulnerability and related complications. The segmentation algorithm will be based on a combination of generative models and semi-supervised learning to address performance in low-data and -annotation regimes. To identify unreliable segmentations, uncertainty-aware predictions will be achieved by model assembling at different granularity levels, and compared to a recently proposed efficient model based on deep deterministic uncertainty.

## Data
We will use OCT data from the PECTUS-obs study (https://pubmed.ncbi.nlm.nih.gov/34233996/) to develop and internally validate the algorithm. In total, this database includes 498 intracoronary OCT pullbacks obtained from patients with an acute myocardial infarction and multi-vessel disease. Each pullback consists of 540 frames, adding up to a total of 268,920 individual frames. Currently, the annotation procedure if as follows. All OCT-frames are divided into a training and validation set (ratio 9:1). Manual annotation is performed on every 40th sample of the training set, and supplemented with frames on which “schoolbook examples” of specific structures are visible. Of the training set, 108/498 pullbacks are currently annotated. External validation will be performed on several other prospective databases including high-risk and low-risk populations.

