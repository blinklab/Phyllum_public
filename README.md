# ***Phyllum*: a *phy* plugin for surveying high-density neural recordings in the cerebellum.**
*Phyllum* is a library of plugins developed for [*phy*](https://github.com/cortex-lab/phy). These plugins can be used to facilitate curating and analyzing extracellular neural recordings obtained in the cerebellar cortex with high-density probes. *Phyllum* is able to automatically identify the layer of the cerebellar cortex in which each probe contact was located. This layer information can be used to improve the accuracy of cerebellar cell-type classifiers such as [*C4*](https://www.biorxiv.org/content/10.1101/2024.01.30.577845v1).

*Phyllum* has been developed and tested to work with recordings sorted with [*Kilosort*](https://github.com/MouseLand/Kilosort) and obtained with [*Neuropixels*](https://www.ucl.ac.uk/neuropixels/) 1.0 probes, which have a sampling frequency of 30 KHz. Although *Phyllum* is designed to work with different probe configurations and contact maps, its performance with other high-density probes will need to be assessed in the future.

[![DOI](https://zenodo.org/badge/755215850.svg)](https://zenodo.org/doi/10.5281/zenodo.10641397)
---


# Table of Contents:

 - [Links](#Links)
 - [How to Install, Configure & Run *Phyllum*](#Installation)
 - [Phyllum GUI](#Phyllum_GUI)
 - [Curating](#Curating)
 - [Layer Identification](#Layer_identification)
 - [Cluster Search Functions](#Cell_search)
 - [Visualization and Exporting Tools](#Visualization_tools)



---


<a id="Links"></a>

# Links
Kilosort sample file:
NEED TO ADD LINK HERE

Pre-processing demo:
https://drive.google.com/file/d/1cenQ2rVdZkUTzhK0sNXeFa7U2GvmI3Ir/view?usp=sharing

Features demo:
https://drive.google.com/file/d/1J5KW_O4jghrym_-vQaAYTujrmlPGQbZr/view?usp=sharing

Poster presented at the 2022 Society for Neuroscience meeting:
https://drive.google.com/file/d/1dnwbfNaofpuaoEPlhY90vSiBP5Jndjyu/view?usp=sharing





---



<a id="Installation"></a>
# How to Install, Configure & Run *Phyllum*
<mark style="background-color: cyan">
<em>Phyllum</em> is currently in beta version and not publicly available for download. To request permission to download, please contact Dr. Javier Medina: jfmedina.at.bcm.edu.  
</mark>

<br>Since *Phyllum* is a set of plugins for [*phy*](https://github.com/cortex-lab/phy), the first step will be to install [*phy*](https://github.com/cortex-lab/phy) and [*phylib*](https://github.com/cortex-lab/phylib) in an [*anaconda*](https://www.anaconda.com/) environment. The second step will be to run [*phy*](https://github.com/cortex-lab/phy) using the sample Kilosort file that can be found in the [Links section](#Links). The third step will be to download *Phyllum* using *https* or *SSH*. Finally, before *Phyllum* can be used, it is necessary to modify the [*phy*](https://github.com/cortex-lab/phy) configuration file ("phy_config.py").

### Install phy & phylib
1. Download and install [*anaconda*](https://www.anaconda.com/).

2. Create a conda environment with the name *"phyllum_dev"* and all these required dependencies:
   ```
   conda create -n phyllum_dev -y cython dask h5py joblib matplotlib numpy pillow pip pyopengl pyqt pyqtwebengine pytest python=3.10 qtconsole requests responses scikit-learn scipy traitlets
   ```
3. Activate the *"phyllum_dev"* conda environment:
   ```
   conda activate phyllum_dev
   ```
4. Install these additional packages required by *Phyllum*:
   ```
   conda install -c conda-forge umap-learn
   conda install seaborn datashader bokeh holoviews
   ```
5. In the terminal, create a new folder for saving the *phy* and *phylib* downloads:
   NOTE: File paths use backslashes in Windows (as in the command below). For Mac and UNIX systems, use forward slashes instead
   ```
   cd \Users\Francisco
   mkdir phy_code
   cd phy_code
   ```
6. Download *phy*:
   ```
   git clone https://github.com/cortex-lab/phy.git
   ```

7. Install *phy*:
   ```
   cd phy
   pip install -r requirements.txt
   pip install -r requirements-dev.txt
   pip install -e .
   cd ..
   ```

8. Download *phylib*:
   ```
   git clone https://github.com/cortex-lab/phylib.git
   ```
9. Install *phylib*:
    ```
    cd phylib
    pip install -e . --upgrade
    cd ..
    ```
### Run *phy*

10. Download the *Kilosort* sample folder ("MedinaLab_Phyllum_Kilosort_example") from the [Links section](#Links) above and save to the local Recordings folder in your computer. We recommend using the recording in the sample folder to test *Phyllum* the first time it is run, but other *Kilosort* recordings can also be used.

11. In Terminal, go to the *Kilosort* sample folder and run *phy* by typing:
    NOTE: File paths use backslashes in Windows (as in the command below). For Mac and UNIX systems, use forward slashes instead
    ```
	e.g. cd\Users\Francisco\Kilosort_files\MedinaLab_Phyllum_kilosort_example
    phy template-gui params.py
    ```
12. The previous step will generate the configuration *.phy* folder in the user account. Quit *phy*.

### Download *Phyllum*

13. Download *Phyllum* using the *https* or [*SSH*](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) protocol.
    ```
    git clone https://github.com/blinklab/phyllum.git
    ```
    or
    ```
    git clone git@github.com:blinklab/phyllum.git
    ```
    
    **NOTE:** Do not try to download *Phyllum* directly from the *GitHub* page (Code->Download ZIP). That option will not properly download the file *"Phyllum_Neuron_Database.h5"* containing the neuron database necessary for *Phyllum* to work correctly.
    
### Edit Configuration File

14. Search your computer for the location of "phy_config.py" files. You should have at least two copies:

	Copy 1: It will be inside the *Phyllum* folder downloaded in step 14, e.g. \Users\Francisco\phyllum\phy_config.py

	Copy 2: It will be in the hidden .phy folder created in step 12, in the home directory of the user doing the installation, e.g. \Users\Francisco\\.phy\phy_config.py

15. Open Copy 1 of the file "phy_config.py" and edit the line specifying the path for the plugin folder so that it points to the location of the *Phyllum* plugin folder downloaded in step 14.
    NOTE: File paths use backslashes in Windows (as in the command below). For Mac and UNIX systems, use forward slashes instead. For example:

	 c.Plugins.dirs = [r'\Users\Francisco\phyllum\plugins']

16. Replace Copy 2 of the "phy_config.py" file with the newly edited Copy 1 of the "phy_config.py" file.

### Run *Phyllum*

17. Open a Terminal window. Activate the conda environment created in step 2:
    ```
    conda activate phyllum_dev
    ```
18. Change your directory to the folder with the *Kilosort* datafile you want to analyze:
    NOTE: File paths use backslashes in Windows (as in the command below). For Mac and UNIX systems, use forward slashes instead
    ```
    e.g. cd \Users\Francisco\Kilosort_files\MedinaLab_Phyllum_kilosort_example
    ```
19. Run *phy* with *Phyllum*:
    ```
    phy template-gui params.py
    ```
---


<a id="Phyllum_GUI"></a>
# Phyllum GUI
The image below shows the *Phyllum* GUI in 'Advanced' mode. In addition to all the normal [*phy*](https://github.com/cortex-lab/phy) fuctionality, the 'Advanced' *Phyllum* GUI has extra menus for: (1) Curating, (2) Searching cells by crosscorrelogram/connectivity patterns, (3) Searching cells by activity-related features, and (4) Exporting analysis windows in multiple vectorial/lossless formats. The GUI also incorporates a new *CerebellarLayerView* window (highlighted in the image below with rectangle #2), which can be used to visualize the location of all the recorded neurons along the length of the probe and the specific layer of cerebellar cortex assigned to each.  

![Phyllum GUI-lowres](https://hackmd.io/_uploads/B1JRb7SoT.jpg)



### Menu functions related to the Phyllum GUI:

- **PhyllumGUI → Configure_interface_level**: This function allows the user to select between three different GUI modes: 'Basic', 'Advanced' and 'Developer'. The default mode is set to 'Basic', which offers limited curating functionality and shows a reduced amount of information about the recorded units in the *ClusterView* window. The 'Advanced' mode adds curating options, search functionality and extra information in the *ClusterView* window. Finally, the 'Developer' mode provides access to internal information used by the *Phyllum* algorithms and includes auxiliary functions used to debug the source code. Before quitting *Phyllum*, the GUI mode is saved and automatically selected next time the same file is opened. 

---

    
 
<a id="Curating"></a> 
# Curating
*Phyllum* includes a library of functions to facilitate the data curation process. These functions can be used (1) to identify and merge clusters that are likely to belong to the same unit, (2) to remove contamination introduced by the spikelets of complex spikes, (3) to manually override any decision taken by the *Phyllum* Layer ID algorithm about the layer of each channel, and (4) to add and edit notes about individual clusters.

## Merge clusters 
Automatic sorting can sometimes produce 'oversplitting', i.e. dividing the spikes belonging to a single unit among 2 or more separate clusters. For example, in the image below, *Kilosort* has separated the spikes belonging to a single unit into 3 separate groups (blue, red and green) and assigned each group of spikes to a different cluster ('Unit 1', 'Unit 2', and 'Unit 3'). During the curating process, it is necessary to identify 'oversplit' clusters and merge them together so that their spikes are all attributed to one single unit (see black histogram).

![Phyllum Merge_lowres](https://hackmd.io/_uploads/S1Q2LQrip.png)


*Phyllum* allows the user to automatize some of the steps in the manual process for merging clusters proposed in the [*phy* Clustering Guide](https://phy.readthedocs.io/en/latest/sorting_user_guide/#user-guide), using the functions described below. Briefly, the user is able to quickly identify clusters that are 'oversplit' and are candidates for 'merge' by searching neighboring channels for clusters with similar mean waveforms, templates, and correlograms (see example of template and autocorrelogram similarity for the 3 clusters in the figure above). For clusters selected for 'merge', *Phyllum* has options for removing duplicated spikes and doing waveform realignment. The user can choose different functions to run the steps of the 'merge' process either automatically or with manual supervision.

<!-- omit in toc -->
### Menu functions related to 'Merge':
-   **Curate → Merge → Use_template_wv_similarity**: This function sets the similarity metric used to identify 'oversplit' clusters to the default metric provided by *phy*. The value of this metric is shown in the *SimilarityView* window. It is a measure of the similarity between the template shapes corresponding to the different clusters. 
-   **Curate → Merge → Use_mean_wv_similarity**: This function sets the similarity metric used to identify 'oversplit' clusters to a waveform-based metric (see [*Writing a custom cluster similarity metrics*](https://phy.readthedocs.io/en/latest/plugins/#writing-a-custom-cluster-similarity-metrics) in the original *phy* documentation for a detailed description).
-   **Curate → Merge → Use_template_and_mean_wv_similarity**: This function sets the similarity metric used to identify 'oversplit' clusters to a metric that takes into account both the mean waveforms and the templates of the individual clusters. *Phyllum* uses this similarity metric by default.
- **Curate → Merge → Set_wv_similarity_threshold**: This function is used to set the waveform similarity threshold, i.e. the minimum waveform similarity value that clusters must have to be considered for 'merge'. The waveform similarity threshold must be set to a value between 1 (different clusters must have identical templates and/or mean waveforms to be considered for 'merge') and 0 (candidate 'merge' clusters can have templates and/or mean waveforms completely different from each other). The default waveform similarity threshold in *Phyllum* is 0.7.
- **Curate → Merge → Assisted_search** [shortcut → alt+q]: This function looks for candidate clusters to 'merge' with the cluster selected in the *ClusterView* window. Candidate 'merge' clusters must pass the correlogram similarity test (see Step 3 in [*Writing a custom cluster similarity metrics*](https://phy.readthedocs.io/en/latest/sorting_user_guide/#a-typical-approach-to-manual-clustering)) and have a higher than threshold waveform and/or template similarity value with the user-selected cluster in the *ClusterView* window. The candidate 'merge' clusters are displayed in the *SimilarityView* window.
-   **Curate → Merge → Sequential_search** [shortcut → alt+w]: This function iteratively looks for clusters to 'merge', starting with the cluster with the highest firing rate in the *ClusterView* window. Candidate 'merge' clusters must pass the correlogram similarity test (see Step 3 in [*Writing a custom cluster similarity metrics*](https://phy.readthedocs.io/en/latest/sorting_user_guide/#a-typical-approach-to-manual-clustering)) and have a higher than threshold waveform and/or template similarity value. The candidate 'merge' clusters are displayed in the *SimilarityView* window.
-   **Curate → Merge → Check_alignment** (Advanced mode): This function tests if the clusters selected for merging by the user have aligned waveforms. The result is displayed on the terminal: If zero, then the clusters have aligned waveforms and can be safely merged. Otherwise, the clusters have misaligned waveforms and should be merged with the function *Merge_and_clean_selected_clusters* (see below). 
- **Curate → Merge → Remove_duplicated_spikes** (Advanced mode): This function automatically removes duplicated spikes from *all* the clusters in the file. Duplicated spikes have a time difference less than 0.7 ms and occur when the same spike is erroneously assigned to two different clusters during the spike-sorting procedure. After the execution of this function, *Phyllum* closes and a message is displayed on the terminal with instructions for overwriting the original data.
- **Curate → Merge → Merge_selected_clusters** [shortcut → g]: This function performs a 'merge' of the selected clusters, as implemented in *phy* by pressing the key *"g"*.
-   **Curate → Merge → Merge_and_clean_selected_clusters** [shortcut → shift+g] (Advanced mode): This function performs a 'merge' of the clusters selected by the user, automatically realigning waveforms and removing duplicated spikes by calling the auxiliary function *Remove_duplicated_spikes* (see above). 
-   **Curate → Merge → Automatic_Merge** (Advanced mode): This function performs a 'merge' of all the clusters that pass the correlogram similarity test (see Step 3 in [*Writing a custom cluster similarity metrics*](https://phy.readthedocs.io/en/latest/sorting_user_guide/#a-typical-approach-to-manual-clustering)) and have a higher than threshold waveform and/or template similarity value. In addition, duplicated spikes are automatically removed by calling the auxiliary function *Remove_duplicated_spikes* (see above).
-   **Curate → Merge → Automatic_Merge_and_clean** (Advanced mode): This function performs a 'merge' of all the clusters that pass the correlogram similarity test (see Step 3 in [*Writing a custom cluster similarity metrics*](https://phy.readthedocs.io/en/latest/sorting_user_guide/#a-typical-approach-to-manual-clustering)) and have a higher than threshold waveform and/or template similarity value. In addition, the function automatically performs temporal alignment of all the waveforms belonging the different clusters and removes all duplicated spikes calling the auxiliary function *Remove_duplicated_spikes* (see above).







## Remove spikelets of complex spikes
The image below illustrates a common sorting problem for Purkinje cells, in which the spikelets of a complex spike (Cspk, indicated in red) are incorrectly assigned as simple spikes (SS, indicated in blue). *Phyllum* can detect this sorting error by examining the Cspk vs SS crosscorrelogram (CCG) of individual Purkinje cells. Instead of the well-known suppression of simple spikes that is always observed in the 10 ms after a complex spike, the CCG of an incorrectly sorted Purkinje cell will show spikelet contamination, i.e. the presence of simple spikes in the 5 ms following the complex spike. *Phyllum* can reduce contamination by performing a 'split' and moving spikes from spikelets into their own cluster.


![Phyllum Spikelet Removal_lowres](https://hackmd.io/_uploads/rk24o7rip.png)



### Menu functions related to 'Spikelet removal':

-   **Curate → Remove_Cspk_spikelets** (Advanced mode): The two clusters corresponding to an individual Purkinje cell's simple spikes and complex spikes must be selected before the execution of this function. The function opens a new window to allow the user to set the *“PC_pause_after_CSpk”* parameter, which establishes the minimum duration of the simple spike pause after a complex spike and is measured in seconds (default value = 0.007). All the simple spikes or complex spikes detected in the *“PC_pause_after_CSpk”* period will be moved to another cluster. 

## Discard Artifact Clusters
In some experiments, electrical artifacts can be incorrectly identified as spikes from a cluster. It is important to find these artifactual clusters and discard them.

### Menu functions related to 'Artifact removal':

-   **Curate → Discard_artifacts** (Advanced mode): This function allows the user to set the Signal to Noise Ratio (SNR) of artifactual clusters to 0. Because *Phyllum* defines a minimum SNR for clusters to be analyzed, this function ensures that artifactual clusters are discarded and excluded from further analysis.

<a id="Override_layer_info"></a>
## Override Layer Info
As detailed below in the section about [Layer Identification](##Layer_identification), *Phyllum* uses a two-step algorithm to automatically identify the layer of cerebellar cortex in which each contact of the recording probe was located: (1) the algorithm first searches among all the recorded clusters for 'anchor' units, i.e. neurons whose cell type and layer location can be unambiguously determined, and then (2) it implements an iterative 'auto-fill' procedure that assigns a layer ID to channels without 'anchor' units based on proximity analysis and allowed layer transitions. Both the 'anchor' type and layer ID are shown in the *ClusterView* window and if they are deemed to be incorrect, they can be manually overridden by the user using the functions described below. When the 'anchor' or layer ID are manually set or overridden, they are shown in a new column in the *ClusterView* window and, after saving them in a .tsv file, *Phyllum* automatically re-runs the Layer ID algorithm and updates the information displayed in all its windows. 

### Menu functions related to 'Override Anchor ID':

* **Curate → Override → Set_anchor_PC_SS_somatic** (Advanced mode): This function sets the 'anchor' type of all the selected units to somatic Purkinje cell simple spike (PC_SS).
* **Curate → Override → Set_anchor_DCS** (Advanced mode): This function sets the 'anchor' type of all the selected units to dendritic Purkinje cell complex spike (DCS), which are also commonly referred to as 'fat spikes'.
* **Curate → Override → Set_anchor_MFB** (Advanced mode): This function sets the 'anchor' type of all the selected units to mossy fiber bouton (MFB).
* **Curate → Override → Set_anchor_UNDEFINED** (Advanced mode): This function sets the 'anchor' type of all the selected units to 'Undefined'. 'Undefined' units are ignored by *Phyllum* during the execution of the layer identification algorithm.

### Menu functions related to 'Override layer info':

-   **Curate → Override → neuron_layer_ML** (Advanced mode): This function sets the cerebellar layer of all the selected units to ML, Molecular Layer.
-   **Curate → Override → neuron_layer_PCL** (Advanced mode): This function sets the cerebellar layer of all the selected units to PCL, Purkinje Cell Layer.
-   **Curate → Override → neuron_layer_GCL** (Advanced mode): This function sets the cerebellar layer of all the selected units to GCL, Granule Cell Layer.
-   **Curate → Override → neuron_layer_unknown** (Advanced mode): This function sets the cerebellar layer of all the selected units to 'Unknown'.
-   **Curate → Override → neuron_layer_not_cortex** (Advanced mode): This function sets the location of all the selected units to outside the cerebellar cortex.


## Add cluster notes
The following functions are defined in the 'Advanced' GUI mode, to help keep track and organize notes taken during the curating process. The notes are displayed in the *ClusterView* window and also stored in the file 'notes.tsv':

-   **Curate → Notes → Append_notes** (Advanced mode): This function opens a separate window for each selected cluster, allowing the user to create a new note or append a new comment to the previous note.
-   **Curate → Notes → Append_notes_all_selected** (Advanced mode): This function opens a single window, allowing the user to append the same comment to the previous notes of all the selected clusters.
-   **Curate → Notes → Edit_notes** (Advanced mode): This function opens a window for each selected cluster, allowing the user to edit old notes.
---

<a id="Layer_identification"></a>
# Layer Identification
*Phyllum* can be used to determine the specific cerebellar layer in which the individual contacts of the recording probe were located. The following sections provide information about (1) the *Phyllum* layer identification algorithm, (2) the performance of the algorithm, validated with histological analysis and reconstruction of probe tracks, and (3) the layer information displayed in the *ClusterView* window, and (4) the *CerebellarLayerView* window, which helps visualize the location of all the channels on the probe and their corresponding layer ID.

## Layer identification algorithm

### Step 1: Set 'anchor' units
The first step of the Layer ID algorithm relies on searching through all the recorded clusters and finding 'anchor' units whose cell type and layer location can be identified with high confidence. 

**Step 1.1: Select stable and well-isolated clusters**
The following metrics are used to assess the isolation and stability of individual clusters, by analyzing 100 blocks of 201 spikes each, distributed throughout the entire recording period.

-   *custom_best_ch*: channel number where the cluster has the largest amplitude in the mean waveform. Note that *"custom_best_ch"* may not be the same as *"ch"*, which is the main channel selected by *phy* using the waveform template. 
-   *wave_N_peaks*: number of local peaks observed in the mean waveform of the main channel (used to remove noisy units with a lot of peaks).
-   *SNR*: Signal to Noise Ratio computed as the mean of all the peak-to-trough amplitudes of the individual spike waveforms divided by the mean amplitude of the noise level before the spike (taken as the mean of all the peak-to-trough amplitudes in the first 10 samples of the 82 used to identify the waveform). 
-   *good_block_ratio*: an indepent *SNR* value is computed for each one of the 100 blocks of 201 spikes. This good_block_ratio indicates the percentage of blocks in which the local *SNR* is higher than the threshold value (1.5 for Kilosort 2.0 or 2.0 for Kilosort 2.5 or 3.0). 
-   *good_SNR*: This value is set to 'True' only if the *SNR* reaches the threshold value and the *good_block_ratio* is bigger than 0.5. Units in which good_SNR is set to 'False' are considered too noisy or unstable and excluded from further analysis.

**Step 1.2: Compute cluster metrics**
The following metrics are computed automatically for each cluster in which *good_SNR* is set to 'True' in Step 1.1:

-   *median_fr_local*: mean of all the median firing rates in the individual blocks of 201 spikes. 
-   *median_typical_ISI*: median interspike interval, considering only the 40% most frequently observed ISIs.
-   *minimum_typical_ISI*: minimum interspike interval, considering only the 40% most frequently observed ISIs. 
-   *peridiocity_ISI*: regularity of interspike intervals (ISI), determined by measuring the ratio between two consecutive ISIs independently of the instantaneous firing frequency (only the 40% most frequently observed ISIs are used).
-   *wave_neg_peak*: mean waveform aligned on most negative peak and normalized such that the value of the negative peak is -1.
-   *wave_pos_peak*: mean waveform aligned on most positive peak and normalized such that the value of the positive peak is +1.
-   *wave_norm_fft*: normalized frequency content of the mean waveform, measured as the power spectrum with fast Fourier transform (FFT).



**Step 1.3: Identify 'anchors'**
*Phyllum* automatically determines whether each cluster with *good_SNR*='True' is one of 3 'anchor' types: (1) A mossy fiber bouton (MFB) in the granule cell layer (GCL), (2) a dendritic complex spike (DCS) in the molecular layer (ML), or (3) a somatic Purkinje cell simple spike (PC_SS) in the Purkinje cell layer (PCL).

To identify 'anchors', *Phyllum* uses 3 separate Uniform Approximation and Projection (UMAP) and Gaussian Process classifiers trained with expert-labelled units in the *Phyllum* repository, which come from Neuropixels recordings in the mouse cerebellar cortex like the one shown in the image below. In these recordings, the 3 'anchor' types can be unambiguously identified: (1) MFB's shown in cyan have a unique triphasic waveform and can fire spikes in high frequency bursts of up to 1KHz, (2) DCS's shown in orange have a wide waveform and a low and irregular firing rate in the 1-2 Hz range, and (3) somatic PC_SS's shown in magenta have a negatively-peaked waveform and high firing rates of >40 Hz, which are interrupted briefly by complex spikes (Cspk), resulting in a Cspk-triggered pause of simple spikes (see arrowheads in the magenta crosscorrelograms of the image below). 

![Phyllum Anchors_lowres](https://hackmd.io/_uploads/ryQ638Bjp.png)


The probability that each cluster with *good_SNR*='True' is one of the 3 'anchor' types is computed by projecting the cluster metrics onto the 2D spaces of the 3 UMAP/Gaussian process classifiers: (1) MFB vs non-MFB space, (2) DCS vs non-DCS space, and (3) PC_SS vs non-PC_SS space. To be identified as an 'anchor', a cluster must have a probability of at least 70% for one specific 'anchor' type (and less than 30% for the other 2 'anchor' types), plus it must also pass a final set of sanity checks to make sure that the cluster metrics are within the known physiological range. In the case of PC_SS 'anchors' it is also necessary to exclude clusters that correspond to non-somatic Purkinje cell simple spikes, something that *Phyllum* achieves by checking the initial component of the waveform and excluding clusters with any signs of a small positivity before the main negative peak. It is important to note that *Phyllum*'s layer ID algorithm performs well even if not all MFB and DCS 'anchor' units are detected; on the other hand, detecting somatic PC_SS 'anchor' units is critical.


### Step 2: Assign layer ID to probe channels
The image below illustrates the procedure that *Phyllum* follows to assign a layer ID, either granule cell layer (GCL), molecular layer (ML), or Purkinje cell layer (PCL), to the different channels on the recording probe.

![Phyllum Layer Autofill](https://hackmd.io/_uploads/B1ofgTYoa.jpg)


**Step 2.1**: Contacts with a MFB 'anchor' unit are assigned to GCL.
**Step 2.2**: Contacts with a DCS 'anchor' unit are assigned to ML.
**Step 2.3**: Contacts with a PC_SS 'anchor' unit are assigned to PCL. 
**Step 2.4**: Contacts in the same row as a PC_SS 'anchor' are assigned to PCL.
**Step 2.5**: Contacts between two PC_SS 'anchors' separated <100 um are assigned to PCL (see (B, magenta) in image above).
**Step 2.6**: Contacts between two PC_SS 'anchors' separated >100 um are assigned to 'unknown' layer if none of them have any 'anchors' (see (B, red) in image above).
**Step 2.7**: Contacts between two PC_SS 'anchors' separated >100 um are assigned to ML if there is at least one DCS 'anchor' unit in them (see (C, orange) in image above).
**Step 2.8**: Contacts between two PC_SS 'anchors' separated >100 um are assigned to GCL if there is at least one MFB 'anchor' unit in them (see (D, cyan) in image above).
**Step 2.9**: Contacts lower than the deepest PC_SS 'anchor' are assigned to 'unknown' if none of them have any 'anchors'.
**Step 2.10**: Contacts higher than the most superficial PC_SS 'anchor' are assigned to 'unknown' layer if none of them have any 'anchors'.
**Step 2.11**: Contacts between the deepest PC_SS 'anchor' and 100 um lower than a deeper DCS 'anchor' unit are assigned to ML (see (D, orange) in image above). Contacts that are even lower are assigned to 'out of cortex' (see (D, blue) in image above).
**Step 2.12**: Contacts between the most superficial PC_SS 'anchor' and 100 um higher than a more superficial DCS 'anchor' unit are assigned to ML (see (D, orange) in image above). Contacts that are even more superficial are assigned to 'unknown' layer.
**Step 2.13**: Contacts between the deepest PC_SS 'anchor' and 100 um lower than a deeper MFB 'anchor' unit are assigned to GCL. Contacts that are even lower are assigned to 'out of cortex'.
**Step 2.14**: Contacts between the most superficial PC_SS 'anchor' and 100 um higher than a more superficial MFB 'anchor' unit are assigned to GCL. Contacts that are even more superficial are assigned to 'unknown' layer.
**Step 2.15**: Contacts between a MFB 'anchor' unit and a DCS 'anchor' unit are assigned to 'unknown' layer if none of them have a PC_SS 'anchor' unit (see (F, red) in image above). Given the geometry of the cerebellar cortex, direct transitions from ML to GCL (or vice versa) are impossible, and contacts flagged as 'undetermined' layer in this step should be carefully inspected to determine whether they may contain a somatic PC_SS 'anchor' unit that *Phyllum*'s layer ID algorithm may have failed to detect in Step 1 (see above).  



## Validation of *Phyllum*'s Layer ID algorithm

Assessing the performance of *Phyllum*'s Layer ID algorithm requires expert analysis of histological sections in which the trajectory of the Neuropixels probe is marked with a fluorescent dye during the recording experiment and accurately reconstructed and visualized postmortem under the microscope (see image below for 8 example histological sections of different regions of the cerebellar cortex with diverse layer organization, in which the GCL is deeply stained in cyan and the probe track is shown in red). Histological analysis of 21 Neuropixels probe tracks demonstrates that on average, *Phyllum*'s Layer ID assigns 82% of all the channels to a specific layer, and that the assignment is highly accurate: >99% correct for ML channels, >98% correct for GCL channels, and >95% for PCL channels. 
 
![Phyllum Histology Validation_lowres](https://hackmd.io/_uploads/HJHi89Hop.jpg)


## Layer information in *ClusterView* window
*Phyllum* displays the following layer-related information in separate columns of the *ClusterView* window. Some of this information can be overridden by the user using the functions described in the [Override Layer Info](##Override_layer_info) section above.

-   *PC_SS_probability* (Advanced mode): probability that the cluster is a somatic Purkinje cell simple spike as determined by the Gaussian process classifier on the PC_SS vs non-PC_SS UMAP space. 
-   *DCS_probability* (Advanced mode): probability that the cluster is a dendritic complex spike as determined by the Gaussian process classifier on the DCS vs non-DCS UMAP space.
-   *MFB_probability* (Advanced mode): probability that the cluster is a mossy fiber bouton as determined by the Gaussian process classifier on the MFB vs non-MFB UMAP space.
-   *PC_SS_distance* (Advanced mode): distance in µm from the cluster to nearest PC_SS 'anchor' unit.
-   *DCS_distance* (Advanced mode): distance in µm from the cluster to the nearest DCS 'anchor' unit.
-   *MFB_distance* (Advanced mode): distance in µm to the nearest MFB 'anchor' unit.
-   *Phyllum_anchor_type*: anchor type assigned by *Phyllum*'s Layer ID algorithm. 
-   *final_anchor_type*: final anchor type, assigned by *Phyllum*'s Layer ID algorithm or by the user during the curating process.
-   *Phyllum_layer_ID*: layer ID assigned by *Phyllum*'s Layer ID algorithm.
-   *final_layer_ID*: final layer ID, assigned by *Phyllum*'s Layer ID algorithm or by the user during the curating process.



## *CerebellarLayerView* window
The image below shows the *CerebellarLayerView* window, which can be used to visualize the location of all the channels on the Neuropixels probe and the layer assigned to them. In the collapsed view, channels with at least one recorded cluster are shown as squares of different colors corresponding to the layer in which each channel was located (GCL=cyan, ML=orange, PCL=magenta, unknown=red, out of cortex=blue). Channels without any  clusters are shown as gray dots. The extended view of the *CerebellarLayerView* window shows all the clusters recorded in every channel, with the square size corresponding to the signal-to-noise ratio of an individual cluster. The user can use the following shortcuts to select clusters directly on the extended *CerebellarLayerView* window, and all the analysis windows in the *Phyllum* GUI will be automatically updated with the data corresponding to the selected clusters: 
-   Shift + left click in a square: single cluster will be selected.
-   Ctrl + left click in a square: all the clusters in that channel will be selected.
-   Alt + left click in a square: If the cluster was not selected, it will be selected together with already selected clusters. If the cluster was selected, it will be deselected.
-   Alt + right click in square: This option is equivalent to “Alt + left click”, but the data for the selected cluster is not immediately updated in the analysis windows. The analysis windows are updated when the “Alt + left click” function is used. This option is very useful when the user wants to select or deselect multiple units without having to wait for all the windows to update with each new selection.

![Phyllum CerebellarLayerView_lowres](https://hackmd.io/_uploads/By-pQjLo6.png)


### Menu functions related to the *CerebellarLayerView* window:

- **View → CerebellarLayerView**: This function brings up the *CerebellarLayerView* window.
- **CerebellarLayerView:window → collapsed**: This function toggles the visualization of the *CerebellarLayerView* window between the expanded representation (one square per cluster) and the collapsed representation (one square per channel).
- **CerebellarLayerView:window → PCL**: This function is used to choose whether channels in the PCL should be displayed or not in the *CerebellarLayerView* window.
- **CerebellarLayerView:window → ML**: This function is used to choose whether channels in the ML should be displayed or not in the *CerebellarLayerView* window.
- **CerebellarLayerView:window → GCL**: This function is used to choose whether channels in the GCL should be displayed or not in the *CerebellarLayerView* window.
- **CerebellarLayerView:window → Out_of_cortex**: This function is used to choose whether channels out of the cerebellar cortex should be displayed or not in the *CerebellarLayerView* window.
- **CerebellarLayerView:window → Unknown**: This function is used to choose whether channels recorded in an unknown layer should be displayed or not in the *CerebellarLayerView* window.
- **CerebellarLayerView:window → min_SNR**: This function sets the minimum SNR value required for a cluster to be represented in the *CerebellarLayerView* window.
- **CerebellarLayerView:window → SNR_plot_scale**: This function is used to set the relationship between the SNR and the square size of the clusters in the *CerebellarLayerView* window.
- **CerebellarLayerView:window → Export**: This function generates a new vectorial and lossless version of the *CerebellarLayerView* window, which can be saved in multiple file formats and imported into any image editor for further processing.





---

<a id="Cell_search"></a>
# Cluster Search Functions 
---
Coming soon...

<a id="Visualization_tools"></a>
# Visualization and Exporting Tools 
---
Coming soon...


