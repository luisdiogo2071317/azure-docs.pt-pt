---
title: SQL Database do Azure - ler consultas em réplicas | Microsoft Docs
description: A base de dados do SQL do Azure fornece a capacidade de carregar saldo só de leitura cargas de trabalho utilizando a capacidade de réplicas de só de leitura - chamado Escalamento de leitura.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/23/2018
ms.author: sashan
ms.openlocfilehash: d2472867c71aedf35e537a29d3912b9e423de2e2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads-preview"></a>Utilizar réplicas de só de leitura para carregar saldo de consulta só de leitura cargas de trabalho (pré-visualização)

**Escalamento de leitura** permite-lhe carregar saldo base de dados SQL do Azure só de leitura cargas de trabalho utilizando a capacidade de réplicas de só de leitura. 

## <a name="overview-of-read-scale-out"></a>Descrição geral de escalamento horizontal de leitura

Cada base de dados no escalão Premium ([DTU com base no modelo de compra](sql-database-service-tiers-dtu.md)) ou na camada de negócio crítico ([vCore com base no modelo de compra (pré-visualização)](sql-database-service-tiers-vcore.md)) é automaticamente aprovisionado com vários Always ON réplicas para suportar o SLA de disponibilidade. Estas réplicas são aprovisionadas com o mesmo nível de desempenho, como a réplica de leitura e escrita utilizado pelas ligações de base de dados normal. O **escalável leitura** funcionalidade permite-lhe carregar saldo base de dados SQL só de leitura cargas de trabalho utilizando a capacidade das réplicas só de leitura em vez de partilhar a réplica de leitura e escrita. Desta forma, a carga de trabalho só de leitura será isolada da carga de trabalho de leitura e escrita principal e não irá afetar o desempenho dele. A funcionalidade destina-se para as aplicações que incluem logicamente separados cargas de trabalho só de leitura, por exemplo, análises e, por conseguinte, foi possível obter os benefícios de desempenho utilizando esta capacidade adicional na não custo extra.

Para utilizar a funcionalidade de ampliação de leitura com uma base de dados específica, tem de ativar explicitamente-lo ao criar a base de dados ou posteriormente alterando a sua configuração através do PowerShell invocando o [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) ou o [ Novo-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlets ou através da API de REST do Azure Resource Manager utilizando o [bases de dados - criar ou atualizar](/rest/api/sql/databases/createorupdate) método. 

Depois de leitura Escalamento horizontal é ativado para uma base de dados, a ligação para a base de dados de aplicações serão direcionadas para a réplica de leitura e escrita ou para uma réplica só de leitura de acordo com a base de dados a `ApplicationIntent` propriedade configurada da aplicação cadeia de ligação. Para obter informações sobre o `ApplicationIntent` propriedade, consulte [especificar tipo de aplicação](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Se leitura Escalamento horizontal está desativado ou defina a propriedade de ReadScale uma camada de serviço não suportado, todas as ligações são direcionadas para a réplica de leitura e escrita, independentemente do `ApplicationIntent` propriedade.

> [!NOTE]
> Durante a pré-visualização, os arquivo de dados de consulta e de eventos expandidos não são suportados nas réplicas só de leitura.

## <a name="data-consistency"></a>Consistência dos dados

Uma das vantagens de Always ON é que as réplicas sempre estão no estado de uma forma consistente, mas em diferentes pontos no tempo poderão existir alguns latência pequena entre réplicas diferentes. Escalamento de leitura suporta a consistência de nível de sessão. Significa que, se a sessão só de leitura fosse ligado depois de um erro de ligação causado pela indisponibilidade de réplica, podem ser redirecionado para uma réplica que não é atualizado com a réplica de leitura-escrita de 100%. Da mesma forma, se uma aplicação escreve dados através de uma sessão de leitura / escrita e imediatamente lê-la através de uma sessão só de leitura, é possível que as atualizações mais recentes não estão imediatamente visíveis. Isto acontece porque a ação de Refazer de registo de transação para as réplicas é assíncrona.

> [!NOTE]
> Latências de replicação na região são baixas e esta situação é raro.


## <a name="connecting-to-a-read-only-replica"></a>Ligar a uma réplica só de leitura

Quando ativa o Escalamento de leitura para uma base de dados, o `ApplicationIntent` opção na cadeia de ligação fornecida pelo cliente determinam se a ligação é encaminhada para a réplica de escrita ou para uma réplica só de leitura. Especificamente, se o `ApplicationIntent` valor é `ReadWrite` (o valor predefinido), a ligação será direcionada para a réplica de leitura / escrita da base de dados. Isto é idêntico ao comportamento existente. Se o `ApplicationIntent` valor é `ReadOnly`, a ligação é encaminhada para uma réplica legível.

Por exemplo, a seguinte cadeia de ligação liga-se o cliente para uma réplica só de leitura (substituir os itens as angulares com os valores corretos para o seu ambiente e remover os angulares):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Uma das seguintes cadeias de ligação liga-se o cliente para uma réplica de leitura / escrita (substituir os itens as angulares com os valores corretos para o seu ambiente e remover os angulares):

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Pode verificar se estiver ligado a uma réplica só de leitura ao executar a consulta seguinte. Devolverá READ_ONLY quando ligado a uma réplica só de leitura.

```SQL
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

## <a name="enable-and-disable-read-scale-out-using-azure-powershell"></a>Ativar e desativar leitura Escalamento horizontal com o Azure PowerShell

A gestão de leitura Escalamento horizontal no Azure PowerShell requer o de 2016 Dezembro Azure PowerShell versão ou mais recente. Para a versão mais recente do PowerShell, consulte [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Ativar ou desativar a leitura Escalamento horizontal no Azure PowerShell invocando o [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) cmdlet e passando no valor pretendido – `Enabled` ou `Disabled` – para o `-ReadScale` parâmetro. Em alternativa, pode utilizar o [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet para criar uma nova base de dados com ler Escalamento horizontal ativado.

Por exemplo, para ativar a leitura de escalamento horizontal para existente da base de dados (substituir os itens as angulares com os valores corretos para o seu ambiente e remover os angulares):

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

Para desativar leitura Escalamento horizontal para uma base de dados (substituir os itens as angulares com os valores corretos para o seu ambiente e remover os angulares):

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```

Para criar uma nova base de dados com a leitura de escalamento horizontal ativada (substituir os itens as angulares com os valores corretos para o seu ambiente e remover os angulares):

```powershell
New-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled -Edition Premium
```

## <a name="enabling-and-disabling-read-scale-out-using-the-azure-sql-database-rest-api"></a>Ativar e desativar leitura Escalamento horizontal utilizando a API de REST de base de dados de SQL do Azure

Para criar uma base de dados com leitura escalável ativado, ou para ativar ou desativar leitura Escalamento horizontal para uma base de dados existente, criar ou atualizar a entidade correspondente da base de dados com o `readScale` propriedade definida como `Enabled` ou `Disabled` do abaixo de exemplo pedido.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body:
{
   "properties":
   {
      "readScale":"Enabled"
   }
} 
```

Para obter mais informações, consulte [bases de dados - criar ou atualizar](/rest/api/sql/databases/createorupdate).

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre como utilizar o PowerShell para definir a leitura de escalamento horizontal, consulte o [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) ou [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlets.
- Para obter informações sobre como utilizar a API REST para definir a leitura de escalamento horizontal, consulte [bases de dados - criar ou atualizar](/rest/api/sql/databases/createorupdate).
