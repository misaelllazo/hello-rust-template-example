# Supporting elements used in Power BI Templates

> [!NOTE]
> The next queries are set in 30 days as an example, you can modify that value

## These are queries over tables created using Microsoft Graph API

The next information can be collected through the use of these scripts:
- MPARR-AzureADUsers
- MPARR-AzureADDomains
- MPARR-AzureADRoles

All these scripts are connected to Microsoft Graph API through the permissions added under the Microsoft Entra(previously called Azure AD) App, normally called MPARR-Datacollector. And this scripts uses a Current User certificate, if yu need a Local Machine certificate some little changes can be do it for that.

### Query used to collect records from AzureADUsers_CL table removing duplicated data
> [!NOTE]
> To obtain values you need previously executed MPARR-AzureADUsers.ps1 script

> [!IMPORTANT]
> Please remember that you can change the attributes collected from Microsoft Entra ID(Azure AD) modifying the script MPARR-AzureADUsers.ps1

```Kusto
AzureADUsers_CL 
| where TimeGenerated >= now(-30d)
| summarize arg_max(TimeGenerated, UserPrincipalName_s, DisplayName_s, AssignedLicenses_s, City_s, JobTitle_s, Department_s, Mail_s, OfficeLocation_s, UserID_g, LastAccess_t) by UserPrincipalName_s
```

### Query used to collect records from AzureADDomains_CL table removing duplicated data
```Kusto
AzureADDomains_CL
| where TimeGenerated >= now(-30d)
| summarize arg_max(TimeGenerated, Type) by Domain_s
```

### Query used to collect records from AzureADRoles_CL table removing duplicated data
```Kusto
AzureADRoles_CL 
| where TimeGenerated >= now(-30d)
| summarize arg_max(TimeGenerated, Description_s, Members_s, RoleID_g) by DisplayName_s
```

### Query used to collect records from MSProducts_CL table removing duplicated data
```Kusto
MSProducts_CL 
| where TimeGenerated >= now(-30d)
| summarize arg_max(Product_Display_Name_s, TimeGenerated, String_Id_s, Service_Plan_Name_s, Service_Plans_Included_Friendly_Names_s) by GUID_g, Service_Plan_Id_g
```
