# Workbook 2: Extending the Model

## Learning Objectives

TBC

## Overview

Through the following exercises, we will learn how to extend the NetBox model through the use of Config Contexts, Custom Objects and Custom Fields.

## Before you Begin

The following prerequisites are required before beginning these exercises:

1. Open your browser and navigate to your NetBox Cloud instance.

## Exercise 1: Working with Config Contexts

In this exercise, we will create Config Contexts and assign them to all devices in the Global Tech tenant.

### Task 1 – Create Config Contexts

1. Click on the **Provisioning** menu item in the sidebar.
2. Under the Provisioning menu, click on **Config Contexts**.
3. In the Config Contexts page, click on the **+ Add** button.
4. In the form that appears, enter **Syslog 1** as the name and set the weight to 900.
5. In the data box, paste the following JSON:

    ```json
    {
      "syslog_servers": [
        "192.168.100.1",
        "192.168.100.2"
      ]
    }
    ```



6. Under **Assignment** select **Global Tech** under **Tenant**.
7. Click **Submit** to create the NTP context.
8. Repeat steps 3-6 to create a context named **Syslog 2** with JSON data for syslog servers (below), and set the weight to 1000.

    ```json
    {
      "syslog_servers": [
        "192.168.200.1",
        "192.168.200.2"
      ]
    }
    ```

### Task 2 – Validate Config Context Data

1. Click on the **Devices** menu item in the sidebar.
2. Under Devices, click on **Devices**.
3. Click on **spine1** to open its details page.
4. Switch to the **Config Contexts** tab.
5. You will now see:

| Context Type     | Description                                                                                          |
|------------------|------------------------------------------------------------------------------------------------------|
| Local Context    | Empty. This is Config Context created directly within the device object.                              |
| Source Context   | Details of the Config Contexts. We can see the Config Contexts just created. Note: The weights are also shown. |
| Rendered Context | The Config Context result. i.e the resulting data based on weights and hierarchy.                     |


6. Make a note of the IPs within the **Rendered Context**. As you will see the **Rendered Context** shows the Syslog data from the source context with the highest weight. 

### Task 3 – Alter Config Context Weight

1. Go back to **Provisioning > Config Contexts** in the sidebar.
2. Edit the **Syslog 1** context.
3. Change the weight to **2000**.
4. Add the following JSON data with a weight of **2000**:
5. Click **Submit** to save the changes.

### Task 4 – Observe Outcome

1. Return to the device page via **Devices > Devices > spine1**.
2. Go to the **Config Context** tab.
3. View the **Rendered Context** data.

**Question**: Has the Rendered Context changed? And if so why?

<details>
<summary><b>Answer</b></summary>
<br>
Yes. The Syslog data has changed due to the data within <b>Syslog 2</b> having a higher weight then <b>Syslog 1</b>. Therefore <b>Syslog 2</b> takes precedence.


</details>

### Task 5 - Add Local Context to Device

In this task, we will add local context data directly to a device to demonstrate how it overrides inherited context data.

1. Navigate to **Devices > Devices** and select **spine1**.
2. Click **Edit**.
3. Click on the **Local Context** section.
4. Add the following JSON data:

    ```json
     {
      "syslog_servers": [
        "10.10.10.5",
        "10.10.10.6"
      ]
    }
    ```

5. Save the changes.

### Task 6 - Validate Config Context

In this task, we will verify that the local context data correctly overrides the inherited context data regardless of weight.

1. Return to the device page via **Devices > Devices > spine1**.
2. Go to the **Config Context** tab.
3. View the **Rendered Context** data.

You will now see the **Rendered Context** updated with the local values from the device context. This is due to the local config context taking precedence over inherited context.


## Exercise 2: Working with Custom Fields

In this exercise, rather than add data at just a device or VM level, like we did with Config Contexts, we will use custom fields to add data to interface objects. Custom fields allow us to extend the data model for specific objects like interfaces.
.
### Task 1 – Create a Custom Field

1. Go to **Customization > Custom Fields** and click **+ Add**.
2. Create a new custom field with the following options:

   | Setting | Description |
   |---------|-------------|
   | Content types | **DCIM > Interface** |
   | Name | **ospf_area** |
   | Type | **Integer** |

3. Click **Create** to save the custom field.
4. Verify the custom field is created successfully by going to any of your device`s interfaces and checking that the new field appears in the custom fields section.

### Task 2 - Populate Custom Field

1. Go to **Devices > Devices**.
2. Select **spine1** and navigate to its **Interfaces** tab.
3. Edit the interface (e.g., `Ethernet1/1`) and fill in the **ospf_area** custom field with `0`.
4. Save the interface changes.

> [NOTE]
> We will later use this data with Config Templates when generating our configuration via Config Templates.

## Exercise 3: Working with Custom Objects

### Task 1 – Create a Custom Object Type

1. Go to **Custom Objects** and select **Custom Object Types** in the sidebar menu.
2. Click **+ Add** to create a new custom object type.
3. Provide a name of **License** and click **Create**.
4. Click **Add Field** to add the following fields. For each add the required data, click Create, and then continue to add fields.


| Name         | Type         | Related Object Type  | Required |
|------------- |------------- |------------------    |----------|
| devices      | Multi Object | DCIM > Device       | Yes      | 
| manufacturer | Object       | DCIM > Manufacturer | Yes      | 
| start_date   | Date         | N/A                  | Yes      | 
| end_date     | Date         | N/A                  | Yes      | 


### Task 2 – Create Custom Objects

1. Go to **Custom Objects** and select **Custom Objects** in the sidebar menu.
2. Click **+ Add** to create a new custom object.
3. Select **License** as the object type.
4. Fill in the fields:

| Field | Value |
|-------|-------|
| devices | Select your devices |
| manufacturer | Select **Cisco** manufacturer |
| start_date | Enter a start date |
| end_date | Enter an end date |

5. Click **Create** to save the custom object.

### Task 3 - Verify Custom Object

1. Go to **Custom Objects > Custom Objects**.
2. Verify that your License custom object is listed and contains the data you entered.
3. Go to either of your devices and under **Custom Objects linking to this object** (bottom of screen), you should see your License object listed there


### BONUS - Update the Custom Object Field Order

> [NOTE]
> This task is a bonus and time permitting!

You will have noticed the field order for our object when we entered the data was not as we would expect. Let's fix that.

1. Go to **Custom Objects > Custom Object Types**.
2. Find the **License** custom object type and click on it.
3. Click on the **Fields** tab.
4. For each field click edit an change the **Display weight** to the following: (Fields with higher weights appear lower in a form.):

| Field | Display Weight |
|-------|----------------|
| manufacturer | 10 |
| devices | 20 |
| start_date | 30 |
| end_date | 40 |

5. Go back to the Custom Object, Click **Add*.

You will now see the fields in the order you just specified.






