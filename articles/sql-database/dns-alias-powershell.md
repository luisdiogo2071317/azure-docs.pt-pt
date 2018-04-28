---
title: PowerShell para o DNS alias SQL do Azure | Microsoft Docs
description: Cmdlets do PowerShell, como novo AzureRMSqlServerDNSAlias permitem-lhe redirecionar novas ligações de cliente para um servidor de SQL Database do Azure diferente, sem ter de touch qualquer configuração de cliente.
keywords: base de dados de sql de DNS
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.devlang: PowerShell
ms.topic: article
ms.date: 02/05/2018
ms.reviewer: genemi;amagarwa;maboja
ms.author: dmalik
ms.openlocfilehash: 06c27a2fda79afe5a7f4a0e9efdbc51e171bcbda
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>PowerShell para o Alias de DNS para a base de dados SQL do Azure

Este artigo fornece um script do PowerShell que demonstra como pode gerir um alias de DNS para a SQL Database do Azure. O script executa os seguintes cmdlets que efetua as seguintes ações:


Os cmdlets utilizados no exemplo de código são os seguintes:
- [Novo AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/New-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): cria um novo alias DNS no sistema de serviço SQL Database do Azure. O alias refere-se ao servidor da SQL Database do Azure 1.
- [Get-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): lista de todos os aliases DNS que estão atribuídos ao servidor de base de dados SQL 1 e obter.
- [Conjunto AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Set-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Modifica o nome do servidor que o alias é configurado para consultar, a partir dos server 1 para o servidor de base de dados SQL 2.
- [Remover AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Remove-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): remova o alias DNS da BD do SQL server 2, utilizando o nome do alias da.


Os cmdlets do PowerShell anteriores foram adicionados para o **AzureRm.Sql** módulo a partir da versão 5.1.1.




#### <a name="dns-alias-in-connection-string"></a>Alias de DNS na cadeia de ligação

Para ligar um determinado servidor da SQL Database do Azure, um cliente como o SQL Server Management Studio (SSMS) pode fornecer o nome de alias DNS em vez do nome de servidor verdadeiro. Na cadeia de server exemplo seguinte, o alias *qualquer-exclusivo--nome de alias* substitui o primeiro nó delimitada por ponto na cadeia de servidor de quatro nós:
- Cadeia de servidor de exemplo: `any-unique-alias-name.database.windows.net`.



## <a name="prerequisites"></a>Pré-requisitos

Se pretender executar a script do PowerShell fornecido neste artigo de demonstração, aplicam-se os seguintes pré-requisitos:

- Uma conta e subscrição do Azure. Para uma avaliação gratuita, clique em [ https://azure.microsoft.com/free/ ] [ https://azure.microsoft.com/free/].

- Módulo do PowerShell do Azure, com o cmdlet **New-AzureRMSqlServerDNSAlias**.
    - Para instalar ou atualizar, consulte o artigo [módulo Azure PowerShell instalar][install-azurerm-ps-84p].
    - Executar `Get-Module -ListAvailable AzureRM;` no powershell\_ise.exe, para localizar a versão.

- Dois servidores de base de dados do Azure SQL.

## <a name="code-example"></a>Exemplo de código

O PowerShell seguinte exemplo de código começa por atribuir valores literais várias variáveis. Para executar o código, primeiro tem de editar todos os valores de marcador de posição para correspondem aos valores reais no seu sistema. Ou apenas pode Estude o código. E o resultado da consola do código também é fornecido.


```powershell
################################################################
###    Assign prerequisites.                                 ###
################################################################
cls;

$SubscriptionName             = '<EDIT-your-subscription-name>';
[string]$SubscriptionGuid_Get = '?'; # The script assigns this value, not you.

$SqlServerDnsAliasName = '<EDIT-any-unique-alias-name>';

$1ResourceGroupName = '<EDIT-rg-1>';  # Can be same or different than $2ResourceGroupName.
$1SqlServerName     = '<EDIT-sql-1>'; # Must differ from $2SqlServerName.

$2ResourceGroupName = '<EDIT-rg-2>';
$2SqlServerName     = '<EDIT-sql-2>';

# Login to your Azure subscription, first time per session.
Write-Host "You must log into Azure once per powershell_ise.exe session,";
Write-Host "  thus type 'yes' only the first time.";
Write-Host " ";
$yesno = Read-Host '[yes/no]  Do you need to log into Azure now?';
if ('yes' -eq $yesno)
{
    Connect-AzureRmAccount -SubscriptionName $SubscriptionName;
}

$SubscriptionGuid_Get = Get-AzureRmSubscription `
    -SubscriptionName $SubscriptionName;

################################################################
###    Working with DNS aliasing for Azure SQL DB server.    ###
################################################################

Write-Host '[1] Assign a DNS alias to SQL DB server 1.';
New-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $1ResourceGroupName `
    -ServerName         $1SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;

Write-Host '[2] Get and list all the DNS aliases that are assigned to SQL DB server 1.';
Get-AzureRMSqlServerDNSAlias `
    –ResourceGroupName $1ResourceGroupName `
    -ServerName        $1SqlServerName;

Write-Host '[3] Move the DNS alias from 1 to SQL DB server 2.';
Set-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -NewServerName      $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName `
    -OldServerResourceGroup  $1ResourceGroupName `
    -OldServerName           $1SqlServerName `
    -OldServerSubscriptionId $SubscriptionGuid_Get;

# Here your client, such as SSMS, can connect to your "$2SqlServerName"
# by using "$SqlServerDnsAliasName" in the server name.
# For example, server:  "any-unique-alias-name.database.windows.net".

# Remove-AzureRMSqlServerDNSAlias  - would fail here for SQL DB server 1.

Write-Host '[4] Remove the DNS alias from SQL DB server 2.';
Remove-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -ServerName         $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;
```


#### <a name="actual-console-output-from-the-powershell-example"></a>Resultado da consola real do exemplo do PowerShell

O seguinte resultado da consola foi copiado e colado de uma execução real.


```
You must log into Azure once per powershell_ise.exe session,
  thus type 'yes' only the first time.
 
[yes/no]  Do you need to log into Azure now?: yes


Environment           : AzureCloud
Account               : gm@acorporation.com
TenantId              : 72f988bf-1111-1111-1111-111111111111
SubscriptionId        : 45651c69-2222-2222-2222-222222222222
SubscriptionName      : mysubscriptionname
CurrentStorageAccount : 

 
[1] Assign a DNS alias to SQL DB server 1.
[2] Get the DNS alias that is assigned to SQL DB server 1.
[3] Move the DNS alias from 1 to SQL DB server 2.
[4] Remove the DNS alias from SQL DB server 2.
ResourceGroupName ServerName         ServerDNSAliasName    
----------------- ----------         ------------------    
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-2       gm-sqldb-dns-2     unique-alias-name-food


[C:\windows\system32\]
>> 
```

## <a name="next-steps"></a>Passos Seguintes

Para obter uma explicação completa da funcionalidade Alias de DNS para a base de dados SQL, consulte [alias de DNS para a SQL Database do Azure][dns-alias-overview-37v].



<!-- Article links. -->

[https://azure.microsoft.com/free/]: https://azure.microsoft.com/free/

[install-azurerm-ps-84p]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps

[dns-alias-overview-37v]: dns-alias-overview.md

