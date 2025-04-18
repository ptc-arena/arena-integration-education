# Arena -- ERP/MES Integrations Guide

## Abstract

This document serves as a comprehensive guide for Arena partners and
customers looking to create integrations between Arena and ERP/MES
systems. It outlines essential preparations, resources,
responsibilities, and various use cases for successful integrations.

## Audience

The guide is aimed at Arena partners and customers who are developing
ERP/MES integration products.

## Preparation -- What Should You Learn About Arena Before Beginning

Your team should review Arena documentation and recorded events provided
as part of onboarding. This material should provide you with a good
understanding of the following:

-   Complete Arena 101 for Integrations training to understand

    -   Lifecycles

    -   Categories

    -   API GUIDs

    -   Triggers & Events

    -   Export definition

    -   Change Management

    -   Custom Attributes

    -   Item Revisions and Working Revisions

    -   File Editions

-   Complete Arena Developer Platform training to understand

    -   Arena REST API

    -   Integration Engine: Event Engine, Export Engine, Import Engine

    -   API machine user license -- requirement for all system-to-system
        integrations

## Resources and Responsibilities

Connected critical enterprise systems such as PLM and ERP/MES requires
participation across business teams and spans multiple data sets as well
as the administrators of these systems. Minimally, the following teams
should be involved in the process.

1.  Arena administrator(s) - the team that manages the Arena
    configuration control

2.  Change management team -- the team that manages engineering change
    processes, including the release of engineering changes and transfer
    to manufacturing/operations

3.  ERP/MES administrator(s) - the team that manages the ERP and/or MES
    configuration control

4.  Manufacturing product team(s) -- the team that manages item
    masters/BOMs in ERP/MES and any associated actions such as quality
    processes or inventory disposition if the integration will be
    bi-directional with data from ERP/MES back to Arena

Additionally, technical resources to build a robust integration are
necessary. Consider the following when assembling technical resources.

1.  For Arena, the Arena Developer Platform provides a REST API and a
    powerful low-code Event Engine to provide data in a format suitable
    for the data flow to ERP/MES. A low-code developer or IT resource
    with ability to understand \....

2.  For ERP/MES, IT or development resources familiar with inputting
    data into item master/BOM, AML/AVL, and file data into appropriate
    modules is necessary. Customers can utilize their ERP/MES
    development resources, which may be in-house, through the ERP/MES
    vendor, or an systems integrator. Arena does not recommend utilizing
    generic development resources with no familiarity or experience with
    the specific ERP/MES as these systems can be complex and customized
    uniquely to a customer environment.

## Most Common ERP / MES Use Cases

### PLM \--\> ERP Integration Use Cases

1.  First release of new Items, new BOM parent-child relationships\
    Push ECO with affected Items to ERP. Affected items have new
    attribute value, new lifecycle and updated supplier information

2.  Additional release of Items, new revisions, changes to Item data

3.  New revisions of BOM parent-child relationships -- changes in BOM
    data fields such as quantity, reference designators

4.  Removal of BOM parent-child relationships -- redlines of child
    assemblies removed from a BOM

5.  Multiple BOMs, multiple Items modified by a change

6.  Multiple sequential changes to same Items, BOMs processed
    sequentially

### ERP \--\> PLM Integration Use Cases

1.  Updates in ERP to non-revision controlled Item and BOM attributes,
    such as inventory dispositions and costing

### Arena \--\> MES Integration Use Cases

1.  First release of new Items, new BOM parent-child relationships\
    Push ECO with affected Items to MES. Affected items have new
    attribute value, new lifecycle and updated supplier information.
    Files may also be included.

2.  Additional release of Items, new revisions, changes to Item data.
    Update of files.

3.  New revisions of BOM parent-child relationships -- changes in BOM
    data fields such as quantity, reference designators

4.  Removal of BOM parent-child relationships -- redlines of child
    assemblies removed from a BOM

5.  Multiple BOMs, multiple Items modified by a change

6.  Multiple sequential changes to same Items, BOMs processed
    sequentially

### MES \--\> Arena Integration Use Case 

1.  The Use Case: If quality issue occurs in MES that meets certain
    criteria, it needs to flow the issue / quality concern to Arena for
    resolution collaboration.

2.  Customer can use the Quality World in Arena with a template for
    MES/vendor issue resolution. Resolutions → Could result in a change
    order or some other action. If a change order, then resulting
    released change order would flow back to MES.

    a.  Does the resolution of the Quality process need to flow back
        into MES? If so - does it got back to that particular ION issue
        and what is updated?

3.  Methods

    a.  API to create a Quality process using a specified template (for
        re-use/durability reasons, this should be easy to map/change by
        reading existing customer templates in a customer workspace and
        selecting the one to use), populate fields as appropriate with
        ETL mapped data from ION

    b.  Quality process reaches a status or completion - can use Event
        Engine to trigger data output for update to MES

## Integration Details -- Things to Consider

-   Typical process flows -- triggers, scheduling/frequency

    -   For data flows from Arena to a target system where the target
        system should receive the latest released Item and BOM
        relationships, the trigger in Arena is a Change Order object
        entering a Completed status where revision changes are made
        effective. This ensures that only approved, released revision
        information is sent downstream or used for planning purposes.

    -   Customers can have different categories of Change Orders to
        manage various types of changes. The Event Engine trigger
        definition allows us to specify which categories of Change
        Orders should result in data being provided in the queue for an
        integration. For example, many customers have a category of
        Change Order to manage documentation changes that would be
        excluded from updates to a downstream ERP/MES system.

    -   Triggers for Change Orders are defined to identify which
        Lifecycle Status the Change Orders are moving from and to.
        Typically, Change Orders for this type of integration will be
        moving from "open" Lifecycle Statuses to "completed" Lifecycle
        Statuses. Customers define these statuses and you'll create the
        trigger to match customer configuration.

    -   Triggers for Item state changes / edits to Item data can be
        defined in ...

-   Minimum required data

    -   Item

    -   Parent -- Child BOM

-   Custom fields

    -   Arena has custom fields that may be made visible and used by
        each customer. Custom fields exist for all major objects in the
        system (Worlds), including Item, Change, Supplier, and Supplier
        Item as well as for BOM parent-child relationships.

-   AML/AVL

-   Setting defaults in ERP

    -   Many ERP systems require default values set across Item and BOM
        objects, including data not created or present in Arena.
        Integrations should ensure that all ERP all ERP required default
        values are set for such fields, either by using logic with the
        ERP based on other attributes, such as type of Item, available
        in Arena, or through applying ERP defaults. Care should be taken
        to ensure that defaults update correctly if a customer makes a
        change in the ERP. If programmatic updates cannot be supported,
        documentation should state that any changes to such ERP defaults
        need to also be made either in Arena and/or the integration
        product.

    -   Transforms of data need to be supported as values may not be
        identical between disparate systems such as Arena and an ERP
        system.

    -   Examples UOM where Arena values may include EACH but ERP
        matching value is EA

    -   Reference designators -- in Arena, reference designators \...

    -   Transforms should be handled in a mapping layer, separate from
        any code, to allow customers to modify as needed. Transforms may
        need to change periodically for a specific customer and should
        not require services or new code to complete. Ideally, these
        should be easy for a customer admin to view and modify in future
        if necessary. A UI mapping is preferable to file mapping.
        Transforms should not be hard coded and require new versions of
        product integrations or compiling.

    -   No material change should be made to a field value by default.
        All data transforms should be handled in a mapping layer.

        -   Example BOM quantity field values -- do not assume whole
            numbers and do not round by default. Provide a transform
            option customers can define to meet specific business need
            and target system configuration.

-   BOM parent-child record filtering

    -   There are 2 fields that relate to BOM assemblies.

        -   isAssembly = true states that this item has children in the
            BOM table

        -   inAssembly = true states that this item has parents in its
            Where Used and it could be the lowest level in the BOM or
            somewhere in the middle of the BOM that has its own children
            under it.

    -   There could be changes for items that aren't in a BOM, and it
        may still need to be processed in the integration depending on
        your use case.

-   Unique records

-   Error handling

-   Bi-directional process flows

-   First-time sync processes

    -   

-   Frequency of integration activity

    -   There may be more than one event that occurs between the
        executions of the API. Therefore, sort based on the create
        date/time of the event must be performed to process all data
        changes in the correct order.

## Getting Started -- The Basic Steps to Creating an Integration in Arena

-   Arena workspace provisioned and setup

-   Arena users, access, and licensing configured

    -   API machine user license - The API machine user license is
        issued per customer organization account and tied to the
        implementation of an integration for a customer.

-   Arena Outbound Event setup in the UI

    -   Workspace setting for Outbound Event must be done by Arena
        Support or Customer Services team (see in FAQ below)

    -   Create a trigger

    -   Create an integration

-   Import sample data to Arena

-   Complete ERP Extraction Process -- this is done using API endpoint
    calls to monitor the queue and then stream the data in the queue

    -   Monitor Event Queue

    -   Export data

    -   Process resulting data

    -   Handle errors

## Common Questions

1.  What is the rate limit for APIs, it is 10,000 but it doesn\'t say
    per what duration - day/week/month?

    a.  Using the Event Engine is not only faster but will also make
        sure API calls are thrifty. API limits are by customer and a
        minimum volume is included in every customer's workspace based
        on package they purchase. API call volume is maximum per 24 hour
        period (read
        [here](https://app.bom.com/static/webhelp/full/en_US/ApplicationHelp.htm#html/api_licensing.html?Highlight=API%20calls%2010000)
        for more info -- need to be logged into workspace). The minimum
        default included in all packages is 10000 per 24-hour period.

2.  How can I re-process data for a trigger while testing integration
    development?

3.  How best to use the RECONCILE with the Event Engine command? Do we
    reconcile at item level or event level? Can certain items remain
    unreconciled, but event is reconciled?

    a.  You can reconcile by either the entire event or individual items
        in the event, depending on how you want to handle the execution
        of the integration. If you do the latter (item-by-item, then the
        entire event will be reconciled once the last item is
        reconciled.

4.  What Arena Access Policy permissions should an Arena Machine User
    have for integration?

    a.  Recommend granting ***just enough*** permissions to operate.
        Determine what the Machine User needs to do -- read, create,
        edit, delete and what objects?

    b.  In Arena, we manage policies by attaching them to user groups
        rather than directly to users. So, typical user profiles are
        created as user groups. Users can then be associated to a number
        of users groups to build up their specific access profile.

    c.  Options --

        i.  use one of the existing \"Level 1 - create/edit\" groups or

        ii. create a new user group to manage a policy that is
            restricted to create/edit mechanical items only (by
            structural category).

5.  If we are using the Event Engine and triggers, what setup is needed
    that is done by Arena?

    a.  The Outbound Integration setting must be created by an SA or
        Arena support in the Admin tool. Please contact
        <arena-support@ptc.com> to request this to be done for the
        workspace.
