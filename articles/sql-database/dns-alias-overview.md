---
title: Alias de DNS para a SQL Database do Azure | Microsoft Docs
description: As aplicações podem ligar a um alias para o nome do servidor da SQL Database do Azure. Entretanto, pode alterar a base de dados do SQL Server o alias aponta para em qualquer altura, para facilitar a testar e assim sucessivamente.
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: DNS alias
ms.topic: article
ms.date: 02/05/2018
ms.reviewer: genemi;ayolubek
ms.author: dmalik
ms.openlocfilehash: 1793f44026db55d17fabde5fb92bc46795109418
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="dns-alias-for-azure-sql-database"></a>Alias de DNS para a SQL Database do Azure

Base de dados SQL do Azure tem um servidor de sistema de nomes de domínio (DNS). PowerShell e REST APIs aceitar [chamadas para criar e gerir os aliases DNS](#anchor-powershell-code-62x) para o nome do servidor de base de dados SQL.

A *alias de DNS* pode ser utilizado em vez do nome de servidor da SQL Database do Azure. Programas de cliente podem utilizar o alias as cadeias de ligação. O DNS alias fornece uma camada de tradução que pode redirecionar os programas de cliente para servidores diferentes. Esta camada spares dificuldades de ter que localizar e editar todos os clientes e as cadeias de ligação.

Utilizações comuns para um alias de DNS incluem os seguintes casos:

- Crie uma fácil de recordar nome para um servidor de SQL do Azure.
- Durante o desenvolvimento inicial, o alias pode fazer referência a um servidor de base de dados SQL do teste. Quando a aplicação fica em direto, pode modificar o alias para fazer referência ao servidor de produção. A transição do teste para produção não necessita de qualquer modificação para as configurações de vários clientes que se ligam ao servidor de base de dados.
- Suponha que a base de dados apenas na sua aplicação é movida para outro servidor de base de dados SQL. Aqui, pode modificar o alias sem ter de modificar as configurações dos vários clientes.

#### <a name="domain-name-system-dns-of-the-internet"></a>Sistema de nomes de domínio (DNS) da Internet

Internet baseia-se no DNS. O DNS converte os nomes amigáveis para o nome do seu servidor de SQL Database do Azure.

## <a name="scenarios-with-one-dns-alias"></a>Cenários com um alias de DNS

Suponha que tem de mudar o sistema para um novo servidor da SQL Database do Azure. No passado necessária para localizar e atualizar cada cadeia de ligação em cada programa de cliente. Mas, agora, se as cadeias de ligação utilizam um alias de DNS, tem de ser atualizada apenas uma propriedade de alias.

A funcionalidade de alias DNS da SQL Database do Azure pode ajudar nos seguintes cenários:

#### <a name="test-to-production"></a>Testar a produção

Quando começar a desenvolver os programas de cliente, que utilize um alias de DNS no respetivas cadeias de ligação. Efetue as propriedades do ponto de alias para uma versão de teste do servidor da SQL Database do Azure.

Mais tarde quando o novo sistema entra em direto em produção, pode atualizar as propriedades de alias para apontar para o servidor de base de dados do SQL Server de produção. Não é necessária nenhuma alteração para os programas de cliente.

#### <a name="cross-region-support"></a>Suporte por várias regiões

Uma recuperação de desastre pode deslocar o servidor de base de dados SQL para uma região demográfica diferentes. Para um sistema que estava a utilizar um alias de DNS, a necessidade de localizar e atualizar as cadeias de ligação para todos os clientes pode ser evitada. Em vez disso, pode atualizar um alias para fazer referência para o novo servidor de base de dados do SQL Server que aloja agora a sua base de dados.




## <a name="properties-of-a-dns-alias"></a>Propriedades de um alias de DNS

As seguintes propriedades de aplicam a cada alias DNS para o servidor de base de dados SQL:

- *Nome exclusivo:* cada nome de alias criar é exclusivo em todos os servidores de base de dados do Azure SQL, tal como servidor de nomes.

- *É necessário servidor:* DNS de um alias não pode ser criado, exceto se referenciar exatamente um servidor e o servidor tem de existir. Um alias atualizado sempre tem de referenciar exatamente um servidor existente.
    - Quando remover um servidor de base de dados SQL, o sistema do Azure também ignora todos os aliases DNS que se referem para o servidor.

- *Não vinculado a qualquer região:* aliases DNS não estiverem vinculadas a uma região. Nenhum alias DNS podem ser atualizadas para fazer referência a um servidor de SQL Database do Azure que reside em qualquer região geográfica.
    - No entanto, ao atualizar um alias para fazer referência a outro servidor, ambos os servidores tem de existir o mesmo Azure *subscrição*.

- *Permissões:* para gerir um alias de DNS, o utilizador tem de ter *contribuinte de servidor* permissões, ou superior. Para obter mais informações, consulte [introdução ao controlo de acesso baseado em funções no portal do Azure](../role-based-access-control/overview.md).





## <a name="manage-your-dns-aliases"></a>Gerir os aliases DNS

Cmdlets do PowerShell e REST APIs estão disponíveis para que possa gerir de forma programática os aliases DNS.

#### <a name="rest-apis-for-managing-your-dns-aliases"></a>RESTO das APIs para gerir os aliases DNS

<!-- TODO
??2 "soon" in the following live sentence, is not the best situation.
TODO update this subsection very soon after REST API docu goes live.
Dev = Magda Bojarska
Comment as of:  2018-01-26
-->

A documentação para as APIs REST está disponível junto da seguinte localização web:
- [Base de dados SQL do Azure REST API](https://docs.microsoft.com/rest/api/sql/)

Além disso, as APIs REST podem ser vistas no GitHub em:
- [Servidor de base de dados SQL do Azure, o DNS alias APIs REST do](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/sql/resource-manager/Microsoft.Sql/preview/2017-03-01-preview/serverDnsAliases.json)

<a name="anchor-powershell-code-62x"/>

#### <a name="powershell-for-managing-your-dns-aliases"></a>PowerShell para gerir os aliases DNS

Cmdlets do PowerShell estão disponíveis que chamar as APIs REST.

Um exemplo de código de cmdlets do PowerShell que está a ser utilizados para gerir os aliases DNS é descrito em:
- [PowerShell para o Alias de DNS para a base de dados SQL do Azure](dns-alias-powershell.md)


Os cmdlets utilizados no exemplo de código são os seguintes:
- [Novo AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/New-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): cria um novo alias DNS no sistema de serviço SQL Database do Azure. O alias refere-se ao servidor da SQL Database do Azure 1.
- [Get-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): lista de todos os aliases DNS que estão atribuídos ao servidor de base de dados SQL 1 e obter.
- [Conjunto AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Set-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Modifica o nome do servidor que o alias é configurado para consultar, a partir dos server 1 para o servidor de base de dados SQL 2.
- [Remover AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Remove-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): remova o alias DNS da BD do SQL server 2, utilizando o nome do alias da.


Os cmdlets anteriores foram adicionados para o **AzureRM.Sql** módulo a partir da versão do módulo 5.1.1.




## <a name="limitations-during-preview"></a>Limitações durante a pré-visualização

Atualmente, um DNS alias tem as seguintes limitações:

- *Atraso de até 2 minutos:* demora até 2 minutos para um alias de DNS ser atualizado ou removido.
    - Independentemente de quaisquer atraso breve, o alias imediatamente deixa de referir-se ligações de cliente para o servidor legado.

- *Pesquisa de DNS:* por agora, a forma apenas autoritativa para verificar que servidor DNS um determinado refere-se alias a é efetuando um [pesquisa de DNS](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup).

- *[Não é suportada a auditoria de tabela](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md):* não é possível utilizar um alias de DNS num servidor SQL Database do Azure que tenha *tabela auditoria* ativado numa base de dados.
    - Auditoria de tabela foi preterido.
    - Recomendamos que mova para [auditoria de Blob](sql-database-auditing.md).




## <a name="related-resources"></a>Recursos relacionados

- [Descrição geral da continuidade do negócio com a SQL Database do Azure](sql-database-business-continuity.md), incluindo a recuperação após desastre.



## <a name="next-steps"></a>Passos Seguintes

- [PowerShell para o Alias de DNS para a base de dados SQL do Azure](dns-alias-powershell.md)

