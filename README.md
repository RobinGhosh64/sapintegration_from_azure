# Azure SAP Integration Lab

## Prerequisites

- Microsoft Azure subscription
- Resource Group to deploy Azure services
- Permissions to create the following resource  
    - Data Gateway Installed on Customer's On Prem with the correct .NET libraries
    - Logic App

## General steps to integrate and call any SAP remote function call

1. SAP connector/adaptor is the gateway to the target SAP system. So before we make any SAP RFC call we need to pass in the correct request and response objects.

   I.E. We need to confirm to a spec and provided the exact XML otherwise the adaptor will reject it and not send it over to the target SAP instance
   
2. It is the job of the SAP adaptor(driver) to know these objects and it will tell you the meta data (external schema definition:- XSD) if you ask him to describe it.
3. This describe feature can be called when you specifically call **Generate Schemas** action in the adaptor which is exposed as an API, just like **Send BAPI message **
4. You will need to pass in the SAP RFC action name
5. By default, the SAP adaptor will return the root elements for request/response objects and followed by the child elements in two objects which is mbedded inside an array.
   Also the data stream is encoded.
7. If you are going to do exercise many integrations, this is going to be a manual nightmare.
8. I decided to automate this and help the community so I am going to ask you to create a separate Logic App to extract these xsd's first. Think about this Logic App as a cookie    cutter to do many integrations in the SAP world.
9. I will automate this completely when I get time.


## Step 1: Create a Logic App that can describe input/output objects (request/response) to run any SAP RFC action
1. In the Azure Portal, search for **Logic App**
2. Click on the **Add** button
3. Fill out the **Basics** tab as follows:
- **Subscription:** Choose your subscription
- **Resource group:** Provide a unique name like **ata-webapp-username-rg**
- **Region:** EastUS

![RG Basic Tab](images/create_logic_app.JPG)  

4. Click the **Next: Review + Create** button
5. Click the **Create** button

## Step 2: Add workflow steps into your Logic App
1. Take meta data code from the repository and then copy-paste and save
2. When you save you will need to create a connection to your SAP instance if you have not done so
3. Go back to the designer and make sure you see the following steps

![App Service Basic Tab](images/GenSchema_LogicApp.JPG)

4. You are now in a state to run this logic app and describe any RFC action's request/response objects 


## Step 3: Let's try and describe a simple SAP action

1. Go to POSTMAN, add correct JSON Content-Type and in the payload add the sapaction action attribute
2. Please make sure you fill rest of the fields. The URI is available from the HttpTrigger step of the Logic App
3. Here is my POSTMAN screen shot as input


![App_Service_Basic_Tab](images/GenSchema_Input_1.JPG)

4. Click the **Send** button
5. You should see schema definitions as response
6. I have added -------- as a delimiter between the RFC xsd and the TYPES xsd. So you will need to extract them as separate files on your local system

7. Here is my POSTMAN screen shot as output

![App Service Basic Tab](images/GenSchema_Output_1.JPG)

5. Collect the sample xsd's from the response tab of the Postman. This is an important step. Make sure it works!!



## Step 4: Now that we have the two XSD's, let us create a xml payload

1. Please use any tool like XMLSpy or Liquid Studio
2. Load the two files are generate the XML payload
3. Here is my screen shot using the two xsd's


![App_Service_Basic_Tab](images/Liquid_Select_Root_element.JPG)

4. Click the **Send** button
5. You should see schema definitions as response
6. I have added -------- as a delimiter between the RFC xsd and the TYPES xsd. So you will need to extract them as separate files on your local system

7. Here is my POSTMAN screen shot as output

![App Service Basic Tab](images/Liquid_Select_Tool.JPG)

5. Collect the sample xsd's from the response tab of the Postman. This is an important step. Make sure it works!!


![App Service Basic Tab](images/Liquid_Select_Tool_1.JPG)



7. Here is my POSTMAN screen shot as output

![App Service Basic Tab](images/Material_Not_OK.JPG)

5. Collect the sample xsd's from the response tab of the Postman. This is an important step. Make sure it works!!


![App Service Basic Tab](images/Material_OK.JPG)



## Step 7: Our App Service has restarted. Hopefully, taken the settings update. Let's go to our Web App

1. Go back to you App Service by selecting the **Overview** on the left blade

![RG Basic Tab](images/app-service-overview.JPG)

2. Click on **URL**

![RG Basic Tab](images/app-service-deployment-happy.JPG)

3. Make sure you web applications renders and shows the connected message in your Web App.

**Blob Storage: Success**

  CONGRATULATIONS. Great job you have finished the LAB successfully!



