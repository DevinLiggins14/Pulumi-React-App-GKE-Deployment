<table>
  <tr>
    <td><h1>Pulumi-React-App-GKE-Deployment</h1></td>
    <td>
      <p align="right">
        <img src="https://www.pulumi.com/logos/brand/avatar-on-black.svg" alt="pulumi" width="60" height="60"/> 
        <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/react/react-original.svg" alt="react" width="60" height="60"/> 
        <img src="https://www.vectorlogo.zone/logos/kubernetes/kubernetes-icon.svg" alt="kubernetes" width="60" height="60"/> 
        <img src="https://www.vectorlogo.zone/logos/google_cloud/google_cloud-icon.svg" alt="gcp" width="60" height="60"/> 
        <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original-wordmark.svg" alt="docker" width="60" height="60"/>
      </p>
    </td>
  </tr>
</table>


<h2>Description</h2>
<br/> 
In this project we will deploy a React application to GKE and we will use Pulumi to write the infratructure as code. JavaScript will be the language used for writing our code to deploy infrastructure. We will create our GK cluster, deployment object, and a load balancer service for deploying our react application on Google Kubernetes Engine.
<br />
<br/> Project Architecture: <br/>
<img src="https://github.com/user-attachments/assets/a34c5b64-9ce4-4139-baba-3a0254a6d60f"/>
<br/> 
There will be a react application directory which will have source, app, and docker files. For Pulumi the code will be written in JS and there will be a kubernetes provider along with the GCP provider. GKE will create the cluster and kubernetes will be used to create the deployment object and load balancer service.

<br/> <br/> 

We will containerize the application with a `Dockerfile` and run docker build to create a Docker image. From there we will push our container to the `GCR (Google Container Registry)` and use the image url to deploy the docker image on `GKE` using the deployment object. The `deployment object` in kubernetes will be defined within the `Pulumi` code. Then to expose the application we will use a `load balancer service` and map it to the port of our container
  <br/>

<h2> Services involved: </h2>

| **Service**            | **Description**                                                                                   |
|-------------------------|---------------------------------------------------------------------------------------------------|
| **React**              | Frontend framework for building the application.                                                  |
| **Docker**             | Used to containerize the React application with a `Dockerfile`.                                   |
| **Google Container Registry (GCR)** | Stores the Docker image for deployment.                                                         |
| **Google Kubernetes Engine (GKE)**  | Hosts the Kubernetes cluster for deploying and managing the React app.                            |
| **Pulumi**             | Infrastructure as Code tool to provision GKE and Kubernetes resources using JavaScript.           |
| **Kubernetes**         | Manages the application deployment with a `Deployment Object` and exposes it via a `Load Balancer Service`. |
| **GCP Providers**      | Pulumi's provider for managing GCP resources like GKE and GCR.                                     |




<p align="center">
  
### **Prerequisites**  
- Have an [Google Cloud Account](https://cloud.google.com/)
- Rhel 9 Linux Server or equivalent   


 ##  Step 1: Test the application

   <br/> Before moving the `Docker image` to GCR I will build a docker image from the Dockerfile and run the application locally to test that it's working properly. `git clone` can be run to follow along with this repository  <br/> 

<img src="https://github.com/user-attachments/assets/44ffd9c5-195d-4ab2-bfb3-ea43546d31ef"/>
<br/>  Here is the containerized react application running in my local browser   <br/>


https://github.com/user-attachments/assets/4337a9aa-9cad-49dd-8224-ff06dc42523d



## Step 2: Push the Docker Image to GCR (Google Container Registry)

<br/> Now we need to push the `docker image` to `GCR` so that it can be deployed on the `GKE cluster` later. However since the container registry is depreciated from within GCP we will use the artifact registry instead. We will need to tag the images and  Click on create repository and choose docker  <br/> 

<br/> Install [gcloud CLI](https://cloud.google.com/sdk/docs/install)  and authenticate in the steps below to avoid permission errors <br/> 

```Bash
# Build the Docker image
docker build -t us.gcr.io/$GCP_PROJECT_ID/react-todolist-app .

# Authenticate GCP (In browser)
gcloud auth login

# Change project
gcloud config set project PROJECT_ID

# Push the image to GCR
docker push us.gcr.io/$GCP_PROJECT_ID/react-todolist
```

<img src="https://github.com/user-attachments/assets/4dfe7ace-16d1-45d8-9f21-78cf52e8b92e"/>
<img src="https://github.com/user-attachments/assets/c9f86a16-346c-4332-9cb5-ede9ff997d9a"/>

https://github.com/user-attachments/assets/a5ceabaa-a1c0-4c95-a1a4-35f19e3318e2



## Step 3: Start writing and configure Pulumi scripts

<br/> Create a directory for the Pulumi scripts. Then initialize a pulumi `gcp-javascript` template for it.  <br/> 

<br/> Enter the GCP project ID <br/> 


<img src="https://github.com/user-attachments/assets/31e0e7aa-ee17-47aa-adc9-d4cb1d56b123"/>
<img src="https://github.com/user-attachments/assets/3b953a19-a1ea-43ef-a9c1-5cba7d5b497a"/>


<br/> Next we need to install a Pulumi kubernetes dependency because we will need a package for kubernetes to deploy the kubernetes object and load-balancer service.  <br/> 


```Bash
# Run to install kubernetes package
npm install @pulumi/kubernetes
# Set location
pulumi config set gcp:location us-central1-c
# Confirm
cat Pulumi.dev.yaml
```

<img src="https://github.com/user-attachments/assets/9de4aae1-5db5-4ab4-9355-4c280dc5ce5d"/>

<br/> Next configure the index.js file for to require kubernetes. Also add the gcp variable and location defined earlier <br/>



https://github.com/user-attachments/assets/8503f680-ed01-4413-8f6d-62565576e994




<img src="https://github.com/user-attachments/assets/df03ca7a-3b6c-40a8-aafd-71fef77c9cc6"/>

<br/> Next will be to write the code to create a `GKE Cluster` in GCP <br/> 

```js
// Create a GKE Cluster
const cluster = new gcp.container.Cluster("app-cluster", {
  name: "app-cluster",
  location: zone,
  initialNodeCount: 3,
  minMasterVersion: "latest",
  nodeConfig: {
    machineType: "g1-small",
    diskSizeGb: 32,
  },
});

exports.clusterIP = cluster.endpoint
```


<br/> Now finally run the `pulumi up` command to provision the cluster <br/> 


<img src="https://github.com/user-attachments/assets/b387a9f0-ebe7-46b0-82c0-26d94cbce371"/>

<br/> quick error I will solve by adding the project ID to the Application Default Credentials (ADC) so pulumi will be able to load my application credentials.   <br/> 

<br/> Run `gcloud auth application-default set-quota-project <project-ID>` to add credentials and `cat /root/.config/gcloud/application_default_credentials.json` to confirm. Then run `pulumi up` again
<br/>



https://github.com/user-attachments/assets/a8d1cd95-0d5b-4021-bc69-636f7f83dfb1


## Step 3: Write the configurations in pulumi for the load balancer service and deployment object   

<br/> Once the the Cluster is created return to the index.js file and configure the GKE Cluster to the kubernetes provider so that deployments and onjects can be within the created Cluster. Since the cluster was created using Pulumi we must make changes to the files instead of using command line access directly so it will be easier to scale and manage. I  will make changes to the index.js file now based on the [pulumi GKE documentation](https://www.pulumi.com/registry/packages/kubernetes/how-to-guides/gke/) to create the kubeconfig.  <br/>

<br/> I will also need to define the kubernetes provider <br/> 


```js
// Kubeconfig
const kubeconfig = pulumi.
    all([ cluster.name, cluster.endpoint, cluster.masterAuth ]).
    apply(([ name, endpoint, masterAuth ]) => {
        const context = ${gcp.config.project}_${gcp.config.zone}_${name};
        return apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: ${masterAuth.clusterCaCertificate}
    server: https://${endpoint}
  name: ${context}
contexts:
- context:
    cluster: ${context}
    user: ${context}
  name: ${context}
current-context: ${context}
kind: Config
preferences: {}
users:
- name: ${context}
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      command: gke-gcloud-auth-plugin
      installHint: Install gke-gcloud-auth-plugin for use with kubectl by following
        https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
      provideClusterInfo: true
;
    });

// Create a Kubernetes provider instance that uses our cluster from above.
const clusterProvider = new k8s.Provider("k8s-provider", {
  kubeconfig: kubeconfig,
});
```

This  generates a `kubeconfig` within index.js for a Kubernetes cluster managed by Pulumi.  
It uses the cluster's `name`, `endpoint`, and authentication details (`masterAuth`) to construct a configuration string in YAML format.  

The `kubeconfig` includes cluster information like `certificate-authority-data` and `server`, along with contexts that define the `cluster`, `user`, and `current-context`.  
It also specifies user credentials for authentication, leveraging the `gke-gcloud-auth-plugin` for integration with GKE.  

Finally, a Kubernetes provider instance (`clusterProvider`) is created using this `kubeconfig`, enabling Pulumi to manage Kubernetes resources within the specified cluster. Now each deployment will be associated with the GKE cluster created.


<br/> Now to create a deployment for the application that uses the docker image within GCR artifact registry and a load balancer kubernetes service to expose the port the container the will publically run on  <br/> 

```js
"use strict";
const pulumi = require("@pulumi/pulumi");
const gcp = require("@pulumi/gcp");
const k8s = require("@pulumi/kubernetes");

const gcpConfig = new pulumi.Config("gcp");
const zone = gcpConfig.require("zone")

// Create a GKE Cluster
const cluster = new gcp.container.Cluster("app-cluster", {
  name: "app-cluster",
  location: zone,
  initialNodeCount: 3,
  minMasterVersion: "latest",
  nodeConfig: {
    machineType: "g1-small",
    diskSizeGb: 32,
  },
});

exports.clusterIP = cluster.endpoint

// Kubeconfig
const kubeconfig = pulumi.
    all([ cluster.name, cluster.endpoint, cluster.masterAuth ]).
    apply(([ name, endpoint, masterAuth ]) => {
        const context = `${gcp.config.project}_${gcp.config.zone}_${name}`;
        return `apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: ${masterAuth.clusterCaCertificate}
    server: https://${endpoint}
  name: ${context}
contexts:
- context:
    cluster: ${context}
    user: ${context}
  name: ${context}
current-context: ${context}
kind: Config
preferences: {}
users:
- name: ${context}
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      command: gke-gcloud-auth-plugin
      installHint: Install gke-gcloud-auth-plugin for use with kubectl by following
        https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
      provideClusterInfo: true
`;
    });

// Create a Kubernetes provider instance that uses our cluster from above.
const clusterProvider = new k8s.Provider("k8s-provider", {
  kubeconfig: kubeconfig,
});

// Create a deployment for the application
const reactAppLabels = {
  app: "react-todo-app"
}
const deployment = new k8s.apps.v1.Deployment("app-deployment",{
  metadata:{
    name: "app-deployment"
  },
  spec:{
    replicas: 1,
    selector:{
      matchLabels: reactAppLabels
    },
    template:{
      metadata:{
        labels: reactAppLabels
      },
      spec:{
        containers: [
          {
            name: "react-app-container",
            image: "us.gcr.io/${GCP_PROJECT_ID}/react-todo-list-app",
            ports: [
              {
                containerPort: 80
              }
            ]
          }
        ]
      }
    }
  }

},{provider: clusterProvider})

const service = new k8s.core.v1.Service("app-service",{
  metadata:{
    name: "react-app-lb-service"
  },
  spec:{
    type: "LoadBalancer",
    ports:[
      {
        port: 80,
        targetPort: 80
      }
    ],
    selector: reactAppLabels
  }
},{provider: clusterProvider})

exports.servicePublicIP = service.status.apply(s=>s.loadBalancer.ingress[0].ip)
```

This code demonstrates how I can use Pulumi to manage the deployment of my React application on Google Kubernetes Engine (GKE). It begins by creating a GKE cluster named `app-cluster` with three nodes in the specified zone, ensuring the infrastructure is tailored to the project's needs. I then generate a kubeconfig file dynamically, allowing me to authenticate and interact with the cluster seamlessly. Using this configuration, I set up a Kubernetes provider, which serves as the foundation for managing resources within the cluster.  

To deploy my React application, I define a Kubernetes deployment that uses a pre-built Docker image stored in Google Container Registry (GCR). This ensures my application is packaged and ready for scaling within the cluster. Finally, I create a Kubernetes service of type `LoadBalancer` to expose the application publicly. The service provides an external IP address, making it accessible to users, and this IP is exported for easy reference in the project.  



## Step 4: Deploy the application 

<br/> Now I will run `pulumi up` to deploy the application as a container on the GKE Cluster and access it using the cluster IP <br/> 

<img src=""/>
<img src=""/>
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>
   <br/>   <br/> 
<img src=""/>



