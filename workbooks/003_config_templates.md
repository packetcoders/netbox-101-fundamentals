# Workbook 3: Config Templates

## Learning Objectives

- Understand how to create Jinja2 configuration templates in NetBox.
- Assign templates to devices and render the configuration.
- Control whitespace and formatting in rendered configs.

## Overview

In this workbook we will create a configuration template using Jinja2 syntax, assign it to devices, and validate the rendered configurations.

## Before You Begin

1. Access your NetBox instance.
2. Ensure the Config Contexts for NTP and Syslog have been created from workbook 2.

## Exercise 1: Creating a Config Template

In this exercise we will build a template that renders a basic device config using Context Config data.

### Task 1 - Observe Available Context Data

1. In the sidebar menu, click on **Devices**.
2. Under Devices, click on **Devices** to access the devices list.
3. Click on **spine1-nxos** to open its details page.
4. Switch to the **Render Config** tab.
5. Observe the data within the **Context Data** panel.

Note: It is this data that will be available to us within the template.

### Task 2 - Create a Template

1. In the sidebar menu, click on **Provisioning**.
2. Click on **Config Templates** to access the template list.
3. Click on the **+ Add** button to create a new template.
4. In the **Name** field, enter **Cisco Template** as the template name.
5. In the **Template code** box, enter the following Jinja2 syntax:

    ```
    hostname {{ device.name }}

    {% for server in ntp_servers %}
    ntp server {{ server }}
    {% endfor %}

    {% for syslog_server in syslog_servers %}
    logging host {{ syslog_server }}
    {% endfor %}

    router ospf 1
      router-id {{ device.cf.ospf_router_id }}
    ```

6. Scroll down and click the **Submit** button to create the template.
7. The new template should now be listed on the templates page.

## Exercise 2: Render Config Template

Here we will assign our template to our devices and validate the rendered configs.

### Task 1 - Assign Template to Devices

1. In the sidebar menu, click on **Devices**.
2. Under Devices, click on **Devices** to access the devices list.
3. Using the check boxes, select all of your device.
4. Click the **Edit Selected** button at the bottom of the screen.
5. In the form that appears, scroll down to the **Configuration** section.
6. Choose the **Cisco Template** option from the **Config Template** dropdown.
7. Click **Save** at the bottom of the form to apply the template.

### Task 2 - Validate Rendered Config

1. In the devices list, click on one of your devices.
2. In the device page, switch to the **Render Config** tab.
3. Observe the output within the **Rendered Config** field.

### Task 3 - Download Rendered Config

1. While viewing the rendered config, click the **Download** button.
2. Save the downloaded file to your local computer.
3. Open the file to inspect the fully rendered configuration.

## Exercise 3: Update Template Whitespace

Here we will adjust the template settings to improve rendered config white space formatting.

### Task 1 - Validate Rendered Config

1. Observe there are unwanted extra lines in the rendered configuration.

**Why is this?**

At the point Jinja renders a template, it removes any delimters such as `{% for x in y %}`. However (by default) any new line characters or spacing that were part of the delimiter remain, and are transfered to the generated output!

### Task 2 - Apply Whitespace Control

1. In the sidebar menu, click on **Provisioning**.
2. Click on **Config Templates** to access the templates list.
3. Click on the **Cisco Template** to open its details.
4. Click on **Edit**.
4. Scroll down to the **Environment params** section.
5. Add the following JSON data:

    ```
    {
      "trim_blocks": true,
      "lstrip_blocks": true
    }
    ```

6. Click **Submit** at the bottom to save the template changes.

<details>
<summary><b>What does trim_blocks and lstrip_blocks do?</b></summary>

The `trim_blocks` and `lstrip_blocks` settings are template parameters that control whitespace and formatting in the rendered output:

- `trim_blocks`: This removes any excess whitespace after a block. For example, it will remove trailing newlines after `{% endfor %}` blocks.

- `lstrip_blocks`: This removes any whitespace indentation from the beginning of blocks. For example, it will strip any initial indentation spaces or newlines before `{% for %}` blocks.

Enabling these parameters removes extraneous whitespace from the rendered configurations.
</details>

### Task 3 - Validate Improvement

1. Go back to the device which has the template assigned.
2. Re-render the configuration and compare to before.
3. Note that excess whitespace has now been removed from the rendered config.
