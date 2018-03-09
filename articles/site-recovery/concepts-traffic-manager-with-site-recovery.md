---
title: Traffic Manager do Azure com o Azure Site Recovery | Microsoft Docs
description: "Descreve como utilizar o Gestor de tráfego do Azure com o Azure Site Recovery para a migração e de recuperação após desastre"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: manayar
ms.openlocfilehash: 8305a354588875926cab52a55d99d3a29bcfb509
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="azure-traffic-manager-with-azure-site-recovery"></a>Gestor de Tráfego do Azure com o Azure Site Recovery

Traffic Manager do Azure permite-lhe controlar a distribuição de tráfego entre pontos finais da sua aplicação. Um ponto final é qualquer serviço de acesso à Internet alojada dentro ou fora do Azure.

Gestor de tráfego utiliza o sistema de nomes de domínio (DNS) para direcionar os pedidos de cliente para o ponto final mais adequado, com base no método de encaminhamento de tráfego e o estado de funcionamento dos pontos finais. Gestor de tráfego fornece uma variedade de [métodos de encaminhamento de tráfego](../traffic-manager/traffic-manager-routing-methods.md) e [ponto final de opções de monitorização](../traffic-manager/traffic-manager-monitoring.md) de acordo com as necessidades de aplicações diferentes e modelos de ativação pós-falha automática. Os clientes ligam diretamente para o ponto final selecionado. Gestor de tráfego não é um proxy ou de um gateway e não consegue ver o tráfego de passar entre o cliente e o serviço.

Este artigo descreve como pode combinar do Monitor de tráfego do Azure encaminhamento inteligente com capacidades de migração e de recuperação de desastres poderosa do Azure Site Recovery.

## <a name="on-premises-to-azure-failover"></a>No local para ativação pós-falha do Azure

Para o primeiro cenário, considere **empresa um** que tem todas as respetiva infraestrutura de aplicação em execução no seu ambiente no local. Por motivos de continuidade e compatibilidade de negócio, **empresa um** decide utilizar o Azure Site Recovery para proteger as suas aplicações.

**A empresa A** está a executar aplicações com pontos finais públicos e pretender que a capacidade de forma totalmente integrada a redirecionar o tráfego para o Azure um caso de desastre. O [prioridade](../traffic-manager/traffic-manager-configure-priority-routing-method.md) método de encaminhamento de tráfego no Gestor de tráfego do Azure permite da empresa para implementar facilmente este padrão de ativação pós-falha.

O programa de configuração é o seguinte:
- **A empresa A** cria um [perfil do Traffic Manager](../traffic-manager/traffic-manager-create-profile.md).
- Utilizar o **prioridade** método de encaminhamento, **empresa um** cria dois pontos finais – **primário** para no local e **ativação pós-falha** para o Azure. **Primário** é atribuída prioridade 1 e **ativação pós-falha** é atribuída prioridade 2.
- Uma vez que o **primário** ponto final está alojado fora do Azure, o ponto final será criado como um [externo](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) ponto final.
- Com o Azure Site Recovery, o site do Azure não tem quaisquer máquinas virtuais ou aplicações em execução antes da ativação pós-falha. Por isso, o **ativação pós-falha** endpoint também é criado como um **externo** ponto final.
- Por predefinição, o tráfego do utilizador é direcionado para a aplicação no local porque esse ponto final com a prioridade mais alta associada à mesma. Nenhum tráfego é direcionado para o Azure se o **primário** ponto final está em bom estado.

![No local-para o Azure antes de ativação pós-falha](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-before.png)

Um caso de desastre, pode acionar a empresa A um [ativação pós-falha](site-recovery-failover.md) para o Azure e recuperar as respetivas aplicações no Azure. Quando o Gestor de tráfego do Azure Deteta que o **primário** ponto final já não está em bom estado, utiliza automaticamente o **ativação pós-falha** ponto final na resposta DNS e os utilizadores ligar à aplicação recuperada no Azure.

![No local-para o Azure após a ativação pós-falha](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-after.png)

Dependendo dos requisitos de negócio, **empresa um** pode escolher um superior ou inferior [pesquisar frequência](../traffic-manager/traffic-manager-monitoring.md) para alternar entre no local para Azure um caso de desastre e certifique-se o período de indisponibilidade mínimo para os utilizadores.

Quando está contido desastre, **empresa um** pode reativação pós-falha a partir do Azure ao seu ambiente no local ([VMware](vmware-azure-failback.md) ou [Hyper-V](hyper-v-azure-failback.md)) utilizando o Azure Site Recovery. Agora, ao Gestor de tráfego Deteta que o **primário** ponto final está novamente em bom estado, utiliza automaticamente o **primário** ponto final no respetivos respostas DNS.

## <a name="on-premises-to-azure-migration"></a>No local para a migração do Azure

Para além de recuperação após desastre do Azure Site Recovery também lhe permite [migrações para o Azure](migrate-overview.md). Utilizar as capacidades de ativação pós-falha do Azure Site Recovery teste poderosa, os clientes podem avaliar o desempenho da aplicação no Azure sem afetar o seu ambiente no local. E, quando estiverem prontos para migrar os clientes, pode escolher a migrar cargas de trabalho completos em conjunto ou optar por migrar e dimensionar gradualmente.

Traffic Manager do Azure do [Weighted](../traffic-manager/traffic-manager-configure-weighted-routing-method.md) método de encaminhamento pode ser utilizado para direcionar algumas partes do tráfego de entrada para o Azure ao instruir a maioria para o ambiente no local. Esta abordagem pode ajudá-lo a avaliar o desempenho do dimensionamento pode continuar a aumentar a ponderação atribuída para o Azure como migrar mais e muito mais das cargas de trabalho para o Azure.

Por exemplo, **empresa B** optar por migrar em fases, mover algumas das respetivos ambiente da aplicação, mantendo a rest no local. Durante as fases iniciais quando a maioria do ambiente no local, uma maior ponderação é atribuída para o ambiente no local. Gestor de tráfego devolve um ponto final com base em ponderações atribuídas aos pontos finais disponíveis.

![Migração no local-para o Azure](./media/concepts-traffic-manager-with-site-recovery/on-premises-migration.png)

Durante a migração, os dois pontos finais são Active Directory e a maioria do tráfego é direcionado para o ambiente no local. Como as prossegue a migração, pode ser atribuída uma maior ponderação para o ponto final no Azure e, finalmente, o ponto final no local pode ser desativada após migração.

## <a name="azure-to-azure-failover"></a>Ativação pós-falha do Azure para o Azure

Neste exemplo, considere **empresa C** que tem todas as respetiva infraestrutura de aplicação com o Azure. Por motivos de continuidade e compatibilidade de negócio, **empresa C** decide utilizar o Azure Site Recovery para proteger as suas aplicações.

**Empresa C** está a executar aplicações com pontos finais públicos e pretender que a capacidade de forma totalmente integrada redirecionar o tráfego para uma região do Azure diferente um caso de desastre. O [prioridade](../traffic-manager/traffic-manager-configure-priority-routing-method.md) método de encaminhamento de tráfego permite **empresa C** facilmente implementar este padrão de ativação pós-falha.

O programa de configuração é o seguinte:
- **Empresa C** cria um [perfil do Traffic Manager](../traffic-manager/traffic-manager-create-profile.md).
- Utilizar o **prioridade** método de encaminhamento, **empresa C** cria dois pontos finais – **primário** para a região de origem (Azure Oriental) e **ativação pós-falha** para a região de recuperação (Azure Sudeste asiático). **Primário** é atribuída prioridade 1 e **ativação pós-falha** é atribuída prioridade 2.
- Uma vez que o **primário** ponto final está alojado no Azure, o ponto final pode ser como uma [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) ponto final.
- Com o Azure Site Recovery, a recuperação de sites do Azure não tem quaisquer máquinas virtuais ou aplicações em execução antes da ativação pós-falha. Por isso, o **ativação pós-falha** ponto final pode ser criado como um [externo](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) ponto final.
- Por predefinição, o tráfego do utilizador é direcionado para a aplicação de região (Oriental) de origem como a prioridade mais alta associada tem a esse ponto final. Nenhum tráfego é direcionado para a região de recuperação se o **primário** ponto final está em bom estado.

![Azure para o Azure antes de ativação pós-falha](./media/concepts-traffic-manager-with-site-recovery/azure-failover-before.png)

Um caso de desastre, **empresa C** pode acionar um [ativação pós-falha](azure-to-azure-tutorial-failover-failback.md) e recuperar as respetivas aplicações na recuperação região do Azure. Quando o Gestor de tráfego do Azure Deteta que o ponto final principal já não está em bom estado, utiliza automaticamente o **ativação pós-falha** ponto final na resposta DNS e os utilizadores ligar à aplicação recuperada na região do Azure (de recuperação Sudeste asiático).

![Azure para o Azure após a ativação pós-falha](./media/concepts-traffic-manager-with-site-recovery/azure-failover-after.png)

Dependendo dos requisitos de negócio, **empresa C** pode escolher um superior ou inferior [pesquisar frequência](../traffic-manager/traffic-manager-monitoring.md) para alternar entre regiões de origem e de recuperação e certifique-se de período de indisponibilidade mínimo para os utilizadores.

Quando está contido desastre, **empresa C** pode reativação pós-falha da recuperação região do Azure para a origem de região do Azure utilizando o Azure Site Recovery. Agora, ao Gestor de tráfego Deteta que o **primário** ponto final está novamente em bom estado, utiliza automaticamente o **primário** ponto final no respetivos respostas DNS.

## <a name="protecting-multi-region-enterprise-applications"></a>Proteger aplicações da empresa de multirregião

As empresas global, muitas vezes, melhorar a experiência do cliente por tailoring às suas aplicações para servir necessidades regionais. Localização e redução de latência podem levar a infraestrutura de aplicação dividida em regiões. As empresas também estão vinculados por leis de dados regionais em determinados áreas e optar por isolar parte respetiva infraestrutura de aplicação dentro de limites regionais.  

Vejamos um exemplo em que **a empresa** tem dividir os respetivos pontos finais de aplicação para servir separadamente Datacenters e o resto do mundo. **Empresa D** utiliza o Azure Traffic Manager [Geographic](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) método de encaminhamento para configurar esta opção. Qualquer tráfego com origem na Alemanha é direcionado para **Endpoint 1** e qualquer tráfego com origem fora Datacenters é direcionado para **ponto final 2**.

O problema com este programa de configuração é que, se **Endpoint 1** deixa de funcionar por qualquer motivo, não há nenhum redirecionamento de tráfego para **ponto final 2**. Tráfego com origem na Alemanha continua a ser direcionado para **Endpoint 1** independentemente do Estado de funcionamento do ponto final, deixando alemão utilizadores sem acesso aos **a empresa**da aplicação. Da mesma forma, se **ponto final 2** ficar offline, não há nenhum redirecionamento de tráfego para **Endpoint 1**.

![Aplicação de multirregião antes](./media/concepts-traffic-manager-with-site-recovery/geographic-application-before.png)

Para evitar a execução deste problema e certifique-se de resiliência de aplicação, **a empresa** utiliza [aninhada perfis do Traffic Manager](../traffic-manager/traffic-manager-nested-profiles.md) com o Azure Site Recovery. Na configuração de perfis aninhados, o tráfego não é direcionado para pontos finais individuais, mas em vez disso, para outros perfis do Traffic Manager. Eis como funciona esta configuração:
- Em vez de utilizar o encaminhamento geográfica com pontos finais individuais, **a empresa** utiliza encaminhamento geográfica com perfis do Traffic Manager.
- Cada elemento subordinado perfil do Traffic Manager utiliza **prioridade** encaminhamento com um site primário e um ponto final da recuperação, por conseguinte, aninhamento **prioridade** encaminhamento dentro **Geographic** encaminhamento.
- Para ativar a resiliência da aplicação, cada distribuição de carga de trabalho utiliza o Azure Site Recovery para a ativação pós-falha para uma região de recuperação com base em caso de um evento de desastre.
- Quando o elemento principal do Traffic Manager recebe uma consulta DNS, é direcionado para o Gestor de tráfego que responda à consulta com um ponto de final disponível de subordinado relevante.

![Aplicação de multirregião depois](./media/concepts-traffic-manager-with-site-recovery/geographic-application-after.png)

Por exemplo, se o ponto final na Alemanha Central falhar, a aplicação rapidamente pode ser recuperada para Nordeste da Alemanha. O novo ponto final processa o tráfego proveniente de Datacenters com período de indisponibilidade mínimo para os utilizadores. Da mesma forma uma falha de ponto final na Europa Ocidental pode ser processada por recuperar a aplicação carga de trabalho Europa do Norte, com o processamento do Traffic Manager do Azure que DNS redireciona para o ponto final disponível.

O programa de configuração acima pode ser expandido até incluem combinações de região e o ponto final tantos necessárias. Gestor de tráfego permite até 10 níveis de perfis aninhados e não permite ciclos dentro da configuração aninhada.

## <a name="recovery-time-objective-rto-considerations"></a>Considerações de objetivo de tempo (RTO) de recuperação

Na maioria das organizações, adicionar ou modificar registos DNS é processado por uma equipa separada ou por alguém fora da organização. Isto torna a tarefa de alteração de registos DNS muito difícil. O tempo necessário para atualizar os registos DNS por outras equipas ou organizações gerir a infraestrutura DNS varia da organização para organização e afeta o RTO da aplicação.

Utilizando o Gestor de tráfego, pode frontload o trabalho necessário para as atualizações de DNS. Não é necessária nenhuma ação manual ou script no momento da ativação pós-falha real. Esta abordagem ajuda a mudança rápida (e, por conseguinte, diminuindo RTO), bem como evitando dispendiosos erros de alteração DNS morosa um caso de desastre. Com o Gestor de tráfego, mesmo o passo de reativação pós-falha é um processo automatizado, caso contrário, que teria de ser gerido separadamente.

Definir o correto [pesquisar intervalo](../traffic-manager/traffic-manager-monitoring.md) através do Estado de funcionamento do intervalo básico ou rápida verificações podem consideravelmente trazer baixo o RTO durante a ativação pós-falha e reduzir o período de indisponibilidade para os utilizadores.

Além disso pode otimizar o tempo de DNS para o valor em direto (TTL) para o perfil do Traffic Manager. TTL é o valor para o qual uma entrada DNS poderia ser colocadas em cache por um cliente. Para um registo DNS não seria possível consultar duas vezes dentro do intervalo de valor de TTL. Cada registo DNS tem um valor de TTL associado à mesma. Reduzir este valor resulta em mais consultas DNS para o Gestor de tráfego, mas pode reduzir o RTO ao detetar falhas mais rapidamente.

O valor de TTL teve pelo cliente também não aumenta a se aumentar o número de resoluções DNS entre o cliente e servidor DNS autoritativo. As resoluções DNS 'contagem ' o valor de TTL e passaram apenas um valor de TTL que reflete o tempo decorrido desde que o registo foi colocado em cache. Isto garante que o registo DNS obtém atualizar o cliente após o TTL, independentemente do número de resoluções DNS na cadeia.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre o Gestor de tráfego [métodos de encaminhamento](../traffic-manager/traffic-manager-routing-methods.md).
- Saiba mais sobre [aninhada perfis do Traffic Manager](../traffic-manager/traffic-manager-nested-profiles.md).
- Saiba mais sobre [monitorização de pontos finais](../traffic-manager/traffic-manager-monitoring.md).
- Saiba mais sobre [planos de recuperação](site-recovery-create-recovery-plans.md) para automatizar a ativação pós-falha de aplicação.
