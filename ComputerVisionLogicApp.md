## Before you start - Prerequisites
- Azure Account (this tutorial assumes you have the Azure Pass Sponsorship Subscription but any subscription will work). 
- Storage Account and Cosmos DB as described in [Lab 1: Technical Requirements](https://github.com/MicrosoftLearning/AI-100-Design-Implement-Azure-AISol/blob/master/Lab1-Technical_Requirements/02-Technical_Requirements.md)

## Describe images from Blob Storage with Computer Vision through a Logic App
We will create a Logic App with which we can automate a workflow. The workflow we'll create is to detect new images in our Blob Storage, generate descriptions and tags with the Computer Vision API and store this metadata in Cosmos DB. We will create a container to store the metadata in Cosmos DB first, then we'll create the Logic App, and finally we'll add images to our Blob Storage to trigger our Logic App and be able to review the metadata in Cosmos DB. 

### Create a container in Cosmos DB
1. Go to your previously created Azure Cosmos DB in the Azure portal.
2. In Overview, click on **+ Add Container** in the top bar.
<img src="/media/LogicApp1.png" alt="drawing" width="800"/>

### Create a Logic App
1.	Open the Azure Portal.
2.	Select **+ Create a Resource** and then enter **logic app** in the search box.
3.	Choose **Logic App** from the available options, then select **Create**.
4.	Select your **Azure Pass – Sponsorship** for Subscription.
5.	Use your existing resource group.
6.	Give your Logic App a name of your own choosing.
7.	Select the location should remain at Region.
8.	Choose a Location nearest to you: for example West Europe.
9.	Keep Log Analytics Off. 
10.	Click on **Review + Create**.
11.	Click on **Create**. 
<img src="/media/LogicApp2.png" alt="drawing" width="800"/>

1. Once deployment is complete. Navigate to your new Logic App. 
1. Scroll down and under Templates, click on **Blank Logic App**. 
<img src="/media/LogicApp3.png" alt="drawing" width="800"/>

First thing we have to do in our Logic Apps Designer, is create a trigger. 
1. Search for “blob” and select **When a blob is added or modified**
<img src="/media/LogicApp4.png" alt="drawing" width="800"/>

This trigger is now added to your workflow. 
Now, we will create a connection to your existing Storage Account.
1. Select the container **images** that you created under Lab 1: Technical Requirements.
1. When your trigger looks like the screenshot below: click on **+ New step** to add a next step and action.
<img src="/media/LogicApp5.png" alt="drawing" width="800"/>

1. Search for **get blob content** and select the action **Get blob content** to add this to your workflow. 
<img src="/media/LogicApp6.png" alt="drawing" width="800"/>

1. When you click on the field next to ***Blob**, a new pop-up window will appear on the right with the **Dynamic content**. From this pop-up window, select **List of Files Path** to use this to specify the blob.
<img src="/media/LogicApp7.png" alt="drawing" width="800"/>

1. Again, click on **+ New step**. 
<img src="/media/LogicApp8.png" alt="drawing" width="800"/>

1. Search for **describe image** and select the **Describe Image (preview)** action to be added to your workflow.
<img src="/media/LogicApp9.png" alt="drawing" width="800"/>

You're now going to connect to your existing Cognitive Service by copying the endpoint and key from your Cognitive Service resource. 
1. Click on the field next to ***Image Source** and select **Image Content**.
1. Click on the field next to **Image Content** and use the **Dynamic content** pop-up window on the right to fill in **File Content**.
<img src="/media/LogicApp10.png" alt="drawing" width="800"/>

