---
title: "Elevada disponibilidade - serviço do SQL Database do Azure | Microsoft Docs"
description: "Saiba mais sobre as funcionalidades e capacidades de elevada disponibilidade do serviço SQL Database do Azure"
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
ms.assetid: 
ms.service: sql-database
ms.custom: 
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 12/13/2017
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: c0a140c959f14c2e8ceaddad5d323f0900be5d2f
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="high-availability-and-azure-sql-database"></a>Base de dados SQL do Azure e de elevada disponibilidade
Desde inception a oferta Azure SQL da base de dados PaaS, Microsoft fez promessa para os nossos clientes que o serviço está incorporada no elevada disponibilidade (ed) e os clientes estão a não ser necessário para operar, adicione lógica especial para ou tomar decisões em torno HA. A Microsoft oferece um SLA de clientes e manter o controlo total sobre a configuração do sistema HA e a operação. O SLA HA aplica-se a uma base de dados do SQL Server numa região e não proporciona proteção em casos de uma falha total região que são decorrentes de factores fora do nosso controlo razoável (por exemplo, desastre natural, war, age terrorismo, riots, ação government ou uma rede ou Falha ao dispositivo externa aos nossos centros de dados, incluindo no site de cliente ou entre o site do cliente e o nosso centro de dados).

Para simplificar o espaço de problema do HA, a Microsoft utiliza os seguintes pressupostos:
1.  Falhas de hardware e software são inevitável
2.  Os funcionários operacionais fizer erros que levar a falhas
3.  Operações de manutenção planeadas causam falhas 

Enquanto esses eventos individuais são pouco frequentes à escala da nuvem, que todas as semanas se não de todos os dias. 

## <a name="fault-tolerant-sql-databases"></a>Bases de dados SQL com tolerância a falhas
Os clientes são mais interessados na resiliência das suas próprias bases de dados e são menos interessados na resiliência do serviço base de dados do SQL Server como um todo. 99,99% de disponibilidade para um serviço é sem significado se "a minha base de dados" faz parte da % 0.01 das bases de dados que estão em baixo. Base de dados de todas as tem de ser com tolerância a falhas e mitigação de falhas nunca deve resultar na perda de uma transação consolidada. 

Para dados de base de dados do SQL Server utiliza o armazenamento local (LS) com base em direto de discos anexado/VHDs e o armazenamento remoto (RS) com base em blobs de páginas de Premium Storage do Azure. 
- Armazenamento local é utilizado na agrupamentos, o que foram concebidos para OLTP as aplicações com requisitos de IOPS elevados e bases de dados Premium. 
- Armazenamento remoto é utilizado para escalões de serviço básico e padrão, concebidas para pequenas, amovíveis ou grandes bases de dados que necessitam de armazenamento e computação de energia para dimensionamento independente. Utilizam um blob de página única para ficheiros de registo e base de dados e mecanismos de replicação e ativação pós-falha de armazenamento incorporadas.

Em ambos os casos, a replicação, a deteção de falha e mecanismos de ativação pós-falha da base de dados do SQL Server são totalmente automatizados operam sem qualquer intervenção humana. Esta arquitetura concebida para garantir que os dados consolidados nunca são perdidos e que durabilidade de dados tem precedência sobre todas as pessoa.

Principais vantagens:
- Os clientes obter os benefícios das bases de dados replicadas sem ter de configurar ou manter complicada hardware, software, sistema operativo ou ambientes de virtualização.
- Propriedades completa ACID das bases de dados relacionais são mantidas pelo sistema.
- As ativações pós-falha são totalmente automatizadas sem perda de quaisquer dados consolidadas.
- Encaminhamento de ligações para a réplica primária dinamicamente é gerido pelo serviço com nenhuma lógica da aplicação necessária.
- O elevado nível de redundância automática é fornecido, sem encargos adicionais.

> [!NOTE]
> A arquitetura de elevada disponibilidade descrito está sujeita a alterações sem aviso prévio. 

## <a name="data-redundancy"></a>Redundância de dados

A solução de elevada disponibilidade na base de dados SQL baseia-se no [AlwaysON](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) tecnologia do SQL Server e torna a trabalhar para bases de dados LS e RS com diferenças mínimas. Numa configuração de LS, AlwaysON é utilizado para a persistência enquanto estiver no RS é utilizado para disponibilidade (baixa RTO). 

## <a name="local-storage"></a>Armazenamento local

No caso de LS, cada base de dados seja colocado online pelo serviço de gestão (MS) dentro da coroa do controlo. Uma réplica primária e, pelo menos, duas réplicas secundárias (conjunto de quórum) estão localizadas dentro de um anel de inquilino que abranja três subsistemas físicos independentes dentro do mesmo centro de dados. Todas as leituras e escritas são enviadas pelo gateway (GW) para a réplica primária e as escritas no modo assíncrono são replicadas para réplicas secundárias. Base de dados do SQL Server utiliza um esquema de consolidação com base em quórum onde os dados são escritos para o site primário e pelo menos uma réplica secundária antes de consolidações de transações.

O [Service Fabric](/azure/service-fabric/service-fabric-overview.md) sistema de ativação pós-falha automaticamente Reconstrói réplicas como nós de falham e mantém a associação ao conjunto de quórum como nós depart e associar o sistema. Manutenção planeada é cuidadosamente coordenada para impedir que o conjunto de quórum atinja para baixo uma contagem mínima de réplica (normalmente, 2). Este modelo funciona bem para bases de dados Premium, mas necessita de redundância de componentes de armazenamento e computação e resulta num custo mais elevado.

## <a name="remote-storage"></a>Armazenamento remoto

Para configurações de armazenamento remoto (escalões básico e padrão), exatamente uma cópia da base de dados é mantida no armazenamento de BLOBs remoto, tirar partido das capacidades de sistemas de armazenamento de durabilidade, redundância e deteção rot bits. 

A arquitetura de elevada disponibilidade é ilustrada diagrama a seguir:
 
![arquitetura de elevada disponibilidade](./media/sql-database-high-availability/high-availability-architecture.png)

## <a name="failure-detection--recovery"></a>Deteção de falha de & recuperação 
Um sistema distribuído em grande escala necessita de um sistema de deteção de falha de elevada disponibilidade fiável pode detetar falhas de forma fiável, rápida e mais próximo possível ao cliente. Para a base de dados do SQL Server, este sistema é baseado no Azure Service Fabric. 

Com a réplica primária, é evidente imediatamente se e quando a réplica primária falhou e trabalho não pode continuar porque todas as leituras e escritas ocorrer na réplica primária primeiro. Este processo de promover uma réplica secundária para o estado de principal tem o objetivo de tempo de recuperação (RTO) = 30 segundos e o objetivo de ponto de recuperação (RPO) = 0. Para mitigar o impacto do RTO de 30 segundos, a melhor prática é tentar voltar a ligar várias vezes com um menor tempo de espera para tentativas de falha de ligação.

Quando uma réplica secundária falha, a base de dados é para um quórum-conjunto mínimo, com nenhuma spares. Recursos de infraestrutura de serviço inicia o processo de reconfiguração semelhante ao processo de que segue a falha da réplica primária, por isso, após uma espera curta para determinar se a falha é permanente, réplica secundária de outra é criada. Em casos do Estado de out-of-service temporário, como uma falha de sistema operativo ou uma atualização, uma nova réplica não está incorporada imediatamente para permitir que o nó com falhas em vez disso, a reiniciar. 

Para as configurações de armazenamento remoto, a base de dados do SQL Server utiliza a funcionalidade AlwaysON para bases de dados de ativação pós-falha durante as atualizações. Para tal, uma nova instância do SQL Server é puserem antecipadamente como parte do evento atualização planeado e anexa e recupera o ficheiro de base de dados do armazenamento remoto. Em caso de falhas de processo ou outros eventos não planeados, o Windows Fabric gere a disponibilidade de instância e, como último passo de recuperação, anexa o ficheiro de base de dados remota.

## <a name="conclusion"></a>Conclusão
Base de dados SQL do Azure está profundamente integrado com a plataforma do Azure e está altamente dependente no Service Fabric para a deteção de falha e de recuperação e no Blobs Storage do Azure para proteção de dados. Ao mesmo tempo, a base de dados SQL do Azure utiliza a tecnologia de AlwaysOn de produto do SQL Server caixa para replicação e ativação pós-falha. A combinação destas tecnologias permite que as aplicações totalmente as vantagens de um modelo de armazenamento misto e suporta os SLAs mais demanding. 

## <a name="next-steps"></a>Passos seguintes

- Saiba mais sobre [Service Fabric](/azure/service-fabric/service-fabric-overview.md)
- Saiba mais sobre [Traffic Manager do Azure](/traffic-manager/traffic-manager-overview.md) 
