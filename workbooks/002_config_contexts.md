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

In this exercise, we will create global Config Contexts and attach them to devices.

### Task 1 – Create Config Contexts

1. Click on the **Extras** menu item in the sidebar.
2. Under the Extras menu, click on **Config Contexts**.
3. In the Config Contexts page, click on the **+ Add** button.
4. In the form that appears, enter `NTP` as the name.
5. In the JSON data box, paste the following:

    ```json
    {
      "ntp_servers": [
        "192.168.100.1",
        "192.168.100.2"
      ]
    }
    ```

6. Click **Submit** to create the ntp context.
7. Repeat steps 3-6 to create a context named `Syslog 1` with JSON data for syslog servers (below).

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
3. Click on a device name to open its details page.
4. Switch to the **Config Contexts** tab.
5. Click on **+ Add** and select the **ntp** and **syslog 1** contexts.
6. Click on the **Rendered** button to view the combined context data.

## Exercise 2: Altering Config Context Weight

In this exercise, we will modify context data precedence using weights.

### Task 1 – Alter Weight

1. Go back to **Extras > Config Contexts** in the sidebar.
2. Click on **+ Add** to create a new context.
3. Name the context `Syslog 2`.
4. Add the following JSON data with a weight of `200`:

    ```json
    {
      "syslog_servers": [
        "192.168.200.3",
        "192.168.200.4"
      ]
    }
    ```

5. Click **Submit** to create Syslog 2.

### Task 2 – Observe Outcome

1. Return to the device page that has syslog contexts attached.
2. View the **Rendered** config context data.
3. Note that Syslog 2 takes precedence due to higher weight.

## Exercise 3: Create Local Config Contexts

In this exercise, we will override global contexts using local device contexts.

### Task 1 – Create Local Config Context

1. While still on the device details page, switch to the **Local Config Contexts** tab.
2. Click on **+ Add** to create a new local context.
3. Name it `syslog` and add JSON data with a higher weight.
4. Click **Submit** to save this local syslog context.

### Task 2 – Validate Local Config Context Data

1. Go back to the **Rendered** tab for the config context data.
2. Observe that the local syslog context now takes precedence.