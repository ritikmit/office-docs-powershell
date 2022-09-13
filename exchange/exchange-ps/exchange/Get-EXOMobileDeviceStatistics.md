---
external help file: Microsoft.Exchange.Management.RestApiClient.dll-Help.xml
Module Name: exchangeonlinemanagement
online version: https://docs.microsoft.com/powershell/module/exchange/get-exomobiledevicestatistics
schema: 2.0.0
---

# Get-EXOMobileDeviceStatistics

## SYNOPSIS
This cmdlet is available in the Exchange Online PowerShell V2 and V3 modules. For more information, see [About the Exchange Online PowerShell V2 module]( https://aka.ms/exov2-module) or for V3 Module see - https://aka.ms/exov3-module). The examples/references described below for V2 are also applicable for V3 Module.

Use the Get-EXOMobileDeviceStatistics cmdlet to retrieve the list of mobile devices configured to synchronize with a specified user's mailbox and return a list of statistics about the mobile devices.

For information about the parameter sets in the Syntax section below, see Exchange cmdlet syntax (https://docs.microsoft.com/powershell/exchange/exchange-cmdlet-syntax).

## SYNTAX

### Identity (Default)
```
Get-EXOMobileDeviceStatistics [-ActiveSync] [-OWAforDevices] [-RestApi] [-UniversalOutlook]
 [-ShowRecoveryPassword] [-NotificationEmailAddresses <System.Collections.Generic.List`1[System.String]>]
 [-GetMailboxLog] [-Identity <String>] [-ExternalDirectoryObjectId <Guid>] [-UserPrincipalName <String>]
 [-PrimarySmtpAddress <String>] [<CommonParameters>]
```

### MobileDeviceIdentity
```
Get-EXOMobileDeviceStatistics [-Mailbox <String>] [-ActiveSync] [-OWAforDevices] [-RestApi] [-UniversalOutlook]
 [-ShowRecoveryPassword] [-NotificationEmailAddresses <System.Collections.Generic.List`1[System.String]>]
 [-GetMailboxLog] [<CommonParameters>]
```

## DESCRIPTION
This cmdlet returns a list of statistics about each mobile device.
Additionally, it allows you to retrieve logs and send those logs to a recipient for troubleshooting purposes.

## EXAMPLES

### Example 1
```
Get-EXOMobileDeviceStatistics -Mailbox john.doe@contoso.com
```

Above example fetches the statstics of all devices linked to user with email john.doe@contoso.com

### Example 2
```
Get-EXOMobileDeviceStatistics -Mailbox john.doe@contoso.com -ActiveSync
```

Above example fetches the statstics for active sync devices linked to user with email john.doe@contoso.com

## PARAMETERS

### -Identity
The Identity parameter specifies the mobile device that you want to view.
You can use any value that uniquely identifies the mobile device.
For example:

- GUID
- DeviceID

You can't use this parameter with the ExternalDirectoryObjectId, Mailbox, PrimarySmtpAddress, or UserPrincipalName parameters.

```yaml
Type: String
Parameter Sets: Identity
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Mailbox
The Mailbox parameter filters the results by the user mailbox that's associated with the mobile device.
You can use any value that uniquely identifies the mailbox.
For example:

- GUID
- User ID or user principal name (UPN)

You can't use this parameter with the Identity, PrimarySmtpAddress, or UserPrincipalName parameters.

```yaml
Type: String
Parameter Sets: MobileDeviceIdentity
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -ActiveSync
The ActiveSync switch filters the results by Exchange ActiveSync devices.
You don't need to specify a value with this switch.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ExternalDirectoryObjectId
The ExternalDirectoryObjectId parameter identifies the mailbox that you want to view by the ObjectId in Azure Active Directory.

You can't use this parameter with the Identity, PrimarySmtpAddress, or UserPrincipalName parameters.

```yaml
Type: Guid
Parameter Sets: Identity
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -GetMailboxLog
This parameter is reserved for internal Microsoft use.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -NotificationEmailAddresses
This parameter is reserved for internal Microsoft use.

```yaml
Type: System.Collections.Generic.List`1[System.String]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OWAforDevices
The OWAforDevices switch filters the results by devices where Outlook on the web for devices is enabled.
You don't need to specify a value with this switch.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -PrimarySmtpAddress
The PrimarySmtpAddress identifies the mailbox that you want to view by primary SMTP email address (for example, navin@contoso.com).

You can't use this parameter with the ExternalDirectoryObjectId, Identity, or UserPrincipalName parameters.

```yaml
Type: String
Parameter Sets: Identity
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -RestApi
The RestApi switch filters the results by REST API devices.
You don't need to specify a value with this switch.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ShowRecoveryPassword
This parameter is reserved for internal Microsoft use.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -UniversalOutlook
The UniversalOutlook switch filters the results by Mail and Calendar devices.
You don't need to specify a value with this switch.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -UserPrincipalName
The UserPrincipalName parameter identifies the mailbox that you want to view by UPN (for example, navin@contoso.onmicrosoft.com).

You can't use this parameter with the ExternalDirectoryObjectId, Mailbox, Identity, or PrimarySmtpAddress parameters.

```yaml
Type: String
Parameter Sets: Identity
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### CommonParameters
This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

## OUTPUTS

## NOTES

## RELATED LINKS
