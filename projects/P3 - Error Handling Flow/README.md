# 💻 Error Handling Flow
This flow detects the error that occured in the `Try` scope block and displays out the error message (either in email or sent thru chat).
## ⭐ What I've Accomplished
- Detects error
- Displays or send error message and it's reason why it occured
- Flow can be used as a template

## 👀 Preview of Flow
![Preview of Flow](/projects/P3%20-%20Error%20Handling%20Flow/images/P3_1.png)

## 📋 Creation of the Flow

### Manually trigger a flow
This action can be replaced depending on the trigger that you want. For this part it was set temporarily for testing purposes.
### Try Scope
In the `Try` scope, you can change any actions contained there in which there is a possibility that could cause error in that scope.
![Try Scope](/projects/P3%20-%20Error%20Handling%20Flow/images/P3_2.png)

### Catch Scope
In the catch block, this is where actions will take place if the `Try` scope encounters an error.
> **Note**: Do not forget to set the `Configure After Run` to only toggled checkbox on `Has failed` or `Has timed out`
![Catch Scope Configure After Run](/projects/P3%20-%20Error%20Handling%20Flow/images/P3_4.png)

The following are the actions added within the `Catch` scope:
![Inside Catch Scope](/projects/P3%20-%20Error%20Handling%20Flow/images/P3_3.png)

**1. Compose Flow Run URL**
This will create a link where it will lead you to the flow run of the failed flow, to preview every steps.

**Code**: `@{concat('https://make.powerautomate.com/environments/'
        ,workflow()?['tags']['environmentName']
        ,'/flows/',workflow()?['name']
        ,'/runs/',workflow()?['run']['name']
        )}
`

**2. Filter Array Failed Actions**
In this action, it will filter out only the result with failed or timed out messages. Later on, it will be used to construct a table to display the errors with labels.
| **Property** | **Value**  |
|--------------|------------|
|   From       | `result('Try')`|
| Expression   | `@or(equals(item()?['status'], 'Failed'), equals(item()?['status'], 'Timeout'))` |

**3. Create HTML Table**

Based on the filtered result of array, it will construct a table to display the errors properly.

| **Property** | **Value**         |
|--------------|-------------------|
|  From        | `body('Filter_Array_Failed_Actions')`|
|  Header      | `Action, Status, Message` |
|  Valye       | `item()?['name'], item()?['Status'], item()?['error']?['message']`|

**4. Send an Email (V2)**

In this action, this is where you compose the message, and you can apply the result here by inserting the table. You have to click the symbol `</>` on the upper right after setting the output of `Create HTML Table` so that you can set the HTML styles for the table and insert the hyperlink of the flow run

![Send an Email](/projects/P3%20-%20Error%20Handling%20Flow/images/P3_5.png)

**4. Terminate**

This will set the status of the flow after it ended whether it's marked as successful, or failed.

![Terminate](/projects/P3%20-%20Error%20Handling%20Flow/images/P3_6.png)

## 🔴 Output of the Flow
After testing the flow, it will encounter an error then proceeds to sending an email about the description of the error.

![Output of the Flow](/projects/P3%20-%20Error%20Handling%20Flow/images/P3_7.png)