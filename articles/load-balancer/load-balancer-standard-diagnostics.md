---
title: Diagnóstico padrão Balanceador de carga do Azure | Microsoft Docs
description: Utilize as informações de estado de funcionamento e as métricas disponíveis para obter um diagnóstico do Azure Standard Balanceador de carga.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: Kumud
ms.openlocfilehash: 9d5d596254f673b86650e8d9754dacdb70be0666
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32179799"
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Diagnóstico de estado de funcionamento e métricas padrão Balanceador de carga

Balanceador de carga do Azure Standard fornece os recursos as seguintes funcionalidades de diagnóstico:
* **Métricas multidimensionais**: fornece novas funcionalidades de diagnóstico multidimensionais carga público e interno configurações de Balanceador de. Pode monitorizar, gerir e resolver problemas relacionados com os recursos de Balanceador de carga.

* **Estado de funcionamento do recurso**: página o Balanceador de carga no portal do Azure e a página de estado de funcionamento do recurso (sob Monitor) expõem a secção de estado de funcionamento do recurso para a configuração de Balanceador de carga público do Load Balancer padrão.

Este artigo fornece uma visita guiada uma destas capacidades e oferece formas para utilizá-los para o Balanceador de carga padrão.

## <a name = "MultiDimensionalMetrics"></a>Métricas multidimensionais

Balanceador de carga do Azure fornece métricas multidimensionais novo através do novo métricas de Azure (pré-visualização) no portal do Azure e ajuda a obter informações de diagnóstico em tempo real para a carga recursos balanceador. 

As várias configurações de Balanceador de carga padrão fornecem as métricas seguintes:

| Métrica | Tipo de recurso | Descrição | Agregação recomendada |
| --- | --- | --- | --- |
| Disponibilidade de VIP (disponibilidade de caminho de dados) | Balanceador de carga públicos | Balanceador de carga padrão exercises continuamente o caminho de dados de dentro de uma região para o carga balanceador front-end, para a pilha SDN que suporte a VM. Bom estado de funcionamento instâncias permanecerem, desde que a medição segue o mesmo caminho que o tráfego com balanceamento de carga da sua aplicação. O caminho de dados que utilizam os seus clientes também está a ser validado. A medida é invisível à sua aplicação e não interfere com outras operações.| Média |
| Disponibilidade do DIP (estado de sonda de estado de funcionamento) |  Balanceador de carga interno e pública | Balanceador de carga padrão utiliza um serviço de pesquisa de estado de funcionamento distribuído que monitoriza o estado de funcionamento do ponto final da aplicação, de acordo com as definições de configuração. Esta métrica fornece um agregado ou o ponto final por filtrar a vista de cada ponto final de instância no agrupamento de Balanceador de carga. Pode ver como o Balanceador de carga visualiza o estado de funcionamento da sua aplicação, como indicado pela sua configuração de pesquisa do Estado de funcionamento. |  Média |
| SIN (sincronizar) pacotes |  Balanceador de carga públicos | Padrão de Balanceador de carga não terminar ligações de protocolo de controlo de transmissão (TCP) ou interagir com fluxos de pacotes TCP ou UDP. Fluxos e os respetivos handshakes são sempre entre a origem e a instância VM. Para resolver melhor os cenários de protocolo TCP, pode efetuar a utilização de SIN contadores de pacotes para compreender quantos ligação de TCP são realizadas tentativas. A métrica reporta o número de pacotes de TCP SIN que foram recebidos.| Média |
| Ligações de realizar o SNAT |  Balanceador de carga públicos |Balanceador de carga padrão reporta o número de fluxos de saída que são masqueraded para o IP público endereço front-end. Portas de tradução (realizar o SNAT) de endereço de rede de origem são um recurso exhaustible. Esta métrica pode dar uma indicação de como descontos elevados a aplicação é depender de realizar o SNAT para saída fluxos teve origem. Contadores para fluxos de realizar o SNAT de saída com êxito e são reportados e podem ser utilizados para resolver problemas e compreender o estado de funcionamento dos seus fluxos de saída.| Média |
| Contadores de bytes |  Balanceador de carga interno e pública | Os dados processados por front-end os relatórios padrão Balanceador de carga.| Média |
| Contadores de pacote |  Balanceador de carga interno e pública | Balanceador de carga padrão relatórios pacotes processados por front-end.| Média |

### <a name="view-your-load-balancer-metrics-in-the-azure-portal"></a>Ver as métricas de Balanceador de carga no portal do Azure

O portal do Azure expõe as métricas de Balanceador de carga através da página de métricas (pré-visualização), que está disponível tanto a carga balanceador recursos página para um determinado recurso e a página de Monitor do Azure. 

Para ver as métricas para os seus recursos de Balanceador de carga padrão:
1. Ir para a página de métricas (pré-visualização) e efetue um dos seguintes procedimentos:
   * Na página de recurso de Balanceador de carga, selecione o tipo de Métrico na lista pendente.
   * Na página de Monitor do Azure, selecione o recurso de Balanceador de carga.
2. Defina o tipo de agregação adequada.
3. Opcionalmente, configure a filtragem necessária e o agrupamento.

![Pré-visualização de métricas para padrão de Balanceador de carga](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Figura: DIP disponibilidade e a métrica de estado de sonda de estado de funcionamento padrão Balanceador de carga*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>Obter métricas multidimensionais programaticamente através de APIs

Para obter orientações sobre a API para obter as definições de métrica multidimensionais e valores, consulte [instruções de API de REST de monitorização do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api).

### <a name = "DiagnosticScenarios"></a>Cenários comuns de diagnóstico e vistas recomendadas

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>É o caminho de dados de cópia de segurança e disponíveis para os meus VIP de Balanceador de carga?

A métrica de disponibilidade de VIP descreve o estado de funcionamento do caminho de dados numa região, para o anfitrião de computação, onde estão localizadas as suas VMs. A métrica é um reflexão do Estado de funcionamento da infraestrutura do Azure. Pode utilizar a métrica para:
- Monitorizar a disponibilidade do seu serviço externa
- Aprofundar e compreender se a plataforma em que o seu serviço é implementado é bom estado de funcionamento ou se o SO convidado ou a instância da aplicação está em bom estada.
- Isole se um evento está relacionado com o serviço ou plane de dados subjacente. Não confunda esta métrica com o estado de sonda de estado de funcionamento ("DIP availability").

Para obter a disponibilidade de VIP para os seus recursos de Balanceador de carga padrão:
1. Certifique-se de que o recurso de Balanceador de carga correta está selecionado. 
2. No **métrica** na lista pendente, selecione **VIP disponibilidade**. 
3. No **agregação** na lista pendente, selecione **médio**. 
4. Além disso, adicionar um filtro no endereço VIP ou a porta VIP que a dimensão com o endereço IP Front-end necessário ou a porta de front-end e, em seguida, agrupá-los para a dimensão selecionada.

![VIP pesquisar](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Figura: VIP de Balanceador de carga pesquisar detalhes*

A métrica é gerada por uma medida de Active Directory, em banda. Um serviço de pesquisa na região origina o tráfego para a medição. O serviço está ativado, assim que criar uma implementação com um público front-end e continua até que remova o front-end. 

>[!NOTE]
>Internas extremidades front-não são suportadas neste momento. 

Um pacote de front-end a implementação e a regra de correspondência é gerado periodicamente. -Atravessa a região de origem para o anfitrião onde está localizada a uma VM no conjunto de back-end. A infraestrutura de Balanceador de carga executa as operações de tradução e de balanceamento de carga mesmas que realiza todo o tráfego restante. Esta pesquisa é na banda no seu ponto final com balanceamento de carga. Depois da pesquisa de chegar a esse anfitrião de computação, onde está localizada a uma VM bom estado de funcionamento no conjunto de back-end, o anfitrião de computação gera uma resposta para o serviço de pesquisa. A VM não consegue ver este tráfego.

Disponibilidade de VIP falha pelos seguintes motivos:
- A implementação tem sem VMs bom restante no conjunto de back-end. 
- Ocorreu uma falha de infraestrutura.

Para fins de diagnóstico, pode utilizar o [métrica de disponibilidade de VIP juntamente com o estado de sonda de estado de funcionamento](#vipavailabilityandhealthprobes).

Utilize **médio** como a agregação para a maioria dos cenários.

#### <a name="are-the-back-end-instances-for-my-vip-responding-to-probes"></a>As instâncias de back-end para os meus VIP estão a responder para pesquisas?

A métrica de estado de sonda de estado de funcionamento descreve o estado de funcionamento da implementação da aplicação tal como foi configurada por si quando configurar a sonda de estado de funcionamento do seu Balanceador de carga. O Balanceador de carga utiliza o estado da sonda de estado de funcionamento para determinar para onde enviar os fluxos de novo. Sondas de estado de funcionamento provenientes de um endereço de infraestrutura do Azure e são visíveis no SO convidado da VM.

Para obter a disponibilidade do DIP para os seus recursos de Balanceador de carga padrão:
1. Selecione o **DIP disponibilidade** métrica com **médio** tipo de agregação. 
2. Aplicar um filtro no endereço IP de VIP necessário ou porta (ou ambos).

![Disponibilidade do DIP](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Figura: Disponibilidade de VIP de Balanceador de carga*

Sondas de estado de funcionamento falharem pelos seguintes motivos:
- Configurar uma sonda do Estado de funcionamento a uma porta não está à escuta ou não responde ou está a utilizar o protocolo errado. Se o serviço está a utilizar a devolução direta do servidor (DSR ou IP flutuante) regras, certifique-se de que o serviço está a escutar no endereço IP da NIC configuração de IP e não apenas nos loopback que está configurado com o IP de front-end de endereços.
- A pesquisa não é permitida pelo grupo de segurança de rede, firewall de SO de convidado da VM ou os filtros de camada de aplicação.

Utilize **médio** como a agregação para a maioria dos cenários.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Como verificar a minha estatísticas de ligação de saída? 

A métrica de ligações de realizar o SNAT descreve o volume de ligações com êxito e para [fluxos de saída](https://aka.ms/lboutbound).

Um volume de falha de ligações de maior que zero indica esgotamento de porta de realizar o SNAT. Tem investigação mais aprofundada para determinar o que poderá estar a causar destas falhas. Manifestos de esgotamento de porta de realizar o SNAT como uma falha ao estabelecer uma [fluxo de saída](https://aka.ms/lboutbound). Consulte o artigo sobre ligações de saída para compreender os cenários e mecanismos no trabalho e para aprender a mitigar e de design para evitar o esgotamento de porta de realizar o SNAT. 

Para obter as estatísticas de ligação de realizar o SNAT:
1. Selecione **realizar o SNAT ligações** tipo métrico e **soma** como agregação. 
2. Agrupar por **estado da ligação** para com êxito e contagens de ligação realizar o SNAT que são representadas por linhas diferentes. 

![Ligação de realizar o SNAT](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Figura: Total de ligações de realizar o SNAT Balanceador de carga*


#### <a name="how-do-i-check-inboundoutbound-connection-attempts-for-my-service"></a>Como posso ver as tentativas de ligação de entrada/saída para o meu serviço?

Uma métrica de pacotes SIN descreve o volume de pacotes de TCP SIN, que chegou ou que foram enviadas (para [fluxos de saída](https://aka.ms/lboutbound)) que estão associados um front-end específico. Pode utilizar esta métrica para compreender as tentativas de ligação TCP ao seu serviço.

Utilize **Total** como a agregação para a maioria dos cenários.

![Ligação SIN](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Figura: Contagem de SIN de Balanceador de carga*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Como verificar a minha consumo de largura de banda de rede? 

Os bytes e contadores de pacote métrica descreve o volume de bytes e pacotes que são enviadas ou recebidas pelo service numa base por-frente-end.

Utilize **Total** como a agregação para a maioria dos cenários.

Para obter estatísticas de contagem de bytes ou pacotes:
1. Selecione o **contagem de Bytes** e/ou **contagem de pacotes** tipo métrico, com **médio** como a agregação. 
2. Efetue um dos seguintes procedimentos:
   * Aplique um filtro um IP de front-end específico, porta de front-end, IP back-end ou a porta de back-end.
   * Obter estatísticas gerais para o seu recurso de Balanceador de carga sem qualquer filtragem.

![Contagem de bytes](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Figura: Contagem de bytes de Balanceador de carga*

#### <a name = "vipavailabilityandhealthprobes"></a>Como posso diagnosticar os meus implementação do Balanceador de carga?

Utilizando uma combinação das métricas de sonda de estado de funcionamento e disponibilidade de VIP num único gráfico pode identificar onde pretende procurar o problema e resolver o problema. Pode obter a garantia que Azure está a funcionar corretamente e utilizar estes conhecimentos para conclusively determinar que a configuração ou a aplicação está a causa raiz.

Pode utilizar as métricas de sonda de estado de funcionamento para compreender a forma como o Azure visualiza o estado de funcionamento da implementação de acordo com a configuração que forneceu. Observar sondas de estado de funcionamento é sempre um excelente primeiro passo na monitorização ou determinar uma causa.

Pode levá-los um passo adicional e utilize as métricas de disponibilidade do VIP para obter conhecimentos aprofundados sobre como o Azure visualiza o estado de funcionamento a subjacente plane de dados é responsável pela sua implementação específica. Ao combinar as métricas, pode isolar onde poderá ser a falha, conforme ilustrado neste exemplo:

![Diagnóstico de VIP](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Figura: Combinar DIP e VIP métricas de disponibilidade*

O gráfico apresenta as seguintes informações:
- A infraestrutura de si próprio foi bom estado de funcionamento, a infraestrutura que aloja as suas VMs estava acessível e mais do que uma VM foi colocada em back-end. Esta informação é indicada pelo rastreio azul para disponibilidade de VIP, que é 100 por cento. 
- No entanto, o estado de sonda de estado de funcionamento (disponibilidade DIP) é 0 por cento no início do gráfico, conforme indicado pelo rastreio laranja. Área de circled destaques verdes onde o estado (disponibilidade DIP) está em bom funcionamento, e em que a implementação do cliente de ponto foi capaz de aceitar novos fluxos.

O gráfico permite aos clientes resolver problemas relacionados com a implementação nos respetivos sem ter de adivinhar ou peça ao suporte se outros problemas estão a ocorrer. O serviço não estava disponível, porque as sondas de estado de funcionamento foram falhar devido a uma configuração incorreta ou uma aplicação que falhou.

### <a name = "Limitations"></a>Limitações

Disponibilidade de VIP está atualmente disponível apenas para terminar público de front.

## <a name = "ResourceHealth"></a>Estado de funcionamento de recursos

Estado de funcionamento dos recursos de Balanceador de carga Standard está exposto através de existente **estado de funcionamento do recurso** em **monitorização > Estado de funcionamento do serviço**.

>[!NOTE]
>Estado de funcionamento do recurso de Balanceador de carga está atualmente disponível para configuração pública apenas padrão de Balanceador de carga. Recursos de Balanceador de carga interno ou recursos básico SKUs do Balanceador de carga não expõe o estado de funcionamento do recurso.

Para ver o estado de funcionamento dos seus recursos de padrão Balanceador de carga públicos:
1. Selecione **Monitor** > **estado de funcionamento do serviço**.

   ![Página objeto da monitorização](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Figura: A estado de funcionamento do serviço ligação no Monitor do Azure*

2. Selecione **estado de funcionamento do recurso**e, em seguida, certifique-se de que **ID de subscrição** e **tipo de recurso = Balanceador de carga** estão selecionadas.

   ![Estado de funcionamento de recursos](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Figura: Selecione o recurso para a vista de estado de funcionamento*

3. Na lista, selecione o recurso de Balanceador de carga para ver o estado de funcionamento históricos.

    ![Estado de funcionamento do Balanceador de carga](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Figura: Vista de estado de funcionamento de recursos de Balanceador de carga*
 
Os vários Estados de funcionamento de recursos e as respetivas descrições estão listadas na seguinte tabela: 

| Estado de funcionamento de recursos | Descrição |
| --- | --- |
| Disponível | O recurso de Balanceador de carga padrão público é bom estado de funcionamento e disponível. |
| Indisponível | O recurso de Balanceador de carga públicos de padrão não está em bom estado. Diagnosticar o estado de funcionamento selecionando **Azure Monitor** > **métricas**.<br>(*Indisponível* estado também poderá significar que o recurso não está ligado com o Balanceador de carga padrão público.) |
| Desconhecido | Estado de funcionamento de recursos para o seu recurso de Balanceador de carga padrão públicos ainda não foi atualizado.<br>(*Desconhecido* estado também poderá significar que o recurso não está ligado com o Balanceador de carga padrão público.)  |

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais o [Balanceador de Carga Standard](load-balancer-standard-overview.md).
- Saiba mais sobre o [conectividade de saída do Balanceador de carga](https://aka.ms/lboutbound).


