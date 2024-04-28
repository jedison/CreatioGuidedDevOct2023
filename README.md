Creatio Training: Development

Section Wizard:
> Make Copy: always on (why?)
Mini-page to Add, Edit, or View record

Detail Wizard:
+ with list = Detail
FIRST Advanced Settings
1) Add Object (table in the database)
   Settings: show system columns & entire list of column types
   Parent Object: Base object
   Standard columns from Base object
   Don't need to add Columns here - can just do it while adding on Detail page
2) Detail Wizard: Over to page, add fields
3) Section Wizard from page: NEW DETAIL
   Detail Column.Id LINKS to Object Column.Id
4) Set up Columns when displaying Detail

https://academy.creatio.com/online-courses/creatio-object-data-model

https://academy.creatio.com/docs/user/setup_and_administration/user_and_access_management/access_management/operation_permissions/object_operation_permissions

-----

============ DAY 1: ============
Windows requirements + database setup + web server installations and deployments


Supervisor
Supervisor

Disable Call Center

Advanced Settings > Configuration

Errors during compile
community.creatio.com

System Settings:
Show Widget on intro page, Show Widget on login page - to FALSE

web.config
    <fileDesignMode enabled="true" />
    <add key="UseStaticFileContent" value="false" />
Compile ALL

http://localhost/Creatio/0/Nui/ViewModule.aspx?vm=PackageDependenciesDiagramModule

... 0/dev: directly to Configuration

============ DAY 2: ============
Configuration: * - unsaved changes > changes are not saved on file system

Lookups: contained data requires "Data" object (formerly Data Binding) to become part of exported items
(Day 2 from ~1:00 to 2:00, Data Binding 1:40)
! Both the New Lookup as part of the list of Lookups (from Object = Lookup) & Lookup content (from Object = Usr Object Name)
!! Copy within Lookup content is not REFRESHED automatically, it must be redone MANUALLY > ACTUALIZE DATA
- When importing data, it will assume INSERT if record with Primary Key (Id) already exists
- To get update, choose "Forced Update" option on Column Settings

System Settings: SysSettings is header, SysSettingsValue are the Values

============ DAY 3: ============

Object: Represents Structure of Database View
- Creatio will NOT create database Table as this object relates to existing table
- Creatio Database VIEW: Add SQL Script
  - Installation type: BeforePackage, AfterPackage, AfterSchemaData, Uninstall: only used for Marketplaec Add-ons
  - Installation type: BeforePackage
  - Backward compabitility SQL Scripts >> very strict and generally not used
  
  Create or alter view UsrVwContactAgeDays
  as select Id as UsrId, Name as UsrName, BirthDate as UsrBirthDate, datediff(day, BirthDate, getdate()) as UsrAgeDays from Contact
  
  - SQL Script: can be run after creation. But can be DANGEROUS in case of errors
- Register object: EXACTLY the same as database view
  > Represents Structure of Database View
  - Manual create Columns with Code = Alias (UsrId), correct Type, and Title as wanted
  - Object-level: Set Primary Column: Object Settings = Id
- Register Object as LOOKUP
  - Create "export" of Lookup list for Lookup_ContactAgeView
  - Do NOT need to export Lookup values as these are calculated by View

Example: aggregated/calculated values at database level rather than application level

Classic UI vs Freedom UI: Composable Applications
- Add and remove Apps from environment (e.g. Marketing)
> Freedom UI catching up with Classic UI from *** v8.1 ***, missing some features in v8.0

System Designer: Application Hub (Application Management)
- Application higher level than Package: contains 1 or more packages
- New Application: Records & Business Processes 
- Section with 1 or more objects inside
> Automatically creates new Object & Package
  - Can link to existing object
  - Created List page + Form Page
  - Navigation and Sections
  - Data Models
  - Advanced Settings: list of related packages
- Dependencies: only CrtCore - Accounts, Contacts, ... not details like Orders, Invoices, ... Add other Packages if needed

- Form Page: small Edit button top right. Add new data columns and fields
- Make new sectiona and fields
> INPUTS are NOT added to Data Object as Columns
  - Change Element Code of fields added to Page (same as column) thisis used in HTML page
  - Name is, by default, not Copyable
  - Dropdown Freedom = Lookup Classic
  > Dropdown creates new Lookup for you
  -- automatically registers in the Lookup section
  - Enable adding new values needs EDIT page for Drop Down (Lookup)

- Selection Window allows sort, search, 

- Automation tool to automatically "export" lookup values

- Client module: for configuring page structure - what items to show
  Localizable Strings for Page
  AMD in JS (Require.js): aynsch module definition, do not load all modules at start of page, load modules independently & asynch
  MVVM: Model-view-viewmodel
  define( arguments: module name, modules this is dependent on, function() { return JS Object }
    return { 
      viewConfigDiff: << VIEW of items on page: **** KEEP THE /**SCHEMA_xxxx_CONFIG_DIFF*/ as they are used by page Editor
      viewModelConfigDiff: << View Attributes, attributes that come object
      modelConfigDiff: << MODEL is the data: use multiple data sources (Primary: PDS + additional)
        DO NOT MODIFY DIFF sections, made by page Editor
      handlers: OK to add/modify handlers
      converters:
      validators:
    }

  NG: Angular constructs

  Classic UI
    return {
      entitySchemaName
      attributes
      modules /**SCHEMA**/
      details /**SCHEMA**/
      businessRules /**SCHEMA**/
      methods
      dataModels /**SCHEMA**/
      diff /**SCHEMA**/
    }

Front-End Development in Academy: explanation of the UI element code (Freedom and Classic documentation)

Remove JS warnings for version of code
/* jshint esverion: 11 */
> Add own handler, change handler on button, 

============ DAY 4: ============
   
Business Rules:
Page-related
- Field Management: mandatory, hidden, ...
  -- Business Rules on Section Wizard
  - Visibility setting
  + IF ... THEN ... also works Vice Versa to hide or unhide

Object-related
- Field on a Page for Filtering: Static Filter
  - Possibly to do with No condition, Apply Filter / Apply Static Filter FREEDOM UI (~0:25-0:30)
  - Connected Filter: e.g. Country > City

(~0:45-1:00)
Detail: a bunch of connected information that is linked to a main Object, e.g. Address of Account or Contact
- Parent = BaseEntity (6 basic columns, ID, Created, Modified, ...)
- When creating Detail Object manually, Columns within Data source, Lookup to ID of Parent object: 
  On Lookup value deletion:
  - Block Deletion if there are connected records in current object with this value
  - Delete records from current object with this value (Cascade Deletes)
- Default value: system variable = for example, current data and time
- Selection Window for complex objects like Account and Contact
- Default value: System variable = Current User (SysAdminUser) or Current User CONTACT

(~0:57)
> Verion 8 Freedom UI: must create a Page for the Object
- Default page for create, view, edit: Create + > Templates, like Mini Page for small Object
- Field to determine the page layout: should they be in the same Object?

Add Detail to Object: Expanded List
> Apply filter by page data: based on Parent ID
! On Object page, next to Detail + (1:06)
  - Which Column values to Set? Link from new Object to Parent ID
  - Add custom Columns for the Detail (ORDER OF SELECTION MATTERS as that is order in Grid)
  - Can you sort based on HIDDEN column? *************************
  - Content of Detail are loaded when page is loaded even if Detail is Collapsed
- Object: Behavior: Enable Live Data Update (Only FreedomUI pages) -- will cause data to reload when updated elsewhere -- apply to Detail for example
  - only works for ADD and EDIT, not DELETE

Marketplace: distributed as Creatio Packages
- Cisel (neo.technologies): maintenance tools for Creatio
  - Flush redis and restart server
- System Designer: Application Hub > New application : Install From File

Data Binding Tool Add-On - GlbDataBinding (installation in 5 minutes with mostly compilation: ~1:36-1:41)
* Especially powerful in Classic UI * to manage Lookups
- BIND LOOKUP button: register new Lookup, and BIND DATA or Bind All Data
- BIND SETTING for System Settings
- Operation Permissions: BIND OPERATION
- Setting up columns in Classic UI: List view (Save for all Users): Bind Columns Setup --> transfer column setup to new env

Freedom UI: can set default columns on List Page. (Day 4: 1:53)
- Order based on what is clicked to add to list, default Sort field (click carefully)
- Save list settings for all users or Reset to Default

SysProfileData: with Contact is user specific, with Contact empty is general values

============ DAY 5: ============

Ensure that IIS_IUSRS are able to save files to disk for Configuration > Actions > Download packages to file system

Data > Installation Type: 
- Installation: inserts and updates - for columns marked with Forced Update
- Update existing: no inserts, only updates - for columns marked with Forced Update
- Initial installation: only inserts, no updates

Day 5: Calculations
- Data: Read Only

Add Input Field to Page
- Advanced: Data source > Related Objects

- Value is only picked up correctly when Object is updated > Clear Redis Cache >> does not help. 
- Restart application. Fixed the Lookup object beign cached on *Application-side*

FormPage: handlers (~0:45)
line 1: /* jshint esversion: 11*/

Bundled JS file hard to   debug: to unbundle > SWITCH Client Side to Debug Mode 
> Terrasoft.SysSettings.postPersonalSysSettingsValue("IsDebug", true)
- Cloud-based is bundled by default. Turn off by turning on DEBUG Mode

Creatio Logs: Log is login. 0 is application. New folder for each day.
- Error.log has the most detail
- SQL.json includes SQL exceptions

Validation
- Universal validation fields (Freedom UI)
- JS validators section
- Return JS Object with same name as Validator (1:38)
- Add validators to viewModelConfig after modelConfig of item to be validated
- Multi-language messages cannot be done, unless return gets Localizable strings (1:50)
- #ResourceString( name of localizable string )# (1:51)

============ DAY 6: ============

Bug on saving Localizable Strings - it claims that it already exists

Customization: add new columns to existing (base) object
- Replacing object
- Replacing view model
> Both Freedom and Classic UI

> Where to save customization of existing base objects
  System Settings: Current package - default "Custom"

Started work on NBAC on Aug 4, 2021

Classic UI: client page --
  diff: fields on screen
  methods: event handlers
- Section Wizard may save empty JS page with no changes (0:43)

Section Wizard: select Existing Object
- Turn on Copy option for field (Copy this value when copying records)

Classic UI: Multiline Text

Saving Section Classic UI: sometimes errors occur - maybe need to compile beforehand
> may need to delete orphaned items << fails, even after application is restarted
Lookup: EXPORT and IMPORT

Buttons cannot be easily added in Classic UI (1:30)
- UI element operation: Insert, Modify, Merge, Delete
- method is where handler is included with simple Javascript function() (1:35)
(BasePageV2 in CrtNUI Package)
- Can rename Classic UI page name - but also rename in Code

> Localizable string seems to be added twice (Save, error, delete)

============ DAY 7: ============

Server Side Development
- Business Process: C#
  Script Task: C# Script
- return true; for normal flow of process. Process continues. return false will stop process

Visual Studio 2019: better compliation speeds than 2022 (.NET 6 libraries)

Button on page - change to Menu Items
- Action: Run Process

Optimization/Performance
Process log: add Duration (milliseconds: system actions + minutes: user feedback)

Run Visual Studio (2019) as Administrator
Tools > Options > Debugging > General > Suppress JIT optimization on module load (Managed only): ON
  > Enable Edit and Continue: OFF
  > Enable Just My Code: ON
Can use JetBrains IDE

Terrasoft.WebApp > TerraSoft.Configuration > Pkg > <Usr package> > Autogenerated > Src: also works with Creatio IDE mode
Connect to w3wp.exe
Debug > Attach to Process: Show Processes for All Users
Breakpoint will not be active (1:10)
Add Symbols: Settings >> Restart Application in Creatio Studio

Can only debug when running in LOCAL, not Cloud
Debug > Detach All

Add User Actions > Pre-configured page
Page Paramaters = Process parameter
Script not changed: save without compliation

Business Process Tasks will show pages that user did not see from Business Process
- Could show page to another person (e.g. Manager)

Cannot search in Creatio when on the Cloud

Select, Insert, Update, Delete, StoredProc, CustomQuery Classes allow Queries << SQL Server libraries
- Not invoking Object Event model
UserConnection: generated when any user connection is created - when a session starts - current user context

============ DAY 8: ============

Handling Object Events at Server Side
- but not if accessing object at database level (e.g. new INSERT)
- Events on Object definition (0:09)

- Start Event Handling as Event Sub-Process
- Object Embedded Process
- Most used: Script task
- Old Fashioned way of Handling Events: but often used.
- Often running a Method: shown in Methods tab: public virtual void Method()
- Can override the Method()

- (NEWER: recommended) Object Events Layer (listening for object events)
  Objects Business Logic article in Academy
- Saving = Inserts + Updates
- C# Source Code
- event.IsCanceled = true does not inform user. User will assume that everything worked.
  Also throw Exception with localizable string to inform user --> appears as popup

Attachments are separate database objects from main object
- Must first save main entity object, then add attachments
- To check if Attachments are present, NOT part of Object Event Handling as two separate Objects 
- Could be part of Business Process when consuming Data (is Attachment present?)

Web Services (0:46)
- Integrations with third-party applications (e.g. Custom REST queries)
- Add C# Code

FREQUENTLY Use Download Packages to File System (and Version Control)

Visual Studio: Open PROJECT - .csproj in folder Pkg > Package name > Files
- Simple example CryptographicService or VisaDataService: complex arguments
  Windows Communication Framework: Data Contract and Data Member: how to serialize and deserialize objects
- GET can be run from Browser URL to check Web Service Availability
  <website address>/0/rest/<classname>/<methodname>

Must use Creatio IDE for Cloud editions. Otherwise, with File System, use external IDE.

Create Handler to run web service (1:25)
- user LookupAttribute > .value (also displayValue, ...)
- Include SCHEMA_DEPS ["@creatio-devkit/common"] with name (sdk) (1:27)
- endpoint = Terrasoft.combinePath()
- params
- httpClientService.post( endpoint, params )

WebService: calculations, business logic, ...

Classic UI call to Web Service (1:40)
- within diff: make new button
- within methods: new handler
line 1: define("name", ["ServiceHelper"], function(ServiceHelper) { : Ajax request
- callService( with callback service )
- getWebServiceResult: response callback

Postman: third-party apps cannot run web services without authentication
- Recommended Cookie-based authentication
  <website address>/ServiceModel/AuthService.svc/Login
  BODY
  UserName:
  UserPassword:
- Cookies saved in Postman
- Call POST
- 403: Cross Site Request Forgery (CSRF) attack prevented (2:02)
- Add special header: BPMCSRF and add value from BPMCSRF *Cookie*
- Swagger: no support out-of-the-box

For 3rd Party Application that is not as flexible as Postman: (headers and cookies)
- Can use Anonymous Web Services
- No UserConnection within the Service: NOT this.UserConnection, but this.AppConnection.SystemUserConnection
> Code REST service to get the correct UserConnection
- ALSO (lots of) web service configurations: svc file, services.config, Web.config, ... on target environment
> Cannot do it easily in CLOUD: need to create a manual for Creatio Support to deploy it

Studio Designer: System Users shows Session information

System Settings: User session timeout (min): default 60, lowest 10, highest 720

============ DAY 9: ============

Compile via ... on Package

ProcessEngineService.svc

Data Exchange: OData (created for integration, common, industry standard from Microsoft) & DataService (Creatio-developed, some advantages)
- Try OData (OData 4) first, easier
- GET, POST, PATCH, DELETE
- OData Warm-up Effect
> Settings: OData operations allowed with specific users with 
  - Operation persmissions: Access to OData
  - Object Permissions: for OData or Read-Only
    - No Object Permissions = OPEN TO EVERYONE

User Access Rights: Creatio Administration course

Setting Default Values on Creation On Object - column definition
- E.g. Current User **CONTACT**
- Publish = Save & Publish

DataService: used internally by Creatio for data access
- Aggregate columns: number of records of lnked records
- Aggregate filter: 

Call web service to call 3rd party services
- e.g. idbanking.am gold prices
> UsrGoldPrice with 2 values to store results of calling web service, Lookup
- Studio: Web Services - REST or SOAP
- Send Test Request - get JSON
- Response Parameters: Quick Setup: from Response Example > Example in JSON
- Business Process to call 3rd Party Web Service > Call Web Service
  - Call Web Service
  - Delete data: clear previous data
  - Collection: script task or Sub-Process
  - Input parameters to pass values from main process to sub-process
  - Add data
  - Main process to call sub-process: SPECIAL MODE
    - Lightning bolt > item WITHIN Collection
    - Changes to Collection processing mode: sequential or parallel
    - NOT run current and following elements in the background
    - Show Trace Data: JSON values

*** Transferring packages. Source Code.
- Export package takes contents from DATABASE (even if development done external to Creatio IDO with VStudio)
1) Download packages to file system (get rid of *)
2) Update packages from file system - checks file contents and updates database
3) Export from DEV to PROD

Combine several ZIP packages into 1 ZIP

CLIO tools: CI/CD tools (1:47) Advance-Technologies-Foundation
- GitHub
- dotnet tool list -g
- dotnet tool install clio -g
- clio apps
Short and long alternative: show-web-app-list or apps
- clio reg-web-app <ENV> --uri <URL> -l <login> -p <password>
- clio ping -e <ENV>
- clio restart
- clio download <CSV list of packages> -e <ENV> --DestinationPath <zip file name>
- clio install-gate -e <ENV>
- clio restart -e <ENV>
- clio install <zip file name> -e <ENV> -r <log file name> << packages are read only, need to unlock
> Save and Load CMD
- clio sql "<sql>" -e <ENV>
> Works even in CLOUD without direct access to database

============ DAY 10: ============

Fast Track: simplified homework + same online test
- Certification for customer: normally paid, free as part of guided learning
- Certification for partner: always free

Automatic Adding of Records
- Business Process
- Run from Edit Page
- Business Process
  - Input parameter: Id of Parent
  - Counter
  - Add Data: Current Time and Date .AddDays ( Counter )
  - formula expression: decrease Counter
  - Conditional Flow: Counter > 0
  - Default Flow to end
Without Default, Process will appear as Running (stuck process)

Versioning of Process is only useful when developing directly on production env

Visit: Enable Live Update - causes records added by Business Process to appear. Does not work for Delete records.

Self-Assessment Exam: run about *10* times to see all exam questions
- Don't worry too much about questions and results

Basic = 25 Qs
Advanced = 28 Qs
Partners: apply for certification an unlimited number of times
Allowed to do more than 1 attempt (not enough prep for exam) - up to 3.

> Create SQL script, "Install" in Workspace or use CLIO

============ CERTIFICATION HOMEWORK ============

Due to 8.0.10?

List page: no copy ability?

Form page: Business rule to make Comment mandatory, allows Attribute to be Filled in or Not, = or not, but not Greater than.

=============================

CERTIFICATION EXAM
#17 ALSO GetEntity(UserConnection, Id)
