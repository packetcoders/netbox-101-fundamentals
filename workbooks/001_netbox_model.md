# Workbook 1: The NetBox Model

## Learning Objectives

By completing these exercises, students will be able to:

* Create and manage key organizational elements in NetBox including tenants, regions, sites, manufacturers, and platforms.
* Import device types from an external library and create devices with specific attributes.
* Manage IP Address space by creating a prefix and assigning IP addresses to devices from that prefix.
* Understand the process of adding devices to a rack and visualizing them in a rack elevation.

## Overview
Through the following exercises, we will be populating a new NetBox instance based on the following.

In reality, many of the elements below will have **dotted lines** back to other elements. For example, our devices will all be placed within the Global Tech tenant. However, the below will act as a basic visualization overview and also as a reference, moving forward.

```
NetBox
|
|-- Organizations
|   |-- Tenants
|   |   |-- Global Tech
|   |-- Regions
|   |   |-- Europe
|   |   |   |-- France
|   |   |   |-- Germany
|   |   |-- North America
|   |   |   |-- United States
|   |   |   |-- Canada
|   |-- Sites
|   |   |-- Paris Data Center
|   |   |-- Berlin Data Center
|   |   |-- New York Data Center
|   |   |-- Toronto Data Center
|
|-- Devices
|   |-- Device Types
|   |   |-- Nexus 92348GC-X
|   |   |-- Catalyst 9300-24P
|   |-- Devices
|   |   |-- spine1-nxos
|   |   |-- spine2-nxos
|   |   |-- leaf1-ios
|   |   |-- leaf2-ios
|   |-- Platforms
|   |   |-- nxos
|   |   |-- ios
|   |-- Manufacturers
|   |   |-- Cisco
|
|-- Racks
|   |-- Rack1
|   |   |-- Devices
|   |   |   |-- spine1-nxos
|   |   |   |-- spine2-nxos
|   |   |   |-- leaf1-ios
|   |   |   |-- leaf2-ios
|
|-- IPAM (IP Address Management)
|   |-- Prefixes
|   |   |-- 172.29.151.0/24
|   |-- IP Addresses
|   |   |-- 172.29.151.1 (Assigned to spine1-nxos)
|   |   |-- 172.29.151.2 (Assigned to spine2-nxos)
|   |   |-- 172.29.151.3 (Assigned to leaf1-ios)
|   |   |-- 172.29.151.4 (Assigned to leaf2-ios)

```

## Before you Begin

The following prerequisites are required before beginning these exercises:

* Open a session to your NetBox instance directly within your browser.

Note: Your username and password would of been supplied to you previously.




## Exercise 1: Working with NetBox Regions and Sites
In this exercise, you'll set up the organizational structure of your network in NetBox by creating tenants, regions, and sites.

### Task 1 - **Creating a Tenant**
Here, you'll define the tenant to segregate resources in NetBox.

1. Under the **Organizations** menu, click **Tenants**.
2. Click the **+ Add** button.
3. Fill in the required fields in the form:

   | Setting        | Description |
   |----------------|-------------|
   | Name           | Enter **Global Tech** |
   | Slug    | This field will auto-populate. |

4. Click on the **Create** button to save the new tenant.



### Task 2 - **Creating Regions**
In this task, you'll create the regions, allowing you to organize your sites geographically.

1. Under the **Organization** menu, click **Regions**.
2. Click the **+ Add** button to add a new region.

3. First, let's create the parent region **Europe**:

   a. Fill in the required fields in the form:

   | Setting | Description |
   |---------|-------------|
   | Name    | Enter **Europe**. |
   | Slug    | This field will auto-populate. |
   | Parent  | Leave this blank, as this is a parent region. |

   b. Click on the **Create** button to save the new region.

4. Repeat the steps for creating the parent region **North America**.

5. Now, let's create child regions. First, we'll create **France** under **Europe**:

   a. Click the **+ Add** button to add a new region.

   b. Fill in the required fields in the form:

   | Setting | Description |
   |---------|-------------|
   | Name    | Enter **France**. |
   | Slug    | This field will auto-populate. |
   | Parent  | Select **Europe** from the dropdown menu. |

   c. Click on the **Create** button to save the new child region.

6. Repeat the above steps to create the second child region **Germany** under **Europe**.

7. Next, let's create child regions under **North America**. Create **United States**:

   a. Click the **+ Add** button to add a new region.

   b. Fill in the required fields in the form:

   | Field | Description |
   |---------|-------------|
   | Name    | Enter **United States**. |
   | Slug    | This field will auto-populate. |
   | Parent  | Select **North America** from the dropdown menu. |

   c. Click on the **Create** button to save the new child region.

8. Repeat the above steps to create the second child region **Canada** under **North America**.

9. Now under the **Organization** menu, click **Regions**, to see the regions you have just created.

### Task 3 - **Creating Sites**
Finally, you'll create sites which represent physical locations in your network.


1. Under the **Organization** menu, click **Sites**.
2. Click the **+ Add** button to add a new site.

3. Now, let's create the first site in France called **Paris Data Center**:

   a. Fill in the required fields in the form:

   | Field | Description |
   |---------|-------------|
   | Name    | Enter **Paris Data Center**. |
   | Slug    | This field will auto-populate. |
   | Status  | Select **Active**. |
   | Region  | Select **France** from the dropdown menu. |
   | Tenant  | Select **Global Tech** |

   b. Click on the **Create** button to save the new site.

4. Repeat the above steps to create the following sites:

| Site Name              | Region       |
|------------------------|--------------|
| Berlin Data Center     | Germany      |
| New York Data Center   | United States|
| Toronto Data Center    | Canada       |

5. Now under the **Organization** menu, click **Sites**, to see the sites you have just created.


## Exercise 2: Working with NetBox Devices
In this exercise, you will define the hardware present in your network by importing device types and creating devices.

### Task 1 – Create a device type via library import
Task 1 - **Creating a Device Type via Library Import**
You'll use the NetBox community's device type library to import definitions for your hardware.

1. Before importing a device type, we need to ensure that the required manufacturers are available.

   **Creating a Manufacturer**

      a. Under the **Devices** menu, click **Manufacturers**.

      b. Click the **+ Add** button to add a new manufacturer.

      c. Fill in the required fields in the form to create the **Cisco** manufacturer:

      | Setting | Description |
      |---------|-------------|
      | Name    | Enter **Cisco**. |
      | Slug    | This field will auto-populate. |

      d. Click the **Create** button to save the new manufacturer.

2. Open your web browser and navigate to the Device Type Library on GitHub: [https://github.com/netbox-community/devicetype-library/](https://github.com/netbox-community/devicetype-library/).

3. Use the search bar to find the required device types. Let's start with **Nexus 92348GC-X**.

   a. Type **92348GC-X** in the search bar and press Enter.

   b. At this point you will only see a preview of the template. Click on the template to see all of it (should be ~129 lines long).

   c. Highlight the YAML for the device type and copy it to your clipboard.

5. Now, go back to NetBox.

   a. Under the **Devices** menu, click **Device Types**.

   b. Click on the **Import** button.

   c. Paste the device type template YAML into the **Data** box.

   d. Click the **Submit** button to import the device type.

6. Repeat the above steps to import the **9300-24P** device type.

7. Once complete, under the **Devices** menu, click **Device Types**.

   a. Click on each device type added
   b. Click on the **Interfaces** tab and view the interfaces for the device type.

Note: The purpose of viewing the device type added is just to understand the new hardware that will be added to NetBox each time we use these device types when creating devices.


### Task 2 - **Creating a Device**
After defining the necessary device types, you'll create instances of these devices, representing specific pieces of hardware in your network.

1. Before creating a device, we need to ensure that the required platforms and roles are available.

   **Creating Platforms**

     a. Under the **Devices** menu, click **Platforms**.

     b. Click the **+ Add** button to add a new platform.

    c. Fill in the required fields in the form to create the **nxos** platform:

      | Setting | Description |
      |---------|-------------|
      | Name    | Enter **nxos**. |
      | Slug    | This field will auto-populate. |
      | Manufacturer | Select **Cisco** from the dropdown menu. |

      iv. Click the **Create** button to save the new platform.

      v. Repeat these steps to add another platform, **ios**.

   **Creating Roles**

      a. Under the **Devices** menu, click **Device Roles**.

      b. Click the **+ Add** button to add a new role.

      c. Fill in the required fields in the form to create the **spine** role:

      | Setting | Description |
      |---------|-------------|
      | Name    | Enter **Spine**. |
      | Slug    | This field will auto-populate. |

      d. Click the **Create** button to save the new role.

      e. Repeat these steps to add another role, **Leaf**.

2. Now, with the platforms and manufacturer ready, we can create our devices. Under the **Devices** menu, click **Devices**.
3. Click the **+ Add** button to add a new device.

4. Let's create the first device, **spine1-nxos**:

   a. Fill in the required fields in the form:

   | Setting | Description |
   |---------|-------------|
   | Name    | Enter **spine1-nxos**. |
   | Device Role | Select the appropriate role (e.g., **Spine**). |
   | Device Type | Select **Nexus 92348GC-X**. |
   | Device Status | Select **Active**. |
   | Platform | Select **nxos**. |
   | Site | Select the **Paris Data Center** site. |
   | Tenant  | Select **Global Tech** |

   b. Click on the **Create** button to save the new device.

6. Repeat the above steps to create the following devices:

| Device Name|   Device Type | Platform |
|-------------|-------------|----------|
| spine2-nxos  | Nexus 92348GC-X | nxos |
| leaf1-ios | Catalyst 9300-24P | ios |
| leaf2-ios | Catalyst 9300-24P | ios |


## Exercise 3: Working with NetBox Racks
In this exercise, you'll be working with the physical layout of your network hardware by placing devices into racks.



### Task 1 - **Adding Devices to a Rack**
Here, you'll place your previously defined devices into a rack.

1. Under the **Organization** menu, click **Racks**.
2. Click the **+ Add** button to create a new rack.
3. Fill in the required fields below to create a new rack and click the **Create** button.

 | Setting | Description |
   |---------|-------------|
   | Site   | Enter **Paris Data Center**. |
   | Name | Enter **R1**. |
   | Status | Select **Active**. |
   | Tenant | Select **Global Tech**. |

4. Now, under the **Devices** menu, click **Devices**.
5. For each device:

   a. Click on the device name to open the device details.

   b. Click **Edit** to open the device edit form.

   c. In the form, find the **Rack** field and select **R1**.

   d. Specify the **Position** in the rack where the device is installed. Positions are shown below:

   | Device Name  | Postion | Rack Face |
   |-------------|-------------| ---- |
   | spine1-nxos  | **U42.0** | Front |
   | spine2-nxos  | **U41.0** | Front |
   | leaf1-ios |    **U40.0** | Front |
   | leaf2-ios |    **U39.0** | Front |

   e. Click on the **Update** button to save the changes.

### Task 2 - **Viewing the Rack Elevation**
After adding devices to the rack, you'll be able to visualize the physical layout of hardware in the rack.


1. Go back to the **Organization** menu and click **Racks**.

2. Notice the Space utilization column on the right hand side.

2. Click on the name of the rack to which you assigned the devices.

3. You should now be able to see a graphical representation of the rack, showing the devices installed in it. This is the rack elevation. Devices are shown at the positions you assigned to them.





## Exercise 4: Working with the NetBox IPAM
In this exercise, you'll work with NetBox's IP address management (IPAM) features to manage your network's IP space.

### Task 1 - **Creating a Prefix**
First, you'll define a block of IP space that you'll be assigning addresses from.


1. Under the **IPAM** menu, click **Prefixes**.
2. Click the **+ Add** button to add a new prefix.
3. Fill in the required fields in the form:

   | Setting | Description |
   |---------|-------------|
   | Prefix | Enter **172.29.151.0/24**. |
   | Status | Select the appropriate status (e.g., **Active**). |
   | Site | Select the **Paris Data Center**. |
   | Tenant | Select **Global Tech**. |

4. Click on the **Create** button to save the new prefix.

### Task 2 - **Assigning an IP from the Prefix**
Next, you'll assign individual addresses from your defined prefix to your devices management interfaces.


1. Go back to the **Devices** menu and click **Devices**.
2. For each device:

   a. Click on the device name to open the device details.

   b. In the device details, click **Interfaces**.

   c. Click **Configure Table**, and select **Management Only** as another selected column. Click **Save**. You will only need to do this once.

   d. Now you will see the management interface, denoted with a green tick.

   e. Click the **+** to the right of the interface, and select **IP Address**.

   f. Add the IP of the management interface. Use the nessecary IP below based on device.

      | Device Name  | Address |
      |-------------|-------------|
      | spine1-nxos  | **172.29.151.1/24** |
      | spine2-nxos  | **172.29.151.2/24** |
      | leaf1-ios |    **172.29.151.3/24** |
      | leaf2-ios |    **172.29.151.4/24** |

   g. Select **Tenant** as **Global Tech**.

   f. Click on the **Update** button to save the changes.

### Task 3 - **Viewing Prefix Utilization**
Finally, you'll check the utilization of your defined prefix to see how many addresses are left.

1. Go back to the **IPAM** menu and click **Prefixes**.
2. Find the prefix **172.29.151.0/24** in the list and click on it to open the prefix details.
3. In the prefix details, you can see the **Utilization** bar.
4. Click on the **IP Addresses** tab to see the assigned IPs.

## Exercise 5: Working with Custom Fields
In this exercise, you'll work with NetBox custom fields.

### Task 1 - **Creating a Custom Field**
You will now create a new custom field to store the OSPF Router ID for each device.

1. Go to **Customization** on the left of the NetBox UI.
2. Click **+** next to **Custom Fields**.
3. Create a new custom field by selecting the following options:

      |   |  |
      |-------------|-------------|
      | **Content types**  | **DCIM > Device** |
      | **Name**  | **ospf_router_id** |
      | **Type** |    **Text** |

### Task 2 - **Populating a Custom Field**

1. Goto each Device.
2. Add their OSPF Router ID using the new custom field created, using the following values:

      | Device Name  | Custom Field Value |
      |-------------|-------------|
      | spine1-nxos  | **1.1.1.1** |
      | spine2-nxos  | **2.2.2.2** |
      | leaf1-ios  |    **3.3.3.3** |
      | leaf2-ios   |    **4.4.4.4** |
