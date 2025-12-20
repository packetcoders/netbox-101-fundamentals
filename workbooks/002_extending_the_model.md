# Workbook 2: Extending the Model

## Learning Objectives

TBC
## Overview

Through the following exercises, we will learn how to extend the NetBox model through the use of Config Contexts, Custom Objects and Custom Fields.

## Before you Begin

The following prerequisites are required before beginning these exercises:

1. Open your browser and navigate to your NetBox Cloud instance.

## Exercise 1: Working with Config Contexts

In this exercise, we will create Config Contexts and assign them to all devices in the Global Tech tenant..

### Task 1 – Create Config Contexts

1. Click on the **Provisioning** menu item in the sidebar.
2. Under the Provisioning menu, click on **Config Contexts**.
3. In the Config Contexts page, click on the **+ Add** button.
4. In the form that appears, enter **NTP** as the name.
5. In the data box, paste the following JSON:

    ```json
    {
      "ntp_servers": [
        "192.168.100.1",
        "192.168.100.2"
      ]
    }
    ```

6. Under **Assignment** select **Global Tech** under **Tenant**.
7. Click **Submit** to create the NTP context.
8. Repeat steps 3-6 to create a context named **Syslog 1** with JSON data for syslog servers (below).

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


6. Make a note of the IPs within the **Rendered Context**.

### Task 3 – Alter Config Context Weight

1. Go back to **Provisioning > Config Contexts** in the sidebar.
2. Click on **+ Add** to create a new context.
3. Name the context **Syslog 2**.
4. Add the following JSON data with a weight of **2000**:

    ```json
    {
      "syslog_servers": [
        "192.168.200.3",
        "192.168.200.4"
      ]
    }
    ```

5. Click **Submit** to create **Syslog 2**.

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
        "192.168.200.5",
        "192.168.200.6"
      ]
    }
    ```

5. Save the changes.

### Task 6 - Validate Config Context

In this task, we will verify that the local context data correctly overrides the inherited context data regardless of weight.

1. Return to the device page via **Devices > Devices > spine1**.
2. Go to the **Config Context** tab.
3. View the **Rendered Context** data.
4. Verify that the syslog_servers now includes the local values from the device context.

## Exercise 2: Working with Custom Fields

In this exercise, we will migrate custom field work from Workbook 1, expanding it to support upcoming configuration templates.

### Task 1 – Create a Custom Field

1. Go to **Customization > Custom Fields** and click **+ Add**.
2. Create a new custom field with the following options:

   | Setting | Description |
   |---------|-------------|
   | Content types | **DCIM > Device** |
   | Name | **ospf_router_id** |
   | Type | **Text** |

3. Click **Create** to save the custom field.

### Task 2 – Assign the Custom Field to Devices

1. Navigate to **Devices > Devices**.
2. Edit **spine1** and set the custom field value to **1.1.1.1** under the **Custom Fields** section, then click **Update**.
3. Repeat for **spine2** using **2.2.2.2**.
4. (Optional) If you created additional devices in Workbook 1, assign values accordingly.

### Task 3 – Confirm Custom Field Availability

1. Return to **Devices > Devices** and open **spine1**.
2. Switch to the **Config Context** tab.
3. Confirm the rendered context now includes the custom field value under `device.cf.ospf_router_id`.

## Exercise 3: Working with Custom Objects

We will model a simple custom object to represent an external service reference and expose it via Config Contexts.

### Task 1 – Create a Custom Object Type

1. Go to **Customization > Custom Object Types** and click **+ Add**.
2. Define the object type with the following values:

   | Setting | Description |
   |---------|-------------|
   | Name | **ServiceEndpoint** |
   | Key | **service_endpoint** |
   | Description | "Represents an external service endpoint referenced by devices." |

3. Add two JSON fields:
   * **name** (Type: Text)
   * **url** (Type: URL)
4. Click **Create** to save the custom object type.

### Task 2 – Create a Custom Object Instance

1. In the **Custom Objects** list, click **+ Add**.
2. Choose **ServiceEndpoint** as the type.
3. Provide the following values:

   | Field | Value |
   |-------|-------|
   | Name | **NTP Service** |
   | JSON | `{ "name": "NTPOperations", "url": "https://ntp.globaltech.example" }` |

4. Click **Create** to save the custom object.

### Task 3 – Reference the Custom Object in a Config Context

1. Navigate back to **Provisioning > Config Contexts**.
2. Edit the **NTP** context created earlier.
3. Append the following key/value pair to the JSON payload:

    ```json
    "service_endpoint": {
      "name": "NTPOperations",
      "url": "https://ntp.globaltech.example"
    }
    ```

4. Click **Submit** to update the context.
5. Return to **spine1 > Config Contexts** and confirm the additional data appears in the rendered context.

## Exercise 4: Validating Config Context Overrides

In this exercise, we will override shared context data using local overrides to observe precedence.

### Task 1 – Create a Local Override

1. While still on the device details page, click **Edit**.
2. Scroll down to **Local Config Context Data**.
3. Add the following JSON:

    ```json
    {
      "syslog_servers": [
        "10.1.200.3",
        "10.1.200.4"
      ]
    }
    ```
4. Click **Save**.




### Task 2 – Validate the Rendered Config Context Data

1. Go back to the **Config Context** tab.
2. Confirm the rendered context reflects the locally defined syslog servers while still including the shared data (NTP servers, service endpoint, custom field values).

<details>
<summary><b>Answer</b></summary>
<br>
Yes. The Syslog data has changed again because the Local Context data overrides values from the shared contexts while leaving the other shared elements (NTP servers, service endpoint, custom fields) intact.

</details>
