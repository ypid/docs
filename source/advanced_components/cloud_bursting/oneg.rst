.. _oneg:

================================================================================
OpenNebula hybrid Driver
================================================================================

Considerations & Limitations
================================================================================
..
    Incude here a description of the limitaions compared to the local OpenNebula cloud this may include but not limited to:
    - Operations on VMs
    - Defintion of VMs based on remote templates, what can be added to the template anything, nothing?
    - Context
    - Monitoring states differences

..
   Side notes:
     - How do you define the local templates? Do we need to know before hand the template id?

Prerequisites
================================================================================

..
  Move to this section all the requirements needed (e.g. remote account, templates etc)
  Include how to test, ex. use onetemplate list with -endpoint -user and -password

OpenNebula Configuration
================================================================================

..
  Include in this section the local OpenNebula configuration
  Add CLI instrunctios, show a new host (onhost create and onehost show)



OpenNebula to OpenNebula  Specific Template Attributes
================================================================================

..
  Describe here how to add the template for OpenNebula 2 OpenNebula and supported attributes
    Add subsections to specific parameters if needed






Configure the remote OpenNebula
--------------------------------------------------------------------------------

Ask the remote OpenNebula administrator to create a new user for you. This user should have access to the VM Templates that you are going to be exposed to the local OpenNebula cloud.

Configure the local Host
--------------------------------------------------------------------------------

First create a new Host with `im` and `vm` drivers set to `opennebula`.

Add a new attributes within local host template:

.. code::

    ONE_USER=<remote_username>
    ONE_PASSWORD=<remote_password>
    ONE_ENDPOINT=<remote_endpoint>
    ONE_CAPACITY=[
        CPU=0,
        MEMORY=0
    ]
..
   Add a table describing all the parameters
   Do we need to modify oned.conf?

Capacity is taken from the user and group quotas of the remote OpenNebula user. Alternatively, you can set a hard limit here

Create a local hybrid VM Template
--------------------------------------------------------------------------------

Your hybrid VM Template must contain this section. Set TEMPLATE_ID to the target VM Template ID in the **remote OpenNebula**.

.. code::

    PUBLIC_CLOUD=[
    TEMPLATE_ID=<remote_template_id>,
    TYPE="opennebula" ]


If this Template does not define a local disk and must be deployed only in the remote OpenNebula instance, add this requirement:

.. code::

    SCHED_REQUIREMENTS = "PUBLIC_CLOUD = YES"

To match the reported allocated Host resources with the actual usage in the remote OpenNebula, set the same CPU and MEMORY as the remote Template.
