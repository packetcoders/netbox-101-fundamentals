# Update Plan for NetBox 4.x Workbooks

## Summary
- Align all workbook content with NetBox 4.x UI/UX and terminology.
- Tighten scope per workbook to avoid duplication while preserving learning flow.
- Apply consistent title/heading casing and workbook naming across the set.

## Preparation
1. Capture the current versions of `001_netbox_model.md`, `002_extending_the_model.md`, and `003_config_generation.md` for reference.
2. Confirm whether any navigation, README, or workbook index files reference the old workbook names.
3. Gather required assets (device elevation PNGs, screenshots if needed) and validate URL accessibility.

## Global Updates
- [ ] Apply proper title casing to main headings and key subheadings in all workbooks.
- [ ] Normalize terminology for menus, buttons, and options based on NetBox 4.x labels.
- [ ] Ensure each workbook’s opening section states prerequisites and expected lab state.
- [ ] Add cross-links or references if tasks in later workbooks depend on earlier ones.

Below is the outline of exercises and tasks for all workbooks:

## Workbook 001 — The NetBox Model
### Expected layout
- Exercise 1: Working with NetBox Regions and Sites
  - Task 1 – Creating a Tenant
  - Task 2 – Creating Regions
  - Task 3 – Creating Sites
- Exercise 2: Working with NetBox Devices
  - Task 1 – Create a Device Type via Library Import
  - Task 2 – Add Elevation Images
  - Task 3 – Creating a Device
- Exercise 3: Working with NetBox Racks
  - Task 1 – Adding Devices to a Rack
  - Task 2 – Viewing the Rack Elevation
- Exercise 4: Working with the NetBox IPAM
  - Task 1 – Creating a VRF
  - Task 2 – Creating a Prefix
  - Task 3 – Assigning an IP from the Prefix
  - Task 4 – Viewing Prefix Utilization


### Scope Adjustments
- [ ] Limit device creation tasks to **spine1** and **spine2** only.
- [ ] Restrict organizational hierarchy creation to a single parent region, one child region, and a single site.

### Device Type Enhancements
- [ ] Add explicit instructions for obtaining and uploading front/rear elevation images.
- [ ] Introduce a checklist-driven flow for the elevation image upload sequence.
- [ ] Highlight the **Full Depth** requirement and the resulting behavior in front/rear rack views.

### Rack Validation Beats
- [ ] Guide learners through verifying rack elevations before and after enabling Full Depth.
- [ ] Include expected outcome notes and troubleshooting tips if images do not display.

## Workbook 002 — Extending the Model 
### Expected layout
- Exercise 1: Working with Config Contexts
  - Task 1 – Create Config Contexts
  - Task 2 – Altering Config Context Weight
  - Task 3 – Create Local Config Contexts
  - Task 4 – Validate the Config Context Data

- Exercise 4: Working with Custom Fields
  - Task 1 – Create Custom Field
  - Task 2 – Assign Custom Field
  - Task 3 – Populate Custom Field

- Exercise 5: Working with Custom Objects
  - Task 1 – Create Custom Object Type
  - Task 2 – Create Custom Object
  - Task 3 – Reference Custom Object


### Content Realignment
- [ ] Keep Config Context exercises as the foundation and refresh screenshots/text for NetBox 4.x.
- [ ] Relocate Custom Field content from Workbook 001, expanding it with creation, assignment, and usage steps.




## Workbook 003 — Config Generation
### Expected layout
- Exercise 1: Creating a Config Template
  - Task 1 – Observe Available Context Data
  - Task 2 – Create a Template
- Exercise 2: Render Config Template
  - Task 1 – Assign Template to Devices
  - Task 2 – Validate Rendered Config
  - Task 3 – Download Rendered Config
- Exercise 3: Update Template Whitespace
  - Task 1 – Validate Rendered Config
  - Task 2 – Apply Whitespace Control
  - Task 3 – Validate Improvement

### Template Usage Refresh
- [ ] Ensure tasks consume Config Context data, Custom Fields, and Custom Objects established in Workbook 002.
- [ ] Rewrite steps to clarify how context data is made available to templates in NetBox 4.x.

### Structure & Reuse
- [ ] Reorder or consolidate overlapping steps from Workbook 002, keeping focus on rendering and validating configs.
- [ ] Surface whitespace control guidance alongside template creation rather than as an afterthought.

## Validation & QA
- [ ] Perform a dry run of each workbook to confirm the final rack/config outputs align with expectations.
- [ ] Cross-check for spelling, grammar, and casing consistency.
- [ ] Update any navigation or workbook index files to reflect renamed workbooks and rearranged topics.
