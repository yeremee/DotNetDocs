---
title: "How to: Develop a WCF Data Service Running on IIS"
ms.date: "03/30/2017"
dev_langs:
  - "csharp"
  - "vb"
helpviewer_keywords:
  - "WCF Data Services, developing"
  - "WCF Data Services, deploying"
  - "WCF Data Services, hosting"
ms.assetid: f6f768c5-4989-49e3-a36f-896ab4ded86e
---
# How to: Develop a WCF data service running on IIS

This article shows how to use WCF Data Services to create a data service that's based on the Northwind sample database that's hosted by an ASP.NET Web app running on Internet Information Services (IIS). For an example of how to create the same Northwind data service as an ASP.NET Web app that runs on the ASP.NET Development Server, see the [WCF Data Services quickstart](quickstart-wcf-data-services.md).

> [!NOTE]
> To create the Northwind data service, first install the Northwind sample database on the local computer. To install the database, run the Transact-SQL script from [Northwind and pubs sample databases for Microsoft SQL Server](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs).

This article shows how to create a data service by using the Entity Framework provider. Other data services providers are available. For more information, see [Data Services Providers](data-services-providers-wcf-data-services.md).

After you create the service, you must explicitly provide access to data service resources. For more information, see [How to: Enable Access to the Data Service](how-to-enable-access-to-the-data-service-wcf-data-services.md).

## Create the ASP.NET web application that runs on IIS

1. In Visual Studio, on the **File** menu, select **New** > **Project**.

2. In the **New Project** dialog box, select the **Installed** > [**Visual C#** or **Visual Basic**] > **Web** category.

3. Select the **ASP.NET Web Application** template.

4. Enter `NorthwindService` as the name of the project.

5. Click **OK**.

6. On the **Project** menu, select **NorthwindService Properties**.

7. Select the **Web** tab, and then select **Use Local IIS Web Server**.

8. Click **Create Virtual Directory** and then click **OK**.

9. From the command prompt with administrator privileges, execute one of the following commands (depending on the operating system):

    - 32-bit systems:

        ```console
        "%windir%\Microsoft.NET\Framework\v3.0\Windows Communication Foundation\ServiceModelReg.exe" -i
        ```

    - 64-bit systems:

        ```console
        "%windir%\Microsoft.NET\Framework64\v3.0\Windows Communication Foundation\ServiceModelReg.exe" -i
        ```

     This makes sure that Windows Communication Foundation (WCF) is registered on the computer.

10. From the command prompt with administrator privileges, execute one of the following commands (depending on the operating system):

    - 32-bit systems:

        ```console
        "%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet_regiis.exe" -i -enable
        ```

    - 64-bit systems:

        ```console
        "%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet_regiis.exe" -i -enable
        ```

     This makes sure that IIS runs correctly after WCF has been installed on the computer. You might have to also restart IIS.

11. When the ASP.NET application runs on IIS7, you must also perform the following steps:

    1. Open IIS Manager and navigate to the PhotoService application under **Default Web Site**.

    2. In **Features View**, double-click **Authentication**.

    3. On the **Authentication** page, select **Anonymous Authentication**.

    4. In the **Actions** pane, click **Edit** to set the security principal under which anonymous users will connect to the site.

    5. In the **Edit Anonymous Authentication Credentials** dialog box, select **Application pool identity**.

    > [!IMPORTANT]
    > When you use the Network Service account, you grant anonymous users all the internal network access associated with that account.

12. By using SQL Server Management Studio, the sqlcmd.exe utility, or the Transact-SQL Editor in Visual Studio, execute the following Transact-SQL command against the instance of SQL Server that has the Northwind database attached:

    ```sql
    CREATE LOGIN [NT AUTHORITY\NETWORK SERVICE] FROM WINDOWS;
    GO
    ```

    This creates a login in the SQL Server instance for the Windows account used to run IIS. This enables IIS to connect to the SQL Server instance.

13. With the Northwind database attached, execute the following Transact-SQL commands:

    ```sql
    USE Northwind
    GO
    CREATE USER [NT AUTHORITY\NETWORK SERVICE]
    FOR LOGIN [NT AUTHORITY\NETWORK SERVICE] WITH DEFAULT_SCHEMA=[dbo];
    GO
    ALTER LOGIN [NT AUTHORITY\NETWORK SERVICE]
    WITH DEFAULT_DATABASE=[Northwind];
    GO
    EXEC sp_addrolemember 'db_datareader', 'NT AUTHORITY\NETWORK SERVICE'
    GO
    EXEC sp_addrolemember 'db_datawriter', 'NT AUTHORITY\NETWORK SERVICE'
    GO
    ```

    This grants rights to the new login, which enables IIS to read data from and write data to the Northwind database.

## Define the data model

1. In **Solution Explorer**, right-click the name of the ASP.NET project, and then click **Add** > **New Item**.

2. In the **Add New Item** dialog box, select **ADO.NET Entity Data Model**.

3. For the name of the data model, type `Northwind.edmx`.

4. In the Entity Data Model Wizard, select **Generate from Database**, and then click **Next**.

5. Connect the data model to the database by doing one of the following steps, and then click **Next**:

    - If you do not have a database connection already configured, click **New Connection** and create a new connection. For more information, see [How to: Create Connections to SQL Server Databases](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90)). This SQL Server instance must have the Northwind sample database attached.

         \- or -

    - If you have a database connection already configured to connect to the Northwind database, select that connection from the list of connections.

6. On the final page of the wizard, select the check boxes for all tables in the database, and clear the check boxes for views and stored procedures.

7. Click **Finish** to close the wizard.

## Create the data service

1. In **Solution Explorer**, right-click the name of your ASP.NET project, and then click **Add** > **New Item**.

2. In the **Add New Item** dialog box, select **WCF Data Service**.

   ![WCF Data Service item template in Visual Studio 2015](./media/wcf-data-service-item-template.png)

   > [!NOTE]
   > The **WCF Data Service** template is available in Visual Studio 2015, but not in Visual Studio 2017 or later.

3. For the name of the service, enter `Northwind`.

     Visual Studio creates the XML markup and code files for the new service. By default, the code-editor window opens. In **Solution Explorer**, the service has the name, Northwind, and the extension .svc.cs or .svc.vb.

4. In the code for the data service, replace the comment `/* TODO: put your data source class name here */` in the definition of the class that defines the data service with the type that is the entity container of the data model, which in this case is `NorthwindEntities`. The class definition should look this the following:

     [!code-csharp[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#servicedefinition)]
     [!code-vb[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#servicedefinition)]

## See also

- [Exposing Your Data as a Service](exposing-your-data-as-a-service-wcf-data-services.md)
