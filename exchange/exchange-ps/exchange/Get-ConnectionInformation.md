---
external help file: Microsoft.Exchange.Management.ExoPowershellGalleryModule.dll-Help.xml
Module Name: exchangeonlinemanagement
online version: https://docs.microsoft.com/powershell/module/exchange/get-connectioninformation
schema: 2.0.0
---

# Get-ConnectionInformation

## SYNOPSIS
This cmdlet is available in the Exchange Online PowerShell V2 and V3 modules. For more information, see [About the Exchange Online PowerShell V2 module]( https://aka.ms/exov2-module) or for V3 Module see - https://aka.ms/exov3-module). The examples/references described below for V2 are also applicable for V3 Module.

Use the Get-ConnectionInformation cmdlet to get information about all REST-based connections in the current PowerShell instance with Exchange Online.

For information about the parameter sets in the Syntax section below, see Exchange cmdlet syntax (https://docs.microsoft.com/powershell/exchange/exchange-cmdlet-syntax).

## SYNTAX

```
Get-ConnectionInformation [<CommonParameters>]
```

## DESCRIPTION
The Get-ConnectionInformation cmdlet returns the information about all active REST-based connections with Exchange Online in the current PowerShell instance.
This cmdlet is the counterpart to Get-PSSession used with Remote PowerShell Sessions.
This cmdlet is available only in version 2.0.6-Preview7 or later.

## EXAMPLES

### Example 1
```
Get-ConnectionInformation
```

This example returns a list of all active REST-based connections with Exchange Online in the current PowerShell instance.

## PARAMETERS

### CommonParameters
This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

## OUTPUTS

## NOTES

## RELATED LINKS
