### Product versioning changes in the .NET Framework 4.6 and later versions

#### Details

Product versioning has changed from the previous releases of the .NET Framework, and particularly from the .NET Framework 4, 4.5, 4.5.1, and 4.5.2. The following are the detailed changes:<ul><li>The value of the <code>Version</code> entry in the <code>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full</code> key has changed to <code>4.6.xxxxx</code> for the .NET Framework 4.6 and its point releases, and to <code>4.7.xxxxx</code> for the .NET Framework 4.7 and 4.7.1. In the .NET Framework 4.5, 4.5.1, and 4.5.2, it had the format <code>4.5.xxxxx</code>.</li><li>The file and product versioning for .NET Framework files has changed from the earlier versioning scheme of 4.0.30319.x to 4.6.X.0 for the .NET Framework 4.6 and its point releases, and to 4.7.X.0 for the .NET Framework 4.7 and 4.7.1. You can see these new values when you view the file's Properties after right-clicking on a file.</li><li>The <xref:System.Reflection.AssemblyFileVersionAttribute> and <xref:System.Reflection.AssemblyInformationalVersionAttribute> attributes for managed assemblies have Version values in the form 4.6.X.0 for the .NET Framework 4.6 and its point releases, and 4.7.X.0 for the .NET Framework 4.7 and 4.7.1.</li><li>In the .NET Framework 4.6, 4.6.1, 4.6.2, 4.7, and 4.7.1, the <xref:System.Environment.Version?displayProperty=nameWithType> property returns the fixed version string <code>4.0.30319.42000</code>. In the .NET Framework 4, 4.5, 4.5.1, and 4.5.2, it returns version strings in the format <code>4.0.30319.xxxxx</code> (for example, &quot;4.0.30319.18010&quot;). Note that we do not recommend application code taking any new dependency on the Environment.Version property.</li></ul>For more information, see [How to: Determine which .NET Framework Versions Are Installed](~/docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md).

#### Suggestion

In general, applications should depend on the recommended techniques for detecting such things as the runtime version of the .NET Framework and the installation directory:<ul><li>To detect the runtime version of the .NET Framework, see [How to: Determine Which .NET Framework Versions Are Installed](~/docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md).</li><li>To determine the installation path for the .NET Framework, use the value of the <code>InstallPath</code> entry in the <code>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full</code> key.</li></ul> <blockquote> [!IMPORTANT] The subkey name is <code>NET Framework Setup</code>, not <code>.NET Framework Setup</code>.</blockquote> <ul><li>To determine the directory path to the .NET Framework common language runtime, call the <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetRuntimeDirectory?displayProperty=nameWithType> method.</li><li>To get the CLR version, call the <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetSystemVersion?displayProperty=nameWithType> method. For the .NET Framework 4 and its point releases (the .NET Framework 4.5, 4.5.1, 4.5.2, and .NET Framework 4.6, 4.6.1, 4.6.2, 4.7, and 4.7.1), it returns the string v4.0.30319.</li></ul>

| Name    | Value       |
|:--------|:------------|
| Scope   |Minor|
|Version|4.6|
|Type|Runtime|

#### Affected APIs

Not detectable via API analysis.

<!--

#### Affected APIs

Not detectable via API analysis.

-->
