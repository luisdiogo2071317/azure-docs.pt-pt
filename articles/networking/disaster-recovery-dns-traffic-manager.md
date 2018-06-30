---
title: Recuperação após desastre com DNS do Azure e o Gestor de tráfego | Microsoft Docs
description: Descrição geral das soluções de recuperação após desastre com DNS do Azure e o Gestor de tráfego.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/08/2018
ms.author: kumud
ms.openlocfilehash: d608378f9b3ff3179f9e37ef13f88c65a645d018
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112991"
---
# <a name="disaster-recovery-using-azure-dns-and-traffic-manager"></a>Recuperação após desastre com o DNS do Azure e o Gestor de Tráfego

Recuperação após desastre centra-se no recuperar a partir de uma perda grave de funcionalidade da aplicação. Para escolher uma solução de recuperação após desastre, empresariais e tecnológicas proprietários primeiro devem determinar o nível de funcionalidade que é necessária durante um desastre, tais como -, indisponível, parcialmente disponível através de funcionalidade reduzida ou disponibilidade atrasada, ou totalmente disponível.
A maioria dos clientes empresariais são escolher uma arquitetura de multirregião para resiliência contra uma aplicação ou a ativação pós-falha de nível de infraestrutura. Os clientes podem optar várias abordagens no vação para alcançar a ativação pós-falha e a elevada disponibilidade através de arquitetura redundante. Seguem-se algumas das abordagens mais populares:

- **Ativo-passivo com modo de espera amovíveis**: nesta solução de ativação pós-falha, as VMs e outras aplicações que executar no modo de espera região não estão ativas até que for necessário para a ativação pós-falha. No entanto, o ambiente de produção é replicado sob a forma de cópias de segurança, imagens VM ou modelos do Resource Manager, noutra região. Este mecanismo de ativação pós-falha é económico mas demora mais tempo para undertake uma concluir a ativação pós-falha.
 
    ![Ativo/passivo com modo de espera amovíveis](./media/disaster-recovery-dns-traffic-manager/active-passive-with-cold-standby.png)
    
    *Figura - ativo/passivo com a configuração de recuperação de desastre em modo de espera amovíveis*

- **Ativo/passivo com piloto leve**: nesta solução de ativação pós-falha, o ambiente de reserva dinâmica é configurado com uma configuração mínima. O programa de configuração tem apenas os serviços necessários para suportar apenas um mínimo e crítico conjunto de aplicações a executar. Na sua forma nativa, este cenário pode apenas executar funcionalidade mínima, mas pode aumentar verticalmente e gerar serviços adicionais entrem em massa da carga de produção se ocorrer uma ativação pós-falha.
    
    ![Ativo/passivo com leve piloto](./media/disaster-recovery-dns-traffic-manager/active-passive-with-pilot-light.png)
    
    *Figura: Ativo/passivo com a configuração de recuperação de desastre do piloto claro*

- **Ativo/passivo com modo de espera transfira**: nesta solução de ativação pós-falha, a região de reserva dinâmica previamente está preparada e estiver pronta para tirar o carregamento de base, dimensionamento automático está ativado e todas as instâncias estão em execução. Esta solução não é dimensionada para assumir a carga de produção completo, mas está funcional, e todos os serviços estão em execução. Esta solução é uma versão augmented da abordagem leve piloto.
    
    ![Ativo/passivo com modo de espera transfira](./media/disaster-recovery-dns-traffic-manager/active-passive-with-warm-standby.png)
    
    *Figura: Ativo/passivo com a configuração de recuperação de desastre transfira de reserva dinâmica*
    
Para saber mais sobre a ativação pós-falha e a elevada disponibilidade, consulte o artigo [recuperação após desastre para aplicações do Azure](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications).


## <a name="planning-your-disaster-recovery-architecture"></a>Planeamento da arquitetura de recuperação após desastre

Existem dois aspetos técnicos para configurar a sua arquitetura de recuperação após desastre:
-  Utilizar um mecanismo de implementação para replicar instâncias, dados e configurações entre ambientes principais e de reserva. Este tipo de recuperação após desastre pode ser feito nativamente através do Azure-a recuperação de sites através do Microsoft Azure parceiro aplicações/serviços como o Veritas ou NetApp. 
- Desenvolver uma solução para divert tráfego de rede/web do site primário para o site de reserva dinâmica. Este tipo de recuperação após desastre pode ser conseguido através de DNS do Azure, Azure tráfego Manager(DNS) ou balanceadores de carga global de terceiros.

Este artigo é limitado a abordagens através de redirecionamento de tráfego de rede e Web. Para obter instruções configurar o Azure Site Recovery, consulte [documentação do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).
O DNS é um dos mecanismos mais eficientes para divert tráfego de rede porque o DNS é frequentemente global e externos para o Centro de dados e é insulated de eventuais falhas regionais ou a disponibilidade de nível de zona (AZ). Um pode utilizar um mecanismo de ativação pós-falha baseado no DNS e no Azure, dois serviços DNS podem realizar os mesmos, de alguma forma - DNS do Azure (DNS autoritativos) e o Gestor de tráfego do Azure (com base no DNS inteligente o encaminhamento de tráfego). 

É importante compreender alguns conceitos no DNS que são amplamente utilizado para discutir as soluções fornecidas neste artigo:
- **Registo a DNS** – registos são indicadores que apontam um domínio para um endereço IPv4. 
- **Nome de CNAME ou Canonical** -este tipo de registo é utilizado para apontar para outro registo DNS. CNAME não responde com um endereço IP, mas em vez disso, o ponteiro para o registo que contém o endereço IP. 
- **Ponderado encaminhamento** – um pode optar por associar uma ponderação de pontos finais de serviço e, em seguida, distribui o tráfego com base em ponderações atribuídas. Este método de encaminhamento é um dos quatro tráfego encaminhamento mecanismos disponíveis dentro do Gestor de tráfego. Para obter mais informações, consulte [ponderado método de encaminhamento](../traffic-manager/traffic-manager-routing-methods.md#weighted).
- **Encaminhamento de prioridade** – prioridade encaminhamento baseia-se nas verificações de estado de funcionamento dos pontos finais. Por predefinição, o Gestor de tráfego do Azure envia todo o tráfego para o ponto final de prioridade mais elevado e, após uma falha ou desastre, o Gestor de tráfego encaminha o tráfego para o ponto final secundário. Para obter mais informações, consulte [método de encaminhamento de prioridade](../traffic-manager/traffic-manager-routing-methods.md#priority).

## <a name="manual-failover-using-azure-dns"></a>Ativação pós-falha manual utilizando o DNS do Azure
A solução de ativação pós-falha manual de DNS do Azure para recuperação após desastre utiliza o padrão mecanismo DNS para a ativação pós-falha para o local de cópia de segurança. A opção manual através do DNS do Azure funciona melhor quando utilizado em conjunto com o modo de espera amovíveis ou a abordagem de leve piloto. 

![Ativação pós-falha manual utilizando o DNS do Azure](./media/disaster-recovery-dns-traffic-manager/manual-failover-using-dns.png)

*Figura - ativação pós-falha Manual utilizando o DNS do Azure*

Os pressupostos estabelecidos para a solução são:
-   Pontos finais primários e secundários tem IPs estáticos que não são alterados frequentemente. Diga para o site primário, o IP está 100.168.124.44 e o IP para o site secundário é 100.168.124.43.
-   Existe uma zona DNS do Azure para o site primário e secundário. Diga para o site primário, o ponto final é prod.contoso.com e para o site de cópia de segurança é dr.contoso.com. Também existe um registo DNS para a aplicação principal conhecido como www.contoso.com.   
-   O valor de TTL é igual ou abaixo o SLA de RTO definido na organização. Por exemplo, se uma empresa define o RTO da resposta de desastre de aplicação a ser 60 minutos, em seguida, o valor de TTL deve ser inferior a 60 minutos, preferencialmente, menor melhor. Pode configurar DNS do Azure para ativação pós-falha manual da seguinte forma:
1. Criar uma zona DNS
2. Criar registos de zona DNS
3. Atualizar o registo CNAME

### <a name="step-1-create-a-dns"></a>Passo 1: Criar um DNS
Crie uma zona DNS (por exemplo, www.contoso.com), conforme mostrado abaixo:

![Criar uma zona DNS no Azure](./media/disaster-recovery-dns-traffic-manager/create-dns-zone.png)

*Figura - criar uma zona DNS no Azure*

### <a name="step-2-create-dns-zone-records"></a>Passo 2: Criar registos de zona DNS

Dentro desta zona crie três registos (por exemplo - www.contoso.com, prod.contoso.com e dr.consoto.com) como mostrados abaixo.

![Criar registos de zona DNS](./media/disaster-recovery-dns-traffic-manager/create-dns-zone-records.png)

*Figura - criar registos de zona DNS no Azure*

Neste cenário, site, www.contoso.com tem um valor de TTL 30 minutos, que é bem abaixo o RTO declarado e está a apontar para o prod.contoso.com de site de produção. Esta configuração é durante operações de negócio normal. O valor de TTL de prod.contoso.com e dr.contoso.com foi definido para 300 segundos ou 5 minutos. Pode utilizar um serviço como o Monitor do Azure ou informações de aplicação do Azure de monitorização do Azure ou qualquer parceiro monitorização de soluções, tais como Dynatrace, mesmo pode utilizar soluções ampliadas inicial que podem monitorizar ou para detetar aplicações ou falhas de nível de infraestrutura virtual.

### <a name="step-3-update-the-cname-record"></a>Passo 3: Atualizar o registo CNAME

Assim que for detetada uma falha, altere o valor de registo para apontar para dr.contoso.com, conforme mostrado abaixo:
       
![Atualizar o registo CNAME](./media/disaster-recovery-dns-traffic-manager/update-cname-record.png)

*Figura - atualizar o registo CNAME no Azure*

Dentro de 30 minutos, durante o qual a maioria das resoluções irão atualizar o ficheiro de zona em cache, qualquer consulta para www.contoso.com será redirecionada para dr.contoso.com.
Também pode executar o seguinte comando da CLI do Azure para alterar o valor CNAME:
 ```azurecli
   az network dns record-set cname set-record \
   --resource-group 123 \
   --zone-name contoso.com \
   --record-set-name www \
   --cname dr.contoso.com
```
Este passo pode ser executado manualmente ou através de automatização. Pode ser feita manualmente através da consola ou a CLI do Azure. O SDK do Azure e a API podem ser utilizada para automatizar a atualização CNAME, de modo a que é necessária nenhuma intervenção manual. Automatização pode ser criada através de funções do Azure ou dentro de uma aplicação de monitorização de terceiros ou mesmo a partir de no local.

### <a name="how-manual-failover-works-using-azure-dns"></a>Manual como funciona a ativação pós-falha utilizando o DNS do Azure
Uma vez que o servidor DNS está fora do horário de ativação pós-falha ou desastre, é insulated em relação a qualquer período de inatividade. Isto permite que utilizador architect um cenário de ativação pós-falha simples que também é rentável e funcionará todos os o tempo partindo do princípio de que o operador tem conectividade de rede durante após desastre e pode tornar a inverte. Se a solução é colocado em script, em seguida, um tem de garantir que o servidor ou o serviço de execução do script deve ser insulated contra o problema que afetam o ambiente de produção. Além disso, tenha em atenção o valor de TTL baixa que foi definido em relação a zona, para que nenhuma resolução em todo o mundo mantém o ponto final em cache para longa e os clientes podem aceder ao site dentro de RTO. Para um modo de espera amovíveis e leve piloto, uma vez que alguns prewarming e outra atividade administrativa poderão ser necessários – um deve também dar tempo suficiente antes de efetuar a inverte.

## <a name="automatic-failover-using-azure-traffic-manager"></a>Ativação pós-falha automática utilizando o Gestor de tráfego do Azure
Quando tiver arquiteturas complexas e vários conjuntos de recursos tem capacidade para efetuar a mesma função, pode configurar o Gestor de tráfego do Azure (com base no DNS) para verificar o estado de funcionamento dos seus recursos de e para encaminhar o tráfego do recurso não-bom estado de funcionamento para o bom estado de funcionamento recurso. No exemplo seguinte, a região primária e a região secundária tem uma implementação completa. Esta implementação inclui os serviços em nuvem e de uma base de dados sincronizada. 

![Ativação pós-falha automática utilizando o Gestor de tráfego do Azure](./media/disaster-recovery-dns-traffic-manager/automatic-failover-using-traffic-manager.png)

*Figura - ativação pós-falha automática utilizando o Gestor de tráfego do Azure*

No entanto, a apenas a região primária está a processar ativamente pedidos de rede de utilizadores. A região secundária fica ativa apenas quando a região primária sofre uma interrupção do serviço. Nesse caso, todos os novos pedidos de rede encaminhar para a região secundária. Desde a cópia de segurança da base de dados é quase instantânea, ambos os balanceadores de carga IPs que pode ser selecionado, de estado de funcionamento e as instâncias são sempre se e em execução, esta topologia fornece uma opção para ficar para uma baixa RTO e ativação pós-falha sem qualquer intervenção manual. A região de ativação pós-falha secundário tem de estar pronta para ir-live imediatamente após a falha da região primária.
Este cenário é ideal para a utilização do Traffic Manager do Azure que tenha sondas integradas para vários tipos de verificações de estado de funcionamento, incluindo http / https e TCP. Traffic manager do Azure também tem um motor de regra que pode ser configurado para ativação pós-falha quando ocorre uma falha, conforme descrito abaixo. Vejamos a seguinte solução utilizando o Gestor de tráfego:
- O cliente tem o ponto final de região n. º 1 conhecido como prod.contoso.com com um IP estático como 100.168.124.44 e um ponto final de região n. º 2 conhecido como dr.contoso.com com um IP estático como 100.168.124.43. 
-   Cada um destes ambientes é fronted através de uma propriedade destinado ao pública, como um balanceador de carga. O Balanceador de carga pode ser configurado para ter um ponto final com base no DNS ou um nome de domínio completamente qualificado (FQDN), conforme mostrado acima.
-   Todas as instâncias 2 região estão em quase em tempo real replicação com 1 região. Além disso, as imagens da máquina estão atualizadas, e todos os dados de configuração do software é aplicado e estão de acordo com a região 1.  
-   Dimensionamento automático é pré-configurado com antecedência. 

Os passos direcionados para configurar a ativação pós-falha com o Gestor de tráfego do Azure são os seguintes:
1. Criar um novo perfil do Traffic Manager do Azure
2. Criar os pontos finais dentro do perfil do Traffic Manager
3. Definir a configuração de ativação pós-falha e verificação de estado de funcionamento

### <a name="step-1-create-a-new-azure-traffic-manager-profile"></a>Passo 1: Criar um novo perfil do Traffic Manager do Azure
Criar um novo perfil de Gestor de tráfego do Azure com o nome contoso123 e selecione o método de encaminhamento como prioridade. Se tiver um grupo de recursos já existente que pretende associar, em seguida, pode selecionar um grupo de recursos existente, caso contrário, crie um novo grupo de recursos.

![Criar perfil do Traffic Manager](./media/disaster-recovery-dns-traffic-manager/create-traffic-manager-profile.png)
*figura - criar um perfil do Traffic Manager*

### <a name="step-2-create-endpoints-within-the-traffic-manager-profile"></a>Passo 2: Criar os pontos finais dentro do perfil do Traffic Manager

Neste passo, cria pontos finais que apontam para a produção e sites de recuperação após desastre. Aqui, escolha o **tipo** como um ponto final externo, mas se o recurso está alojado no Azure, em seguida, pode escolher **ponto final do Azure** bem. Se optar por **ponto final do Azure**, em seguida, selecione um **recurso de destino** que está a um **do serviço de aplicações** ou um **IP público** que foi alocada por Azure. A prioridade é definida como **1** , uma vez que o serviço principal para 1 região.
Da mesma forma, crie o desastre recuperação ponto final dentro do Gestor de tráfego bem.

![Criar pontos finais de recuperação de desastre](./media/disaster-recovery-dns-traffic-manager/create-disaster-recovery-endpoint.png)

*Figura - criação de pontos finais de recuperação de desastre*

### <a name="step-3-set-up-health-check-and-failover-configuration"></a>Passo 3: Definir a configuração de ativação pós-falha e verificação de estado de funcionamento

Neste passo, definir o valor de TTL do DNS para 10 segundos, que é cumpridas pela maioria das resoluções de recursiva de acesso à internet. Esta configuração significa que nenhum resolução DNS serão colocados em cache as informações de mais de 10 segundos. Para as definições de monitorização do ponto final, o caminho é o conjunto atual em / ou raiz, mas poderá personalizar as definições de endpoint para avaliar um caminho, por exemplo, prod.contoso.com/index. O exemplo abaixo mostra o **https** como o protocolo de pesquisa. No entanto, pode escolher **http** ou **tcp** bem. A escolha do protocolo depende a aplicação de fim. O intervalo de pesquisa está definido para 10 segundos, que lhe permite pesquisar rápida e a repetição está definida para 3. Como resultado, o Gestor de tráfego irão ativação pós-falha para o segundo ponto final se três intervalos consecutivos registar uma falha. A fórmula seguinte define o tempo total de uma ativação pós-falha automática: tempo para ativação pós-falha = TTL + Repita * intervalo de pesquisa e neste caso, o valor é 10 + 3 * 10 = 40 segundos (Max).
Se a tentativas estiver definida como 1 e TTL está definida para 10 seg, em seguida, a hora de ativação pós-falha 10 + 1 * 10 = 20 segundos. Defina o novamente para um valor maior do que **1** para eliminar as possibilidades de ativações pós-falha devido a falsos positivos ou qualquer blips de rede secundárias. 


![Configurar a verificação de estado de funcionamento](./media/disaster-recovery-dns-traffic-manager/set-up-health-check.png)

*Figura - definir a configuração de ativação pós-falha e verificação de estado de funcionamento*

### <a name="how-automatic-failover-works-using-traffic-manager"></a>Como automática funciona de ativação pós-falha utilizando o Gestor de tráfego

Durante um desastre, o ponto final principal obtém sondado e o estado muda para **degradado** e o site de recuperação após desastre permanece **Online**. Por predefinição, o Gestor de tráfego envia todo o tráfego para o ponto de final (mais alta prioridade) principal. Se o ponto final principal aparecer degradado, o Gestor de tráfego encaminha o tráfego para o segundo ponto final, desde que permanece bom estado de funcionamento. Um tem a opção para configurar mais pontos finais dentro do Gestor de tráfego que podem servir como pontos finais de ativação pós-falha adicionais ou, como balanceadores de carga entre pontos finais de partilha de carga.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [Traffic Manager do Azure](../traffic-manager/traffic-manager-overview.md).
- Saiba mais sobre [DNS do Azure](../dns/dns-overview.md).









