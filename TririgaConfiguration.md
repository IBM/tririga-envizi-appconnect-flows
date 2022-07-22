For this solution, we will need to add four fields, which will hold the
group name values and path, and make sure that the payload is sent when
there is an update

Import OM Package

Here are the manual changes that needs to be done:

**Data Modeler**

On data modeler, open BO Location-\>triBuilding. Revise the BO and add
four fields: cstEnviziParentOneTX, cstEnviziParentTwoTX,
cstEnviziParentThreeTX and cstEnviziGroupNamePathTX

<img width="512" alt="Data Modeler Fields" src="https://media.github.ibm.com/user/348712/files/3ddd5780-09b5-11ed-9014-fbd85b21e6eb">

Name and Label should be the following:

Name: cstEnviziParentOneTX

Label: Envizi Group 1

Name: cstEnviziParentTwoTX

Label: Envizi Group 2

Name: cstEnviziParentThreeTX

Label: Envizi Group 3

Name: cstEnviziGroupNamePathTX

Label: Envizi Path

After that, Publish the BO

**Form Builder**

Go to Form Builder and open Form location -\> triBuilding.

Revise the triBuilding form

Click on Add Tab and insert name "cstEnvizi". Click Apply

Select the new tab and click on Add Section

Insert the name cstEnviziDetails and Label Envizi Details

Click on Add Field and select fieldname "cstEnviziParentOneTX". Click
Apply

Select the section, Click on Add Field and select fieldname
"cstEnviziParentTwoTX". Click Apply

Select the section, Click on Add Field and select fieldname
"cstEnviziParentThreeTX". Click Apply

Select the section again, Click on Add Field and select fieldname
"cstEnviziGroupNamePathTX". Click Apply

Select EnviziPathTX and modify Start Row to 3 and Col Span to 2 on the
properties window. Also mark this field as ReadOnly. Click Apply

<img width="256" alt="Form Builder" src="https://media.github.ibm.com/user/348712/files/4f266400-09b5-11ed-9ca4-4253b2fa9baa">

Click on triBuilding on the left panel and then click on Sort Tab. Move
this new tab to the second position and click Apply

<img width="256" alt="Form Builder Properties" src="https://media.github.ibm.com/user/348712/files/4f266400-09b5-11ed-8a73-f526a24a86db">

Publish the form

**Security Manager**

GO To Administration -\> Security Manager

On this page you will limit the users that can access this new created
tab. So users without access to the tab will not be able to change the
group names

Click on the desired group, and select Access tab

On this tab select Location -\> triBuilding -\> cstEnvizi

Select No access for groups that should not have access to this tab and
Save

<img width="423" alt="Security Manager Access" src="https://media.github.ibm.com/user/348712/files/4fbefa80-09b5-11ed-99e5-b92fca943ddf">

**Workflow Builder**

Go to Workflow Builder. Select Location -\> Building.

Create a new Workflow name cst - triBuilding - Update Envizi path 

This Worklow will look like that:

<img width="564" alt="Workflow" src="https://media.github.ibm.com/user/348712/files/4f266400-09b5-11ed-9919-1f2fb10fd860">

The first switch should have the following condition:

<img width="484" alt="1st Switch Expression" src="https://media.github.ibm.com/user/348712/files/4fbefa80-09b5-11ed-8738-62e0ccb761f9">

Modify Parent One Modify task should have this mapping:

<img width="569" alt="Modify Parent One Mapping" src="https://media.github.ibm.com/user/348712/files/4fbefa80-09b5-11ed-9306-f120dffa38e8">

Set Blank Modify Task should have this mapping:

<img width="680" alt="Set Blank Task" src="https://media.github.ibm.com/user/348712/files/50579100-09b5-11ed-80b3-0bc45efda653">

The second switch should have the following condition:

<img width="473" alt="2nd Switch Expression" src="https://media.github.ibm.com/user/348712/files/4fbefa80-09b5-11ed-8db9-31cb358f4b60">

Modify Parent Two Modify Task should have this mapping:

<img width="629" alt="Modify Parent Two Mapping" src="https://media.github.ibm.com/user/348712/files/361eb280-09b7-11ed-9e3f-00b94068ebd6">

<img width="587" alt="Parent Two Inputs and Formula" src="https://media.github.ibm.com/user/348712/files/36b74900-09b7-11ed-875c-e60f87afefcf">

The third switch should have the following condition:

<img width="474" alt="3rd Switch Expression" src="https://media.github.ibm.com/user/348712/files/36b74900-09b7-11ed-89c5-71da43fe6e50">

Modify Parent Three Modify Task should have this mapping:

<img width="487" alt="Modify Parent Three Mapping" src="https://media.github.ibm.com/user/348712/files/36b74900-09b7-11ed-8058-03b7ed9d8bc6">

<img width="589" alt="Modify Parent Three Mapping-2" src="https://media.github.ibm.com/user/348712/files/374fdf80-09b7-11ed-80d9-3911aa5f031e">

Publish cst - triBuilding - Update Envizi path workflow

Search for existing WF triBuilding - Synchronous - Permanent Save
Validation

Revise this workflow and after Call Module Level Validation add a new WF
call to

"cst - triBuilding - Update Envizi path" like displayed below:

<img width="736" alt="WF call in Update Envizi Path" src="https://media.github.ibm.com/user/348712/files/374fdf80-09b7-11ed-8f47-3c588b667a58">

Publish the workflow

Expand Data Utilities -\> cstEnviziUtility on the left panel and revise
Workflow cst - cstEnviziUtility - Synchronous - Update Buildings with
Group Names 

This workflow has some mapping missing. You need to update three Modify
Task

Open Modify Group Name1 modify task and enter the following

<img width="759" alt="Group Name1 Modify Task" src="https://media.github.ibm.com/user/348712/files/36b74900-09b7-11ed-8722-b1dbeba27e23">

Open Modify Group Name2 modify task and enter the following

<img width="768" alt="Group Name2 Modify Task" src="https://media.github.ibm.com/user/348712/files/374fdf80-09b7-11ed-96b8-9634174db7a1">

<img width="608" alt="Group Name2 Modify Task-2" src="https://media.github.ibm.com/user/348712/files/361eb280-09b7-11ed-8044-c0861e85aa4d">

Open Modify Group Name3 modify task and enter the following

<img width="638" alt="Group Name3 Modify Task" src="https://media.github.ibm.com/user/348712/files/35861c00-09b7-11ed-8a4b-8f4ed004675e">

<img width="600" alt="Group Name3 Modify Task-2" src="https://media.github.ibm.com/user/348712/files/361eb280-09b7-11ed-84be-438ae45f0d7f">

Publish this workflow

OPTIONAL STEP: Just add if you want to run the patch helper to update
old records

If that's the case you need to modify workflow cst - triPatchHelper -
Update All Envizi location fields, located on triHelper -\>
triPatchHelper

Revise the Workflow and modify the following modifyTask:

Modify Parent One task(if you want to use a custom value, set it on
those mapping):

<img width="529" alt="Patch Helper Parent One" src="https://media.github.ibm.com/user/348712/files/73d10a80-09ba-11ed-958f-6c3df5f47ad3">

Modify Parent Two task(if you want to use a custom value, set it on
those mapping)::

<img width="619" alt="Patch Helper Parent Two" src="https://media.github.ibm.com/user/348712/files/73d10a80-09ba-11ed-9bca-cdf074e7cf37">

Modify Parent TwoPath task:

<img width="629" alt="Modify Parent Two Mapping" src="https://media.github.ibm.com/user/348712/files/361eb280-09b7-11ed-9e3f-00b94068ebd6">

<img width="587" alt="Parent Two Inputs and Formula" src="https://media.github.ibm.com/user/348712/files/36b74900-09b7-11ed-875c-e60f87afefcf">

Modify Parent Three task(if you want to use a custom value, set it on
those mapping)::

<img width="592" alt="Patch Helper Parent Three" src="https://media.github.ibm.com/user/348712/files/73387400-09ba-11ed-8a8a-9f412982a9ce">

Modify Parent ThreePath task:

<img width="487" alt="Modify Parent Three Mapping" src="https://media.github.ibm.com/user/348712/files/36b74900-09b7-11ed-8058-03b7ed9d8bc6">

<img width="589" alt="Modify Parent Three Mapping-2" src="https://media.github.ibm.com/user/348712/files/374fdf80-09b7-11ed-80d9-3911aa5f031e">

SetBlank:

<img width="683" alt="Patch Helper Set Blank 1" src="https://media.github.ibm.com/user/348712/files/73387400-09ba-11ed-81fa-ab59763a2de0">

Set Parent Two and Three Blank:

<img width="679" alt="Patch Helper Set Blank 2" src="https://media.github.ibm.com/user/348712/files/729fdd80-09ba-11ed-95df-381836697eed">

Set Parent Three Blank:

<img width="674" alt="Patch Helper Set Blank 3" src="https://media.github.ibm.com/user/348712/files/73387400-09ba-11ed-9aea-16908b7c9861">

**My Reports/OSLC**

Go to My Reports, and add those four new fields to existing query
triAPICBuilding - OSLC -- Outbound and save the report

Open Tools -\> System Setup -\> Integration -\> OSLC Resource manager
and search for triAPICOutboundBuildingRS. On this resource, add the four
new fields(you can use Import all Fields)

<img width="683" alt="OSLC Manager" src="https://media.github.ibm.com/user/348712/files/73387400-09ba-11ed-912f-569fce1dc420">

**How to use**

This tool will allow user to make changes on this new Envizi group name
fields. But we must consider the existing records too. If you want those
records to be populated, there is a patch helper workflow that can
handle that.

This workflow by default adds the groupnames with the value from
Hierarchy Path. So for example, if the HierarchyPath is
Location/NorthAmerica/USA/Texas/Dallas, then the group names will be:

Envizi Group 1: Dallas

Envizi Group 2: Texas

Envizi Group 3: USA

Envizi Path: USA/Texas/Dallas

You can customize this values by changing workflow "cst -
triPatchHelper - Update All Envizi location fields" like described
before

Also, you can filter to change only the desired records by changing
query "cst - triBuilding - Query - Get All Buildings for envizi". The
list of buildings displayed on this query will be the list of buildings
that the patch helper will modify

If you want to run this patch helper, go To Administration -\> Data
Integration to run patch helper. Upload a file using triNameTx = Envizi

<img width="682" alt="Data Integrator" src="https://media.github.ibm.com/user/348712/files/7469a100-09ba-11ed-9111-c30331bb797b">

TXT file should have this content

<img width="82" alt="TXT file" src="https://media.github.ibm.com/user/348712/files/73d10a80-09ba-11ed-97e9-d1655dfec1f8">

After that the records will be populated with the initial values
defined. Please note that Patch helpers can be executed only once. So,
if you want to run this patch helper one more time, you need to remove
Envizi from the list that can be found using the query "

To use the solution, you have two possible scenarios:

1.  Modify only one record

    a.  Open the building you want to change and click on tab Envizi.
        Modify the values and save the record

2.  Modify multiple records

    a.  To do that, go to My Reports, and run report cst -
        cstEnviziUtility - QUERY - Update Envizi GroupNames

    b.  Click on Add

    c.  Define the values for each groupName and filter the buildings
        you want to apply that on the Buildings section

    d.  Select the buildings and click on Create.

    e.  After that, click on Mass Update
