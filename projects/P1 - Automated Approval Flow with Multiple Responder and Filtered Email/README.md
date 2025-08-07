# 💻 Approval Flow with Multiple Responder and Filtered Email
This automated flow includes approval flow wherein all assigned members are required to respond `Approve/Reject` to the notification that will be sent to **Microsoft Teams**. Approval flow will only occur if the assigned email address (**Outlook**) has received an email with a title containing "Contract" in the subject, and if there is an attached PDF file.

> 🔴 **Note**: You can change the part of the starting trigger (*when a new email arrives*) to trigger the approval flow, it can be also like when there is a modified list from **SharePoint**!
## ⭐ What I've Accomplished
- Filter incoming emails with certain keywords ("Contract") and file type when sending to a specific email.
- Send approval to assigned members in Teams after receiving the contract.
- Send follow-up email whether the reviewed contract has been approved or rejected.

## 👀 Preview of Flow
![Entire flow](/projects/P1%20-%20Automated%20Approval%20Flow%20with%20Multiple%20Responder%20and%20Filtered%20Email/images/P1_1.png)

## 📋 Creation of the Flow
You can see in this section what I have set for the values for each property in the following actions and triggers.

### When a New Email Arrives (v3)
This trigger occurs whenever a new email arrives in the matching specific criteria.

| **Property** | **Value**     | **Purpose**                  |
|--------------|---------------|------------------------------|
| **Folder**   | `Inbox`       | Monitors incoming emails in `Inbox` folder|
| **To**       |`Email Address`| Makes sure that flow only triggers for email sent directly to the address |
| **From**     |`Email Address`| Filters email from specific senders to narrow scope of the flow
| **Include Attachments** | `Yes` | Allows the flow to access email attachments
| **Subject Filter** | `Keyword` | Triggers only on emails with specific keywords
| **Only with Attachments** | `Yes` | This makes the flow only runs when an email contains attachments |

![When an email arrives](/projects/P1%20-%20Automated%20Approval%20Flow%20with%20Multiple%20Responder%20and%20Filtered%20Email/images/P1_2.png)
 
### Initialize a Variable 
This is for setting value to a variable that can be used all throughout the current flow.

| **Property** | **Value**      | **Purpose**                 |
|--------------|----------------|-----------------------------|
| **Name**     | `PdfNames`     | Stores the name of PDF Attachments |
| **Type**     | `String`       | Allows to hold text values |
| **Value**    | `Blank`        | Leave it blank for later, will be used for populating |

![Initialize a variable](/projects/P1%20-%20Automated%20Approval%20Flow%20with%20Multiple%20Responder%20and%20Filtered%20Email/images/P1_3.png)

### Initialize Email Variable
This is a variable used for setting the `Assign To` property in the start and wait for approval flow later.

| **Property** | **Value**      | **Purpose**                 |
|--------------|----------------|-----------------------------|
| **Name**     | `AssignedMembersArray` | Holds a list of email array|
| **Type**     | `Array`        | Enables to store multiple email in array format |
| **Value**    | `[email1, email2]` | Predefined list of emails

> 🔴 **Note**: This part of the flow can be modified depending on the source of the email list—whether it's manually set, pulled from a SharePoint list, Excel file, or another connector.

![Initialize Email Variable](/projects/P1%20-%20Automated%20Approval%20Flow%20with%20Multiple%20Responder%20and%20Filtered%20Email/images/P1_4.png)
### Apply to Each
This loop iterates through all attachments and appends the file name to the `PdfNames` variable, which will be used later to display output in sending an email whenever there are attachments that ends with *.pdf* format.
- **Input**: `Attachments` from when an email arrives v3
- **Purpose**: Iterates through each attachment item.

#### **Steps involved**:
1. **Compose**
    - **Expression**: `toLower(items('Apply_to_each')?['name'])`
    - **Purpose**: Converts file name into lowercase for consistent extension checking (*.pdf*)
2. **Condition**
    - **Expression**: `endsWith(outputs('Compose'), '.pdf')`
    - **Purpose**: Checks if the file ends with `.pdf`
    - 
3. **If Yes**
    - **Action**: `Append to String Variable`
    - **Variable**: `PdfNames`
    - **Value**: `items('Apply_to_each')?['name']`
    - **Purpose**: Adds the original file name to the string variable.

![Apply to Each](/projects/P1%20-%20Automated%20Approval%20Flow%20with%20Multiple%20Responder%20and%20Filtered%20Email/images/P1_5.png)

### Start and Wait for Approval

| **Property** | **Value**      | **Purpose**                 |
|--------------|----------------|-----------------------------|
| **Approval Type** | `Approve/Reject - Everyone must approve` | Requires every responder assigned to respond
| **Title** | `New Contract` | Sets the subject line of appproval request |
| **Assigned To** | `join(variables('AssignedMembersArray'), ';')` | Dynamically assign multiple approvers
| **Details** | `[Message]` | Provides context to the PDF files any content to approval request |

![Start and wait for approval request](/projects/P1%20-%20Automated%20Approval%20Flow%20with%20Multiple%20Responder%20and%20Filtered%20Email/images/P1_6.png)

### Filter Array - Approved Responses
This step only filters collection of responses to include those who have responded "Approve"
| **Action** | **Filter Array** |
|------------|------------------|
| **From**   | `Responses` (output from approval action) |
| **Conditon** | `item()?['approverResponse']` is equal to `Approve` |

![Filter Array](/projects/P1%20-%20Automated%20Approval%20Flow%20with%20Multiple%20Responder%20and%20Filtered%20Email/images/P1_7.png)


### Length of Array (Compose)
This compose calculates the number of approved responses based on the length of the array
- **Input**: `Responses` from start and wait for approval request
- **Condition**: `item()?['approverResponse']` is equal to `Approve`
- **Purpose**: This is meant for counting how many responders answered Approve

![Length of array](/projects/P1%20-%20Automated%20Approval%20Flow%20with%20Multiple%20Responder%20and%20Filtered%20Email/images/P1_8.png)


### Condition 2
This action checks whether the length of filter array equals to the numbers of responder assigned in start and wait for approval. 
- **Condition**: `outputs('length_of_array')` is equal to `length(variables('AssignedMembersArray'))`

![Condition 2](/projects/P1%20-%20Automated%20Approval%20Flow%20with%20Multiple%20Responder%20and%20Filtered%20Email/images/P1_9.png)