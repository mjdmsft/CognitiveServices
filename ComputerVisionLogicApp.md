## Before you start - Prerequisites
- Azure Account (this tutorial assumes you have the Azure Pass Sponsorship Subscription but any subscription will work). 
- Storage Account and Cosmos DB as described in [Lab 1: Technical Requirements](https://github.com/MicrosoftLearning/AI-100-Design-Implement-Azure-AISol/blob/master/Lab1-Technical_Requirements/02-Technical_Requirements.md)

## Describe images from Blob Storage with Computer Vision through a Logic App
We will create a Logic App with which we can automate a workflow. The workflow we'll create is to detect new images in our Blob Storage, generate descriptions and tags with the Computer Vision API and store this metadata in Cosmos DB. We will create a container to store the metadata in Cosmos DB first, then we'll create the Logic App, and finally we'll add images to our Blob Storage to trigger our Logic App and be able to review the metadata in Cosmos DB. 

### Create a container in Cosmos DB
1. Go to your previously created Azure Cosmos DB in the Azure portal.
2. In Overview, click on **+ Add Container** in the top bar.
<img src="/media/LogicApp1.png" alt="drawing" width="600"/>

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
<img src="/media/LogicApp2.png" alt="drawing" width="400"/>

12. Once deployment is complete. Navigate to your new Logic App. 
13. Scroll down and under Templates, click on **Blank Logic App**. 
<img src="/media/LogicApp3.png" alt="drawing" width="400"/>

First thing we have to do in our Logic Apps Designer, is create a trigger. 
14. Search for “blob” and select **When a blob is added or modified**
<img src="/media/LogicApp4.png" alt="drawing" width="800"/>

This trigger is now added to your workflow. 
15. Create a connection to your existing Storage Account by specifying a name for the connection and the Storage Account you want to connect with.
16. Select the container **images** that you created under Lab 1: Technical Requirements. You can select a container by clicking on the **folder** icon on the right of the field. 
17. When your trigger looks like the screenshot below: click on **+ New step** to add a next step and action.
<img src="/media/LogicApp5.png" alt="drawing" width="800"/>

18. Search for **get blob content** and select the action **Get blob content** to add this to your workflow. 
<img src="/media/LogicApp6.png" alt="drawing" width="800"/>

19. When you click on the field next to **Blob**, a new pop-up window will appear on the right with the **Dynamic content**. From this pop-up window, select **List of Files Path** to use this to specify the blob.
<img src="/media/LogicApp7.png" alt="drawing" width="800"/>

20. Again, click on **+ New step**. 
<img src="/media/LogicApp8.png" alt="drawing" width="800"/>

21. Search for **describe image** and select the **Describe Image (preview)** action to be added to your workflow.
<img src="/media/LogicApp9.png" alt="drawing" width="800"/>

22. Connect to your existing Cognitive Service by copying the endpoint and key from your Cognitive Service resource. Paste them both into your Logic App. It should look something like this:
<img src="/media/LogicApp9B.PNG" alt="drawing" width="800"/>

23. Click on the field next to **Image Source** and select **Image Content**.
24. Click on the field next to **Image Content** and use the **Dynamic content** pop-up window on the right to fill in **File Content**.
<img src="/media/LogicApp10.png" alt="drawing" width="800"/>

25. Add a **+ New step**. 
26. Search for **initialize variable** and choose **Initialize variable** as an action to add to your workflow. 
27. After **Name** put **ID**.
28. For **Type** select **Integer**.
29. For **Value** type in **999**.
<img src="/media/LogicApp11.png" alt="drawing" width="800"/>

30. Add a **+New step**.
31. Search for **compose** and add the **Compose** action to your workflow. 
<img src="/media/LogicApp12.png" alt="drawing" width="800"/>

32. Click on the field of **Inputs**.

We are going to create a JSON object to store in our Cosmos DB. It should end up looking like the screenshot below. 
- Create this by typing into the **Inputs** and using the **Dynamic Content** to add the necessary variables. 
- Make sure all keys and values are between quotation marks (“key”:”value”), except for Tag Names.
- When you add **Captions Caption Text**, it will add a for each loop for you. **Accept this** change and don’t remove it.
- You can copy the JSON below as a starting point and add the Dynamic Content between the righter quotations marks.

`{
"Captions":"",
"ImageId":"",
"Tags":"",
"id":""
}`


<img src="/media/LogicApp13.png" alt="drawing" width="800"/>

33. **Within** your **For each loop**, choose to **Add an action**. 
34. Search for **cosmos db** and select **Create or update document** action to add to your workflow. 
35. Connect to your previously created Cosmos DB account. 
36. For **Database ID** select **Images**.
37. For **Collection ID** select **results**.
38. For **Document** use the **Dynamic Content** to fill in **Outputs**. 
39. **Add new parameter** and add the **Partition key value**.
40. For **Partition key value**, fill in **“List of Files Name”**. Don’t forget the quotation marks!
<img src="/media/LogicApp14.png" alt="drawing" width="800"/>

41. On the left top, **save** your workflow and then **Run** it. 

### Trigger the Logic App by adding images to your Blob Storage
42. Now, add images from the sample-images folder to your Storage Account container called images and you should see the results in your Cosmos DB. **Make sure you only add the images!**

