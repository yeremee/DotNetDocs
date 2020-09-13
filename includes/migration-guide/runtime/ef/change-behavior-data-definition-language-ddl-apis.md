### Change in behavior in Data Definition Language (DDL) APIs

#### Details

The behavior of DDL APIs when AttachDBFilename is specified has changed as follows:<ul><li>Connection strings need not specify an Initial Catalog value. Previously, both AttachDBFilename and Initial Catalog were required.</li><li>If both AttachDBFilename and Initial Catalog are specified and the given MDF file exists, the <xref:System.Data.Objects.ObjectContext.DatabaseExists%2A> method returns <code>true</code>. Previously, it returned <code>false</code>.</li><li>If both AttachDBFilename and Initial Catalog are specified and the given MDF file exists, calling the <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A> method deletes the files.</li><li>If <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A> is called when the connection string specifies an AttachDBFilename value with an MDF that doesn't exist and an Initial Catalog that doesn't exist, the method throws an <xref:System.InvalidOperationException> exception. Previously, it threw a <xref:System.Data.SqlClient.SqlException> exception.</li></ul>

#### Suggestion

These changes make it easier to build tools and applications that use the DDL APIs. These changes can affect application compatibility in the following scenarios:<ul><li>The user writes code that executes a <code>DROP DATABASE</code> command directly instead of calling <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A> if <xref:System.Data.Objects.ObjectContext.DatabaseExists%2A> returns <code>true</code>. This breaks existing code If the database is not attached but the MDF file exists.</li><li>The user writes code that expects the <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A> method to throw a <xref:System.Data.SqlClient.SqlException> rather than an <xref:System.InvalidOperationException> when the Initial Catalog and MDF file don't exist.</li></ul>

| Name    | Value       |
|:--------|:------------|
| Scope   |Minor|
|Version|4.5|
|Type|Runtime|

#### Affected APIs

Not detectable via API analysis.

<!--

#### Affected APIs

Not detectable via API analysis.

-->
