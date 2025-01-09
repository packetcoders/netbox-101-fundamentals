# Workbook 2: Config Contexts

## Learning Objectives

- Understand how to create, validate, and manage Config Contexts in NetBox.
- Understand how weight effects Config Context data.
- Understand how to create local Config Contexts for individual devices.

## Overview

Through the following exercises, we will learn how to utilize Config Contexts in NetBox to manage configuration data for devices.

## Before you Begin

The following prerequisites are required before beginning these exercises:

1. Open your browser and navigate to your NetBox Cloud instance.

## Exercise 1: Creating Config Contexts

In this exercise, we will create Config Contexts and assign them to a tenant.

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
3. Click on a **spine1-nxos** to open its details page.
4. Switch to the **Config Contexts** tab.
5. You will now see:

| Context Type     | Description                                                                                          |
|------------------|------------------------------------------------------------------------------------------------------|
| Local Context    | Empty. This is Config Context created directly within the device object.                              |
| Source Context   | Details of the Config Contexts. We can see the Config Contexts just created. Note: The weights are also shown. |
| Rendered Context | The Config Context result. i.e the resulting data based on weights and hierarchy.                     |


6. Make a note of the IPs within the **Rendered Context**.
  # Exercise 2: Altering Config Context Weight

In this exercise, we will modify Config Context precedence using weights.

### Task 1 – Alter Weight

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

### Task 2 – Observe Outcome

1. Return to the device page via **Devices > Devices > spine1-nxos**.
2. Goto the **Config Context** tab.
3. View the **Rendered Context** data.

**Question**: Has the Rendered Context changed? And if so why?

<details>
<summary><b>Answer</b></summary>
<br>
Yes. The Syslog data has changed due to the data within <b>Syslog 2</b>b having a higher weight then <b>Syslog 1</b>b>. Therefore <b?Syslog 2</b> takes precedence.


</details>



## Exercise 3: Create Local Config Contexts

In this exercise, we will override the "global" Config Contexts using local Config Contexts.

### Task 1 – Create Local Config Context

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

3. Go back to the **Config Context** tab.

**Question**: Has the Rendered Context changed? And if so why?

<details>
<summary><b>Answer</b></summary>
<br>
Yes. The Syslog data has changed again. This is because the Local Context data takes full precedence over "global" Config Context data.

</details>
