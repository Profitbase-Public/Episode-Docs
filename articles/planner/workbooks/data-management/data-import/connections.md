# Connections
## Overview

From Datamart version 5.3.0, the concept of reusable connections has been added. Instead of setting up all the connection fields one by one for each ruleset, you can now define all avaliable connections in one place, and reuse them.

The connections can be SQL Databases or Azure Blob Storages, and can be used for import and export. They are identified by a user defined name and presented as a dropdown list within each ruleset.

When upgrading from a previous version, all existing connections will be created using all existing rulesets.


## SQL Database

Required Fields:
<br/>
- SQL Server: The name or the IP used to connect.
<br/>
- Database: The name of the database within the server.
<br/>
- User / Password: Credentials used to connect.

## Azure Blob Storage

Required Fields:
<br/>
- Azure Blob ConnectionString: The connection strong used to connect.
<br/>
- Azure blob storage container: The name of the azure blob container.
