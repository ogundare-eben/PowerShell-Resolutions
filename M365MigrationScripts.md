# PowerShell-Resolutions
My Support PowerShells Scripts
# This repository contains some of my PowerShell script while supporting enterprise customer as a L3 Engineer

# M I G R A T I O N S
>> Get-MoveRequestStatistics user@consto.com -IncludeReport -DiagnosticsInfo "verbose,SlowTimeslot,SlowTimeline" | Export-Clixml -Path .\sample.xml 
#this cmd returns details of onprem-o365 migration for analysis

>> Get-MoveRequestStatistics -Syntax      #to view MoveRequestStatistics syntax
# for more syntax - Get-MoveRequestStatistics - https://docs.microsoft.com/en-us/powershell/module/exchange/get-moverequeststatistics?view=exchange-ps

xml - https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/export-clixml?view=powershell-7#:~:text=The%20Export%2DClixml%20cmdlet%20creates,the%20contents%20of%20that%20file.

---------- cmd to interpret ----------
$r = Import-Clixml <path to xml file customer sent you e.g. MoveReport.xml>
$r = Import-Clixml .\RSuttlermovereport.xml

$r | fl | more
$r.Report.Failures | Select -Last 1
$i=0;$r.report.Failures | foreach { $_ | Select-Object @{name="index";expression={$i}},timestamp,failurecode,failuretype,failureside;$i++} | ft

$r.Report.Failures | fl
$r.report.failure [34]
$r.report.failure [60]
$r.Report.SourceMailboxSize
$r.Report.TargetMailboxSize
$r.Report.Connectivity
$r.Report.SourceThrottles
$stats.report.Failures | group failuretype
$r.Report.TargetThrottles
$r.Report.SessionStatistics
$r.Report.Diagnosticinfo
$r.AllowLargeItems
$r.BadItemLimit
$r.LargeItemLimit
$r.DataConsistencyScore
$r.DataConsistencyScoringFactors

R E M O T E MOVE
Test-MigrationServerAvailability -ExchangeRemoteMove -Autodiscover -EmailAddress administrator@contoso.com -Credentials $Credentials -Verbose

S T A G E D /CUTOVER/IMAP/REMOTEMOVE
Get-MigrationUserStatistics <identity> -IncludeReport -DiagnosticsInfo Verbose | Export-Clixml MigUserStats.xml

I M A P
Get-SyncRequestUserStatistics <identity> -IncludeReport -DiagnosticInfo Verbose | Export-Clixml MigUserStats.xml

O u t l o o k Anywhere:
Test-MigrationServerAvailability -ExchangeOutlookAnywhere -ExchangeServer server01.contoso.local -RPCProxyServer mail.contoso.com -EmailAddress administrator@contoso.com
#https://docs.microsoft.com/en-us/powershell/module/exchange/test-migrationserveravailability?view=exchange-ps
