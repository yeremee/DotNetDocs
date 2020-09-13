---
title: "Enabling Cross-Database Access in SQL Server"
ms.date: "03/30/2017"
ms.assetid: 10663fb6-434c-4c81-8178-ec894b9cf895
---
# Enabling Cross-Database Access in SQL Server
Cross-database ownership chaining occurs when a procedure in one database depends on objects in another database. A cross-database ownership chain works in the same way as ownership chaining within a single database, except that an unbroken ownership chain requires that all the object owners are mapped to the same login account. If the source object in the source database and the target objects in the target databases are owned by the same login account, SQL Server does not check permissions on the target objects.  
  
## Off By Default  
 Ownership chaining across databases is turned off by default. Microsoft recommends that you disable cross-database ownership chaining because it exposes you to the following security risks:  
  
- Database owners and members of the `db_ddladmin` or the `db_owners` database roles can create objects that are owned by other users. These objects can potentially target objects in other databases. This means that if you enable cross-database ownership chaining, you must fully trust these users with data in all databases.  
  
- Users with CREATE DATABASE permission can create new databases and attach existing databases. If cross-database ownership chaining is enabled, these users can access objects in other databases that they might not have privileges in from the newly created or attached databases that they create.  
  
## Enabling Cross-database Ownership Chaining  
 Cross-database ownership chaining should only be enabled in environments where you can fully trust highly-privileged users. It can be configured during setup for all databases, or selectively for specific databases using the Transact-SQL commands `sp_configure` and `ALTER DATABASE`.  
  
 To selectively configure cross-database ownership chaining, use `sp_configure` to turn it off for the server. Then use the ALTER DATABASE command with SET DB_CHAINING ON to configure cross-database ownership chaining for only the databases that require it.  
  
 The following sample turns on cross-database ownership chaining for all databases:  
  
```sql
EXECUTE sp_configure 'show advanced', 1;  
RECONFIGURE;  
EXECUTE sp_configure 'cross db ownership chaining', 1;  
RECONFIGURE;  
```  
  
 The following sample turns on cross-database ownership chaining for specific databases:  
  
```sql
ALTER DATABASE Database1 SET DB_CHAINING ON;  
ALTER DATABASE Database2 SET DB_CHAINING ON;  
```  
  
### Dynamic SQL  
 Cross-database ownership chaining does not work in cases where dynamically created SQL statements are executed unless the same user exists in both databases. You can work around this in SQL Server by creating a stored procedure that accesses data in another database and signing the procedure with a certificate that exists in both databases. This gives users access to the database resources used by the procedure without granting them database access or permissions.  
  
## External Resources  
 For more information, see the following resources.  
  
|Resource|Description|  
|--------------|-----------------|  
|[Extending Database Impersonation by Using EXECUTE AS](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms188304(v=sql.105)) and [Cross DB Ownership Chaining Option](/sql/database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option).|Articles describe how to configure cross-database ownership chaining for an instance of SQL Server.|  
  
## See also

- [Securing ADO.NET Applications](../securing-ado-net-applications.md)
- [Overview of SQL Server Security](overview-of-sql-server-security.md)
- [Managing Permissions with Stored Procedures in SQL Server](managing-permissions-with-stored-procedures-in-sql-server.md)
- [Writing Secure Dynamic SQL in SQL Server](writing-secure-dynamic-sql-in-sql-server.md)
- [Signing Stored Procedures in SQL Server](signing-stored-procedures-in-sql-server.md)
- [ADO.NET Overview](../ado-net-overview.md)
