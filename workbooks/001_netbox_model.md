# Workbook 1: The NetBox Model

## Learning Objectives

By the end of this workbook, you will be able to:
- Understand the NetBox data model and how core objects relate.
- Create tenants, regions, and sites.
- Import device types and add elevation images.
- Create and rack devices with roles and platforms.
- Configure full-depth rack elevations.
- Define VRFs, prefixes, and assign IP addresses.

## Overview
Through the following exercises, we will be populating a new NetBox instance based on the following.

In reality, many of the elements below will have **dotted lines** back to other elements. For example, our devices will all be placed within the Global Tech tenant. However, the below will act as a basic visualization overview and also as a reference, moving forward.

```
NetBox
|
|-- Organization
|   |-- Tenant
|   |   |-- Global Tech
|   |-- Regions
|   |   |-- Europe
|   |       |-- France
|   |-- Sites
|       |-- Paris Data Center
|
|-- Devices
|   |-- Device Types
|   |   |-- Nexus 92348GC-X
|   |-- Devices
|   |   |-- spine1
|   |   |-- spine2
|   |-- Platforms
|   |   |-- nxos
|   |-- Manufacturers
|   |   |-- Cisco
|
|-- Racks
|   |-- Rack1
|       |-- Devices
|           |-- spine1
|           |-- spine2
|
|-- IPAM (IP Address Management)
|   |-- VRFs
|   |   |-- Global Tech VRF
|   |-- Prefixes
|   |   |-- 172.29.151.0/24
|   |-- IP Addresses
|       |-- 172.29.151.1 (Assigned to spine1)
|       |-- 172.29.151.2 (Assigned to spine2)

```

## Before You Begin

The following prerequisites are required before beginning these exercises:

* Open a session to your NetBox instance directly within your browser.

> [!NOTE]Your username and password would have been supplied to you previously.




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
In this task, you'll establish a simple regional hierarchy that supports a single site.

1. Under the **Organization** menu, click **Regions**.
2. Click the **+ Add** button to add a new region.
3. Create the parent region **Europe** and leave the **Parent** field blank.
4. After saving, click **+ Add** again and create the child region **France**, selecting **Europe** as the parent.
5. Return to the **Regions** list and confirm both regions appear in the hierarchy.

### Task 3 - **Creating a Site**
Finally, you'll create a single site that represents the location housing your devices.

1. Under the **Organization** menu, click **Sites** and then **+ Add**.
2. Create the **Paris Data Center** site using the following values:

   | Field | Description |
   |---------|-------------|
   | Name    | Enter **Paris Data Center**. |
   | Status  | Select **Active**. |
   | Region  | Select **France**. |
   | Tenant  | Select **Global Tech**. |

3. Click **Create** and verify the new site is listed.


## Exercise 2: Working with NetBox Devices
In this exercise, you will import a device type, upload elevation imagery, and create two spine devices for the Paris Data Center.

### Task 1 – Import a Device Type from the Community Library
You'll use the NetBox community device type library to seed your environment with the required hardware definition.

1. Under the **Devices** menu, click **Manufacturers** and create the **Cisco** manufacturer if it does not already exist.
2. Visit the Device Type Library on GitHub. Click (here)[https://github.com/netbox-community/devicetype-library/].
3. Navigate to `device-types/Cisco/N9K-C9236C.yaml` to get the FULL YAML content.
4. Copy the YAML contents to your clipboard.
5. Return to NetBox and navigate to **Devices > Device Types > Import**.
6. Paste the YAML into the **Data** box, then click **Submit** to create the device type.

### Task 2 – Add Elevation Images to the Device Type
1. Download the elevation images from the GitHub repository:
   * `cisco-n9k-c9236c.front.jpg` [(link)](https://github.com/netbox-community/devicetype-library/blob/master/elevation-images/Cisco/cisco-n9k-c9236c.front.jpg)
   * `cisco-n9k-c9236c.rear.jpg` [(link)](https://github.com/netbox-community/devicetype-library/blob/master/elevation-images/Cisco/cisco-n9k-c9236c.rear.jpg)
2. In NetBox, open **Devices > Device Types**, select the newly imported **N9K-C9236C** device type, and click **Edit**.
3. Use the **Front Image** section to upload the front elevation JPG.
4. Use the **Rear Image** section to upload the front elevation JPG.
5. Set **Full Depth** to **Yes**.

### Task 3 – Create Spine Devices
Before creating devices, ensure the **nxos** platform and the **Spine** device role exist (create them if needed).

1. Navigate to **Devices > Devices** and click **+ Add**.
2. Create the device **spine1** with the following values:

   | Setting | Description |
   |---------|-------------|
   | Name    | **spine1** |
   | Device Role | **Spine** |
   | Device Type | **N9K-C9236C** |
   | Status  | **Active** |
   | Platform | **nxos** |
   | Site | **Paris Data Center** |
   | Tenant | **Global Tech** |

3. Repeat the process to create **spine2** using the same values.
4. Verify that both devices now appear in the device list.

## Exercise 3: Working with NetBox Racks
In this exercise, you'll install the spine devices into a rack and verify the front and rear elevation views.

### Task 1 – Create the Rack
1. Under the **Racks** menu, click **Racks** and then **+ Add**.
2. Create rack **R1** with the following values:

   | Setting | Description |
   |---------|-------------|
   | Site   | **Paris Data Center** |
   | Name | **R1** |
   | Status | **Active** |
   | Tenant | **Global Tech** |

3. Click **Create** to save the rack.

### Task 2 – Mount Devices in the Rack
1. Open **Devices > Devices**.
2. Edit **spine1** and set:
   * **Rack**: R1
   * **Position**: U42.0
   * **Face**: Front
   Then click **Update**.
3. Repeat for **spine2** using position **U41.0** on the **Front** face.

### Task 3 – Review Front and Rear Elevations
1. Navigate back to **Racks > Racks** and open **R1**.
2. View the **Front** elevation and confirm both devices appear in the expected positions.
3. View the **Rear** elevation and confirm both devices appear in the expected positions.

## Exercise 4: Working with the NetBox IPAM
In this exercise, you'll define a VRF and prefix, then assign management addresses to the spine devices.

### Task 1 – Create a VRF
1. Navigate to **IPAM > VRFs** and click **+ Add**.
2. Create the VRF using the following values:

   | Setting | Description |
   |---------|-------------|
   | Name | **Global Tech Production** |
   | RD   | Leave blank (NetBox will generate one). |
   | Tenant | **Global Tech** |

3. Click **Create** to save the VRF.
4. Leave the **RD Description** field blank.

### Task 2 – Create a Prefix
1. Go to **IPAM > Prefixes** and click **+ Add**.
2. Add the prefix with these values:

   | Setting | Description |
   |---------|-------------|
   | Prefix | **172.29.151.0/24** |
   | VRF | **Global Tech Production** |
   | Status | **Active** |
   | Site | **Paris Data Center** |
   | Tenant | **Global Tech** |

3. Click **Create** to save the prefix.

### Task 3 – Assign Management IPs to Devices
1. Open **Devices > Devices** and select **spine1**.
2. On the **Interfaces** tab, locate the management interface (for example, **mgmt0**).
3. Click **+** and choose **IP Address**.
4. Assign **172.29.151.1/24**, ensuring the **VRF** is **Global Tech Production** and the **Tenant** is **Global Tech**.
5. Repeat for **spine2** using **172.29.151.2/24**.

### Task 4 – Review Prefix Utilization
1. Return to **IPAM > Prefixes** and open **172.29.151.0/24**.
2. Confirm the utilization reflects the two assigned addresses.
3. Note the VRF relationship; this data will be consumed when generating device configurations in a later workbook.

