# PushServer
## about_pushserver

# SHORT DESCRIPTION
A "Push Server" for PowerShell DSC.

# LONG DESCRIPTION
Push Server provides an alternate to PowerShell's "Pull Server" configuration deployment option. Features includes:

- Nodes can be assigned to one or more roles
- Roles can specify one or more configuration fragments
- Configuration fragments are dynamically assembled and compiled into a MOF
- MOFs are automatically deployed to each node for application
- Node LCM configurations are automatically compiled into meta-configuration MOFs, which are pushed to the nodes
- Node LCM configurations are verified by the Push Server and updated as needed

Push Server is compatible with PowerShell v5 and later only. It will fail if run against PowerShell v4.

Push Server stores all information in a SQL Server database (which can include free editions of SQL Server, if you wish). Push Server can operate entirely in "manual mode," meaning it doesn't take any actions against a node until you tell it to. It can also operate in a "queued" mode. In queued mode, actions against nodes are queued, and processed on a regular basis. Queued mode allows for the observance of maintenance windows that you define, meaning queued actions will be held until within a valid maintenance window. Actions against nodes are logged, and any configuration errors or problems are retained in the database. Queued mode provides for automated maintenance of the database (removal of old logging records, for example); manual mode requires you to periodically maintain the database yourself. The SQL Server database must be backed up, if desired, separately, and the SQL Server transaction logs must be maintained (unless the database uses the Simple recovery mode). 

You can run Push Server (that is, install the scheduled task) on multiple nodes in your environment. When adding managed nodes to the database, you specify which Push Server will "service" that node. All Push Servers can read and write from a single SQL Server database. This enables a central database, for example at a corporate headquarters. Push Servers could then be deployed to each company site, and only deal with nodes co-located at the same site. The "which Push Server will service a given node" is manually determined by you; there is no automated "site selection" logic. If a node is not assigned a specific Push Server, then any Push Server you have created may service that node. This entire concept really applies only when using the scheduled task and queued mode; in manual mode, you can determine which site's nodes you are serving each time you run Invoke-PushQueue.

Configuration fragments are essentially normal DSC configuration scripts, without the "Configuration {}" construct or "Node {}" construct. That is, each fragment is simply one or more resource settings. Push Server does not deliver resource modules to nodes; you should configure nodes to use a pull server to retrieve needed resource modules. Such a pull server can simply be an SMB file share that you create on your network. Resource modules are deployed to pull servers in the usual fashion, as ZIP files with the proper versioning in the filename, and accompanied by the appropriate checksum file. 

Node status is tracked. Use Get-PushNode to retrieve a list of configured nodes and their current configuration status. 

