# Install-PushServer

## SYNOPSIS
Creates a new Push Server instance, including setting up necessary SQL Server database structure, DNS entries, and other dependencies. 

## SYNTAX

```
```

## DESCRIPTION
This command will perform the following tasks:

1. Create an SRV record in DNS that contains connection information for the Push Server database. This computer's DNS server will be contacted; the user running this command must have permissions to create the record. Use -SkipDNS to skip this step.
2. Connect to the specified SQL Server computer, and database, and attempt to create the Push Server tables. Any existing tables will first be dropped. The user running this command must have permissions to create the tables. Use -SkipDatabase to skip this step. 
3. On the specified computer (or the local machine, if a computer is not specified), create a scheduled task that periodically runs Invoke-PushQueue to process the Push Server queue. You should specify credentials for the scheduled task to run as, and those credentials should have permission to read and write to the SQL Server, and to push configurations (using Start-DscConfiguration) to nodes. Use -SkipTask to skip this step.

See the NOTES section for additional important notes.

## EXAMPLES

### Example 1: xxx
```
```

## PARAMETERS

### -Parameter
xxx

```yaml
Type: PSObject[]
Parameter Sets: (All)
Aliases: 

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### CommonParameters
This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable. For more information, see about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216)

## NOTES
The SQL Server computer hosting the Push Server database must be configured to accept Windows authentication; this is the default in most recent versions of SQL Server. You must create a database in advance on that server, and provide the appropriate permissions. The user running Install-PushServer must have permission to drop and create tables in the database. The user account used for the Push Server scheduled task must have read/write permissions in the database. It is usually easiest if the computer running the scheduled task is in the same domain, or a trusted domain, as the SQL Server computer.

The DNS record created will be an SRV record, and will contain only the SQL Server computer name and database name. Push Server will not use "SQL Server authentication," and cannot be configured to use a username/password combination. 

The scheduled task can be skipped if you do not plan to have a Push Server queue processed automatically on a regular basis.

See about_pushserver for more details, including theory of operation.

## RELATED LINKS
[Command-Name](command-name.md)
