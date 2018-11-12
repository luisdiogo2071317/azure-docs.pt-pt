
## <a name="start-your-powershell-session"></a>Iniciar a sessão do PowerShell
Primeiro tem de ter a versão mais recente [do Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) instalado e em execução. Para informações detalhadas, consulte [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Os exemplos neste tópico uso [modelo de implementação Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md), por isso, os exemplos utilizam a [cmdlets do Azure Resource Manager](https://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Executar o [ **Connect-AzureRmAccount** ](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet e verá um ecrã de início de sessão para introduzir as suas credenciais. Utilize as mesmas credenciais que utiliza para iniciar sessão no Portal do Azure.

    Connect-AzureRmAccount

Se tiver várias subscrições, utilize o cmdlet [**Set-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext) para selecionar a subscrição que a sessão do PowerShell deve utilizar. Para ver que subscrição a sessão atual do PowerShell está a utilizar, execute [**Get-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermcontext). Para ver todas as subscrições, execute [**Get-AzureRmSubscription**](https://docs.microsoft.com/powershell/module/servicemanagement/azurerm.profile/get-azurermsubscription).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

