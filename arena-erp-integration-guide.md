# Arena -- ERP/MES Integrations Guide

**Version:** Draft

## Abstract

This document serves as a comprehensive guide for Arena partners and customers looking to create integrations between Arena and ERP/MES systems. It outlines essential preparations, resources, responsibilities, and various use cases for successful integrations. This guide is provided as-is, based on best practices of Arena team with customers and partners. It is up to the user to review and adapt.

## Audience

The guide is aimed at Arena partners and customers who are developing ERP/MES integration products.

---

## Preparation -- What Should You Learn About Arena Before Beginning

Your team should review Arena documentation and recorded events provided as part of onboarding. This material should provide you with a good understanding of the following:

- Complete Arena 101 for Integrations training to understand:
  - Lifecycles
  - Categories
  - API GUIDs
  - Triggers & Events
  - Export definition
  - Change Management
  - Custom Attributes
  - Item Revisions and Working Revisions
  - File Editions

> **Note:** Training links will be provided in future updates.

- Complete [Arena Developer Platform training](https://github.com/ptc-arena/arena-integration-education) to understand:
  - Arena REST API
  - Integration Engine: Event Engine, Export Engine, Import Engine
  - API machine user license (requirement for all system-to-system integrations)

---

## Resources and Responsibilities

Connected critical enterprise systems such as PLM and ERP/MES require participation across business teams and span multiple data sets as well as the administrators of these systems. Minimally, the following teams should be involved in the process:

1. **Arena administrator(s):** The team that manages the Arena configuration control.
2. **Change management team:** Manages engineering change processes, including the release of engineering changes and transfer to manufacturing/operations.
3. **ERP/MES administrator(s):** Manages the ERP and/or MES configuration control.
4. **Manufacturing product team(s):** Manages item masters/BOMs in ERP/MES and any associated actions, such as quality processes or inventory disposition if the integration is bi-directional.

> **Tip:** For technical resources, ensure a mix of low-code developers for Arena and IT/development experts familiar with ERP/MES systems.

---

## Most Common ERP / MES Use Cases

### PLM → ERP Integration Use Cases

1. First release of new Items, new BOM parent-child relationships. Push ECO with affected Items to ERP. Affected items have:
   - New attribute value
   - New lifecycle
   - Updated supplier information
2. Additional release of Items, new revisions, changes to Item data.
3. New revisions of BOM parent-child relationships (e.g., changes in quantity, reference designators).
4. Removal of BOM parent-child relationships (redlines of removed child assemblies).
5. Multiple BOMs, multiple Items modified by a change.
6. Multiple sequential changes to same Items, BOMs processed sequentially.

### ERP → PLM Integration Use Cases

1. Updates in ERP to non-revision-controlled Item and BOM attributes (e.g., inventory dispositions and costing).

### Arena → MES Integration Use Cases

1. First release of new Items, new BOM parent-child relationships. Push ECO with affected Items to MES:
   - Affected items have new attribute value, lifecycle, and updated supplier information.
   - Files may also be included.
2. Additional release of Items, new revisions, changes to Item data.
3. New revisions of BOM parent-child relationships (e.g., quantity, reference designators).
4. Removal of BOM parent-child relationships (redlines of removed child assemblies).
5. Multiple BOMs, multiple Items modified by a change.
6. Multiple sequential changes to same Items, BOMs processed sequentially.

### MES → Arena Integration Use Case

1. **Quality Issue Flow:** If a quality issue occurs in MES that meets certain criteria, it needs to flow into Arena for resolution collaboration.

   - Use the Quality World in Arena with a template for MES/vendor issue resolution.
   - Resolutions could result in a change order or another action. If a change order, the released change order flows back to MES.

> **Methods:**
> - Use API to create a Quality process using a specified template and map ETL data.
> - Use Event Engine to trigger data output for updates to MES.

---

## Integration Details -- Things to Consider

- **Process Flows:** Define triggers, scheduling, and frequency.
  - For Arena → ERP/MES, trigger on a Change Order entering a "Completed" status.
  - Use Event Engine to specify which Change Order categories should result in data being queued.
- **Minimum Required Data:**
  - Item
  - Parent → Child BOM
- **Custom Fields:** Arena allows custom fields for objects (e.g., Item, Change, Supplier, BOM relationships).
- **Bi-directional Flows:** Consider error handling, first-time sync, and frequency of updates.

---

## Getting Started -- The Basic Steps to Creating an Integration in Arena

1. Provision and set up the Arena workspace.
2. Configure Arena users, access, and licensing.
   - Use an API machine user license.
3. Set up Arena Outbound Events in the UI:
   - Contact [Arena Support](mailto:arena-support@ptc.com) to enable Outbound Event settings.
4. Create triggers and integrations.
5. Import sample data into Arena for testing.
6. Complete ERP extraction process:
   - Monitor Event Queue.
   - Export data.
   - Process resulting data.
   - Handle errors.

---

## Common Questions

1. **What is the rate limit for APIs?**
   - The rate limit is typically **10,000 calls per 24-hour period**. API limits depend on the customer's package. Reference [API Licensing Guide](https://app.bom.com/static/webhelp/full/en_US/ApplicationHelp.htm#html/api_licensing.html?Highlight=API%20calls%2010000).

2. **How can I re-process data for a trigger while testing integration development?**
   - Reconcile data either at the event level or item level.

3. **What Arena Access Policy permissions should an API Machine User have?**
   - Grant "just enough" permissions for the required actions (e.g., read, create, edit, delete).

---

Let me know if you'd like further refinements or specific visual aids added!
