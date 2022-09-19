# Deploy an Azure Kubernetes Service cluster using the Azure CLI
Azure Kubernetes Service (AKS) is a managed Kubernetes service that lets you quickly deploy and manage clusters. In this lab, you will:

Deploy an AKS cluster using the Azure CLI.<br>
Run a sample multi-container application with a web front-end and a Redis instance in the cluster.<p>

# The Azure CLI Commands Used in this Lab
Firstly, I created a Resource Group that will house the AKS Cluster. A Resource Group on Azure is the logical folder that holds your resources in one place
for ease of management. 

The commands to create the resource are: *az group create --name myResourceGroup --location  <YourPreferredLocation>*

Secondly, I created Azure AKS cluster using: 
*az aks create -g rg1 -n imohcluster  --enable-managed-identity --node-count 1 --enable-addons monitoring --enable-msi-auth-for-monitoring  --generate-ssh-keys*
   
 <img src ="https://github.com/imohweb/deploy-azure-voting-app-using-redis-yaml/blob/master/images/Create%20%20AKS%20Cluster.png">

In the az aks create command above, I added *--enable-addons monitoring* and *--enable-msi-auth-for-monitoring* parameter 
to enable Azure Monitor Container insights with managed identity authentication (preview).
  

Thirdly, Install the kubectl command on my local PC by running *az aks install-cli* command. Installing the kubectl cli enables connection to the AKS Cluster on Azure.

  <img src ="https://github.com/imohweb/deploy-azure-voting-app-using-redis-yaml/blob/master/images/Install%20Kubectl%20Cli.png">

In the fourth step, I added the kubectl to my local system path so when I run *kubectl.exe* the system will recognize it. 
You can do that by following the prompt on the screen while installing the kubectl cli in step 3 above. Usually, the path to add will look similar to
this *C:\Users\username.azure-kubectl* and then, run *$env:path += 'C:\Users\imoh_\.azure-kubelogin'* in the PowerShell or Git Bash Terminal.
 
<img src ="https://github.com/imohweb/deploy-azure-voting-app-using-redis-yaml/blob/master/images/Add%20to%20system%20path.png">

In the fifth step, I configured *kubectl* to connect to your Kubernetes cluster using the *az aks get-credentials* command as shown below:
*az aks get-credentials --resource-group myResourceGroup --name myAKSCluster*. 

If you run the *kubectl config get-contexts* command, you will be able to see the node in which your cluster is running on. 
Also, you can run *kubectl get nodes* to returns a list of the cluster nodes.

<img src ="https://github.com/imohweb/deploy-azure-voting-app-using-redis-yaml/blob/master/images/kubectl%20get%20nodes%20command.png">

Now using the vim editor, I created the yaml file called *azure-vote.yaml* and added the Yaml File content as defined in 
the Microsoft Official docs. See the <a href="https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-cli" target="_blank">Microsoft Official Docs</a>
  
<img src ="https://github.com/imohweb/deploy-azure-voting-app-using-redis-yaml/blob/master/images/Create%20Yaml%20File.png">

# Deploy the Application 
Here, after defining the Yaml file run the *kubectl apply -f azure-vote.yaml* to deploy the application. Note: This can take a little while to deploy depending on your internet speed.
If it's throws *connection timed out error* keep trying it. 
Your output should look like the screenshot below:
  
  <img src ="https://github.com/imohweb/deploy-azure-voting-app-using-redis-yaml/blob/master/images/Kubectl%20apply%20the%20Yaml%20file.png">
  
 Run *kubectl get pods* to see the list of running pods in your cluster. <br>
 <img src ="https://github.com/imohweb/deploy-azure-voting-app-using-redis-yaml/blob/master/images/Run%20Kubectl%20get%20pods.png">
 
 To see the Azure Vote app in action, copy the external IP Address and run on your browser.<br>
 <img src ="https://github.com/imohweb/deploy-azure-voting-app-using-redis-yaml/blob/master/images/Testing%20the%20Ext%20IP%20-%20.png">
 
 
The UI of the app should look exactly as shown below:
  
 <img src ="https://github.com/imohweb/deploy-azure-voting-app-using-redis-yaml/blob/master/images/Azure%20Voting%20App%20UI%20.png">

# Delete the cluster
To avoid Azure charges, if you don't plan on keeping the Voting App, clean up your unnecessary resources. 
Use the az group delete command as shown below to remove the resource group, container service, and all related resources.

*az group delete --name myResourceGroup --yes --no-wait*
