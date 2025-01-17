---
ms.subservice: office-365-service-communications-api
ms.service: office-365
ms.TocTitle: Office 365 Management Activity API schema
title: Azure Information Protection (AIP) scanner events
description: Audit events from the Azure Information Protection (AIP) Unified Labeling client
ms.ContentId: 4c8793f3-d0b4-4d62-87b4-931cc31d99f5
ms.topic: reference (API)
ms.date: 09/26/2022
ms.localizationpriority: high
---

# Azure Information Protection (AIP) scanner events

The AIP Unified Labeling client includes the Add-in for Office, the Scanner, the Viewer for Windows, the client PowerShell, and the Classify-and-Protect shell extension for Windows. All these components generate audit events that appear in the Office 365 activity logs and can be queried using the Office 365 Management Activity API. For more information, see the [Office 365 Management Activity API reference](/office/office-365-management-api/office-365-management-activity-api-reference).

The five events (also called “AuditLogRecordType”) specific to AIP listed below, and more details about each can be found within the [API reference](/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype).

|Value| Member name | Description|
|:--|:--|:--|
|[93](#aipdiscover)|AipDiscover|AIP scanner events|
|[94](#aipsensitivitylabelaction)|AipSensitivityLabelAction|AIP sensitivity label events|
|[95](#aipprotectionaction)|AipProtectionAction|AIP protection events|
|[96](#aipfiledeleted)|AipFileDeleted|AIP file deletion events|
|[97](#aipheartbeat)|AipHeartBeat|AIP heartbeat events|
 
## AipDiscover

The following tables contain information related to Azure Information Protection (AIP) scanner events.

| Column | Example value | Description |
|:--|:--|:--|
|ClientIP | 20.237.230.167 | The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null. The IP address is displayed in either an IPv4 or IPv6 address format.|
|CreationTime | 2022-08-20T20:14:15Z | The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|DataState | Use | Describes the state of the data.
|Id| 811f76b9-6e2b-269a-4fa5-32f8fc58c9c7 | GUID of the current record. |
|ObjectId |C:\\452Documentcreated.docx | File full path (URL). For SharePoint and OneDrive for Business activity, the full path name of the file or folder accessed by the user.|
|Operation | Access | Describes type of access.
|OrganizationId | 4b080626-0acc-4940-8af8-bfc836ff1a59 | The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.|
|RecordType | 93 | The type of operation indicated by the record. See the AuditLogRecordType table for details on the types of audit log records. For a complete updated list and full description of the Log RecordType, see the [Microsoft 365 Compliance audit log activities via O365 Management API](https://techcommunity.microsoft.com/t5/security-compliance-and-identity/microsoft-365-compliance-audit-log-activities-via-o365/ba-p/2957297) blog post. Here we only list the relevant MIP Record types.|
|Scope | 1 | Was this event created by a hosted O365 service or an on-premises server? Possible values are online and onprem. Note that SharePoint is the only workload currently sending events from on-premises to O365.|
|SensitiveInfoTypeData|[]| Stores the datatype of the Sensitive Info type data.|

| SensitivityLabelEventData  | Example value | Description |  
|:--|:--|:--|
|SensitivityLabelId|dd861593-4fe9-4cd7-8b05-6cf364b34fd4| The current MIP sensitivity label GUID. Use cmdlt Get-Label to get the full values of the GUID.|
|UserId| AdeleV@M365x23987777.OnMicrosoft.com| The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records. |
|UserKey| AdeleV@M365x23987777.OnMicrosoft.com | An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange.| 
|UserType| 0| The type of user that performed the operation. See the UserType table for details on the types of users.</br>0 = Regular</br>1 = Reserved</br>2 = Admin </br>3 = DcAdmin</br>4 = Systeml</br>5 = Application</br>6 = ServicePrincipal</br>7 = CustomPolicy</br>8 = SystemPolicy|
|Version|1| Version ID of the file in the operation. |
|Workload|Aip|Stores The Office 365 service where the activity occurred.|
 
| Common data regarding application and data | Example value | Description |  
|:--|:--|:--|
|ApplicationId   | c00e9d32-3c8d-4a7d-832b-029040e7db99 | The ID of the application performing the operation.|
|ApplicationName | Microsoft Azure Information Protection Word Add-In |	Friendly name of the application performing the operation. Outlook (for email), OWA (for email), Word (for file), Excel (for file), PowerPoint (for file).|
|ProcessName     | WINWORD | The relevant process name, eg. Outlook, msip.app, WinWord.|
|Platform   | 1  | Device platform (Win, OSX, Android, iOS) |
|DeviceName | AdeleVanceWindo | The device on which the activity happened.|
|Location   | On-premises file shares | The location of the document with respect to the user's device. The possible values are unknown, localMedia, removableMedia, fileshare and cloud.|
|ProductVersion  | 2.13.49.0 | Version of the AIP client. |


| ProtectionEventData | Example Value | Description |
|:--|:--|:--|
|ProtectionType|Template|Protection type can be template or ad-hoc.|
|TemplateId |7243257a-99da-42f8-a690-e75383bfc9e7| TemplateID parameter to get a specific template. The Get-AipServiceTemplate cmdlet gets all existing or selected protection templates from Azure Information Protection.|
|IsProtected|false| Whether protected: True/False|
|ProtectionOwner|adelev@m365x23987777.onmicrosoft.com| Rights Management owner in UPN format.|

## AipSensitivityLabelAction

The following tables contain information related to AIP sensitivity label events.

| Column | Example value | Description |
|:--|:--|:--|
|PSComputerName | outlook.office365.com| Computer Name |
|RunspaceId | f228e608-4439-4916-a88b-a1accfcff0ad| The Runspace is a specific instance of PowerShell which contains modifiable collections of commands, providers, variables, functions, and language elements that are available to the command line user.|
|PSShowComputerName | False | The value is False for documented edited in Office 365.  |
|RecordType| AipSensitivityLabelAction | Shows the value of Lable Action. The operation type indicated by the record. For more information, see the [full list of record types](/office/office-365-management-api/office-365-management-activity-api-schema#auditlogrecordtype).|
|CreationDate|	2022-08-15T20:25:58 | The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|UserId| AdeleV@M365x23987777.OnMicrosoft.com| The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records. |
|Operation|SensitivityLabelApplied | The operation type for the audit log.The name of the user or admin activity. For a description of the most common operations/activities:</br>SensitivityLabelApplied, SensitivityLabelUpdated, SensitivityLabelRemoved, SensitivityLabelPolicyMatched, SensitivityLabeledFileOpened.|
|Identity| 13fd12a4-fb5e-487e-f0d7-fda35a221d00| The identity of the user or service to be authenticated.|
|IsValid| true | Boolean | 
|ObjectState| Unchanged | Specifies the state of the object.|

|AuditData| Example value | Description |
|:--|:--|:--|
|SensitiveInfoTypeData|[]| Stores the datatype of the Sensitive Info Type Data |

|ProtectionEventData| Example value | Description |
|:--|:--|:--|
|ProtectionType|Template|Protection type can be template or ad-hoc.|
|TemplateId |7243257a-99da-42f8-a690-e75383bfc9e7|TemplateID parameter to get a specific template.The Get-AipServiceTemplate cmdlet gets all existing or selected protection templates from Azure Information Protection.|
|IsProtected|false|Whether protected: True/False|
|IsProtectedBefore|false|	Whether the content was protected before change: True/False|

|Common| Example value | Description |
|:--|:--|:--|
|ApplicationId| c00e9d32-3c8d-4a7d-832b-029040e7db99| Corresponds to the Azure AD Application ID. |
|ApplicationName| Microsoft Azure Information Protection Word Add-In|	Application friendly name of the application performing the operation.</br>Outlook (for email), OWA (for email), Word (for file), Excel (for file), PowerPoint (for file).|
|ProcessName| WINWORD|Process that hosts MIP SDK.|
|Platform|1|	Device platform (Win, OSX, Android, iOS). |
|DeviceName|AdeleVanceWindow| The name of the user's device.|
|Location|On-premises file shares| The location of the document with respect to the user's device. The possible values are unknown, localMedia, removableMedia, fileshare and cloud.|
|ProductVersion|2.13.49.0| Version of the Azure Information Protection client that performed the audit action.|
|DataState|Use| Specifies the state of the data.|

## AipProtectionAction

This contains information related to AIP protection events.

## AipFileDeleted

This event contains information related to AIP file deletion events.

## AipHeartBeat

The following tables contain information related to AIP heartbeat events.

| Column | Example value | Description |
|:--|:--|:--|
|PSComputerName| outlook.office365.com| Computer Name |
|RunspaceId|8fd1995d-57fe-4010-b3b7-4e09f074d0d0| The Runspace is a specific instance of PowerShell which contains modifiable collections of commands, providers, variables, functions, and language elements that are available to the command line user.|
|PSShowComputerName| False| The value is false for a documented edited in Office 365.|
|RecordType|AipHeartBeat| Shows the value of Label Action. The operation type indicated by the record. For a complete updated list and full description of the Log RecordType, please refer to this article. Here we are only listing the relevant MIP Record types.|
|CreationTime|2022-08-3T16:14:49| The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|UserId| AdeleV@M365x23987777.OnMicrosoft.com| The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records. |
|Operation|SensitivityLabelApplied|The operation type for the audit log.The name of the user or admin activity. For a description of the most common operations/activities. SensitivityLabelApplied ,SensitivityLabelUpdated,SensitivityLabelRemoved ,SensitivityLabelPolicyMatched ,SensitivityLabeledFileOpened.|
|Identity|22041f38-45e3-25d3-50f5-043590dae98c|The identity of the user or service to be authenticated.|
|ObjectState|Unchanged|State of the Object after the current event|

| Column | Example value | Description |
|:--|:--|:--|
|ApplicationId | c00e9d32-3c8d-4a7d-832b-029040e7db9 | The application that where the activity happened and displayed in GUID.|
|ApplicationName | Microsoft Azure Information Protection Word Add-In |	Application friendly name of the application performing the operation.Outlook (for email), OWA (for email), Word (for file), Excel (for file), PowerPoint (for file).|
|ProcessName WINWORD | Process name of the Office application. |
|Platform | 1 | The platform on which the activity happened. For example, Windows. |
|DeviceName | AdeleVanceWindo | Device the event was recorded on. |
|ProductVersion |2.13.49.0 | Version of the Azure Information Protection client that performed the audit action.|
|UserId | AdeleV@M365x23987777.OnMicrosoft.com | The UPN (User Principal Name) of the user who performed the action (specified in the Operation property) that resulted in the record being logged; for example, my_name@my_domain_name. Note that records for activity performed by system accounts (such as SHAREPOINT\system or NT AUTHORITY\SYSTEM) are also included. In SharePoint, another value display in the UserId property is app@sharepoint. This indicates that the "user" who performed the activity was an application that has the necessary permissions in SharePoint to perform organization-wide actions (such as search a SharePoint site or OneDrive account) on behalf of a user, admin, or service. For more information, see the app@sharepoint user in audit records. |
|ClientIP | 20.237.230.167|The IP address of the device that was used when the activity was logged. The IP address is displayed in either an IPv4 or IPv6 address format. For some services, the value displayed in this property might be the IP address for a trusted application (for example, Office on the web apps) calling into the service on behalf of a user and not the IP address of the device used by person who performed the activity. Also, for Azure Active Directory-related events, the IP address isn't logged and the value for the ClientIP property is null.The IP address is displayed in either an IPv4 or IPv6 address format.|
|Id|22041f38-45e3-25d3-50f5-043590dae98c| GUID of the current record |
|RecordType|97|Shows the value of Label Action. The operation type indicated by the record. For a complete updated list and full description of the Log RecordType, please refer to this article. Here we are only listing the relevant MIP Record types.|
|CreationTime|2022-08-3T16:14:49| The date and time in Coordinated Universal Time (UTC) in ISO8601 format when the user performed the activity.|
|Operation|HeartBeat|The name of the user or admin activity. For a description of the most common operations/activities, see Search the audit log in the Office 365 Protection Center ([title](/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance?view=o365-worldwide)|
|OrganizationId| 4b080626-0acc-4940-8af8-bfc836ff1a59|The GUID for your organization's Office 365 tenant. This value will always be the same for your organization, regardless of the Office 365 service in which it occurs.|
|UserType| 0| The type of user that performed the operation. See the UserType table for details on the types of users.</br>0 = Regular</br>1 = Reserved</br>2 = Admin </br>3 = DcAdmin</br>4 = Systeml</br>5 = Application</br>6 = ServicePrincipal</br>7 = CustomPolicy</br>8 = SystemPolicy|
|UserKey| AdeleV@M365x23987777.OnMicrosoft.com| An alternative ID for the user identified in the UserId property. This property is populated with the passport unique ID (PUID) for events performed by users in SharePoint, OneDrive for Business, and Exchange.| 
|Workload|Aip|Stores the The Office 365 service where the activity occurred.|
|Version|1|Version of the Azure Information Protection client that performed the audit action|
|Scope|1| Specifies scope.|
