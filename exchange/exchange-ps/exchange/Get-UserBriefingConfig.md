---
external help file: Microsoft.Exchange.Management.RestApiClient.dll-Help.xml
Module Name: exchangeonlinemanagement
online version: https://docs.microsoft.com/powershell/module/exchange/get-userbriefingconfig
schema: 2.0.0
---

# Get-UserBriefingConfig

## SYNOPSIS
This cmdlet is available in the Exchange Online PowerShell V2 and V3 modules. For more information, see [About the Exchange Online PowerShell V2 module]( https://aka.ms/exov2-module) or for V3 Module see - https://aka.ms/exov3-module). The examples/references described below for V2 are also applicable for V3 Module.

Use the Get-UserBriefingConfig cmdlet to get the current state of the Briefing email flag for the specified user.
For more details about configuring the Briefing email, see Configure Briefing email (https://docs.microsoft.com/Briefing/be-admin).

For information about the parameter sets in the Syntax section below, see Exchange cmdlet syntax (https://docs.microsoft.com/powershell/exchange/exchange-cmdlet-syntax).

## SYNTAX

```
Get-UserBriefingConfig -Identity <String>
 [-ResultSize <Microsoft.Exchange.Management.RestApiClient.Unlimited`1[System.UInt32]>] [<CommonParameters>]
```

## DESCRIPTION
This cmdlet requires the .NET Framework 4.7.2 or later.
To run this cmdlet, you need to be a member of one of the following directory role groups in the destination organization:

- Global Administrator
- Exchange Administrator
- Insights Administrator

To learn more about administrator role permissions in Azure Active Directory, see Role template IDs (https://docs.microsoft.com/azure/active-directory/roles/permissions-reference#role-template-ids).

## EXAMPLES

### Example 1
```
Get-UserBriefingConfig -Identity lila@contoso.com
```

This example returns the Briefing configuration for the user lila@contoso.com.

## PARAMETERS

### -Identity
The Identity parameter specifies the user that you want to view (for example, lila@contoso.com).

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResultSize
This parameter is reserved for internal Microsoft use.

```yaml
Type: Microsoft.Exchange.Management.RestApiClient.Unlimited`1[System.UInt32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

## OUTPUTS

## NOTES

## RELATED LINKS

[Configure Briefing email](https://docs.microsoft.com/Briefing/be-admin)

