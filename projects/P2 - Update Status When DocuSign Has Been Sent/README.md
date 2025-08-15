# 💻 Automated Flow - Update When DocuSign Has Been Sent
This automated flow will trigger once a document from the DocuSign website has been sent or delivered to the signee. It will then update the value in the **SharePoint List** to show the status of the envelope.
## ⭐ What I've Accomplished
- Detect if the document from DocuSign has been sent
- Retrieve related data in SharePoint based on DocuSign's sent envelope id
- Update column in SharePoint list

## 👀 Preview of Flow
![Preview of the Flow](/projects/P2%20-%20Update%20Status%20When%20DocuSign%20Has%20Been%20Sent/images/P2_1.png)

## 📋 Creation of the Flow

### When an Envelope Status Changes (Connect) (V3)
In this trigger, you must login your DocuSign account because it will track all of the operations done with your documents in the website. This will trigger if one of the document envelope status changed to a specific status such as `signed` `sent` and so on..

**Property** | **Value**      | **Description**|
|------------|----------------|----------------|
|Account     |`[Account]`| Select the DocuSign you want to login to the connector|
|Connect Name| `[Name]`  | Set the name of the connector
|Envelope Event | `envelope-sent` | This is the status that you want to catch in order for this action to trigger

![Creation of Flow](/projects/P2%20-%20Update%20Status%20When%20DocuSign%20Has%20Been%20Sent/images/P2_2.png)

## Get Items
This is for retrieving the Envelope ID that has been stored on the SharePoint previously when the envelope draft has been created. The value that will be retrieved from this action will be used later for condition.

**Property** | **Value**        | **Description**
|------------|------------------|-------------------|
|Site Address|`[File Path Link]`| This is where the action will target to retrieve the data
|List Name   | `SharePoint List Name` | Specifically retrieve on the existing SharePoint list in the provided directory
|Filter Query| `Envelopid eq [Envelope ID from Docusign Connector]`| This filters out and selects data that matches the query (In this case if it equals to the Envelope Id)
|Top Count | 1 | This tells how many records will be retrieved

![Get Items](/projects/P2%20-%20Update%20Status%20When%20DocuSign%20Has%20Been%20Sent/images/P2_3.png)

### Apply to Each
This action will make the retrieved data go through a condition if:
- Envelope ID that has changed status in the DocuSign matches the one that is saved in SharePoint list
AND
- EnvelopeStatus column value is `Draft Crated`, then it will proceed to the True Block

**If it's true**:
Update the column EnvelopeStatus to `Sent` on the specific `ID` that was retrieved from **Get Items** with the use of `Envelope ID` from the trigger.

![Apply to Each](/projects/P2%20-%20Update%20Status%20When%20DocuSign%20Has%20Been%20Sent/images/P2_4.png)
