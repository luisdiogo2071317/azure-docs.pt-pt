---
title: Elevada disponibilidade - serviço do SQL Database do Azure | Microsoft Docs
description: Saiba mais sobre as funcionalidades e capacidades de elevada disponibilidade do serviço SQL Database do Azure
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.topic: article
ms.date: 04/24/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 839cadffc37a1c4a6ceae77fbe1e01020c28fe1d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="high-availability-and-azure-sql-database"></a>Base de dados SQL do Azure e de elevada disponibilidade
Desde inception a oferta Azure SQL da base de dados PaaS, Microsoft fez promessa aos seus clientes que o serviço está incorporada no elevada disponibilidade (ed) e os clientes não são necessários para funcionar, adicione lógica especial para ou tomar decisões em torno HA. A Microsoft tem controlo total sobre a configuração do sistema HA e a operação, oferecendo aos clientes um SLA. O SLA HA aplica-se a uma base de dados do SQL Server numa região e não proporciona proteção em caso de uma falha de região total é decorrentes de factores fora do controlo razoável da Microsoft (por exemplo, desastre natural war, atos terrorismo, riots, ação government, ou um rede falha ou de dispositivo externa aos datacenters da Microsoft, incluindo nos sites de cliente ou entre sites de cliente e o Centro de dados da Microsoft).

Para simplificar o espaço de problema do HA, a Microsoft utiliza os seguintes pressupostos:
1.  Falhas de hardware e software são inevitável
2.  Os funcionários operacionais fizer erros que levar a falhas
3.  Operações de manutenção planeadas causam falhas 

Enquanto que esses eventos individuais são pouco frequentes à escala da nuvem, ocorrem todas as semanas se não for todos os dias. 

## <a name="fault-tolerant-sql-databases"></a>Bases de dados SQL com tolerância a falhas
Os clientes são mais interessados na resiliência das suas próprias bases de dados e são menos interessados na resiliência do serviço base de dados do SQL Server como um todo. 99,99% de disponibilidade para um serviço é sem significado se "a minha base de dados" faz parte da % 0.01 das bases de dados que estão em baixo. Base de dados de todas as tem de ser com tolerância a falhas e mitigação de falhas nunca deve resultar na perda de uma transação consolidada. 

Para dados de base de dados do SQL Server utiliza o armazenamento local (LS) com base em direto de discos anexado/VHDs e o armazenamento remoto (RS) com base em blobs de páginas de Premium Storage do Azure. 
- Armazenamento local é utilizado no Premium ou o missão bases de dados críticos de negócio (pré-visualização) e conjuntos elásticos, que foram concebidos para crítico OLTP aplicações com requisitos de IOPS elevados. 
- Armazenamento remoto é utilizado para escalões de serviço básico e padrão, concebidas para cargas de trabalho do orçamento orientado para empresas que necessitam de armazenamento e computação de energia para dimensionamento independente. Utilizam um blob de página única para ficheiros de registo e base de dados e mecanismos de replicação e ativação pós-falha de armazenamento incorporadas.

Em ambos os casos, a replicação, a deteção de falha e mecanismos de ativação pós-falha da base de dados do SQL Server são totalmente automatizados operam sem qualquer intervenção humana. Esta arquitetura concebida para garantir que os dados consolidados nunca são perdidos e que durabilidade de dados tem precedência sobre todas as pessoa.

Principais vantagens:
- Os clientes obter os benefícios das bases de dados replicadas sem ter de configurar ou manter complicada hardware, software, os sistemas operativos ou ambientes de virtualização.
- Propriedades completa ACID das bases de dados relacionais são mantidas pelo sistema.
- As ativações pós-falha são totalmente automatizadas sem perda de quaisquer dados consolidadas.
- Encaminhamento de ligações para a réplica primária dinamicamente é gerido pelo serviço com nenhuma lógica da aplicação necessária.
- O elevado nível de redundância automática é fornecido, sem encargos adicionais.

> [!NOTE]
> A arquitetura de elevada disponibilidade descrito está sujeita a alterações sem aviso prévio. 

## <a name="data-redundancy"></a>Redundância de dados

A solução de elevada disponibilidade na base de dados SQL baseia-se no [grupos de disponibilidade Always ON](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) tecnologia do SQL Server e torna a trabalhar para bases de dados LS e RS com diferenças mínimas. Configuração de LS, a tecnologia de grupo de disponibilidade Always ON é utilizada para a persistência enquanto em RS é utilizado para disponibilidade (RTO baixa por georreplicação ativa). 

## <a name="local-storage-configuration"></a>Configuração de armazenamento local

Nesta configuração, cada base de dados seja colocado online pelo serviço de gestão (MS) dentro da coroa do controlo. Uma réplica primária e, pelo menos, duas réplicas secundárias (conjunto de quórum) estão localizadas dentro de um anel de inquilino que abranja três subsistemas físicos independentes dentro do mesmo centro de dados. Todas as leituras e escritas são enviadas pelo gateway (GW) para a réplica primária e as escritas no modo assíncrono são replicadas para réplicas secundárias. Base de dados do SQL Server utiliza um esquema de consolidação com base em quórum onde os dados são escritos para o site primário e pelo menos uma réplica secundária antes de consolidações de transações.

O [Service Fabric](../service-fabric/service-fabric-overview.md) sistema de ativação pós-falha automaticamente Reconstrói réplicas como nós de falham e mantém a associação ao conjunto de quórum como nós depart e associar o sistema. Manutenção planeada é cuidadosamente coordenada para impedir que o conjunto de quórum atinja para baixo uma contagem mínima de réplica (normalmente, 2). Este modelo funciona bem para Premium e críticos de negócio bases de dados (pré-visualização), mas necessita de redundância de componentes de armazenamento e computação e resulta num custo mais elevado.

## <a name="remote-storage-configuration"></a>Configuração de armazenamento remoto

Para configurações de armazenamento remoto (escalões básico e padrão), é mantida exatamente uma cópia no armazenamento de BLOBs remoto, utilizando as capacidades de sistemas de armazenamento de durabilidade, redundância e deteção rot bits. 

A arquitetura de elevada disponibilidade é ilustrada diagrama a seguir:
 
![arquitetura de elevada disponibilidade](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection-and-recovery"></a>A deteção de falha e de recuperação 
Um sistema distribuído em grande escala necessita de um sistema de deteção de falha de elevada disponibilidade fiável pode detetar falhas de forma fiável, rápida e mais próximo possível ao cliente. Para a base de dados do SQL Server, este sistema é baseado no Azure Service Fabric. 

Com a réplica primária, é evidente imediatamente se e quando a réplica primária falhou e trabalho não pode continuar porque todas as leituras e escritas ocorrer na réplica primária primeiro. Este processo de promover uma réplica secundária para o estado de principal tem o objetivo de tempo de recuperação (RTO) = 30 segundos e o objetivo de ponto de recuperação (RPO) = 0. Para mitigar o impacto do RTO de 30 segundos, a melhor prática é tentar voltar a ligar várias vezes com um menor tempo de espera para tentativas de falha de ligação.

Quando uma réplica secundária falha, a base de dados é para um quórum-conjunto mínimo, com nenhuma spares. Recursos de infraestrutura de serviço inicia o processo de reconfiguração semelhante ao processo de que segue a falha da réplica primária, por isso, após uma espera curta para determinar se a falha é permanente, réplica secundária de outra é criada. Em casos do Estado de out-of-service temporário, como uma falha de sistema operativo ou uma atualização, uma nova réplica não está incorporada imediatamente para permitir que o nó com falhas em vez disso, a reiniciar. 

Para as configurações de armazenamento remoto, a base de dados do SQL Server utiliza a funcionalidade Always ON para bases de dados de ativação pós-falha durante as atualizações. Para tal, uma nova instância do SQL Server é puserem antecipadamente como parte do evento atualização planeado e anexa e recupera o ficheiro de base de dados do armazenamento remoto. Em caso de falhas de processo ou outros eventos não planeados, o Windows Fabric gere a disponibilidade de instância e, como último passo de recuperação, anexa o ficheiro de base de dados remota.

## <a name="zone-redundant-configuration-preview"></a>Configuração redundante de zona (pré-visualização)

Por predefinição, as réplicas de conjunto de quórum para configurações de armazenamento local são criadas no mesmo datacenter. Com a introdução de [zonas de disponibilidade do Azure](../availability-zones/az-overview.md), tem a capacidade de colocar as réplicas diferentes conjuntos de quórum para zonas de disponibilidade diferente na mesma região. Para eliminar um ponto único de falha, o toque de controlo também está duplicado em várias zonas como três anéis de gateway (GW). O encaminhamento para um anel de gateway específico é controlado pelo [Traffic Manager do Azure](../traffic-manager/traffic-manager-overview.md) (ATM). Porque a configuração de redundante de zona não criar redundância da base de dados adicionais, a utilização de zonas de disponibilidade no Premium ou camadas de serviços críticos de negócio (pré-visualização) está disponível em nenhum extra de custo. Ao selecionar uma base de dados redundantes de zona, pode efetuar o Premium ou críticos de negócio (pré-visualização) bases de dados sejam resilientes a um conjunto muito maior de falhas, incluindo falhas catastrófica do Centro de dados, sem quaisquer alterações da lógica da aplicação. Também pode converter qualquer agrupamentos (pré-visualização) ou bases de dados Premium ou crítico de negócio existentes para a configuração de redundante de zona.

Uma vez o redundante quórum-conjunto de zona tem réplicas em datacenters diferentes, com algumas distância entre eles, a latência de rede de maior pode aumentar o tempo de consolidação e assim afeta o desempenho de algumas cargas de trabalho OLTP. Pode sempre regressar para a configuração de zona único, desativando a definição de redundância de zona. Este processo tem um tamanho de operação de dados e é semelhante para a atualização do serviço regular objetivo de nível (SLO). No final do processo, a base de dados ou o conjunto é migrado de uma zona redundante em anel para um anel de zona único ou vice-versa.

> [!IMPORTANT]
> Conjuntos elásticos e bases de dados redundantes do zona só são suportados em críticos de negócio (pré-visualização) e de Premium escalões de serviço. Durante a pré-visualização pública, as cópias de segurança, auditoria e registos são armazenados no armazenamento RA-GRS e, por conseguinte, não podem ser automaticamente disponíveis em caso de uma falha de toda a zona. 

A versão de redundante da zona da arquitetura de elevada disponibilidade é ilustrada o diagrama a seguir:
 
![zona de arquitetura de elevada disponibilidade redundante](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="read-scale-out"></a>Leitura de escalamento horizontal
Tal como descrito, Premium e críticos de negócio (pré-visualização) do serviço tire partido de camadas quórum-conjuntos e tecnologia de AlwaysON para disponibilidade elevada nas única zona e configurações de zona redundante. Uma das vantagens de AlwasyON é que as réplicas sempre estão no estado de uma forma consistente. Porque as réplicas tenham o mesmo nível de desempenho como principais, a aplicação pode tirar partido dessa capacidade extra para a manutenção as cargas de trabalho só de leitura em nenhum extra Custo (leitura Escalamento horizontal). Desta forma, as consultas só de leitura serão isoladas da carga de trabalho de leitura e escrita principal e não irão afetar o desempenho dele. Leitura destina-se a funcionalidade de escalamento horizontal para aplicações que incluem logicamente separados só de leitura cargas de trabalho, tais como a análise e, por conseguinte, foi possível tirar partido desta capacidade adicional sem ligar para o site primário. 

Para utilizar a funcionalidade de ampliação de leitura com uma base de dados específica, que tem explicitamente a ativa ao criar a base de dados ou posteriormente alterando a sua configuração através do PowerShell invocando o [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) ou o [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlets ou através da API de REST do Azure Resource Manager utilizando o [bases de dados - criar ou atualizar](/rest/api/sql/databases/createorupdate) método.

Depois de leitura Escalamento horizontal é ativado para uma base de dados, a ligação para a base de dados de aplicações serão direcionadas para a réplica de leitura e escrita ou para uma réplica só de leitura de acordo com a base de dados a `ApplicationIntent` propriedade configurada da aplicação cadeia de ligação. Para obter informações sobre o `ApplicationIntent` propriedade, consulte [especificar tipo de aplicação](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent). 

Se leitura Escalamento horizontal está desativado ou defina a propriedade de ReadScale uma camada de serviço não suportado, todas as ligações são direcionadas para a réplica de leitura e escrita, independentemente do `ApplicationIntent` propriedade.  

> [!NOTE]
> É possível ativar a leitura de dimensionamento horizontal em padrão ou uma base de dados de objetivo geral, apesar de não irá resultar no encaminhamento a só de leitura se destina a sessão para uma réplica separada. Isto é feito para suportar as aplicações existentes que aumentar vertical ou entre camadas de objetivo geral/Standard e Premium/negócio crítico.  

A funcionalidade de ampliação leitura suporta consistência ao nível da sessão. Se a sessão só de leitura fosse ligado depois do fazer com que a um erro de ligação pela indisponibilidade de réplica, podem ser redirecionado para uma réplica diferentes. Embora seja pouco provável, pode resultar no processamento do conjunto de dados que está obsoleto. Da mesma forma, se uma aplicação escreve dados através de uma sessão de leitura / escrita e imediatamente lê-lo utilizando a sessão só de leitura, é possível que os novos dados não são imediatamente visíveis.

## <a name="conclusion"></a>Conclusão
Base de dados SQL do Azure está profundamente integrado com a plataforma do Azure e altamente depende de Service Fabric para a deteção de falha e recuperação, em Blobs Storage do Azure para proteção de dados e zonas de disponibilidade para tolerância a falhas superior. Ao mesmo tempo, SQL database do Azure totalmente tira partido da tecnologia Always On de produto do SQL Server caixa para replicação e ativação pós-falha. A combinação destas tecnologias permite que as aplicações totalmente as vantagens de um modelo de armazenamento misto e suporta os SLAs mais demanding. 

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [zonas de disponibilidade do Azure](../availability-zones/az-overview.md)
- Saiba mais sobre [Service Fabric](../service-fabric/service-fabric-overview.md)
- Saiba mais sobre [Traffic Manager do Azure](../traffic-manager/traffic-manager-overview.md) 
