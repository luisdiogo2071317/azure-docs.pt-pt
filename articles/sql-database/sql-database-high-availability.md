---
title: Elevada disponibilidade - serviço de base de dados do Azure SQL | Documentos da Microsoft
description: Saiba mais sobre as capacidades de elevada disponibilidade do serviço de base de dados do Azure SQL e os recursos
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: jovanpop
ms.reviewer: carlrab, sashan
ms.openlocfilehash: 2283b7559bb0dc7e8333949a8e6382d562162123
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092492"
---
# <a name="high-availability-and-azure-sql-database"></a>Base de dados SQL do Azure e de elevada disponibilidade

Base de dados SQL do Azure é a plataforma como serviço que garante que a sua base de dados está ativo e em execução 99,99% de tempo, sem se preocupar sobre a manutenção e tempos de inatividade de base de dados altamente disponível. Este é um processo de motor de base de dados do SQL Server totalmente gerido alojado na cloud do Azure que garante que a base de dados do SQL Server está sempre atualizado/corrigido sem afetar a carga de trabalho. Base de dados SQL do Azure pode recuperar rapidamente até mesmo nas circunstâncias mais críticas garantir que seus dados estão sempre disponíveis.

Plataforma do Azure totalmente gerencia cada base de dados do SQL do Azure e garante sem perda de dados e uma alta porcentagem de disponibilidade de dados. Azure processa automaticamente a aplicação de patches, as cópias de segurança, replicação, deteção de falhas, hardware potencial subjacente, falhas de software ou de rede, correções de erros de implementação, as ativações pós-falha, atualizações de base de dados e outras tarefas de manutenção. Engenheiros do SQL Server tem implementado as práticas mais conhecidas, garantindo que todas as operações de manutenção são concluídas em menos de 0,01% de tempo da sua vida de base de dados. Esta arquitetura foi concebida para garantir que os dados confirmados nunca são perdidos e que as operações de manutenção são efetuadas sem afetar a carga de trabalho. Não são janelas de manutenção ou tempos de inatividade do que devem exigir a parar a carga de trabalho, enquanto a base de dados é atualizado ou mantida. Elevada disponibilidade incorporada na base de dados do Azure SQL garante que essa base de dados nunca serão ponto único de falha em sua arquitetura de software.

Existem dois modelos de alta disponibilidade aplicados no SQL do Azure:

- Modelo de fins gerais/Standard que fornece 99,99% de disponibilidade, mas com alguma degradação do desempenho potencial durante as atividades de manutenção.
- Modelo de crítico de negócios/Premium que fornece também fornece disponibilidade de 99,99% com o impacto de desempenho mínimos na carga de trabalho, mesmo durante as atividades de manutenção.

O Azure é atualizado e patches do sistema operacional subjacente, drivers e o motor de base de dados do SQL Server transparente com o mínimo de tempo de inatividade para os utilizadores finais. Base de dados SQL do Azure é executado na versão estável mais recente do motor de base de dados do SQL Server e o SO Windows e a maioria dos usuários não percebam que as atualizações são realizadas continuamente.

## <a name="standard-availability"></a>Disponibilidade padrão

Disponibilidade padrão refere-se para o SLA de 99,99%, que é aplicado em escalões fins básico/Standard/gerais. Disponibilidade é conseguida através de separação das camadas de computação e armazenamento. No modelo padrão de disponibilidade, temos duas camadas:

- Uma camada de computação sem monitoração de estado que está a executar o processo de sqlserver.exe e contém apenas transitórios e em cache dados (por exemplo – cache de planos, conjunto de memória intermédia, agrupamento de armazenamento de coluna). Este nó do SQL Server sem monitoração de estado é operada por recursos de infraestrutura do serviço do Azure que inicializa o processo, controla o estado de funcionamento do nó e efetua a ativação pós-falha para outro lugar, se necessário.
- Uma camada de dados com monitoração de estado com ficheiros de base de dados (.mdf/.ldf) que são armazenados em discos de armazenamento Premium do Azure. O armazenamento do Azure garante que haverá sem perda de dados de qualquer registo que é colocado em qualquer arquivo de banco de dados. O armazenamento do Azure tem disponibilidade/redundância de dados incorporada que garante que todos os registos no ficheiro de registo ou a página no arquivo de dados será preservada mesmo no caso de falha de processo do SQL Server.

Sempre que o motor de base de dados ou o sistema operativo é atualizado ou se detetar algum problema crítico no processo do Sql Server, o Azure Service Fabric moverá o processo do SQL Server sem monitoração de estado para outro nó de computação sem monitoração de estado. Dados na camada de armazenamento do Azure não são afetados e arquivos de dados/do registo são anexados ao processo do SQL Server recentemente inicializado. Tempo de ativação pós-falha esperada pode ser medido em segundos. Este processo garante 99,99% de disponibilidade, mas ele pode ter alguns impactos no desempenho na carga de trabalho pesada que estejam a executar devido ao tempo de transição e o fato do novo nó do SQL Server é iniciado com a cache de frio.

## <a name="premium-availability"></a>Disponibilidade de Premium

Disponibilidade de Premium está ativada no escalão Premium da base de dados do Azure SQL e foi concebido para cargas de trabalho intensivas que não toleram nenhum impacto no desempenho devido a operações de manutenção contínua.

No modelo de premium, base de dados SQL do Azure integra-se de computação e armazenamento no nó único. O processo de motor de base de dados do SQL Server e subjacentes arquivos mdf/ldf são colocados no mesmo nó, com armazenamento SSD localmente ao fornecimento de baixa latência para a sua carga de trabalho.

Elevada disponibilidade é implementada com o padrão [grupos de Disponibilidade AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Cada base de dados é um cluster de nós de base de dados com uma base de dados principal que está acessível para a carga de trabalho do cliente e alguns processos secundários que contêm cópias dos dados. O nó principal constantemente envia as alterações para nós secundários para garantir que os dados estão disponíveis em réplicas secundárias, no caso de falha de nó primário por qualquer motivo. Ativação pós-falha é processada pelo motor de base de dados do SQL Server – uma réplica secundária torna-se o nó principal e uma nova réplica secundária é criada para garantir que o suficiente nós do cluster. A carga de trabalho é automaticamente redirecionada para o novo nó primário. Tempo de ativação pós-falha é medido em milissegundos e a instância primária nova é imediatamente pronta para continuar a servir pedidos.

## <a name="zone-redundant-configuration"></a>Configuração com redundância de zona

Por predefinição, as réplicas de conjunto de quórum para configurações de armazenamento local são criadas no mesmo datacenter. Com a introdução da [zonas de disponibilidade do Azure](../availability-zones/az-overview.md), tem a capacidade de colocar as réplicas diferentes em conjuntos de quórum para zonas de disponibilidade diferente na mesma região. Para eliminar um ponto único de falha, a cadência de controle também está duplicada em várias zonas como três anéis de gateway (GW). O encaminhamento para um anel de gateway específico é controlado pelas [Gestor de tráfego do Azure](../traffic-manager/traffic-manager-overview.md) (ATM). Uma vez que a configuração com redundância de zona não cria a redundância de base de dados adicionais, a utilização das zonas de disponibilidade nos escalões de serviço Premium ou críticas para a empresa está disponível nenhuma adicionais custo. Ao selecionar uma base de dados com redundância de zona, pode fazer suas bases de dados Premium ou críticas para a empresa resiliente para um conjunto muito maior de falhas, incluindo falhas de datacenter catastróficos, sem quaisquer alterações da lógica do aplicativo. Também pode converter a quaisquer bases de dados Premium ou críticas para a empresa ou conjuntos existentes para a configuração com redundância de zona.

Uma vez que o quórum com redundância de zona-conjunto tem réplicas em datacenters diferentes com alguma distância entre eles, a latência de rede maior pode aumentar o tempo de consolidação e, portanto, afeta o desempenho de algumas cargas de trabalho OLTP. Pode sempre regressar à configuração de zona única, desativando a definição de redundância de zona. Este processo é um tamanho de operação de dados e é semelhante à atualização de objetivo de nível (SLO) do serviço regular. No final do processo, a base de dados ou conjunto é migrado a partir de um anel com redundância de zona para um anel de zona única ou vice-versa.

A versão com redundância de zona da arquitetura de elevada disponibilidade é ilustrada pelo diagrama seguinte:
 
![redundância de zona de arquitetura elevada disponibilidade](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Leia o Escalamento horizontal
Conforme descrito, escalões de serviço Premium e crítico para a empresa tirar partido de conjuntos de quórum e tecnologia Always On para elevada disponibilidade no única zona e configurações com redundância de zona. Uma das vantagens do AlwaysOn é que as réplicas estão sempre no Estado transacionalmente consistente. Uma vez que as réplicas tenham o mesmo nível de desempenho como principal, o aplicativo pode aproveitar essa capacidade extra para a manutenção de cargas de trabalho só de leitura não adicionais Custo (leitura Escalamento horizontal). Desta forma, as consultas só de leitura serão isoladas de carga de trabalho de leitura / escrita principal e não irão afetar o desempenho dele. Leitura destina-se a funcionalidade de escalamento horizontal para as aplicações que incluem logicamente separados só de leitura cargas de trabalho, tais como análise e, portanto, poderia usar essa capacidade adicional sem ligar para o primário. 

Para utilizar a funcionalidade de expansão de leitura com um banco de dados específico, deve explicitamente ativá-lo ao criar a base de dados ou, depois, alterando a respetiva configuração com o PowerShell, invocando o [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) ou o [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlets ou por meio da API de REST do Azure Resource Manager com o [bases de dados - criar ou atualizar](/rest/api/sql/databases/createorupdate) método.

Depois de leitura horizontal é ativada para uma base de dados, ligar a esse banco de dados de aplicações serão direcionadas para a réplica de leitura / escrita ou para uma réplica só de leitura dessa base de dados em conformidade com o `ApplicationIntent` propriedade configurada do aplicativo cadeia de ligação. Para obter informações sobre o `ApplicationIntent` propriedade, veja [especificação de tipo de aplicação](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Se o Escalamento de leitura está desabilitado ou defina a propriedade ReadScale numa camada de serviço não suportado, todas as ligações sejam direcionadas para a réplica de leitura / escrita, independentemente do `ApplicationIntent` propriedade.  

> [!NOTE]
> É possível ativar o Escalamento de leitura num padrão ou uma base de dados para fins gerais, mesmo que não irá resultar no encaminhamento a só de leitura se destina a sessão para uma réplica separada. Isso é feito para oferecer suporte a aplicativos existentes que aumente e diminua entre os escalões fins gerais da/Standard e Premium/comerciais críticas.  

A funcionalidade de expansão de leitura suporta consistência ao nível da sessão. Se a sessão só de leitura fosse ligado depois de um erro de ligação causar por indisponibilidade de réplica, pode ser redirecionado para uma réplica diferente. Embora seja pouco provável, pode resultar no processamento do conjunto de dados que está obsoleto. Da mesma forma, se uma aplicação escreve dados através de uma sessão de leitura / escrita e imediatamente lê-lo com a sessão só de leitura, é possível que os novos dados não estão imediatamente visíveis.

## <a name="conclusion"></a>Conclusão
Base de dados SQL do Azure está profundamente integrado com a plataforma Azure e é altamente dependente no Service Fabric para deteção de falhas e recuperação, em Blobs de armazenamento do Azure para proteção de dados e as zonas de disponibilidade para maior tolerância de falhas. Ao mesmo tempo, a base de dados SQL do Azure aproveita totalmente a tecnologia do grupo de Disponibilidade AlwaysOn do produto de caixa de SQL Server para a replicação e ativação pós-falha. A combinação dessas tecnologias permite que os aplicativos a serem totalmente perceber os benefícios de um modelo de armazenamento misto e suporta os SLAs mais exigentes. 

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [zonas de disponibilidade do Azure](../availability-zones/az-overview.md)
- Saiba mais sobre [do Service Fabric](../service-fabric/service-fabric-overview.md)
- Saiba mais sobre [o Gestor de tráfego do Azure](../traffic-manager/traffic-manager-overview.md) 
