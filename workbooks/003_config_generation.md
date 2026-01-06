# Workbook 3: Config Generation

## Learning Objectives

By the end of this workbook, you will be able to:

- Create a configuration template using Jinja2 in NetBox.
- Consume data from Config Contexts, Custom Fields, and device objects.
- Assign config templates to devices.
- Render and validate device configurations.
- Download rendered configurations for review or deployment.

## Overview

In this workbook we will create a configuration template using Jinja2 syntax, assign it to devices, and validate the rendered configurations.

## Before You Begin

1. Access your NetBox instance.
2. Ensure the Config Contexts (`syslog_servers`), and the custom field (`ospf_area`) are configured.


## Exercise 1: Creating a Config Template

In this exercise we will build a template that consumes the data created in the previous workbooks.

### Task 1 – Observe Available Context Data

1. In the sidebar menu, click on **Devices**.
2. Under Devices, click on **Devices** to access the devices list.
3. Click on **spine1** to open its details page.
4. Switch to the **Render Config** tab.
5. Review the **Context Data** panel and confirm it includes:
   * `syslog_servers` from Config Contexts.
   
> [!NOTE]
> We reference the custom field data via the device object - `<Device: spine1>`.

### Task 2 – Create the Config Template

1. In the sidebar menu, click on **Provisioning**.
2. Click on **Config Templates** to access the template list.
3. Click the **+ Add** button to create a new template.
4. Enter **NXOS Config Template** in the **Name** field.
5. In the **Template code** box, enter the following Jinja2 syntax:

    ```
    hostname {{ device.name }}

    {% for syslog_server in syslog_servers %}
    logging host {{ syslog_server }}
    {% endfor %}

    {% for interface in device.interfaces.all() %}
      interface {{ interface.name }}
      {% if interface.ip_addresses.all() %}
        {% for ip in interface.ip_addresses.all() %}
        ip address {{ ip.address  }} 
        {% if interface.cf.ospf_area %}
        ip router ospf 1 area {{ interface.cf.ospf_area }}
        {% endif %}
        no shutdown
        {% endfor %}
      {% else %}
        no ip address
        shutdown
      {% endif %}
    {% endfor %}

    ip route 0.0.0.0 0.0.0.0 172.29.151.254

    router ospf 1
      router-id 1.1.1.1
    ```

6. Scroll down to **Environment params** and add the following JSON to control whitespace:

    ```
    {
      "trim_blocks": true,
      "lstrip_blocks": true
    }
    ```

> [!NOTE]
> This will ensure no erroneous white space is carried over to the rendered output.

7. Click **Submit** to create the template.

### Task 3 – Assign Template to Device

1. Go to the **Devices** list.
2. Click on **spine1** to edit its configuration.
3. Scroll down to the **Management** section.
4. Choose **NXOS Config Template** from the **Config Template** dropdown and click **Save**.

## Exercise 2: Rendering the Template

Here we will view the generated configuration for our device.

### Task 1 – View Rendered Configuration

1. From the devices list, open **spine1** and switch to the **Render Config** tab.
2. The rendered configuration should now be displayed.


### Task 2 – Download Rendered Config

1. While viewing the rendered config, click **Download**.
2. Save the file locally and review the configuration.

> [!TIP]
> If the rendered configuration does not include the `ospf_area` custom field output (for example, `ip router ospf 1 area ...` under an interface), return to Workbook 2 and verify the **ospf_area** custom field is populated for each interface before re-rendering.

🎉 **CONGRATULATIONS!**
You have now successfully modeled a simple network, extended the model, and automated the generation of device configuration all from within NetBox.

