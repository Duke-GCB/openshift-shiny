# openshift-shiny
Allows a user to simply deploy a R Shiny app to an OpenShift cluster.
This is accomplished by providing OpenShift compatible Docker images and OpenShift Templates for running shiny apps.
The docker images add OpenShift specific setup to [rocker/shiny* images](https://github.com/rocker-org/rocker-versioned2).

## Prerequisites
- Shiny R code stored in a git repository

## Steps to Deploy your code

### Create a Dockerfile in your git repo
Add your Rcode and requirements in a file named `Dockerfile`.
This Dockerfile should extend a dukegcb/openshift-shiny* image (such as `dukegcb/openshift-shiny-verse:4.0.2`).
For example if your R shiny code is under directory named `src` and you want to install the [here shiny package](https://github.com/jennybc/here_here) create a file named `Dockerfile` with the following contents:
```
FROM dukegcb/openshift-shiny-verse:4.0.2
RUN install2.r here
ADD ./src /srv/code
```
The `install2.r` script is a simple utility to install R packages.

Another example can be seen at [examples/hello-shiny/Dockerfile](examples/hello-shiny/Dockerfile).

### Deploy your shiny app
In this step we will run an OpenShift template to deploy your shiny app.
The following steps should be performed from the OpenShift console:
- Create a new project and lock onto it
- In the top right corner Click "Add To Project" then "Import YAML/JSON" - this will open up a "Import YAML/JSON" dialog
- Copy the contents of [openshift/shiny-server.yaml](openshift/shiny-server.yaml) and paste it into the "Import YAML/JSON" dialog
- Click "Create
- Leave "Process the template" checked and click "Continue"
- Update the parameters that are appropriate for your app. Minimally set APP_GIT_URI to your git repo location and REPO_DOCKERFILE_PATH to your dockerfile path location.
- Click "Create"
