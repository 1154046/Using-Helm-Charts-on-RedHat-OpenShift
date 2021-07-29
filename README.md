# Using-Helm-Charts-on-RedHat-OpenShift
Repo for Deploy Java-based applications using Helm Charts on Red Hat OpenShift 4 Webinar: Access the replay here: https://www.crowdcast.io/e/JavaAppHelmChart


## Creating Helm Charts from CLI.

### Prerequisites:
  - oc client( https://docs.openshift.com/container-platform/4.2/cli_reference/openshift_cli/getting-started-cli.html )
  - helm ( https://helm.sh/docs/intro/install/ )
  - Provision an OpenShift Cluster (https://helm-os.mybluemix.net/) where key=oslab and IBMid = Your email used to create IBM Cloud Account. You will receive an email with an invitation to join the Advowork Cloud Account, you need to confirm before having access to the cluster.

####
### Step 1: Log in to your OpenShift Cluster
Visit https://cloud.ibm.com/resources on the Advowork Account to view your cluster. Select the cluster, and click on "OpenShift Web Console" button(blue button). 

You want to click on your account email(top right corner) then "copy login command". This will generate a token for you to use to login to your cluster from your CLI. 

```
$ oc login YOUR_SECRET_LOGIN_TOKEN
```

####
### Step 2: Create an OpenShift project for Tiller
After installing the oc CLI tool, we can now run CLI commands against our cluster.
```
$ oc new-project tiller
$ oc project tiller
```

####
### Step 3: Install and verify Helm
After Installing helm, you want run the following command to verify the installation.

```
$ helm version
```

####
### Step 4: Install the Tiller Server

First we set up our Tiller Namespace Env variable.
#### Linux:
```
$  export TILLER_NAMESPACE=tiller 
```

#### Windows:
```
$  SETX TILLER_NAMESPACE tiller 
```

Then run the following command to install the server: 
```
$ oc process -f https://github.com/openshift/origin/raw/master/examples/helm/tiller-template.yaml -p TILLER_NAMESPACE="${TILLER_NAMESPACE}" -p HELM_VERSION=v2.9.0 | oc create -f -
```

####
### Step 5: Rollout your deployment
```
$ oc rollout status deployment tiller
```

####
### Step 5: Create a Separate project to install our Helm Chart
```
$ oc new-project myapp
```

####
### Step 6: Grant the Tiller server edit access to the current project
```
$ oc policy add-role-to-user edit "system:serviceaccount:${TILLER_NAMESPACE}:tiller"
```

####
### Step 7: Install a trusted Helm Chart
```
$ helm install https://github.com/jim-minter/nodejs-ex/raw/helm/helm/nodejs-0.1.tgz -n nodejs-ex
```

### Step 8: Verify installation in the OpenShift Console
Navigate to cluster and view your deployment.

####
####

## Creating Helm Charts from UI

####
### Step 1: Navigate to your OpenShift Console

Visit https://cloud.ibm.com/resources and click on your provisioned cluster under the Advowork Account

#### 
### Step 2: Create a new Project 
Click on Home -> Project in your sidebar, then create a new project. Give your project a name and a description.

#### 
### Step 3: Add a Helm Chart
Change account from "Administator" to "Developer". Click on "Add" on your sidebar then choose "Helm Chart". 

Scroll all the way down and select the Quarkus vX.X.X chart. Then click on install chart.

Should you wish to add an existing project repo, change the "configure via" option to "YAML view" and add a custom uri. You also have the option of integrating an existing landmark helm chart from https://github.com/IBM/landmark. 

#### Please note: The stable/progresql is depreciated, you may use bitnami/progresql as a possible alternative.

Click on install and give your project a few minutes to build and deploy. 

####
### Step 4: Verify Installation
As soon as the deploy finishes, you can click OpenURL button on your interface and view your sample Quarkus application running on OpenShift.

#### 
#### Thank you. 
#### Sbusiso Mkhombe 
#### (Sbusiso.Mkhombe@ibm.com)


