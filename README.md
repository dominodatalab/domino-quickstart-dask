# Welcome to Dask tutorial session in Domino!

- The purpose of this quick start project is to go through some fundamental Dask parallel computing capabilities on the Domino platform.
- This Quick Start Project will be useful for data scientists who plan to leverage Dask in their projects.
- Before starting this Dask Quick Start project, it is recommended to have Domino 101 and Domino 201 completed.
- This README.md file describes how to set up the environment and get started with Dask. Please take the following steps:

## Step 0: Setup the environments for workspace base and compute cluster

Before launching a Dask cluster in Domino, you will need to have two Compute Environments set up:
- "Dask Workspace Environment" (please refer to appendix A for more information).
- "Dask base cluster environment" (please refer to appendix B for more information).

You may want to check with your Domino admin to see if suitable Dask environments are already available before creating your own, or ask them to follow the instructions in appendix A and B to create "Global" environments so that any users can make use of them.

## Step 1: Create the project and download the files 

This project consists of this README file and three tutorial notebooks, hosted at https://github.com/dominodatalab/domino-quickstart-dask. Follow these steps to create your Dask quick-start project in Domino:
1. Create a new Project named "Dask-QuickStart" (or whatever name you like), hosted by Domino File System.
2. Download the files from this repo, then upload them to the Files section of your project.
    - `1-Calculate-Pi.ipynb`
    - `2-Dask-DataFrame.ipynb`
    - `3-Dask-ML-Tutorial.ipynb`
    - `README.md`

If you are reading this README in a Domino project that has been shared with you, simply Copy the project.

## Step 2: Launch a workspace with attached On-Demand Dask cluster

- Navigate to the "Workspaces" section of your project
- Create a new workspace by pressing the "+ Create New Workspace" on the top right side of the window and give the following configuration:
  1. In "Environment & Hardware" 
      - (optional) Specify a name to the workspace
      - Choose your "Dask Workspacce Environment" from the "Workspace Environment" dropdown (refer to step 0 in this README)
      - Select Jupyterlab under "Workspace IDE"
      - (optional) Choose a Hardware Tier - we recommend staying with the default, typically 1 core and 4GiB RAM
      - Click on the "Next" button
  2. In "Data"
      - No need to modify anything here; click on the "Next" button
  3. In "Compute Cluster"
      - Select Dask
      - Set the min number of workers to "3" (do not select Auto-scale workers if it is available)
      - (optional) Choose a Hardware Tier for the Worker and Scheduler - again we recommend staying with the default
      - Choose "Dask base cluster environment" from the "Cluster Compute Environment" dropdown (refer to step 0 in this README)
      - (optional) Select the checkbox of dedicated storage (we will not make use of this in the example notebooks)
- Now hit the green "Launch" button

    
## Step 3: Check the Dask Web UI for cluster readiness and run the sample notebooks

- On the top right within the domino page tab there should be an icon indicating: "Dask Web UI [Status]"
- Note: if Domino needs to provision new nodes to accommodate your Dask cluster, the Status may not read "Ready" immediately.
- This tab provides all the details about the logs of the scheduler, worker nodes, and other Dask features. We recommend paying particular attention to the following:
  - **Workers**: Note that in some circumstances, not all workers may be ready when the Dask cluster first shows as Ready. This may happen if there is room on existing nodes for your Workspace and the Dask Scheduler, but not for all of the worker nodes. If they are just waiting for new nodes to be provisioned, they should come up in a few minutes, and you should see them join the list.
  - **Task Stream**: There are a few places in the example notebooks where we recommend looking at the Task Stream side by side with running the code, so that you can get some intuition for what the Dask workers are really doing during that time.
  - **Info**: This page contains a link to the **logs** for each worker, which are extremely useful for troubleshooting package installation issues or any other error that happens on the Dask workers, where the errors are not clear from the messages in the notebook alone.
- Once you have verified on the Dask Web UI that your cluster is ready, run each of the sample notebooks.
  - `1-Calculate-Pi.ipynb`: Example of Embarassingly Parallel workloads in Dask, which will also test the basic compute environment and cluster setup.
  - `2-Dask-DataFrame.ipynb`: General introduction to Dask Dataframes.
  - `3-Dask-ML-Tutorial.ipynb`: Example of clustering analysis with Dask ML, which will also test the installation of `dask-ml` in the compute environment.

## By completing this project it is expected to:

- Know how to set up a project with proper Dask configuration, including the required Compute Environments.
- Understand how to write code that runs on Domino On-Demand Dask clusters.
- Be ready to implement your own custom Dask project and use the Dask Web UI to aid your development efforts.

## Appendix A: creating a Dask workspace environment

For a general introduction to Compute Environments in Domino, see the [Domino documentation](https://docs.dominodatalab.com/en/5.0/reference/environments/index.html) (be sure to select your current Domino version). To create a Dask workspace environment for use with this project simply follow these steps, adapted for our specific example from the [Domino Dask documentation](https://docs.dominodatalab.com/en/latest/reference/dask/Configuring_prerequisites.html):

1. Create a new Environment and name it "Dask Workspace Environment" (or a name of your choice)
2. For the base image select "custom image" and enter `quay.io/domino/standard-environment:ubuntu18-py3.8-r4.1-domino5.0`
3. Do not select "Dask" as a supported cluster; that is only needed for the cluster environment (see appendix B)
4. Edit visibility if desired, then create the environment.
5. Edit the Dockerfile, then add the following to the Dockerfile instructions. Note that only `dask` and `dask-ml` are strictly required for our examples, but installing the specified versions of the remaining packages will prevent warnings about version mismatch, and reduce the chance of future errors.
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
5. Add the following to the Pluggable Workspace Tools.
    ```
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
    ```
6. Click "Build".

## Appendix B: creating a Dask base cluster environment

1. Create a new Environment and name it "Dask base cluster environment" (or a name of your choice)
2. For the base image select "custom image" and enter `daskdev/dask:2021.10.0`
3. Select "Dask" under "Supported Clusters"
4. Edit visibility if desired, then create the environment.
5. Edit the Dockerfile, then add the following to the Dockerfile instructions. 
    ```
    ENV DASK_ML_VERSION=2022.1.22
    RUN pip install dask-ml[complete]==$DASK_ML_VERSION
    ```
6. Click "Build".

__

For more information about on-demand Dask please check out the [Domino documentation](https://docs.dominodatalab.com/en/5.0.1/reference/dask/On_demand_dask_overview.html).
This quick-start was developed and tested on Domino 5.0, so please select your Domino version on the docs site to see the most up-to-date Dask reference for your version.
- For Domino 5.0, the validated and supported version of Dask is 2021.10.0. This is likely to work on other versions as well, but please check the docs for your Domino version to find the official supported version of Dask.
