# 📖 Power Automate Exercise - Send Automated Emails with HTML Table Data
This README document hands-on exercise came from the Microsoft Learn module:
[Guided Project - Create and Manage Automate Processes with Power Automate](https://learn.microsoft.com/en-us/training/modules/create-manage-automated-processes-with-power-automate/3-exercise-build-flow-send-automated-emails)


## 🎯 Learning Objectives
- Create a custom table in Dataverse
- Build automated cloud flow with recurrence trigger
- Filter data of table before formatting it into HTML table
- Send automated emails

## ⭐ Steps accomplished
### 1. Create custom table in [Dataverse](https://make.powerapps.com/)
![Create Custom Table Screenshot](/exercises/images/CreateCustomTable1.png)
- **Table Name**: `Opportunity`
- **Columns Added**:
    - `Opportunity Subject` (Single line of text (Default value))
    - `Customer` (Single line of text)
    - `Opportunity Status` (Choice: New = 1, Open = 2, Closed = 3) (Required: Business required)
    - `Amount` (Currency)


> ✅ Sync the `Opportunity Status` with a global choice set  

> 📌 Note: The logical name of every column is used when it comes to filtering the table (OData). To get their information, navigate to **Tables -> Opportunity -> Columns** take note of their logical names (right side)
![Finding Logical Name in of Column](/exercises/images/CreateCustomTable1b.png)

### 2. Create a Scheduled Flow 
- **Flow Name**: `New Sales Opportunities`
- **Trigger**: `Reccurrence` (Every weekdays only - no Saturday and Sunday) 

### 3. Add Actions to the Flow
- **List Rows** from `Opportunities` table
    - **Filter**: `opportunitystatus eq 1` (❗: Your name of the opportunitystatus may vary due to logical name, consider on checking the logical name of every columns and save it for reference later)

- **Create HTML Table action** from listed rows
- **Send Email (V2)**
    - **To**: [Email address]
    - **Subject**: `Daily Sales Opportunities`
    - **Body**: Output from HTML Table

Based on the module:\
![](/exercises/images/CreateCustomTable1c.png)

My actions and triggers of my output:\
![](/exercises/images/CreateCustomTable1d.png)

> **Note**: I added in my output that lets me filter specific column data of Dataverse tables and then format it into HTML table before I attach it to the email

#### Inside the List Rows Action:
![Contents of List Row](/exercises/images/CreateCustomTable1e.png) \
The table `Opportunities` from Dataverse tables is applied in this action. The select columns is populated with logical names of columns that exists from the provided Dataverse table which is also filtered to only get data that has the `Opportunity Status` column value "New" or 1.

#### Inside the Select Rows Action:
![Contents of Select Action](/exercises/images/CreateCustomTable1f.png) \
As you can see I added an additional select action, this will help formatting the HTML table to properly display its headers and their actual values, instead of displaying it as an object.

#### Inside the Create HTML table
![Contents of Create HTML Table](/exercises/images/CreateCustomTable1g.png) \
The `From` property was set with the output from the `Select` actions

#### Inside the Send an Email (V2)
![Contents of Send an Email (v2)](/exercises/images/CreateCustomTable1h.png) \
The dynamic content that was set in the email body is the output from HTML table. Now if there were any data from Dataverse table that has been created with `Opportunity Status` "New", it should email the person that was set in `To`

### Output of the Exercise Module
For every data that was entered in Dataverse Table that was marked as "New" project clients, it should send the details to the provided email address.
![Output of automated email with HTML Table](/exercises/images/CreateCustomTable1i.png)