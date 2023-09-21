# Workbook 3: Config Templates

## Learning Objectives

- Understand how to create Jinja2 configuration templates in NetBox
- Assign templates to devices and render the configuration
- Control whitespace and formatting in rendered configs

## Overview

In this workbook we will create a configuration template using Jinja2 syntax, assign it to devices, and validate the rendered configurations.

## Before You Begin

1. Access your NetBox instance.
2. Ensure config contexts for NTP and Syslog have been created.

## Exercise 1: Creating a Config Template

In this exercise we will build a template that renders device configs using context data.

### Task 1 - Observe Available Context Data

1. In the sidebar menu, click on **Devices**.
2. Under Devices, click on **Devices** to access the devices list.
3. Click on a device name to open its details page.
4. Switch to the **Config Contexts** tab.
5. Click on **Rendered** to view the combined config context data for the device.
6. Scroll through the rendered data and take note of the available variable names like `ntp_servers` and `syslog_servers`. These will be used in the template.

### Task 2 - Create a Template

1. In the sidebar menu, click on **Extras**.
2. Under Extras, click on **Config Templates** to access the template list.
3. Click on the **+ Add** button to create a new template.
4. In the **Name** field, enter `Cisco Template` as the template name.
5. In the template editor field, enter the following Jinja2 syntax:

    ```
    hostname {{ device.name }}

    {% for server in ntp_servers %}
    ntp server {{ server }}
    {% endfor %}

    {% for syslog_server in syslog_servers %}
    logging host {{ syslog_server }}
    {% endfor %}
    ```

6. Scroll down and click the **Submit** button to create the template.
7. The new template should now be listed on the templates page.

## Exercise 2: Render Config Template

Here we will assign our template to devices and validate the rendered configs.

### Task 1 - Assign Template to Devices

1. In the sidebar menu, click on **Devices**.
2. Under Devices, click on **Devices** to access the devices list.
3. Click the **Edit Selected** button in the top right.
4. Check the boxes next to one or more devices to select them.
5. In the form that appears, scroll down to the **Config Template** field.
6. Choose the `Cisco Template` option from the dropdown.
7. Click **Save** at the bottom of the form to apply the template.

### Task 2 - Validate Rendered Config

1. In the devices list, click on a device that now has the template assigned.
2. In the device page, switch to the **Config Contexts** tab.
3. Click on **Rendered** to view the combined config context.
4. Verify the template has rendered correctly using the device's context data.

### Task 3 - Download Rendered Config

1. While viewing the rendered config, click the **Download** button.
2. Save the downloaded file to your local computer.
3. Open the file to inspect the fully rendered configuration.

## Exercise 3: Update Template Whitespace

Here we will adjust the template to improve rendered config formatting.

### Task 1 - Validate Rendered Config

1. Observe there are extra new lines and indentation in the rendered configuration.

### Task 2 - Apply Whitespace Control

1. In the sidebar menu, click on **Extras**.
2. Under Extras, click on **Config Templates** to access the templates list.
3. Click on the `Cisco Template` to open its details.
4. Scroll down to the **Environment Parameters** section.
5. Add the following JSON data:

    ```
    {
      "trim_blocks": True,
      "lstrip_blocks": True
    }
    ```

6. Click **Submit** at the bottom to save the template changes.

**Note:**

The `trim_blocks` and `lstrip_blocks` settings are template parameters that control whitespace and formatting in the rendered output:

- `trim_blocks`: This removes any excess whitespace after a block. For example, it will remove trailing newlines after `{% endfor %}` blocks.

- `lstrip_blocks`: This removes any whitespace indentation from the beginning of blocks. For example, it will strip any initial indentation spaces or newlines before `{% for %}` blocks.

Enabling these parameters removes extraneous whitespace from the rendered configurations.

### Task 3 - Validate Improvement

1. Go back to the device which has the template assigned.
2. Re-render the configuration and compare to before.
3. Note that excess whitespace has now been removed from the rendered config.