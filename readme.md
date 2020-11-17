# The Ultimate Azure Inventory Dashboard

## related blog post
https://www.cloudsma.com/2020/10/ultimate-azure-inventory-dashboard/

## related video
https://www.youtube.com/watch?v=Kglnm_jDoQ4&feature=youtu.be

### Contribute
This workbook represents a very detailed view of any Azure environment, imo. Is it 100%? Absolutely not, as I don't have access to all Azure resources to test with. Not to mention how long this has taken me as is, I wanted to get it out there so others can contribute. If you find some of your resources aren't collected by this workbook, please add them with a pull request and help me and your fellow community members out.

### Deployment

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
  -workbookSerializedData ($workbookSerializedData | ConvertTo-Json -Depth 20) `
  -Confirm -ErrorAction Stop

```