# IBM TAS Connector for Envizi

The **IBM TAS Connector for Envizi** is released under the name "MAS Connector for Envizi". This connector supports the following capabilities through an App Connect flow:

-Automatically sync space and occupancy data from TRIRIGA with Envizi to enable energy usage calculations across entire facility portfolio with advanced analytics by location, by SQF, and by occupant.

***

## Use Case Examples

Corporate sustainability managers can gather and maintain accurate sustainability data for their entire global real estate portfolio directly from TRIRIGA where those facilities are managed. Location data from TRIRIGA serves as a baseline for all other Sustainability reports in Envizi; space classification, floor space, and headcount data allows Sustainability managers to normalize data (by square meter, or by employee) to enable meaningful comparisons between buildings across the entire portfolio to identify opportunities to reduce environmental impact.

***

## Connector Architecture

**IBM App Connect** provides a flexible environment for integration solutions to transform, enrich, route, and process business messages and data. 

**App Connect Flows** enable specific integration use cases by connecting to predefined APIs to route and map data. Mapping has been pre-defined, but it can be customized.

**Native API framework** is used for Maximo and enabled thorugh provided packages that can be imported.

![TRIRIGA-Envizi Integration](https://media.github.ibm.com/user/348712/files/fdc31706-9350-4aee-a909-e10d2bb0f3c7)

*TRIRIGA to Envizi Integration Diagram*

***

## Data Mapping

The image below illustrates the type of data that is being sent by the API and App Connect Flows.

<img width="450" height="650" alt="TAS-Envizi Architecture" src="https://media.github.ibm.com/user/348712/files/b21644cd-0b16-450e-aff4-1720bc8cea43">

*TRIRIGA to Envizi Data Map*

***

## App Connect Flows

Included with this connector are two flows that export locations and accounts, along with all the required fields they contain. The table below shows the naming convention for these flows and the current integration use case.

File | Flow | Destination | Operation
-- | -- |--|--
TririgaBuildings_Always_On_v1_1_1.yaml | Space Data | TRI to Envizi | Changes Only
TririgaBuildings_On_Demand_v1_1_1.yaml | Space Data | TRI to Envizi | Bulk Initial Load

***

## Installation & Configuration Guide


>## Before you begin you will need:
>
>1. An instance of App Connect Enterprise or App Connect Pro with the Designer component.
>2. Admin access to TRIRIGA with user/pw for integration
>3. Envizi instance with a AWS S3 Bucket
>4. [Import AppConnect Cert to TRIRIGA](#tririga-certificates) to enable encrypted communication

***

## Installation Steps Overview
1. App Connect Configuration<br>
   a. Import Flows into App Connect<br>
   b. Configure Flows<br>
2. TRIRIGA <br>
   a. Security Role Configuration<br>
   b. Group Name Configuration<br>
3. Test <br>
   a. TRIRIGA outbound connectivity<br>

***

## Part 1: App Connect Configuration

*Note: IBM Cloud App Connect Professional or Enterprise is needed to run this flow.*

*Note: The names in the screenshots are generic, the elements in this integration will not have the same names during setup.*


### Adding Accounts

Before importing the flow to App Connect, add Accounts for **Amazon S3** and **HTTP** connectors. While adding the HTTP connector account, include credentials for the TRIRIGA user which can consume the OSLC API.

1. Navigate to Catalog section of the App Connect instance
<img width="960" alt="Create Account 1" src="https://media.github.ibm.com/user/375131/files/eb8b5a00-eb1d-11ec-8401-35b47d561ce4">
*Create Account 1*

2. In the **Search application** field, type name of the connector.

3. If the App Connect instance does not have an account for the connector, click on **Connect** to create a new account. Else, open the account selection drop down, and click on **Add a new account ...**
<img width="948" alt="Create Account 2b" src="https://media.github.ibm.com/user/348712/files/02ef8867-555d-4949-a87b-5ffc68d5dca2">
*Add a New Account*

4. Enter the necessary details for the connector<br>
    a. For Amazon S3, it will be the Secret Access Key and Access Key ID provided by Envizi.<br>
    b. For HTTP, it will be the Authentication Key or username and password needed for TRIRIGA.
<img width="960" alt="Create Account 3" src="https://media.github.ibm.com/user/348712/files/07bdaf7e-7538-46e8-aaff-27b6acfb8738">
*Connect to S3/HTTP*

5. Click on **Connect**
<img width="960" alt="Create Account 4" src="https://media.github.ibm.com/user/375131/files/ed551d80-eb1d-11ec-983c-c276d43e0654">
*Connect the Account*

6. From the account selection drop down, select the newly created account. e.g., The default name will be `Account 2` if `Account 1` is already present
7. Click on the kebab menu (three dots) and select **Rename Account**
<img width="960" alt="Create Account 5" src="https://media.github.ibm.com/user/348712/files/f0d73883-70c5-4892-a996-fae0cd33a00f">
*Rename the Account step 1*

8. Enter an account name and click on **Rename Account**. This name can now be used by the connector in the flow.
<img width="958" alt="Create Account 6" src="https://media.github.ibm.com/user/375131/files/ee864a80-eb1d-11ec-96d1-fdc42c679896">
*Rename the Account step 2*

### Importing the flow

1. Open the Dashboard of the App Connect instance.
2. Click on the **New** button and select **Import Flow**.
<img width="861" alt="Flow Import 0" src="https://media.github.ibm.com/user/375131/files/ea582e00-eb19-11ec-9912-0881d321f3c7">
*Import the flow*

3. Browse to the flow's YAML file and click on **Import**.
<img width="960" alt="Flow Import 1" src="https://media.github.ibm.com/user/375131/files/ea582e00-eb19-11ec-88a7-abebff4e0534">
*Select the desired flow*

4. The flow will now be imported and opened.
<img width="960" alt="Flow Import 2" src="https://media.github.ibm.com/user/375131/files/eaf0c480-eb19-11ec-885b-919da812499b">
*The main page for the flow*

### Configuring the flow to use the right accounts

When importing a flow, it is important to check if the flow is using the right accounts for the different connectors.

1. Click on **Edit Flow**

2. See if the connectors are using the right accounts.

3. To change the account for any connector, select the connector and click on the dropdown icon next to the Account's name
<img width="960" alt="Select Account 1" src="https://media.github.ibm.com/user/348712/files/e61b9ed4-61ab-4782-b0c4-a42bb432467b">
*Change associated Account*

4. Select the account name to use from the list
<img width="659" alt="Select Account 2" src="https://media.github.ibm.com/user/348712/files/52de25b1-d449-4aa8-a1d7-2b6af7e8b24e">
*Select desired Account*

### Configuring the Scheduler

1. Click on the **Scheduler** node
<img width="659" alt="Scheduler node" src="https://media.github.ibm.com/user/375131/files/ea694d80-01ed-11ed-8852-16b625339675">
*The Scheduler node*

2. Configure the schedule as needed

<img width="659" alt="Scheduler configuration 1" src="https://media.github.ibm.com/user/375131/files/d70ab200-01ee-11ed-9af7-4f66738aea7c">
*Options for configuration- Hourly*
<img width="659" alt="Scheduler configuration 2" src="https://media.github.ibm.com/user/375131/files/00c3d900-01ef-11ed-86aa-29008ff86450">
*Options for configuration- Daily*

## Part 2: TRIRIGA Configuration

### Step 1: Security Role Configuration
In order to properly configure TRIRIGA, a user needs to be configured with the proper security access for Object Migration and TRIRIGA APIs

#### Object Migration Access

Object Migration is a task managed by administrators. If a TRIRIGA User needs to have full access to Object Migration in TRIRIGA, access is granted at the group level. Follow the steps given in [Chapter 1](https://www.ibm.com/docs/en/SSFCZ3_11.2/pdf/pdf_tri_app_admin.pdf) to create a new security group.

1. Select the newly created group or desired existing group and switch to the **Access** tab and add the appropriate access for importation of Object Migration Packages. There are 2 panes in **Access**: **Object** and **Permissions**. 
2. Scroll and find the Object Migration Object on the left pane and click **Full Access** on the right pane. The users in this group, granted through the Members tab, will be able to import the TRIRIGA API Object Migration package. 

<img width="1002" alt="Access Permissions" src="https://media.github.ibm.com/user/348712/files/baf46d80-1a45-11ed-924e-acda1e9bfe24">

*Access Permissions*


Additional information on [TRIRIGA Security groups](https://www.ibm.com/docs/en/tap/3.6.1?topic=security-groups) 

#### Import OM Package

Import the OM Package labeled *APIConnector* into the TRIRIGA instance. Go to **Tools -> Administration -> Object Migration** and select **New Import Package** to begin the import process.

Please refer to the [IBM® TRIRIGA documentation](https://www.ibm.com/docs/en/tap/3.6.1?topic=objects-object-migration-overview ) for more information on Object Migration.

#### TRIRIGA API User Access

In order for App Connect to be able to use TRIRIGA APIs, it will need a user with certain permissions. These user's credentials will be configured in App Connect.

1. Create a new user by following the steps given in [Chapter 2](https://www.ibm.com/docs/en/SSFCZ3_11.2/pdf/pdf_tri_app_admin.pdf).

2. Choose a security group or create a new group (refer to [the above OM Migration steps](#object-migration-access) if a new security group needs to be created) and add the newly created user to it.

3. Add the permissions below for the new user's group:

  | Module        |     Business Object     |     Permissions    |
  |---------------|-------------------------|--------------------|
  | Location      |     triBuilding         |     Read           |
  | triAPIConnect |     triAPICTimestamp    | Read and Update    |
  
  
4. Follow the steps given in [Chapter 1](https://www.ibm.com/docs/en/SSFCZ3_11.2/pdf/pdf_tri_app_admin.pdf) and add **any one of the licenses below** for the new user's group:<br>
  a. IBM TRIRIGA Portfolio Data Manager<br>
  b. IBM Facilities and Real Estate Management on Cloud Self Service<br>
  c. Any other license that grants access to the modules

Please refer to the [TRIRIGA Documentation on Security and Licenses](https://www.ibm.com/docs/en/tap/3.6.1?topic=platform-license-files) for additional information.
  
The user will now be able to interact with the proper TRIRIGA Modules.

#### Outbound Traffic from TRIRIGA

For outbound traffic from TRIRIGA, grant at least READ access on the Business Objects that will be used. The table below shows the various supported business Objects the API can pull from: 

Module | Business Object Label
--|--
Asset | [Building Equipment](https://github.com/IBM/tririga-api/blob/main/markdowns/Asset.md)
Classification | [Request Class](https://github.com/IBM/tririga-api/blob/main/markdowns/RequestClass.md)
Classification | [Space Class Current](https://github.com/IBM/tririga-api/blob/main/markdowns/SpaceClass.md)
Classification | [Asset Spec Class](https://github.com/IBM/tririga-api/blob/main/markdowns/AssetSpecClass.md)
People | [People](https://github.com/IBM/tririga-api/blob/main/markdowns/People.md)
Location |[Property](https://github.com/IBM/tririga-api/blob/main/markdowns/Property.md)
Location |[Building](https://github.com/IBM/tririga-api/blob/main/markdowns/Building.md)
Location |[Floor](https://github.com/IBM/tririga-api/blob/main/markdowns/Floor.md)
Location |[Space](https://github.com/IBM/tririga-api/blob/main/markdowns/Space.md)
Organization |[Organization](https://github.com/IBM/tririga-api/blob/main/markdowns/Organization.md)
Request |[Service Request](https://github.com/IBM/tririga-api/blob/main/markdowns/ServiceRequest.md)
Task |[Work Task](https://github.com/IBM/tririga-api/blob/main/markdowns/WorkTask.md)

In the example below, the API user is able to pull data from the Building Business Object: 

<img width="1097" alt="Outbound Business Object" src="https://media.github.ibm.com/user/348712/files/e5633b80-3b58-11ed-81a4-46540898ff50">

#### Inbound traffic to TRIRIGA

For inbound traffic, Data Access needs to be enabled as well as Application Access permissions to the **triAPIConnect Module** or the individual Objects. To enable an API user to create a building, grant access to the triAPICBuilding Business object as shown below: 

<img width="1000" alt="Inbound Traffic Business Object" src="https://media.github.ibm.com/user/348712/files/e5633b80-3b58-11ed-842e-cea368ad86dd">

#### Minimum requirements

For users to pull from these URLs, the minimum requirements are:

URL | Requirement
--|--
GET /oslc/spq/triAPICOutboundBuildingQC  | READ access to triBuilding Business object
GET /oslc/spq/triAPICTimeStampQC  | READ access to triAPICTimestamp Business Object 
POST /oslc/so/triAPICTimeStampRS/<ID> | Write access to triAPICTimestamp Business Object 

### Step 2: Group Name Configuration

The following steps outline the necessary configurations for the Envizi Group Name Configuration page.

#### Data Modeler

1. Go to **Tools -> Builder Tools -> Data Modeler** and using the Object Browser navigate to **Location->triBuilding**.

2. Revise the BO and add four fields: **cstEnviziParentOneTX**, **cstEnviziParentTwoTX**,
**cstEnviziParentThreeTX** and **cstEnviziGroupNamePathTX**

<img width="512" alt="Data Modeler Fields" src="https://media.github.ibm.com/user/348712/files/3ddd5780-09b5-11ed-9014-fbd85b21e6eb">

Name and Label should be the following:

Name | Label
--|--
cstEnviziParentOneTX | Envizi Group 1
cstEnviziParentTwoTX | Envizi Group 2
cstEnviziParentThreeTX | Envizi Group 3
cstEnviziGroupNamePathTX | Envizi Path

After entering these values, click **Publish** to publish the BO

#### Form Builder

1. Under **Tools -> Builder Tools -> Form Builder**, click on the Location module on the left side of the screen and then click on **triBuilding**.

2. Revise the triBuilding form by clicking **Revise** in the top right corner of the pop-up

3. In the Navigation pane on the left side of the screen, click on **triBuilding** and then **Add Tab**. Enter **cstEnvizi** as the **Name** and **Envizi** as the Label. Click **Apply**

4. Select this new tab and click on **Add Section**

5. Enter **cstEnviziDetails** as the **Name** and **Envizi Details** as the **Label**. Click **Apply**.

6. Now click on the newly created Section and select **Add Field**. Select each of the four created business objects from the previous step as fields under **Envizi Details**.

7. Select **cstEnviziGroupNamePathTX** and modify **Start Row** to 3 and **Col Span** to 2 on the
properties window. Mark this field as **ReadOnly** and click **Apply**. The form should look like this:

<img width="1028" alt="Form Builder" src="https://media.github.ibm.com/user/348712/files/4f266400-09b5-11ed-9ca4-4253b2fa9baa">
8. Click on **triBuilding** on the left panel and then click on **Sort Tab**. Move
the **cstEnvizi** tab to the second position and click **Apply**

<img width="256" alt="Form Builder Properties" src="https://media.github.ibm.com/user/348712/files/4f266400-09b5-11ed-8a73-f526a24a86db">
9. Publish the form


#### Object Migration

1. Go to **Import Migration** and import package EnviziConfig.zip

2. To do that, click on New Import Package, and select the zip file and click Ok.

3. A new window will be displayed. If it is not displayed, just select the package from the list.

4. On this package, click on **Validate** and wait for the validation to complete. If no errors are displayed, import the package.

#### Security Manager

1. Go to **Tools -> Administration -> Security Manager**

2. This application sets who can and cannot access this newly created tab. Click on the desired group, and navigate to the **Access** tab

3. On this tab select **Location -> triBuilding -> cstEnvizi**

4. Choose the access level for the group and **Save**

<img width="423" alt="Security Manager Access" src="https://media.github.ibm.com/user/348712/files/4fbefa80-09b5-11ed-99e5-b92fca943ddf">

#### Workflow Builder

1. Go to **Tools -> Builder Tools -> Workflow Builder**. Select **Location -> triBuilding**.

2. Within the Location object, search for the existing Workflow **triBuilding - Synchronous - Permanent Save Validation**.

3. Revise this workflow and after Call Module Level Validation add a new WF
call to **triBuilding - Update Envizi fields with defined options** like displayed below:

<img width="736" alt="WF call in Update Envizi Path" src="https://media.github.ibm.com/user/348712/files/374fdf80-09b7-11ed-8f47-3c588b667a58">

It will be defined as the image below:
 
<img width="1062" alt="Call Envizi" src="https://media.github.ibm.com/user/348712/files/478e2400-4e19-11ed-81eb-e5462bbcdff1">

4. Click on the **Start** task at the top and publish the workflow

#### My Reports and OSLC

1. Go to **My Reports** and navigate to **System Reports**. Add those four new fields to the existing query
**triAPICBuilding - OSLC -- Outbound** by clicking the **Columns** tab and adding the four fields like below:
<img width="800" alt="System-Report-Columns" src="https://media.github.ibm.com/user/348712/files/f0d51a00-4e19-11ed-9e28-a658b12af215">

2. Save the report.

3. Open **Tools -> System Setup -> Integration -> OSLC Resource manager** and search for **triAPICOutboundBuildingRS**. On this resource, add the four new fields either individually or using **Import all Fields**

<img width="683" alt="OSLC Manager" src="https://media.github.ibm.com/user/348712/files/73387400-09ba-11ed-912f-569fce1dc420">

#### Navigation Builder

1. Go to **Tools->Builder Tools-> Navigation Builder** and find TRIRIGA Global Menu (or the menu associated to the user that will need access to the app). Select and click **Edit**
<img width="700" alt="Navigation-builder-1" src="https://media.github.ibm.com/user/348712/files/27c34480-3a95-11ed-89ca-17ce43fdeb1d">

2. On navigation Items section, expand **Landing Page –Tools -> Menu Group –>System Setup**. Select **Menu group –>Integration** and expand **Navigations Item Library**
<img width="700" alt="Navigation-builder-2" src="https://media.github.ibm.com/user/348712/files/272aae00-3a95-11ed-85a2-1cb9a42d6900">

3. Search for Envizi, select the item and click on **Add to Collection**
<img width="414" alt="Navigation-builder-3" src="https://media.github.ibm.com/user/348712/files/272aae00-3a95-11ed-8777-57d9809f75dd">

4. Click **Save**. Logout and Login again to the system

#### Time Stamp Pre-requisite

The triAPICTimestamp is a TRIRIGA record needed to set the baseline for when API connect runs for the first time. 

To enable this functionality:

1. Go to **My Reports -> System Reports** and search for **Timestamp** in the **Name** section.
2. Run the system report **triAPICTimestamp – Display – Manager Query** as shown below:
<img width="1792" alt="Time-Stamp-1" src="https://media.github.ibm.com/user/348712/files/e5fbd200-3b58-11ed-8cb3-21aef581d0d3">
 
3. Click **Add**, and create a new record without details, as shown below, and close it

<img width="800" alt="Time-Stamp-2" src="https://media.github.ibm.com/user/348712/files/6a495780-3b4f-11ed-940d-137135ba303d">

The default date and time the record gets automatically applied to the record, and consequent opening of the record shows the default date and time as shown below:

<img width="800" alt="Time-Stamp-3" src="https://media.github.ibm.com/user/348712/files/69b0c100-3b4f-11ed-95fc-1848213524cf">

#### Using the Integration

This tool will allow user to make changes on this new Envizi group name fields. But we must consider the existing records too. If you want those records to be populated, there is a patch helper workflow that can handle that.

The first thing that must be defined is which fields will be used to populate groups 1, 2 and 3 to be used on Envizi. To access the Envizi tool, go to **Tools -> System Setup -> Integration -> Envizi Integation**.

When you open the page, the fields will be displayed as Group1/Group2/Group3. By default the values are World Region/Country/City. The field Envizi Hierarchy Path shows how the Envizi groupnames will be configured according to the selected option. 

Enable Envizi checkbox is available too. The Envizi tab will be displayed only when this checkbox is marked

One more item that must be configured is the Number of levels to be used on the Envizi configuration. Envizi hierarchy path will match this selection.

<img width="900" alt="How-to-use" src="https://media.github.ibm.com/user/348712/files/21cd6380-3a95-11ed-800d-aa0c22556421">

Also, notice that there is a section named **Active/Retire with missing data** and **Draft/Revision with Missing Data**. This section will list the buildings that don’t have data defined for Envizi group 3, so it means that no Envizi group will be populated on those buildings.

You can filter to change only the desired records by changing query **cst -triBuilding -Query -Get All Buildings for envizi**. The list of buildings displayed on this query will be the list of buildings that the patch helper will modify.

To use the tool, just select the desired Envizi group names and click **Save**. On the moment Save is triggered, all buildings will be populated with the desired options. This process may take a few minutes depending on how many buildings you have on your system. After that Envizi groups and path will be updated according to the selections made on Envizi Integration page.

<img width="900" alt="How-to-use-2" src="https://media.github.ibm.com/user/348712/files/27c34480-3a95-11ed-9dc8-b6347de21bee">

Also, every time a building is saved and there are changes on the defined fields, or a new building is created, the Envizi groups and path will be modified according to the selected options. You can find the groups on tab Envizi on the building record

## Operating the Connector

#### On-demand Flow

This flow is for the initial sync or to sync data that was added/updated between specific dates. This flow is meant to be executed just once whenever needed and then stopped.

1. The following parameters in the initial **Set variable** node need to be configured in order to use this flow:
![Set variable configuration](https://media.github.ibm.com/user/375131/files/06799700-0932-11ed-90c4-5842714c7033)
*Override Dates in Set Variable*

Parameter | Value
--|--
OverrideFromDate | The start timestamp of the window between which the data will be pulled from. e.g., `2022-06-26T23:09:30-07:00`
OverrideToDate | The end timestamp of the window between which the data will be pulled from. e.g., `2023-06-26T23:09:30-07:00`

<i>**These dates must be specified in ISO 8601 format**</i>


#### Always-On Flow
This flow is to keep syncing the data after the initial sync. This flow is meant to be kept running and will only sync the data that has been added or updated after its previous execution event. For example, if the flow executes at 2 PM and it's previous execution was at 1 PM, the flow will pull data that has been added or updated after 1 PM.

#### Configuring the Flow Parameters

1. Click on the initial **Set variable** node
![Set variable configuration](https://media.github.ibm.com/user/375131/files/06799700-0932-11ed-90c4-5842714c7033)
*Fields in the initial Set variable node*

- In **Variable -> config -> customer**, enter the value provided by Envizi
- In **Variable -> config -> triURL**, enter URL for the TRIRIGA instance. (e.g., https://example.com:9080)


### Starting and Stopping the flow

1. Click on the kebab menu (three dots) on either the flow's tile or the specific flow page.
<img width="656" alt="Flow start 1" src="https://media.github.ibm.com/user/375131/files/5be4ac00-eb1b-11ec-85b0-b7b2d400a7e1">
*Start the flow from the dashboard*
<img width="959" alt="Flow start 2" src="https://media.github.ibm.com/user/375131/files/5be4ac00-eb1b-11ec-93f2-4031220e144f">
*Start the flow from the page*

2. Click on **Start API** or **Stop API** depending on which action is desired.

## Part 3: Testing

A good way to test the TRIRIGA Outbound connectivity is to use the *Always_On* flow.

1. Start the *Always_On* flow and add a test building in the system.
2. If configured correctly, the integration should pick up this change and deliver a .csv file with just that test building.

See [below](#common-errors-that-arise-from-tririga) for any errors that arise in App Connect.

## Troubleshooting

### Common errors that arise from TRIRIGA
 
The below errors are found in the App Connect logs.

 Error | Cause | Resolution
 -- |-- |--
 404 - API doesn't exist | Flow is not running | Double check that the flow is Active in App Connect
 404 - The HTTP request returned with an error 404 "Not Found" | Incorrect App Connect connector config | Double check that the credentials being used in the HTTP post node in App Connect are correct
 
 - If these do not resolve the issue, try clearing the OSLC Cache in TRIRIGA Admin Console in case the integrations do not work in intended manner.
 


## Reference for Pre-requisite

### TRIRIGA Certificates

> **Note** If you have not already done so, please import App Connect Cert to TRIRIGA to enable encrypted communication. Provide the Cert from App Connect as a secret to the instance of TAS as such:

```
cat <<EOF | oc create -f -
apiVersion: truststore-mgr.ibm.com/v1
kind: Truststore
metadata:
    name: my-tas-truststore
spec:
    license:
        accept: true
    includeDefaultCAs: true
    servers:
    - "example.com:443"
    certificates:
    - alias: alias_1 
      crt: |
        -----BEGIN CERTIFICATE-----
        ...
        Certificate 1   
        ...
        -----END CERTIFICATE-----

        ...

    - alias: alias_n 
      crt: |
        -----BEGIN CERTIFICATE-----
        ...
        Certificate n   
        ...
        -----END CERTIFICATE-----
EOF        

```
