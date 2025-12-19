## Update Plan for NetBox 4.x Workbooks

You goal is to make the required edits to the workbooks files within the workbook folder.

### Changes

**Changes for all workbooks**
- [ ] Update all workbook titles to use proper title case
- [ ] Ensure consistent heading casing throughout each workbook

**001_netbox_model.md**
- [ ] Only have the users create 2 devices: spine1 and spine2
- [ ] Have them only create 1 region, 1 parent region, 1 site
- [ ] Add steps to add elevation image to rack
- [ ] Add step to enable option so front and rear faces are visible
Below is some steps for reference:

Under the creation of the device type,

Task 2 – Adding an Elevation Image
We will now add a front and rear image to the device type we just created, which will then show within the Rack Elevation view once we add the device to a rack.

Go to https://github.com/netbox-community/devicetype-library/tree/master/elevation-images
Download the following images:
cisco-ws-c2960s-48lpd-l.front.png
cisco-ws-c2960s-48lpd-l.rear.png
Under the Devices menu, click Device Types.
Click on the device type added
Click on the Front Image tab and upload the cisco-ws-c2960s-48lpd-l.front.png image.
Click on the Rear Image tab and upload the cisco-ws-c2960s-48lpd-l.rear.png image.

then at the right point add this to the rack part 

Task 3 – Check Rack Elevation
Before proceeding, confirm how your device appears in the rack.

Go to Organization > Racks.
Click on your rack R1.
Review the Front elevation view.
Switch to the Rear elevation view.
At this point, you will notice that the device only appears on the Front view. This is expected because the device type is not yet marked as full depth.

Task 4 – Fix Rear Elevation
To enable the rear elevation image, you must update the device type to be Full Depth.

Go to Devices > Device Types.
Select your device type (for example, <student_id>_Cisco IOSv).
Click Edit.
Set Full Depth to Yes.
Click Update to save the change.
Return to Organization > Racks, open rack R1, and switch between Front and Rear views. You should now see both elevation images displayed correctly.

**002_extending_the_model.md**
This workbook will have excercises for:
- [ ] Config Contexts (keep original)
- [ ] Custom Fields (move from 001_netbox_model.md)
- [ ] Custom objects (add one)

**003_config_generation.md**
- [ ] Update tasks to use data from config contexts, custom fields, and custom objects from previous steps
- [ ] reuse existing steps from previous workbook but reorder or restructure them as needed
