---
title: Arquitetura de conectividade da base de dados SQL do Azure | Documentos da Microsoft
description: Este documento explica a arquitetura de conectividade SQL Database do Azure de dentro do Azure ou a partir de fora do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 11/02/2018
ms.openlocfilehash: 11133a24f4446478dcc7f38ed50eb36de8843442
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978406"
---
# <a name="azure-sql-database-connectivity-architecture"></a>Arquitetura de conectividade de banco de dados SQL do Azure

Este artigo explica a arquitetura de conectividade da base de dados do Azure SQL e explica como os diferentes componentes funcionam para direcionar o tráfego para a sua instância de base de dados do Azure SQL. Estes função componentes conectividade SQL Database do Azure que se para direcionar o tráfego de rede para a base de dados do Azure com os clientes ligar a partir de dentro do Azure e com os clientes ligar a partir de fora do Azure. Este artigo também fornece exemplos de script para alterar a forma como ocorre a conectividade e as considerações relacionadas com a alteração das definições de conectividade do padrão.

## <a name="connectivity-architecture"></a>Arquitetura de conectividade

O diagrama seguinte fornece uma visão geral da arquitetura de conectividade SQL Database do Azure.

![Descrição geral da arquitetura](./media/sql-database-connectivity-architecture/architecture-overview.png)

Os passos seguintes descrevem como é estabelecida uma ligação para uma base de dados SQL do Azure através do base de dados do Azure SQL software Balanceador de carga (SLB) e o gateway de base de dados do Azure SQL.

- Os clientes ligam-se para o SLB, que tem um endereço IP público e escuta na porta 1433.
- O SLB reencaminha o tráfego para o gateway de base de dados do Azure SQL.
- O gateway, dependendo da política de ligação em vigor, redirecionamentos ou proxies o tráfego para o middleware de proxy correta.
- O middleware de proxy reencaminha o tráfego para a base de dados SQL do Azure adequada.

> [!IMPORTANT]
> Cada um desses componentes distribuiu negação de proteção de serviço (DDoS) incorporada em rede e a camada de aplicação.

## <a name="connection-policy"></a>Política de ligação

Base de dados SQL do Azure suporta as seguintes três opções para a definição de política de ligação de um servidor de base de dados SQL:

- **Redirecionamento (recomendado):** clientes estabelecem ligações diretamente para o nó que aloja a base de dados. Para ativar a conectividade, os clientes têm de permitir regras de firewall de saída para todos os endereços de IP do Azure na região (tentar fazer isso usando grupos de segurança de rede (NSG) com [etiquetas de serviço](../virtual-network/security-overview.md#service-tags)), não apenas os endereços IP de Gateway de base de dados de SQL do Azure. Porque pacotes ir diretamente para a base de dados, latência e débito melhoraram o desempenho.
- **Proxy:** neste modo, todas as ligações são transmitidas por proxy através de gateways de base de dados do Azure SQL. Para ativar a conectividade, o cliente tem de ter regras de firewall de saída que permitir que apenas os endereços de IP de Gateway do Azure SQL da base de dados (normalmente, dois endereços IP por região). Escolher este modo pode resultar numa maior latência e débito mais baixo, dependendo da natureza da carga de trabalho. É altamente recomendável a diretiva de conexão de redirecionamento sobre a política de ligação de Proxy para a menor latência e o débito mais elevado.
- **Predefinição:** esta é a ligação política em vigor em todos os servidores após a criação, a menos que alterar explicitamente a diretiva de conexão para o Proxy ou de redirecionamento. A política em vigor depende se conexões têm origem no Azure (redirecionamento) ou fora do Azure (Proxy).

## <a name="connectivity-from-within-azure"></a>Conectividade a partir do Azure

Se estiver a ligar de dentro do Azure num servidor criado depois de 10 de Novembro de 2018, as suas ligações têm uma política de ligação de **redirecionar** por predefinição. Uma política do **redirecionar** significa que as ligações após a sessão TCP é estabelecida para a base de dados SQL do Azure, a sessão de cliente, em seguida, é redirecionado para o middleware de proxy com uma alteração para o IP virtual de destino do que o Azure Gateway de base de dados SQL para que o middleware de proxy. Por esse motivo, todos os pacotes subsequentes fluxo diretamente via o middleware de proxy, ignorando o gateway de base de dados do Azure SQL. O diagrama seguinte ilustra esse fluxo de tráfego.

![Descrição geral da arquitetura](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

> [!IMPORTANT]
> Se criou servidor da base de dados de SQL antes de 10 de Novembro de 2018 sua política de ligação foi definido explicitamente como **Proxy**. Quando utilizar pontos finais de serviço, recomendamos vivamente a política de ligação para a alteração **redirecionar** para permitir um melhor desempenho. Se alterar a sua política de ligação para **redirecionar**, não será suficiente permitir a saída em seu NSG ao gateway de base de dados do Azure SQL IPs listados abaixo, tem de permitir saída para todos os IPs de base de dados do SQL Azure. Isso pode ser feito com a ajuda de etiquetas de serviço do NSG (grupos de segurança de rede). Para obter mais informações, consulte [etiquetas de serviço](../virtual-network/security-overview.md#service-tags).

## <a name="connectivity-from-outside-of-azure"></a>Conectividade a partir de fora do Azure

Se estiver a ligar de fora do Azure, as suas ligações têm uma política de ligação de **Proxy** por predefinição. Uma política do **Proxy** significa que a sessão TCP é estabelecida através do gateway da base de dados do Azure SQL e todos os pacotes subsequentes fluir através do gateway. O diagrama seguinte ilustra esse fluxo de tráfego.

![Descrição geral da arquitetura](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Endereços de IP do gateway da base de dados SQL do Azure

Para ligar a uma base de dados SQL do Azure a partir de recursos no local, terá de permitir tráfego de rede de saída para o gateway de base de dados do Azure SQL para a sua região do Azure. As suas ligações aceda apenas através do gateway ao ligar-se no modo de Proxy, que é o padrão ao ligar a partir de recursos no local.

A tabela seguinte lista os IPs primário e secundário do gateway para todas as regiões de dados SQL Database do Azure. Para algumas regiões, existem dois endereços IP. Nestas regiões, o endereço IP principal é o endereço IP atual do gateway e o segundo endereço IP é um endereço IP de ativação pós-falha. O endereço de ativação pós-falha é o endereço para o qual estamos pode ser movida seu servidor para manter a alta disponibilidade do serviço. Para estas regiões, recomendamos que permite que saída para os endereços IP. O segundo endereço IP pertence à Microsoft e não escutar em quaisquer serviços até que seja ativado pela base de dados do Azure SQL para aceitar ligações.

| Nome da Região | Endereço IP primário | Endereço IP secundário |
| --- | --- |--- |
| Leste da Austrália | 191.238.66.109 | 13.75.149.87 |
| Sudeste da Austrália | 191.239.192.109 | 13.73.109.251 |
| Sul do Brasil | 104.41.11.5 | |
| Canadá Central | 40.85.224.249 | |
| Leste do Canadá | 40.86.226.166 | |
| EUA Central | 23.99.160.139 | 13.67.215.62 |
| Leste da China 1 | 139.219.130.35 | |
| Leste da China 2 | 40.73.82.1 | |
| Norte da China 1 | 139.219.15.17 | |
| Norte da China 2 | 40.73.50.0 | |
| Ásia Oriental | 191.234.2.139 | 52.175.33.150 |
| E.U.A. Leste 1 | 191.238.6.43 | 40.121.158.30 |
| EUA Leste 2 | 191.239.224.107 | 40.79.84.180 * |
| França Central | 40.79.137.0 | 40.79.129.1 |
| Alemanha Central | 51.4.144.100 | |
| Centro-Norte leste da Alemanha | 51.5.144.179 | |
| Índia Central | 104.211.96.159 | |
| Índia do Sul | 104.211.224.146 | |
| Oeste da Índia | 104.211.160.80 | |
| Leste do Japão | 191.237.240.43 | 13.78.61.196 |
| Oeste do Japão | 191.238.68.11 | 104.214.148.156 |
| Coreia do Sul Central | 52.231.32.42 | |
| Coreia do Sul | 52.231.200.86 |  |
| EUA Centro-Norte | 23.98.55.75 | 23.96.178.199 |
| Europa do Norte | 191.235.193.75 | 40.113.93.91 |
| EUA Centro-Sul | 23.98.162.75 | 13.66.62.124 |
| Sudeste Asiático | 23.100.117.95 | 104.43.15.0 |
| Norte do Reino Unido | 13.87.97.210 | |
| Sul do Reino Unido 1 | 51.140.184.11 | |
| Sul do Reino Unido 2 | 13.87.34.7 | |
| Reino Unido Oeste | 51.141.8.11 | |
| EUA Centro-Oeste | 13.78.145.25 | |
| Europa Ocidental | 191.237.232.75 | 40.68.37.158 |
| E.U.A. oeste 1 | 23.99.34.75 | 104.42.238.205 |
| EUA Oeste 2 | 13.66.226.202 | |
||||

\* **Nota:** *E.U.A. Leste 2* também tem um endereço IP terciário de `52.167.104.0`.

## <a name="change-azure-sql-database-connection-policy"></a>Alterar a política de ligação de base de dados do Azure SQL

Para alterar a política de ligação de base de dados do Azure SQL para um servidor de base de dados do Azure SQL, utilize o [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy) comando.

- Se a política de ligação é definida como **Proxy**, todo o fluxo de pacotes através do gateway da base de dados do Azure SQL de rede. Para esta definição, terá de permitir a saída para apenas o IP de gateway do Azure SQL Database. Usar uma definição de **Proxy** tem uma latência mais que uma definição de **redirecionar**.
- Se a sua política de ligação é a definição **redirecionar**, todo o fluxo de pacotes diretamente para o proxy de middleware de rede. Para esta definição, terá de permitir a saída para vários IPs.

## <a name="script-to-change-connection-settings-via-powershell"></a>Script para alterar as definições de ligação através do PowerShell

> [!IMPORTANT]
> Este script requer os [módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps).
>

O script do PowerShell seguinte mostra como alterar a diretiva de conexão.

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <Subscription Name>

# Azure Active Directory ID
$tenantId = "<Azure Active Directory GUID>"
$authUrl = "https://login.microsoftonline.com/$tenantId"

# Subscription ID
$subscriptionId = "<Subscription GUID>"

# Create an App Registration in Azure Active Directory.  Ensure the application type is set to NATIVE
# Under Required Permissions, add the API:  Windows Azure Service Management API

# Specify the redirect URL for the app registration
$uri = "<NATIVE APP - REDIRECT URI>"

# Specify the application id for the app registration
$clientId = "<NATIVE APP - APPLICATION ID>"

# Logical SQL Server Name
$serverName = "<LOGICAL DATABASE SERVER - NAME>"

# Resource Group where the SQL Server is located
$resourceGroupName= "<LOGICAL DATABASE SERVER - RESOURCE GROUP NAME>"


# Login and acquire a bearer token
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$result = $AuthContext.AcquireToken(
"https://management.core.windows.net/",
$clientId,
[Uri]$uri,
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto
)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#Get current connection Policy
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#Set connection policy to Proxy
$connectionType="Proxy" <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

# Apply Changes
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>Script para alterar as definições de ligação através da CLI do Azure

> [!IMPORTANT]
> Este script requer os [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

O script CLI seguinte mostra como alterar a diretiva de conexão.

```azurecli-interactive
<pre>
# Get SQL Server ID
sqlserverid=$(az sql server show -n <b>sql-server-name</b> -g <b>sql-server-group</b> --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $id

# Update connection policy
az resource update --ids $id --set properties.connectionType=Proxy

</pre>
```

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre como alterar a política de ligação de base de dados do Azure SQL para um servidor de base de dados do Azure SQL, consulte [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Para obter informações sobre o comportamento de ligação de base de dados do Azure SQL para clientes que utilizam o ADO.NET 4.5 ou posterior, consulte [portas para além do 1433 para ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Para informações de descrição geral de desenvolvimento de aplicações gerais, veja [descrição geral do desenvolvimento de aplicações de base de dados do SQL](sql-database-develop-overview.md).
