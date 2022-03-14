# Welcome to Dask tutorial session in Domino!

- The purpose of the quick start project is to go through some fundamental Dask parallel computing capabilities on the Domino platform.
- This Quick Start Project will be useful for data scientists who plan to leverage Dask in their projects.
- Before starting this Dask Quick Start project, it is recommended to have Domino 101 and Domino 201 completed.
- README.md file describes how to set up the environment and get started with Dask. Please take the following steps:

## Step 0: Setup the environments for workspace base and compute cluster
- For Domino 5.0, the validated and supported version of Dask is 2021.10.0.

- To create the workspace environment please refer to appendix A for more information.
- To create the cluster environment please refer to appendix B for more information.


## Step 1: Create the project and download the files 

- Open a new tab in the browser and navigate to the link below for each Jupyter notebook and download them:
    1. [calculatePi.ipynb](https://github.com/alireza-domino/Dask-Quick-Start-Domino/blob/main/1-Calculate-Pi.ipynb)
    2. [Dask-DataFrame.ipynb](https://github.com/alireza-domino/Dask-Quick-Start-Domino/blob/main/2-Dask-DataFrame.ipynb)
    3. [Dask-Quick-Start-Tutorial.ipynb](https://github.com/alireza-domino/Dask-Quick-Start-Domino/blob/main/3-Dask-ML-Tutorial.ipynb)
- Create Project Dask-QuickStart, hosted by Domino File System
- Upload the notebooks to the files section of your Dask-Quick-Start project.


## Step 2: Create the Workspace

- Navigate to the "Workspaces" section under the run tab  
- Create a new workspace by pressing the "+ Create New Workspace" on the top right side of the window (Don't hit "Launch Now" yet)
    1. In the "Environment & Hardware," 
        - Specify a name to the workspace
        - select/change the "Workspace Environment" dropdown to "Dask Workspace Environment" -pls refer to step "0"
        - Select the favorite "Workspace IDE"
        - Provide appropriate hardware tier
        - Click on the "Next" button
    2. In the Data tab don't modify anything for this project
        - Click on the "Next" button
    3. In the Computer Cluster (optional) select Dask
        - Select the min number of workers to "5" (This number can go as high as indicated limit)
        - For the Worker and Scheduler select the hardware tier (again it is not recommended modifying the default)
        - Select/Change the "Cluster Compute Environment" dropdown to "base Dask cluster environment" -pls refer to step "0"
        - Select the checkbox of dedicated storage(by default there should be a 30 GiB associated in front of the checkbox)
    4. Now hit the green "Launch" button (please expect around 5 min of launch time)


    
## Step 3: Launch the workspace

- Please find the following Jupyter notebook files:
    1. calculatePi.ipynb
    2. Dask-DataFrame.ipynb
    3. Dask-Quick-Start-Tutorial.ipynb
- First, let's run Notebook, calculatePi.ipynb. It aims to ensure that the environment is setup correctly and Domino Cluster is working properly.
- Second, let's run Notebook, Dask-DataFrame.ipynb. It aims to cover dataframes in Dask in general.
- Notebook, Dask-Quick-Start-Tutorial.ipynb aims to benchmark training under three conditions for the same problem:
    1. Train the model without Dask
    2. Train the model using Dask on the local Node (only a single machine)
    3. Train the model using Dask on the on-demand-clusters (using Dask workers and schedulers directly)
    

## Step 4: Dask Web UI in Domino

- On the top right within the domino page tab there should be an icon indicating: "Dask Web UI <Status>"
- Note: if the Status is not reading "Ready," let's wait until all Dask nodes get loaded to the compute environment.
- This tab provides all the details about the logs of the scheduler, worker nodes, and other Dask features.
- Other features such as Status, Workers, Tasks, System, Profile, Graph, Groups, Info, etc.


  
## By completing this project it is expected to:

- Know how to know Setup a project with proper Dask configuration such as the environment and so.
- Know how Domino on-demand cluster would benefit from the computation acceleration.
- Be ready to implement your own custom Dask project and run it with a variety of hardware settings.


## Appendix A: creating a workspace environment


### create an environment and name it: 
- "Dask Workspace Environment"
### for the base image select: 
- quay.io/domino/standard-environment:ubuntu18-py3.8-r4.1-domino5.0
### for supported cluster: 
- None

### docker instruction is as follows:
```
USER root
ENV DASK_VERSION=2021.10.0
ENV DASK_ML_VERSION=2022.1.22

RUN pip install dask[complete]==$DASK_VERSION \
                dask-ml[complete]==$DASK_ML_VERSION \
                blosc==1.10.2 \
                lz4==3.1.3 \
                msgpack==1.0.2 \
                numpy==1.21.1 \
                pandas==1.3.0 \
                toolz==0.11.1 

USER ubuntu
```


### Plugable Workspace Tools are as follows:

```
jupyter:
  title: "Jupyter (Python, R, Julia)"
  iconUrl: "/assets/images/workspace-logos/Jupyter.svg"
  start: [ "/var/opt/workspaces/jupyter/start" ]
  httpProxy:
    port: 8888
    rewrite: false
    internalPath: "/{{ownerUsername}}/{{projectName}}/{{sessionPathComponent}}/{{runId}}/{{#if pathToOpen}}tree/{{pathToOpen}}{{/if}}"
    requireSubdomain: false
  supportedFileExtensions: [ ".ipynb" ]
jupyterlab:
  title: "JupyterLab"
  iconUrl: "/assets/images/workspace-logos/jupyterlab.svg"
  start: [  /var/opt/workspaces/Jupyterlab/start.sh ]
  httpProxy:
    internalPath: "/{{ownerUsername}}/{{projectName}}/{{sessionPathComponent}}/{{runId}}/{{#if pathToOpen}}tree/{{pathToOpen}}{{/if}}"
    port: 8888
    rewrite: false
    requireSubdomain: false
vscode:
  title: "vscode"
  iconUrl: "/assets/images/workspace-logos/vscode.svg"
  start: [ "/var/opt/workspaces/vscode/start" ]
  httpProxy:
    port: 8888
    requireSubdomain: false
rstudio:
  title: "RStudio"
  iconUrl: "/assets/images/workspace-logos/Rstudio.svg"
  start: [ "/var/opt/workspaces/rstudio/start" ]
  httpProxy:
    port: 8888
    requireSubdomain: false
```

## Appendix B: creating base Dask cluster environment

### specify your convention name such as: 
- "base Dask cluster environment"
### base image is: 
- ```daskdev/dask:2021.10.0```
### supported clusters: 
- Domino managed Dask
### dockerfile instructions:
```
ENV DASK_ML_VERSION=2022.1.22
RUN pip install dask-ml[complete]==$DASK_ML_VERSION
```

__

For more information about on-demand Dask please check out [Domino documentation](https://docs.dominodatalab.com/en/5.0.1/reference/dask/On_demand_dask_overview.html). On the Domino's official Doc's webpage please ensure that correct Domino version is selected.
