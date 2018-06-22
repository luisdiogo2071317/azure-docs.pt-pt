---
title: Elevada disponibilidade - serviço do SQL Database do Azure | Microsoft Docs
description: Saiba mais sobre as funcionalidades e capacidades de elevada disponibilidade do serviço SQL Database do Azure
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: jovanpop
ms.reviewer: carlrab, sashan
ms.openlocfilehash: 4e1963e97a7458db8badb63e28dbc3d215ad88b2
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309635"
---
# <a name="high-availability-and-azure-sql-database"></a>Base de dados SQL do Azure e de elevada disponibilidade

Base de dados SQL do Azure é plataforma como um serviço que garante que a base de dados está ativo e em execução 99,99% de tempo, sem ter de se preocupar sobre a manutenção e tempos de inatividade de base de dados altamente disponível. Trata-se de um processo de motor de base de dados do SQL Server completamente gerido alojado na nuvem do Azure que assegura que a base de dados do SQL Server está sempre atualizado/aplicado sem afetar a carga de trabalho. Base de dados SQL do Azure pode recuperar rapidamente mesmo nas circunstâncias mais críticas garantir que os dados estão sempre disponíveis.

Plataforma do Azure totalmente gere cada base de dados do SQL do Azure e garante que nenhuma perda de dados e uma percentagem elevada de disponibilidade de dados. Azure processa automaticamente a aplicação de patches, cópias de segurança, replicação, a deteção de falha, hardware potencial subjacente, falhas de software ou de rede, correções de erros de implementação, as ativações pós-falha, as atualizações da base de dados e outras tarefas de manutenção. Engenheiros de SQL Server tiverem implementado as práticas best-known, garantir que todas as operações de manutenção são concluídas em menos de 0.01% de tempo de vida da base de dados. Esta arquitetura é concebida para garantir que os dados consolidados nunca são perdidos e que as operações de manutenção são efetuadas sem afetar a carga de trabalho. Não são janelas de manutenção ou tempos de inatividade devem solicitar a parar a carga de trabalho, enquanto a base de dados é atualizado ou mantida. Elevada disponibilidade incorporada na SQL Database do Azure garante que a base de dados nunca serão ponto único de falha na sua arquitetura de software.

Existem dois modelos de elevada disponibilidade aplicados em SQL do Azure:

- Modelo de objetivo geral/padrão que fornece 99,99% de disponibilidade, mas com algumas degradação do desempenho potencial decorrer das atividades de manutenção.
- Modelo de crítico de negócio/Premium que fornece também fornece 99,99% de disponibilidade com impacto mínimo na sua carga de trabalho mesmo decorrer das atividades de manutenção.

Azure atualiza e patches transparente subjacente do sistema operativo, controladores e o motor de base de dados do SQL Server com a hora de baixo mínimo para os utilizadores finais. Base de dados SQL do Azure é executado na versão mais recente estável do motor de base de dados do SQL Server e o sistema operativo Windows e a maioria dos utilizadores seria não tenha em atenção que as atualizações são efetuadas continuamente.

## <a name="standard-availability"></a>Disponibilidade padrão

Disponibilidade padrão refere-se ao SLA de 99,99% que é aplicada em camadas de objetivo de Basic/padrão/geral. Disponibilidade é conseguida através da separação de camadas de armazenamento e computação. No modelo de disponibilidade padrão temos de duas camadas:

- Uma camada de computação sem monitorização de estado que está a executar o processo de sqlserver.exe e contém apenas transitórios e em cache dados (por exemplo – plano cache, conjunto de memória intermédia, conjunto de arquivo de colunas). Este nó do SQL Server sem monitorização de estado é operada por Azure Service Fabric que inicia o processo, controla o estado de funcionamento do nó e efetua a ativação pós-falha para outro local, se necessário.
- Uma camada de dados de monitorização de estado com ficheiros de base de dados (.mdf/.ldf) que estão armazenadas nos discos de armazenamento do Azure Premium. Armazenamento do Azure garante que não existirá nenhuma perda de dados de qualquer registo colocado em qualquer ficheiro de base de dados. Armazenamento do Azure tem disponibilidade/redundância de dados incorporada que garante que todos os registos no ficheiro de registo ou a página no ficheiro de dados será mantida, mesmo se a falhas de processos do SQL Server.

Sempre que o motor de base de dados ou o sistema operativo é atualizado ou, se for detetado algum problema crítico no processo do SQL Server, o Azure Service Fabric irá mover o processo de SQL Server sem monitorização de estado para outro nó de computação sem monitorização de estado. Dados na camada de armazenamento do Azure não são afetados e ficheiros de registo de dados/estão ligados ao processo de SQL Server recentemente inicializado. Hora de ativação pós-falha esperada pode ser medida em segundos. Este processo garante 99,99% de disponibilidade, mas poderá ter alguns impactos no desempenho no pesada carga de trabalho que estão em execução devido a hora de transição e inicia o facto do novo nó de SQL Server com a cache de amovíveis.

## <a name="premium-availability"></a>Disponibilidade de Premium

Disponibilidade de Premium está ativada no escalão Premium da SQL Database do Azure e foi concebido para cargas de trabalho que consomem muita que não toleram qualquer impacto no desempenho devido as operações de manutenção em curso.

No modelo de premium, SQL database do Azure integra-se num único nó de armazenamento e computação. O processo de motor de base de dados do SQL Server e subjacente ficheiros mdf/ldf são colocados no mesmo nó com ligado localmente armazenamento SSD, fornecendo a sua carga de trabalho de latência baixa.

Elevada disponibilidade é implementada com o padrão [em grupos de disponibilidade Always](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Cada base de dados é um cluster de nós de base de dados com uma base de dados principal que seja acessível para carga de trabalho do cliente e alguns processos secundários que contenham cópias dos dados. O nó principal pushes constantemente as alterações a nós secundários para garantir que os dados estão disponíveis nas réplicas secundárias, se o nó principal falhar por algum motivo. Ativação pós-falha é processada pelo motor de base de dados do SQL Server – uma réplica secundária torna-se o nó principal e é criada uma nova réplica secundária para garantir que nós suficientes no cluster. A carga de trabalho é automaticamente redirecionada para o novo nó principal. Hora de ativação pós-falha é medida em milissegundos e a nova instância primária é imediatamente pronta para continuar a processar pedidos.

## <a name="zone-redundant-configuration-preview"></a>Configuração redundante de zona (pré-visualização)

Por predefinição, as réplicas de conjunto de quórum para configurações de armazenamento local são criadas no mesmo datacenter. Com a introdução de [zonas de disponibilidade do Azure](../availability-zones/az-overview.md), tem a capacidade de colocar as réplicas diferentes conjuntos de quórum para zonas de disponibilidade diferente na mesma região. Para eliminar um ponto único de falha, o toque de controlo também está duplicado em várias zonas como três anéis de gateway (GW). O encaminhamento para um anel de gateway específico é controlado pelo [Traffic Manager do Azure](../traffic-manager/traffic-manager-overview.md) (ATM). Porque a configuração de redundante de zona não criar redundância da base de dados adicionais, a utilização de zonas de disponibilidade no Premium ou camadas de serviços críticos de negócio (pré-visualização) está disponível em nenhum extra de custo. Ao selecionar uma base de dados redundantes de zona, pode efetuar o Premium ou críticos de negócio (pré-visualização) bases de dados sejam resilientes a um conjunto muito maior de falhas, incluindo falhas catastrófica do Centro de dados, sem quaisquer alterações da lógica da aplicação. Também pode converter qualquer agrupamentos (pré-visualização) ou bases de dados Premium ou crítico de negócio existentes para a configuração de redundante de zona.

Uma vez o redundante quórum-conjunto de zona tem réplicas em datacenters diferentes, com algumas distância entre eles, a latência de rede de maior pode aumentar o tempo de consolidação e assim afeta o desempenho de algumas cargas de trabalho OLTP. Pode sempre regressar para a configuração de zona único, desativando a definição de redundância de zona. Este processo tem um tamanho de operação de dados e é semelhante para a atualização do serviço regular objetivo de nível (SLO). No final do processo, a base de dados ou o conjunto é migrado de uma zona redundante em anel para um anel de zona único ou vice-versa.

> [!IMPORTANT]
> Conjuntos elásticos e bases de dados redundantes do zona são atualmente suportados apenas da camada de serviço Premium. Durante a pré-visualização pública, as cópias de segurança, auditoria e registos são armazenados no armazenamento RA-GRS e, por conseguinte, não podem ser automaticamente disponíveis em caso de uma falha de toda a zona. 

A versão de redundante da zona da arquitetura de elevada disponibilidade é ilustrada o diagrama a seguir:
 
![zona de arquitetura de elevada disponibilidade redundante](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Leitura de escalamento horizontal
Tal como descrito, Premium e crítico de negócio (pré-visualização) tire partido de camadas quórum-conjuntos e serviço tecnologia Always On para elevada disponibilidade nas única zona e configurações de zona redundante. Uma das vantagens de AlwasyON é que as réplicas sempre estão no estado de uma forma consistente. Porque as réplicas tenham o mesmo nível de desempenho como principais, a aplicação pode tirar partido dessa capacidade extra para a manutenção as cargas de trabalho só de leitura em nenhum extra Custo (leitura Escalamento horizontal). Desta forma, as consultas só de leitura serão isoladas da carga de trabalho de leitura e escrita principal e não irão afetar o desempenho dele. Leitura destina-se a funcionalidade de escalamento horizontal para aplicações que incluem logicamente separados só de leitura cargas de trabalho, tais como a análise e, por conseguinte, foi possível tirar partido desta capacidade adicional sem ligar para o site primário. 

Para utilizar a funcionalidade de ampliação de leitura com uma base de dados específica, que tem explicitamente a ativa ao criar a base de dados ou posteriormente alterando a sua configuração através do PowerShell invocando o [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) ou o [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlets ou através da API de REST do Azure Resource Manager utilizando o [bases de dados - criar ou atualizar](/rest/api/sql/databases/createorupdate) método.

Depois de leitura Escalamento horizontal é ativado para uma base de dados, a ligação para a base de dados de aplicações serão direcionadas para a réplica de leitura e escrita ou para uma réplica só de leitura de acordo com a base de dados a `ApplicationIntent` propriedade configurada da aplicação cadeia de ligação. Para obter informações sobre o `ApplicationIntent` propriedade, consulte [especificar tipo de aplicação](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Se leitura Escalamento horizontal está desativado ou defina a propriedade de ReadScale uma camada de serviço não suportado, todas as ligações são direcionadas para a réplica de leitura e escrita, independentemente do `ApplicationIntent` propriedade.  

> [!NOTE]
> É possível ativar a leitura de dimensionamento horizontal em padrão ou uma base de dados de objetivo geral, apesar de não irá resultar no encaminhamento a só de leitura se destina a sessão para uma réplica separada. Isto é feito para suportar as aplicações existentes que aumentar vertical ou entre camadas de objetivo geral/Standard e Premium/negócio crítico.  

A funcionalidade de ampliação leitura suporta consistência ao nível da sessão. Se a sessão só de leitura fosse ligado depois do fazer com que a um erro de ligação pela indisponibilidade de réplica, podem ser redirecionado para uma réplica diferentes. Embora seja pouco provável, pode resultar no processamento do conjunto de dados que está obsoleto. Da mesma forma, se uma aplicação escreve dados através de uma sessão de leitura / escrita e imediatamente lê-lo utilizando a sessão só de leitura, é possível que os novos dados não são imediatamente visíveis.

## <a name="conclusion"></a>Conclusão
Base de dados SQL do Azure está profundamente integrado com a plataforma do Azure e altamente depende de Service Fabric para a deteção de falha e recuperação, em Blobs Storage do Azure para proteção de dados e zonas de disponibilidade para tolerância a falhas superior. Ao mesmo tempo, o SQL database do Azure tira total partido da tecnologia sempre no grupo de disponibilidade de produto do SQL Server caixa para replicação e ativação pós-falha. A combinação destas tecnologias permite que as aplicações totalmente as vantagens de um modelo de armazenamento misto e suporta os SLAs mais demanding. 

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [zonas de disponibilidade do Azure](../availability-zones/az-overview.md)
- Saiba mais sobre [Service Fabric](../service-fabric/service-fabric-overview.md)
- Saiba mais sobre [Traffic Manager do Azure](../traffic-manager/traffic-manager-overview.md) 
