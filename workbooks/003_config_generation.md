# Workbook 3: Config Generation

## Learning Objectives

TBC
## Overview

In this workbook we will create a configuration template using Jinja2 syntax, assign it to devices, and validate the rendered configurations.

## Before You Begin

1. Access your NetBox instance.
2. Ensure the Config Contexts (NTP, Syslog, Service Endpoint), custom field (`ospf_router_id`), and ServiceEndpoint custom object were completed in Workbook 2.
3. Confirm the spine devices from Workbook 1 have management IPs assigned within the Global Tech Production VRF.

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
   * `device.cf.ospf_router_id` from the custom field.
   * The device’s assigned management address.

### Task 2 – Create the Cisco Template

1. In the sidebar menu, click on **Provisioning**.
2. Click on **Config Templates** to access the template list.
3. Click the **+ Add** button to create a new template.
4. In the **Name** field, enter **Cisco Template**.
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

7. Click **Submit** to create the template.

### Task 3 – Verify Template Compilation

1. Open the newly created **Cisco Template** and click **Preview**.
2. Choose **spine1** in the preview form and render the template.
3. Confirm the output contains the management interface, OSPF router ID, NTP/Syslog blocks, and service endpoint comment.

## Exercise 2: Rendering the Template

Here we will assign the template to the spine devices and validate the rendered configuration.

### Task 1 – Assign Template to Devices

1. In the sidebar menu, click on **Devices**.
2. Under Devices, click on **Devices** to access the devices list.
3. Using the check boxes, select **spine1** and **spine2**.
4. Click the **Edit Selected** button at the bottom of the screen.
5. In the form that appears, scroll down to the **Configuration** section.
6. Choose **Cisco Template** from the **Config Template** dropdown and click **Save**.

### Task 2 – Validate Rendered Output

1. From the devices list, open **spine1** and switch to the **Render Config** tab.
2. Confirm the rendered configuration matches the preview results, including:
   * Management interface stanza with VRF assignment.
   * NTP and Syslog blocks reflecting the weighted Config Contexts.
   * OSPF router ID sourced from the custom field.
   * Service endpoint comment from the custom object data.
3. Repeat for **spine2** and verify the router ID reflects its custom field value.

### Task 3 – Download Rendered Config

1. While viewing the rendered config, click **Download**.
2. Save the file locally and review it to ensure whitespace is trimmed as expected.

## Exercise 3: Iterating on Data-Driven Outputs

This exercise highlights how data changes propagate through the rendered configuration.

### Task 1 – Update Shared Data

1. Navigate to **Provisioning > Config Contexts** and edit the **Syslog 2** context.
2. Replace one of the syslog server IPs with a new value and click **Submit**.

### Task 2 – Re-render and Observe

1. Return to **Devices > Devices > spine1** and open the **Render Config** tab.
2. Verify the syslog section reflects the updated value.
3. Optionally adjust the `ospf_router_id` custom field or the ServiceEndpoint object to see how those changes appear in the rendered output.
