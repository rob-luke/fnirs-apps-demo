# fNIRS Apps Example

[*fNIRS Apps*](http://fnirs-apps.org) are standalone neuroimaging pipelines inspired by BIDS Apps.

This repository demonstrates how you can use fNIRS Apps to specify your complete analysis pipleline, from raw data to group level GLM statistical analysis.

This example utilises the boutiques framework.
[*Boutiques*](http://boutiques.github.io) is a descriptive command-line framework automating analysis.
It allows you to specify the app you wish to run and the parameters you wish to use.
Boutiques will validate your parameters before running the app, minimising the risk of error.
All *fNIRS Apps* are registered with boutiques, you can search for fNIRS apps using the `bosh` tool from *boutiques* by running:

```bash
➜  ~ bosh search 'fNIRS Apps'
```
which will return something like...
```bash
[ INFO ] Showing 4 of 4 result(s), exluding 0 deprecated result(s).
ID              TITLE                             DESCRIPTION                                
zenodo.5112758  fNIRS Apps: Sourcedata to BIDS    Create fNIRS BIDS datasets from source d...
zenodo.5112722  fNIRS Apps: Scalp Coupling Index  Compute Scalp Coupling Index and mark BI...
zenodo.5112773  fNIRS Apps: Quality Reports       Generate quality reports for fNIRS data    
zenodo.5113225  fNIRS Apps: GLM Pipeline          A GLM pipeline for fNIRS data              
```
which includes the app ID (which is automatically updated each time a new version is release),
the app names, and brief description.

We must also provide the parameters to be used with each app.
These are provided in the attached code as json files.

Combining the above information we would run a single app as:

```bash
bosh exec launch zenodo.5112758 01-raw-data-to-bids.json -v /path/to/data:/bids_dataset
```

In this example we run the following apps in turn with:

* Convert raw data in vendor format to a BIDS dataset  
  `bosh exec launch zenodo.5112758 01-raw-data-to-bids.json -v /path/to/data:/bids_dataset`
* Calculate the scalp coupling index for each channel and mark channels with SCI<0.7 as bad  
  `bosh exec launch zenodo.5112722 02-compute-sci.json -v /path/to/data/:/bids_dataset`
* Generate a data quality report for each file  
  `bosh exec launch zenodo.5112773 03-generate-quality-reports.json -v /path/to/data/:/bids_dataset`
* Execute a GLM analysis on the dataset, including a group level mixed effects analysis  
  `bosh exec launch zenodo.5113225 04-run-glm-analysis.json -v /path/to/data/:/bids_dataset`

This repository uses GitHub actions to automatically run this entire chain of analysis in the could.
It is run each time an app is updated to ensure it does not break.
You can view the analysis and output by clicking on the `Actions` tab at the top of the page.
