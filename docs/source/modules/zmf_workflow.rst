
:github_url: https://github.com/IBM/ibm_zosmf/tree/master/plugins/modules/zmf_workflow.py

.. _zmf_workflow_module:


zmf_workflow -- Operate z/OS workflows
======================================


.. contents::
   :local:
   :depth: 1


Synopsis
--------
- Operate z/OS workflows through the use of z/OSMF workflow REST services.
- This module supports to compare, start, delete, and check the status of a workflow.





Parameters
----------


 

zmf_credential
  Authentication credentials, returned by module :ref:`zmf_authenticate <zmf_authenticate_module>`, for successful authentication with the z/OSMF server.


  If *zmf_credential* is supplied, *zmf_host*, *zmf_port*, *zmf_user*, *zmf_password*, *zmf_crt* and *zmf_key* are ignored.


  | **required**: False
  | **type**: dict


 

  ltpa_token_2
    The value of the Lightweight Third Party Access (LTPA) token, which supports strong encryption.


    If *jwt_token* is not supplied, *ltpa_token_2* is required.


    | **required**: False
    | **type**: str


 

  jwt_token
    The value of the JSON web token, which supports strong encryption.


    If *ltpa_token_2* is not supplied, *jwt_token* is required.


    | **required**: False
    | **type**: str


 

  zmf_host
    Hostname of the z/OSMF server.

    | **required**: True
    | **type**: str


 

  zmf_port
    Port number of the z/OSMF server.

    | **required**: False
    | **type**: int



 

zmf_host
  Hostname of the z/OSMF server.

  If *zmf_credential* is supplied, *zmf_host* is ignored.

  If *zmf_credential* is not supplied, *zmf_host* is required.

  | **required**: False
  | **type**: str


 

zmf_port
  Port number of the z/OSMF server.

  If *zmf_credential* is supplied, *zmf_port* is ignored.

  | **required**: False
  | **type**: int


 

zmf_user
  User name to be used for authenticating with z/OSMF server.

  If *zmf_credential* is supplied, *zmf_user* is ignored.

  If *zmf_credential* is not supplied, *zmf_user* is required when *zmf_crt* and *zmf_key* are not supplied.


  If *zmf_credential* is not supplied and *zmf_crt* and *zmf_key* are supplied, *zmf_user* and *zmf_password* are ignored.


  | **required**: False
  | **type**: str


 

zmf_password
  Password to be used for authenticating with z/OSMF server.

  If *zmf_credential* is supplied, *zmf_password* is ignored.

  If *zmf_credential* is not supplied, *zmf_password* is required when *zmf_crt* and *zmf_key* are not supplied.


  If *zmf_credential* is not supplied and *zmf_crt* and *zmf_key* are supplied, *zmf_user* and *zmf_password* are ignored.


  | **required**: False
  | **type**: str


 

zmf_crt
  Location of the PEM-formatted certificate chain file to be used for HTTPS client authentication.


  If *zmf_credential* is supplied, *zmf_crt* is ignored.


  If *zmf_credential* is not supplied, *zmf_crt* is required when *zmf_user* and *zmf_password* are not supplied.


  | **required**: False
  | **type**: str


 

zmf_key
  Location of the PEM-formatted file with your private key to be used for HTTPS client authentication.


  If *zmf_credential* is supplied, *zmf_key* is ignored.

  If *zmf_credential* is not supplied, *zmf_key* is required when *zmf_user* and *zmf_password* are not supplied.


  | **required**: False
  | **type**: str


 

state
  The desired final state for the specified workflow.

  If *state=existed*, checks whether a workflow instance exists or not.
    - If only *workflow_name* is specified, the module looks for a workflow instance with same name.
    - If *workflow_file*, *workflow_vars*, *workflow_vars_file* are also specified,
      the module not only looks for workflow instance with same name,
      but also validates if content of workflow definition and variables are consistent.


  If *state=started*, starts the workflow instance.
    - If *workflow_key* is specified, finds the workflow instance and starts it.
    - If *workflow_key* is not specified, checks if workflow exists by *workflow_name*,
        - If exists, starts the workflow instance.
        - If not exist, creates a new workflow instance and starts it.


  If *state=deleted*, delete a workflow instance if it exists.

  If *state=check*, check the status of a workflow.
    -
      If the status of the workflow is 'automation-in-progress', return message\:
      Workflow instance with key:{} is still in progress. Current step is {}.Percent complete is xx%.

    -
      If the status of the workflow is 'complete', return message:
      Workflow instance with key:{} is is completed.

    -
      If the status of the workflow is not 'automation-in-progress' or 'complete', return message\:

        - Workflow instance with key:{} is not completed\: No step is started.
        -
          Workflow instance with key:{} is not completed\: In step {}\:
          You can manually complete this step in z/OSMF Workflows task,
          and start this workflow instance again with next step name: {}
          specified in argument: workflow_step_name.
        - Workflow instance with key:{} is not completed\:
          In step {}\: While one or more steps may be skipped.


  | **required**: True
  | **type**: str
  | **choices**: existed, started, deleted, check


 

workflow_name
  Descriptive name of the workflow.

  The workflow name is case insensitive, for example, ``MyWorkflow`` and ``MYWORKFLOW`` are the same workflow.


  It is recommended that you use the naming rule ``ansible_workflowName_{{ workflow_host }}`` when *state=started*.


  Required when *state=existed*.

  Either *workflow_name* or *workflow_key* is required when *state=started/deleted/check*.


  | **required**: False
  | **type**: str


 

workflow_file
  Location of the workflow definition file.

  | **required**: False
  | **type**: str


 

workflow_host
  Nickname of the target z/OS system on which the workflow is to be performed.


  This variable should be specified as ``{{ inventory_hostname }}``. Its value should be specified in the inventory file as a managed node.


  | **required**: False
  | **type**: str


 

workflow_owner
  User name of the workflow owner.

  If this value is omitted, *zmf_user* is used as workflow owner.

  | **required**: False
  | **type**: str


 

workflow_file_system
  Nickname of the system on which the specified workflow definition file and any related files reside.


  | **required**: False
  | **type**: str


 

workflow_vars_file
  Location of the optional properties file to be used to pre-specify the values of one or more variables that are defined in workflow definition file.


  | **required**: False
  | **type**: str


 

workflow_vars
  Values of one or more workflow variables in JSON format.

  For example, ``{"user_to_list": "DEBUG1", "tsocmd_to_issue": "TIME"}``


  | **required**: False
  | **type**: dict


 

workflow_resolve_global_conflict_by_using
  Version of the variable to be used if the supplied workflow variable conflicts with an existing global variable in z/OSMF Workflows task.


  | **required**: False
  | **type**: str
  | **default**: global
  | **choices**: global, input


 

workflow_comments
  User-specified information to be associated with the workflow at creation time.


  | **required**: False
  | **type**: str


 

workflow_assign_to_owner
  Specifies whether the workflow steps are assigned to the workflow owner when the workflow is created.


  | **required**: False
  | **type**: bool
  | **default**: True


 

workflow_access_type
  Access type for the workflow when the workflow is created.

  The access type determines which users can view the workflow steps and edit the step notes.


  | **required**: False
  | **type**: str
  | **default**: Public
  | **choices**: Public, Restricted, Private


 

workflow_account_info
  For a workflow step that submits a batch job, this variable specifies the account information for the JCL JOB statement.


  | **required**: False
  | **type**: str


 

workflow_job_statement
  For a workflow that submits a batch job, this variable specifies the JOB statement JCL for the job.


  | **required**: False
  | **type**: str


 

workflow_delete_completed_jobs
  For a workflow that submits a batch job, this variable specifies whether the job is deleted from the JES spool after it completes.


  | **required**: False
  | **type**: bool
  | **default**: False


 

workflow_resolve_conflict_by_using
  Specifies how to handle variable conflicts if any are detected at workflow creation time.


  Such conflicts can be found when z/OSMF Workflows task reads the output file from a step that runs a REXX exec or UNIX shell script.


  | **required**: False
  | **type**: str
  | **default**: outputFileValue
  | **choices**: outputFileValue, existingValue, leaveConflict


 

workflow_step_name
  Name of the workflow step at which automation processing is to begin when the workflow is started.


  | **required**: False
  | **type**: str


 

workflow_perform_subsequent
  Specifies whether the subsequent automated steps are performed when the workflow is started.


  | **required**: False
  | **type**: bool
  | **default**: True


 

workflow_notification_url
  URL to be used for receiving notifications when the workflow is started.


  | **required**: False
  | **type**: str


 

workflow_category
  Category of the workflow, which is general or configuration.

  | **required**: False
  | **type**: str
  | **choices**: general, configuration


 

workflow_vendor
  Name of the vendor that provided the workflow definition file.

  | **required**: False
  | **type**: str


 

workflow_key
  A string value, generated by z/OSMF to uniquely identify the workflow instance.


  Either *workflow_name* or *workflow_key* is required when *state=started/deleted/check*.


  | **required**: False
  | **type**: str




Examples
--------

.. code-block:: yaml+jinja

   
   - name: Authenticate with z/OSMF server by username/password, and register the result for later use.
     zmf_authenticate:
       zmf_host: "{{ zmf_host }}"
       zmf_port: "{{ zmf_port }}"
       zmf_user: "{{ zmf_user }}"
       zmf_password: "{{ zmf_password }}"
     register: result_auth

   - name: Compare whether a workflow with the given name already exists
     ibm.ibm_zosmf.zmf_workflow:
       state: "existed"
       zmf_credential: "{{ result_auth }}"
       workflow_name: "ansible_sample_workflow_SY1"
       workflow_file: "/zosmf/workflow_def/workflow_sample_automation_steps.xml"
       workflow_host: "SY1"

   - name: Create a workflow if it does not exist, and start it
     ibm.ibm_zosmf.zmf_workflow:
       state: "started"
       zmf_credential: "{{ result_auth }}"
       workflow_name: "ansible_sample_workflow_{{ inventory_hostname }}"
       workflow_file: "/zosmf/workflow_def/workflow_sample_automation_steps.xml"
       workflow_host: "{{ inventory_hostname }}"

   - name: Start the existing workflow from the specified step `workflow_step_name`
     ibm.ibm_zosmf.zmf_workflow:
       state: "started"
       zmf_credential: "{{ result_auth }}"
       workflow_name: "ansible_sample_workflow_{{ inventory_hostname }}"
       workflow_step_name: "StepName"

   - name: Delete a workflow if it exists
     ibm.ibm_zosmf.zmf_workflow:
       state: "deleted"
       zmf_credential: "{{ result_auth }}"
       workflow_name: "ansible_sample_workflow_SY1"

   - name: Check the status of a workflow
     ibm.ibm_zosmf.zmf_workflow:
       state: "check"
       zmf_credential: "{{ result_auth }}"
       workflow_name: "ansible_sample_workflow_SY1"



Notes
-----

.. note::
   - Submitting a z/OSMF workflow found on Ansible control node is currently not supported.


   - Only automated steps are supported when starting a z/OSMF workflow.

   - This module is considered to be "weakly" idempotent. That is, this module achieves an idempotent result for the final state of the workflow instance, rather than for the target z/OS systems. A strong idempotent result for the final state of the target z/OS systems depends on the idempotency of the workflow instance steps.


   - This module does not support check mode.








Return Values
-------------


      changed
        Indicates if any change is made during the module operation.

        If `state=existed/check`, always return false.

        If `state=started` and the workflow is started, return true.

        If `state=deleted` and the workflow is deleted, return true.

        | **returned**: always
        | **type**: bool

      message
        The output message generated by the module.

        If `state=existed`, indicate whether a workflow with the given name does not exist, or exists with same or different definition file, variables and properties.


        If `state=started`, indicate whether the workflow is started.

        If `state=deleted`, indicate whether the workflow to be deleted does not exist or is deleted.


        If `state=check`, indicate whether the workflow is completed, is not completed, or is still in progress.


        | **returned**: on success
        | **type**: str
        | **sample**:

          Workflow instance named: ansible_sample_workflow_SY1 with same definition file, variables and properties is found.

          Workflow instance named: ansible_sample_workflow_SY1 with different definition file is found.

          Workflow instance named: ansible_sample_workflow_SY1 is found. While it could not be compared since the argument: workflow_file is required, and please supply variables by the argument: workflow_vars rather than the argument:  workflow_vars_file."

          Workflow instance named: ansible_sample_workflow_SY1 is started, you can use state=check to check its final status.

          Workflow instance named: ansible_sample_workflow_SY1 is still in progress. Current step is 1.2 Step title. Percent complete is 28%.

          Workflow instance named: ansible_sample_workflow_SY1 is completed.

          Workflow instance named: ansible_sample_workflow_SY1 is not completed: No step is started.

          Workflow instance named: ansible_sample_workflow_SY1 is not completed: In step 1.2 Step title: IZUWF0145E: Automation processing for the workflow `ansible_sample_workflow_SY1` stopped at step `Step title`. This step cannot be performed automatically. You can manually complete this step in z/OSMF Workflows task, and start this workflow instance again with next step name: subStep3 specified in argument: workflow_step_name.

          Workflow instance named: ansible_sample_workflow_SY1 is not completed: In step 1.2 Step title: IZUWF0162I: Automation processing for workflow `ansible_sample_workflow_SY1` is complete. While one or more steps may be skipped.

          Workflow instance named: ansible_sample_workflow_SY1 is deleted.

          Workflow instance named: ansible_sample_workflow_SY1 does not exist.


      workflow_key
        Generated key to uniquely identify the existing or started workflow.

        | **returned**: on success when `state=existed/started/check/deleted`
        | **type**: str
        | **sample**: 2535b19e-a8c3-4a52-9d77-e30bb920f912


      workflow_name
        Descriptive name of the workflow.

        | **returned**: on success when `state=existed/started/check/deleted`
        | **type**: str
        | **sample**: ansible_sample_workflow_SY1


      same_workflow_instance
        Indicate whether the existing workflow has the same or different definition file, variables and properties.


        | **returned**: on success when `state=existed`
        | **type**: bool

      waiting
        Indicate whether it needs to wait and check again because the workflow is still in progress. Return True if the status of the workflow is 'automation-in-progress'. Otherwise (the workflow is either completed or paused/failed at some step), return False.


        | **returned**: on success when `state=check`
        | **type**: bool

      completed
        Indicate whether the workflow is completed. Return True if the status of the workflow is 'complete'. Otherwise, return False.


        | **returned**: on success when `state=existed/check`
        | **type**: bool

      deleted
        Indicate whether the workflow is deleted.

        | **returned**: on success when `state=deleted`
        | **type**: bool

