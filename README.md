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



## Step 2: 

<br/> <br/> 


<img src=""/>

<br/> <br/> 


<img src=""/>

<br/> <br/> 


<img src=""/>

<br/> <br/> 


<img src=""/>

<br/> <br/> 


<img src=""/>

<br/> <br/> 


<img src=""/>

<br/> <br/> 


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
   <br/>   <br/> 
<img src=""/>



