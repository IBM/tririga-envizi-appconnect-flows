For this solution, we will need to add four fields, which will hold the
group name values and path, and make sure that the payload is sent when
there is an update

Import OM Package

Here are the manual changes that needs to be done:

**Data Modeler**

On data modeler, open BO Location-\>triBuilding. Revise the BO and add
four fields: cstEnviziParentOneTX, cstEnviziParentTwoTX,
cstEnviziParentThreeTX and cstEnviziGroupNamePathTX

![Table Description automatically
generated](./media/image1.png){width="6.5in" height="4.075in"}

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

![Table Description automatically
generated](./media/image2.png){width="6.5in"
height="0.8104166666666667in"}

Click on triBuilding on the left panel and then click on Sort Tab. Move
this new tab to the second position and click Apply

![Table Description automatically
generated](./media/image3.png){width="3.4444444444444446in"
height="4.534288057742782in"}

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

![Table Description automatically
generated](./media/image4.png){width="5.194444444444445in"
height="2.726162510936133in"}

**Workflow Builder**

Go to Workflow Builder. Select Location -\> Building.

Create a new Workflow name cst - triBuilding - Update Envizi path 

This Worklow will look like that:

![Diagram Description automatically
generated](./media/image5.png){width="6.5in"
height="6.9840277777777775in"}

The first switch should have the following condition:

![](./media/image6.png){width="6.5in" height="0.725in"}

Modify Parent One Modify task should have this mapping:

![](./media/image7.png){width="6.5in" height="0.5138888888888888in"}

Set Blank Modify Task should have this mapping:

![](./media/image8.png){width="6.5in" height="0.3347222222222222in"}

The second switch should have the following condition:

![](./media/image9.png){width="6.5in" height="0.4673611111111111in"}

Modify Parent Two Modify Task should have this mapping:

![](./media/image10.png){width="6.5in" height="0.4236111111111111in"}

![Graphical user interface Description automatically
generated](./media/image11.png){width="6.5in"
height="1.8381944444444445in"}

The third switch should have the following condition:

![](./media/image12.png){width="6.5in" height="0.4798611111111111in"}

Modify Parent Three Modify Task should have this mapping:

![](./media/image13.png){width="6.5in" height="0.48055555555555557in"}

![Graphical user interface Description automatically
generated](./media/image14.png){width="6.5in"
height="2.2180555555555554in"}

Publish cst - triBuilding - Update Envizi path workflow

Search for existing WF triBuilding - Synchronous - Permanent Save
Validation

Revise this workflow and after Call Module Level Validation add a new WF
call to

"cst - triBuilding - Update Envizi path" like displayed below:

![Diagram Description automatically
generated](./media/image15.png){width="6.5in"
height="6.146527777777778in"}

Publish the workflow

Expand Data Utilities -\> cstEnviziUtility on the left panel and revise
Workflow cst - cstEnviziUtility - Synchronous - Update Buildings with
Group Names 

This workflow has some mapping missing. You need to update three Modify
Task

Open Modify Group Name1 modify task and enter the following

![Graphical user interface, text, application Description automatically
generated](./media/image16.png){width="6.5in"
height="1.0451388888888888in"}

Open Modify Group Name2 modify task and enter the following

![Graphical user interface, application Description automatically
generated](./media/image17.png){width="6.5in"
height="1.1090277777777777in"}

![Graphical user interface Description automatically
generated](./media/image18.png){width="6.5in"
height="1.8069444444444445in"}

Open Modify Group Name3 modify task and enter the following

![A picture containing table Description automatically
generated](./media/image19.png){width="6.5in" height="1.01875in"}

![Graphical user interface Description automatically
generated](./media/image20.png){width="6.5in"
height="2.1777777777777776in"}

Publish this workflow

OPTIONAL STEP: Just add if you want to run the patch helper to update
old records

If that's the case you need to modify workflow cst - triPatchHelper -
Update All Envizi location fields, located on triHelper -\>
triPatchHelper

Revise the Workflow and modify the following modifyTask:

Modify Parent One task(if you want to use a custom value, set it on
those mapping):

![](./media/image21.png){width="6.5in" height="0.7986111111111112in"}

Modify Parent Two task(if you want to use a custom value, set it on
those mapping)::

![](./media/image22.png){width="6.5in" height="0.4201388888888889in"}

Modify Parent TwoPath task:

![](./media/image10.png){width="6.5in" height="0.4236111111111111in"}

![Graphical user interface Description automatically
generated](./media/image11.png){width="6.5in"
height="1.8381944444444445in"}

Modify Parent Three task(if you want to use a custom value, set it on
those mapping)::

![](./media/image23.png){width="6.5in" height="0.4722222222222222in"}

Modify Parent ThreePath task:

![](./media/image13.png){width="6.5in" height="0.48055555555555557in"}

![Graphical user interface Description automatically
generated](./media/image14.png){width="6.5in"
height="2.2180555555555554in"}

SetBlank:

![Graphical user interface, application Description automatically
generated](./media/image24.png){width="6.5in"
height="1.2277777777777779in"}

Set Parent Two and Three Blank:

![](./media/image25.png){width="6.5in" height="0.6604166666666667in"}

Set Parent Three Blank:

![](./media/image26.png){width="6.5in" height="0.35694444444444445in"}

**My Reports/OSLC**

Go to My Reports, and add those four new fields to existing query
triAPICBuilding - OSLC -- Outbound and save the report

Open Tools -\> System Setup -\> Integration -\> OSLC Resource manager
and search for triAPICOutboundBuildingRS. On this resource, add the four
new fields(you can use Import all Fields)

![Graphical user interface, application Description automatically
generated](./media/image27.png){width="6.5in"
height="1.8576388888888888in"}

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

![Graphical user interface, text, application Description automatically
generated](./media/image28.png){width="6.5in"
height="3.5743055555555556in"}

TXT file should have this content

![Text Description automatically generated with medium
confidence](./media/image29.png){width="1.1388888888888888in"
height="0.5416666666666666in"}

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
