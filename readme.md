# The Ultimate Azure Inventory Dashboard

## related blog post
https://www.cloudsma.com/2020/10/ultimate-azure-inventory-dashboard/

## related video
https://www.youtube.com/watch?v=Kglnm_jDoQ4&feature=youtu.be

### Contribute
Version 2 of this workbook is much more accurate, but I still welcome contributions, ideas etc. Please contribute with pull requests of your own.

### Deployment
How to import Gallery template https://www.cloudsma.com/2020/11/import-azure-monitor-workbooks/


#### Deployment through the PowerShell
------------

```ps

# Variables
$AzureRmSubscriptionName = "Your-Subscription-Name"
$RgName = "Workbook-Rg-Name"
$workbookDisplayName = "Azure Inventory"
$workbookSourceId = "Azure Monitor"
$workbookType = "workbook"
$templateUri = "https://raw.githubusercontent.com/scautomation/Azure-Inventory-Workbook/master/armTemplate/template.json"
$workbookSerializedData = Invoke-RestMethod -Uri "https://raw.githubusercontent.com/scautomation/Azure-Inventory-Workbook/master/galleryTemplate/template.json"

## Connectivity
# Login first with Connect-AzAccount if not using Cloud Shell
$AzureRmContext = Get-AzSubscription -SubscriptionName $AzureRmSubscriptionName | Set-AzContext -ErrorAction Stop
Select-AzSubscription -Name $AzureRmSubscriptionName -Context $AzureRmContext -Force -ErrorAction Stop

## Action
Write-Host "Deploying : $workbookType-$workbookDisplayName in the resource group : $RgName" -ForegroundColor Cyan
New-AzResourceGroupDeployment -Name $(("$workbookType-$workbookDisplayName").replace(' ', '')) -ResourceGroupName $RgName `
  -TemplateUri $TemplateUri `
  -workbookDisplayName $workbookDisplayName `
  -workbookType $workbookType `
  -workbookSourceId $workbookSourceId `
  -Confirm -ErrorAction Stop

```
