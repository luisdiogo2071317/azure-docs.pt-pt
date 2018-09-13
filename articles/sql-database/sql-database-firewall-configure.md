---
title: Regras de firewall de base de dados SQL e o armazém de dados do Azure | Documentos da Microsoft
description: Saiba como configurar uma base de dados SQL e o firewall do SQL Data Warehouse com regras de firewall ao nível do servidor para gerir o acesso e configurar regras de firewall ao nível da base de dados de base de dados SQL.
keywords: firewall de base de dados do servidor
services: sql-database
author: VanMSFT
manager: craigg
ms.service: sql-database
ms.prod_service: sql-database, sql-data-warehouse
ms.custom: security
ms.topic: conceptual
ms.date: 09/07/2018
ms.author: vanto
ms.openlocfilehash: 118911687326bfa938c7276337377e3d3cb66bf2
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44715402"
---
# <a name="azure-sql-database-and-sql-data-warehouse-firewall-rules"></a>Regras de firewall de base de dados SQL e SQL Data Warehouse do Azure 

Microsoft Azure [base de dados SQL](sql-database-technical-overview.md) e [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) fornecer um serviço de base de dados relacional para o Azure e outros aplicativos baseados na Internet. Para ajudar a proteger os seus dados, as firewalls impedem todos os acessos ao seu servidor de base de dados enquanto não especificar que computadores têm acesso. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido.

> [!NOTE]
> Este tópico aplica-se ao servidor SQL do Azure, bem como às bases de dados da Base de Dados SQL e do SQL Data Warehouse que são criadas no servidor SQL do Azure. Para simplificar, a Base de Dados SQL é utilizada para referenciar a Base de Dados SQL e o SQL Data Warehouse.

#### <a name="virtual-network-rules-as-alternatives-to-ip-rules"></a>Regras de rede virtual como alternativas às regras IP

Para além das regras IP, o firewall também gerencia *regras de rede virtual*. Regras de rede virtual se baseiam nos pontos finais de serviço de rede Virtual. Regras de rede virtual podem ser preferíveis para regras IP em alguns casos. Para obter mais informações, consulte [pontos finais de serviço de rede Virtual e regras para a base de dados do Azure SQL](sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="overview"></a>Descrição geral

Inicialmente, todos os acessos Transact-SQL ao servidor SQL Azure são bloqueados pela firewall. Para começar a utilizar o servidor SQL do Azure, tem de especificar uma ou mais regras de firewall ao nível do servidor que permitem o acesso ao seu servidor SQL do Azure. Utilize as regras de firewall para especificar quais os intervalos de endereços IP da Internet permitidos e se as aplicações do Azure podem ou não tentar ligar ao servidor do SQL Azure.

Para conceder acesso seletivamente a apenas uma das bases de dados no seu servidor do SQL Azure, tem de criar uma regra ao nível da base de dados para essa base de dados. Especifique um intervalo de endereços IP para a regra de firewall da base de dados que esteja para lá do intervalo de endereços IP especificado na regra de firewall ao nível do servidor e confirme que o endereço IP do cliente está compreendido no intervalo especificado na regra ao nível da base de dados.

> [!IMPORTANT]
> Apenas o SQL Data Warehouse suporta regras de firewall ao nível do servidor e não suporta regras de firewall ao nível da base de dados.

As tentativas de ligação a partir da Internet e do Azure têm, primeiro, de passar pela firewall antes de chegarem ao servidor do SQL Azure ou à base de dados SQL, conforme mostrado no diagrama seguinte:

   ![Diagrama com descrição da configuração da firewall.][1]

* **Regras de firewall ao nível do servidor:** estas regras permitem que os clientes acedam a todo o seu servidor do SQL Azure, ou seja, todas as bases de dados dentro do mesmo servidor lógico. Estas regras são armazenadas na base de dados **mestra**. A configuração das regras de firewall ao nível do servidor pode ser feita no portal ou através de instruções de Transact-SQL. Para criar regras de firewall ao nível do servidor com o portal do Azure ou com o PowerShell, tem de ser o proprietário da subscrição ou um contribuinte da subscrição. Para criar uma regra de firewall ao nível do servidor através de Transact-SQL, deve ligar à instância da Base de dados SQL como o início de sessão principal ao nível do servidor ou como administrador do Azure Active Directory (o que significa que uma regra de firewall ao nível do servidor tem primeiro de ser criada por um utilizador com permissões ao nível do Azure).
* **Regras de firewall de nível de base de dados:** estas regras permitem que os clientes acedam a determinadas bases de dados (seguras) no mesmo servidor lógico. Pode criar estas regras para cada base de dados (incluindo o **mestre** base de dados) e são armazenados nas bases de dados individuais. Regras de firewall ao nível da base de dados para bases de dados mestra e o utilizador só podem ser criadas e geridas através de instruções de Transact-SQL e apenas depois de ter configurado a primeira firewall ao nível do servidor. Se especificar um intervalo de endereços IP na regra de firewall ao nível da base de dados que esteja fora do intervalo especificado na regra de firewall ao nível do servidor, só os clientes que tiverem endereços IP no intervalo ao nível da base de dados podem aceder à base de dados. Pode ter um máximo de 128 regras de firewall ao nível da base de dados para uma base de dados. Para obter mais informações sobre como configurar regras de firewall ao nível da base de dados, consulte o exemplo mais à frente neste artigo e ver [sp_set_database_firewall_rule (Azure SQL Database)](https://msdn.microsoft.com/library/dn270010.aspx).

**Recomendação:** a Microsoft recomenda a utilização de regras de firewall ao nível da base de dados sempre que possível, para melhorar a segurança e tornar a sua base de dados mais portátil. Utilize as regras de firewall ao nível do servidor para os administradores e se tiver muitas bases de dados com os mesmos requisitos de acesso e não quer perder tempo a configurar individualmente cada uma.

> [!Important]
> Base de dados do Windows Azure SQL suporta um máximo de 128 regras de firewall.
>

> [!Note]
> Para obter informações sobre bases de dados portáteis no contexto de continuidade do negócio, veja [Authentication requirements for disa ster recovery (Requisitos de autenticação para a recuperação após desastre)](sql-database-geo-replication-security-config.md).
>

### <a name="connecting-from-the-internet"></a>Ligar a partir da Internet

Quando um computador tenta estabelecer ligação ao servidor da base de dados a partir da Internet, a firewall verifica primeiro o endereço IP de origem do pedido em relação às regras de firewall ao nível da base de dados, para a base de dados que a ligação está a pedir:

* Se o endereço IP do pedido estiver dentro dos intervalos especificados nas regras de firewall ao nível da base de dados, é concedida ligação à Base de Dados SQL que contém a regra.
* Se o endereço IP do pedido não estiver dentro de um dos intervalos especificados na regra de firewall ao nível da base de dados, são verificadas as regras de firewall ao nível do servidor. Se o endereço IP do pedido estiver dentro dos intervalos especificados nas regras de firewall ao nível do servidor, é concedida ligação. As regras de firewall ao nível do servidor aplicam-se a todas as bases de dados do SQL no servidor SQL do Azure.  
* Se o endereço IP do pedido não estiver dentro dos intervalos especificados em qualquer uma das regras de firewall ao nível da base de dados ou ao nível do servidor, o pedido de ligação falha.

> [!NOTE]
> Para aceder à Base de Dados SQL do Azure a partir do seu computador local, confirme que a firewall na sua rede e no seu computador local permite comunicação de saída na porta TCP 1433.
> 

### <a name="connecting-from-azure"></a>Ligar a partir do Azure
Para permitir que as aplicações do Azure se liguem ao servidor do SQL Azure, as ligações do Azure têm de estar ativadas. Quando uma aplicação do Azure tenta ligar ao servidor de base de dados, a firewall verifica se as ligações do Azure são permitidas. Uma definição de firewall com o endereço de início e de fim igual a 0.0.0.0 indica que estas ligações são permitidas. Se a tentativa de ligação não for permitida, o pedido não chega ao servidor da Base de Dados SQL do Azure.

> [!IMPORTANT]
> Esta opção configura a firewall para permitir todas as ligações a partir do Azure, incluindo ligações de subscrições de outros clientes. Quando seleciona esta opção, certifique-se de que as permissões de início de sessão e de utilizador limitam o acesso a utilizadores autorizados apenas.
> 

## <a name="creating-and-managing-firewall-rules"></a>Criar e gerir regras de firewall
A primeira definição de firewall ao nível do servidor pode ser criada com o [portal do Azure](https://portal.azure.com/) ou através de programação utilizando [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql), [da CLI do Azure](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create), ou o [ REST API](https://docs.microsoft.com/rest/api/sql/firewallrules). As regras seguintes podem ser criadas e geridas com estes métodos e com o Transact-SQL. 

> [!IMPORTANT]
> Regras de firewall ao nível da base de dados só podem ser criadas e geridos com o Transact-SQL. 
>

Para melhorar o desempenho, as regras de firewall ao nível do servidor são colocadas em cache temporariamente ao nível da base de dados. Para atualizar a cache, veja [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). 

> [!TIP]
> Pode usar [a auditoria de base de dados SQL](sql-database-auditing.md) para auditar alterações de firewall ao nível do servidor e ao nível da base de dados.
>

## <a name="manage-firewall-rules-using-the-azure-portal"></a>Gerir regras de firewall com o portal do Azure

Para definir uma regra de firewall ao nível do servidor no portal do Azure, pode optar por aceder à página de descrição geral para a base de dados SQL do Azure ou a página de descrição geral do seu servidor lógico de base de dados do Azure.

> [!TIP]
> Para obter um tutorial, veja [criar uma BD com o portal do Azure](sql-database-get-started-portal.md).
>

**Na página de descrição geral da base de dados**

1. Para definir uma regra de firewall ao nível do servidor a partir da página de descrição geral da base de dados, clique em **definir firewall do servidor** na barra de ferramentas, conforme mostrado na imagem seguinte: O **definições da Firewall** página para o servidor de base de dados SQL é aberto.

      ![regra de firewall do servidor](./media/sql-database-get-started-portal/server-firewall-rule.png) 

2. Clique em **Adicionar IP de cliente** na barra de ferramentas para adicionar o endereço IP do computador que estiver a utilizar e, em seguida, clique em **guardar**. É criada uma regra de firewall ao nível do servidor para o seu endereço IP atual.

      ![configurar regra de firewall do servidor](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

**Na página de descrição geral do servidor**

Abre a página de descrição geral do seu servidor, que mostra o nome de servidor completamente qualificado (como **mynewserver20170403.database.windows.net**) e oferece opções para configuração adicional.

1. Para definir uma regra ao nível do servidor a partir da página de descrição geral do servidor, clique em **Firewall** no menu da esquerda em definições: 

2. Clique em **Adicionar IP de cliente** na barra de ferramentas para adicionar o endereço IP do computador que estiver a utilizar e, em seguida, clique em **guardar**. É criada uma regra de firewall ao nível do servidor para o seu endereço IP atual.

## <a name="manage-firewall-rules-using-transact-sql"></a>Gerir regras de firewall com o Transact-SQL
| Vista de Catálogo ou Procedimento Armazenado | Nível | Descrição |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Servidor |Apresenta as regras de firewall ao nível do servidor atuais |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Servidor |Cria ou atualiza as regras de firewall ao nível do servidor |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Servidor |Remove as regras de firewall ao nível do servidor |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Base de Dados |Apresenta as regras de firewall ao nível da base de dados atuais |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Base de Dados |Cria ou atualiza as regras de firewall ao nível da base de dados |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Bases de Dados |Remove as regras de firewall ao nível da base de dados |


Os exemplos seguintes revisar as regras existentes, ativar um intervalo de endereços IP no servidor do Contoso e elimina uma regra de firewall:
   
```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```
  
Em seguida, adicione uma regra de firewall.
   
```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Para eliminar uma regra de firewall ao nível do servidor, execute o procedimento armazenado sp_delete_firewall_rule. O exemplo seguinte elimina a regra com o nome ContosoFirewallRule:
   
```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```   

## <a name="manage-firewall-rules-using-azure-powershell"></a>Gerir regras de firewall com o Azure PowerShell
| Cmdlet | Nível | Descrição |
| --- | --- | --- |
| [Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule) |Servidor |Devolve as regras de firewall ao nível do servidor atuais |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) |Servidor |Cria uma regra de firewall ao nível do servidor nova |
| [Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule) |Servidor |Atualiza as propriedades de uma regra de firewall ao nível do servidor existente |
| [Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule) |Servidor |Remove as regras de firewall ao nível do servidor |


O exemplo seguinte define uma regra de firewall ao nível do servidor com o PowerShell:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> Para exemplos do PowerShell no contexto de um início rápido, consulte [criar BD - PowerShell](sql-database-powershell-samples.md) e [criar uma base de dados e configurar uma regra de firewall com o PowerShell](scripts/sql-database-create-and-configure-database-powershell.md)
>

## <a name="manage-firewall-rules-using-azure-cli"></a>Gerir regras de firewall com a CLI do Azure
| Cmdlet | Nível | Descrição |
| --- | --- | --- |
|[Criar AZ sql server firewall-rule](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Servidor|Cria uma regra de firewall do servidor|
|[lista do AZ sql server firewall-rule](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Servidor|Lista as regras de firewall num servidor|
|[show do AZ sql server firewall-rule](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Servidor|Mostra os detalhes de uma regra de firewall|
|[atualização do AZ sql server firewall-rule](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Servidor|Atualiza uma regra de firewall|
|[eliminação do AZ sql server firewall-rule](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Servidor|Elimina uma regra de firewall|

O exemplo seguinte define uma regra de firewall ao nível do servidor com a CLI do Azure: 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Para obter um exemplo da CLI do Azure no contexto de um início rápido, consulte [criar BD - CLI do Azure](sql-database-cli-samples.md) e [criar uma base de dados e configurar uma regra de firewall com a CLI do Azure](scripts/sql-database-create-and-configure-database-cli.md)
>

## <a name="manage-firewall-rules-using-rest-api"></a>Gerir regras de firewall com a REST API
| API | Nível | Descrição |
| --- | --- | --- |
| [Listar Regras de Firewall](https://docs.microsoft.com/rest/api/sql/FirewallRules/ListByServer) |Servidor |Apresenta as regras de firewall ao nível do servidor atuais |
| [Criar ou Atualizar Regra de Firewall](https://docs.microsoft.com/rest/api/sql/FirewallRules/CreateOrUpdate) |Servidor |Cria ou atualiza as regras de firewall ao nível do servidor |
| [Eliminar Regra de Firewall](https://docs.microsoft.com/rest/api/sql/FirewallRules/Delete) |Servidor |Remove as regras de firewall ao nível do servidor |

## <a name="server-level-firewall-rule-versus-a-database-level-firewall-rule"></a>Regra de firewall ao nível do servidor em comparação com uma regra de firewall ao nível da base de dados
P. Os usuários de uma base de dados devem ser totalmente isolados da outra base de dados?   
  Em caso afirmativo, conceda acesso ao utilizar regras de firewall ao nível da base de dados. Isso evita usando regras de firewall ao nível do servidor, que permitem o acesso através da firewall para todos os bancos de dados, reduzindo a profundidade das suas defesas.   
 
P. Os utilizadores para o endereço de IP precisam acesso a todas as bases de dados?   
  Utilize regras de firewall ao nível do servidor para reduzir o número de vezes que tem de configurar regras de firewall.   

P. A pessoa ou equipa configurando as regras de firewall apenas tem acesso através do portal do Azure, PowerShell ou a API REST?   
  Tem de utilizar as regras de firewall ao nível do servidor. Regras de firewall ao nível da base de dados só podem ser configuradas com o Transact-SQL.  

P. É a pessoa ou equipa configurando as regras de firewall ao nível da base de dados pode ter permissão de alto nível?   
  Utilize as regras de firewall ao nível do servidor. Configurar regras de firewall ao nível da base de dados com o Transact-SQL, necessita de pelo menos `CONTROL DATABASE` permissão ao nível da base de dados.  

P. É a pessoa ou equipa, configuração ou as regras de firewall, a auditoria gerir centralmente as regras de firewall para muitos (talvez 100) de bases de dados?   
  Esta seleção depende das suas necessidades e ambiente. Regras de firewall ao nível do servidor podem ser mais fácil de configurar, mas o script pode configurar regras ao nível da base de dados. E mesmo que use regras de firewall ao nível do servidor, poderá ter de auditar as regras de firewall de base de dados, para ver se os utilizadores com `CONTROL` permissão na base de dados tenha criado as regras de firewall ao nível da base de dados.   

P. Pode utilizar uma combinação de ambas as regras de firewall ao nível do servidor e ao nível da base de dados?   
  Sim. Alguns usuários, como os administradores poderão ter regras de firewall ao nível do servidor. Outros utilizadores, tais como utilizadores de uma aplicação de base de dados, poderão ter regras de firewall ao nível da base de dados.   

## <a name="troubleshooting-the-database-firewall"></a>Resolver problemas da firewall de base de dados
Se o comportamento do acesso ao serviço Base de Dados SQL do Microsoft Azure não for o esperado, considere o seguinte:

* **Configuração da firewall local:** antes de o computador poder aceder à Base de Dados SQL do Azure, poderá ter de criar uma exceção de firewall no computador para a porta TCP 1433. Se estiver a fazer ligações dentro do limite da cloud do Azure, poderá ter de abrir portas adicionais. Para obter mais informações, consulte a **base de dados SQL: fora vs dentro** secção [portas para além do 1433 para ADO.NET 4.5 e base de dados SQL](sql-database-develop-direct-route-ports-adonet-v12.md).
* **Tradução de endereços de rede (NAT):** devido a NAT, o endereço IP utilizado pelo seu computador para ligar à Base de Dados SQL do Azure poderá ser diferente do endereço IP mostrado nas definições de configuração de IP do computador. Para ver o endereço IP que o computador está a utilizar para ligar ao Azure, inicie sessão no portal e navegue para o separador **Configurar** no servidor que aloja a base de dados. Na secção **Endereços IP permitidos**, é apresentado o **Endereço IP de Cliente Atual**. Clique em **Adicionar** aos **endereços IP Permitidos** para permitir que este computador aceda ao servidor.
* **As alterações à lista de permissões ainda não foram aplicadas:** as alterações à configuração da firewall da Base de Dados SQL do Azure poderão demorar até cinco minutos a serem aplicadas.
* **O início de sessão não está autorizado ou foi utilizada uma palavra-passe incorreta:** se um início de sessão não tiver permissões no servidor da Base de Dados SQL do Azure ou a palavra-passe utilizada estiver incorreta, a ligação ao servidor da Base de Dados SQL do Azure é negada. Criar uma definição de firewall só proporciona aos clientes uma oportunidade para tentar ligar ao seu servidor; cada cliente tem de indicar as credenciais de segurança necessárias. Para obter mais informações sobre a preparação dos inícios de sessão, consulte [gerir bases de dados, inícios de sessão e utilizadores na base de dados do Azure SQL](sql-database-manage-logins.md).
* **Endereço IP dinâmico:** se tiver uma ligação à Internet com endereçamento IP dinâmico e estiver a ter dificuldades em passar a firewall, pode experimentar uma das soluções seguintes.
  
  * Peça ao seu Fornecedor de Serviços Internet (ISP) o intervalo de endereços IP que foi atribuído aos seus computadores clientes que acedem ao servidor da Base de Dados SQL do Azure e, depois, adicione esse intervalo como regra de firewall.
  * Em alternativa, obtenha o endereçamento IP estático para os computadores cliente e adicione os endereços IP como regras de firewall.

## <a name="next-steps"></a>Passos Seguintes

- Para um início rápido sobre como criar uma base de dados e uma regra de firewall ao nível do servidor, consulte [criar uma base de dados SQL do Azure](sql-database-get-started-portal.md).
- Para obter ajuda para ligar a uma base de dados SQL do Azure a partir de aplicações de código aberto ou de terceiros, veja [Client quick-start code samples to SQL Database (Exemplos de código de início rápido de cliente para a Base de Dados SQL)](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Para obter informações sobre portas adicionais que poderá ter de abrir, consulte a **base de dados SQL: fora vs dentro** secção [portas para além do 1433 para ADO.NET 4.5 e base de dados SQL](sql-database-develop-direct-route-ports-adonet-v12.md)
- Para uma descrição geral de segurança da base de dados do Azure SQL, consulte [proteger a sua base de dados](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
