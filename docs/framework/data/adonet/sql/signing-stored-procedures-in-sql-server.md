---
title: "Signing Stored Procedures in SQL Server"
ms.date: "01/05/2018"
ms.assetid: eeed752c-0084-48e5-9dca-381353007a0d
---
# Signing Stored Procedures in SQL Server

A digital signature is a data digest encrypted with the private key of the signer. The private key ensures that the digital signature is unique to its bearer or owner. You can sign stored procedures, functions (except for inline table-valued functions), triggers, and assemblies.

You can sign a stored procedure with a certificate or an asymmetric key. This is designed for scenarios when permissions cannot be inherited through ownership chaining or when the ownership chain is broken, such as dynamic SQL. You can then create a user mapped to the certificate, granting the certificate user permissions on the objects the stored procedure needs to access.

You can also create a login mapped to the same certificate, and then grant any necessary server-level permissions to that login, or add the login to one or more of the fixed server roles. This is designed to avoid enabling the `TRUSTWORTHY` database setting for scenarios in which higher level permissions are needed.

When the stored procedure is executed, SQL Server combines the permissions of the certificate user and/or login with those of the caller. Unlike the `EXECUTE AS` clause, it does not change the execution context of the procedure. Built-in functions that return login and user names return the name of the caller, not the certificate user name.

## Creating Certificates

When you sign a stored procedure with a certificate or asymmetric key, a data digest consisting of the encrypted hash of the stored procedure code, along with the execute-as user, is created using the private key. At run time, the data digest is decrypted with the public key and compared with the hash value of the stored procedure. Changing the execute-as user invalidates the hash value so that the digital signature no longer matches. Modifying the stored procedure drops the signature entirely, which prevents someone who does not have access to the private key from changing the stored procedure code. In either case, you must re-sign the procedure each time you change the code or the execute-as user.

There are two required steps involved in signing a module:

1. Create a certificate using the Transact-SQL `CREATE CERTIFICATE [certificateName]` statement. This statement has several options for setting a start and end date and a password. The default expiration date is one year.

1. Sign the procedure with the certificate using the Transact-SQL `ADD SIGNATURE TO [procedureName] BY CERTIFICATE [certificateName]` statement.

Once the module has been signed, one or more principals needs to be created in order to hold the additional permissions that should be associated with the certificate.

If the module needs additional database-level permissions:

1. Create a database user associated with that certificate using the Transact-SQL `CREATE USER [userName] FROM CERTIFICATE [certificateName]` statement. This user exists in the database only and is not associated with a login unless a login has also been created from that same certificate.

1. Grant the certificate user the required database-level permissions.

If the module needs additional server-level permissions:

1. Copy the certificate to the `master` database.

1. Create a login associated with that certificate using the Transact-SQL `CREATE LOGIN [userName] FROM CERTIFICATE [certificateName]` statement.

1. Grant the certificate login the required server-level permissions.

> [!NOTE]
> A certificate cannot grant permissions to a user that has had permissions revoked using the DENY statement. DENY always takes precedence over GRANT, preventing the caller from inheriting permissions granted to the certificate user.

## External Resources

For more information, see the following resources.

|Resource|Description|
|--------------|-----------------|
|[Module Signing](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/ms345102(v=sql.100))|Describes module signing, providing a sample scenario and links to the relevant Transact-SQL articles.|
|[Signing Stored Procedures with a Certificate](/sql/relational-databases/tutorial-signing-stored-procedures-with-a-certificate)|Provides a tutorial for signing a stored procedure with a certificate.|

## See also

- [Securing ADO.NET Applications](../securing-ado-net-applications.md)
- [Overview of SQL Server Security](overview-of-sql-server-security.md)
- [Application Security Scenarios in SQL Server](application-security-scenarios-in-sql-server.md)
- [Managing Permissions with Stored Procedures in SQL Server](managing-permissions-with-stored-procedures-in-sql-server.md)
- [Writing Secure Dynamic SQL in SQL Server](writing-secure-dynamic-sql-in-sql-server.md)
- [Customizing Permissions with Impersonation in SQL Server](customizing-permissions-with-impersonation-in-sql-server.md)
- [Modifying Data with Stored Procedures](../modifying-data-with-stored-procedures.md)
- [ADO.NET Overview](../ado-net-overview.md)
