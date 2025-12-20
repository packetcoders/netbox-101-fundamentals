# Workbook 3: Config Generation

## Learning Objectives

TBC

## Overview

In this workbook we will create a configuration template using Jinja2 syntax, assign it to devices, and validate the rendered configurations.

## Before You Begin

1. Access your NetBox instance.
2. Ensure the Config Contexts (NTP, Syslog), custom field (`ospf_area`) is configured


## Exercise 1: Creating a Config Template

In this exercise we will build a template that consumes the data created in the previous workbooks.

### Task 1 – Observe Available Context Data

1. In the sidebar menu, click on **Devices**.
2. Under Devices, click on **Devices** to access the devices list.
3. Click on **spine1** to open its details page.
4. Switch to the **Render Config** tab.
5. Review the **Context Data** panel and confirm it includes:
   * `ntp_servers` and `syslog_servers` from Config Contexts.
   * `service_endpoint` data from the ServiceEndpoint custom object.

### Task 2 – Create the Nexus Template

1. In the sidebar menu, click on **Provisioning**.
2. Click on **Config Templates** to access the template list.
3. Click the **+ Add** button to create a new template.
4. In the **Name** field, enter **Nexus Template**.
5. In the **Template code** box, enter the following Jinja2 syntax (update the interface name if your management interface differs):

    ```
    hostname {{ device.name }}

    interface mgmt0
      description Management Interface
    {% if device.primary_ip4 %}
      ip address {{ device.primary_ip4.address }}
      ip vrf forwarding {{ device.primary_ip4.vrf.name }}
    {% endif %}

    {% if ntp_servers %}
    ! NTP servers delivered via Config Context
    {% for server in ntp_servers %}
    ntp server {{ server }}
    {% endfor %}
    {% endif %}

    {% if syslog_servers %}
    ! Syslog servers delivered via Config Context (respecting weights and overrides)
    {% for syslog_server in syslog_servers %}
    logging host {{ syslog_server }}
    {% endfor %}
    {% endif %}

    router ospf 1
      router-id {{ device.cf.ospf_router_id }}

    {% if service_endpoint %}
    ! External service reference provided by Custom Object
    ! {{ service_endpoint.name }} -> {{ service_endpoint.url }}
    {% endif %}
    ```

6. Scroll down to **Environment params** and add the following JSON to control whitespace:

    ```
    {
      "trim_blocks": true,
      "lstrip_blocks": true
    }
    ```

> [NOTE] This will ensure no erroneous white space is carried over to the rendered output.

7. Click **Submit** to create the template.

### Task 3 – Assign Template to Device

1. Go to the **Devices** list.
2. Click on **spine1** to edit its configuration.
3. Scroll down to the **Management** section.
4. Choose **Nexus Template** from the **Config Template** dropdown and click **Save**.

## Exercise 2: Rendering the Template

Here we will view the generated configuration for our device.

### Task 1 – View Rendered Configuration

1. From the devices list, open **spine1** and switch to the **Render Config** tab.
2. The rendered configuration should now be displayed.


### Task 2 – Download Rendered Config

1. While viewing the rendered config, click **Download**.
2. Save the file locally and review the configuration.

