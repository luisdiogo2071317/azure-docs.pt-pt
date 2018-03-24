---
title: Balanceador de carga Standard do Azure - diagnóstico | Microsoft Docs
description: Utilizar as informações de estado de funcionamento e as métricas disponíveis para obter um diagnóstico do Azure Standard Balanceador de carga
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
ms.openlocfilehash: 1d39cdc13e69740dc99e67f935b60db218536044
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Diagnóstico de estado de funcionamento e métricas padrão Balanceador de carga

Balanceador de carga do Azure Standard fornece as seguintes capacidades de diagnóstico para os seus recursos:
* Métricas multidimensionais: Padrão de Balanceador de carga fornece novas funcionalidades de diagnóstico multidimensionais para configurações de Balanceador de carga públicas e internas. Isto permite-lhe monitorizar, gerir e resolver problemas relacionados com os recursos de Balanceador de carga.

* Estado de funcionamento de recursos: A página de Balanceador de carga no portal do Azure e a página de estado de funcionamento do recurso (sob Monitor) expõem o estado de funcionamento de recursos para a configuração de Balanceador de carga público o padrão de Balanceador de carga.

Este artigo fornece uma visita guiada destas capacidades e formas de utilizar estes padrão Balanceador de carga.

## <a name = "MultiDimensionalMetrics"></a>Métricas multidimensionais

Balanceador de carga do Azure fornece as métricas multidimensionais novo através do novo métricas de Azure (pré-visualização) no portal e ajuda-o a obter informações de diagnóstico em tempo real para os recursos de Balanceador de carga. 

Métricas seguintes estão atualmente disponíveis para diferentes configurações de Balanceador de carga Standard (LB):

| Métrica | Tipo de recurso | Descrição | Agregação recomendada |
| --- | --- | --- | --- |
| Disponibilidade de VIP (disponibilidade de caminho de dados) | LB público | Balanceador de carga padrão exercises continuamente o caminho de dados de dentro de uma região para o front-end de Balanceador de carga para a pilha SDN que suporte a VM. Bom estado de funcionamento instâncias permanecerem, desde que a medição segue o mesmo caminho que o tráfego com balanceamento de carga da sua aplicação. O caminho de dados que é utilizado pelos seus clientes também está a ser validado. A medida é invisível à sua aplicação e não interfere com outras operações.| Média |
| Disponibilidade do DIP (estado de sonda de estado de funcionamento) |  LB pública & interno | Balanceador de carga padrão utiliza um serviço que monitoriza o estado de funcionamento do ponto final da aplicação, de acordo com as definições de configuração de pesquisa de estado de funcionamento distribuído. Esta métrica fornece um agregado ou por ponto final-vista filtrada de cada ponto final de instância individual no balanceador de carga do agrupamento.  Pode ver como o Balanceador de carga visualiza o estado de funcionamento da sua aplicação, como indicado pela sua configuração de pesquisa do Estado de funcionamento. |  Média |
| Pacotes de SIN |  LB público | Padrão de Balanceador de carga não terminar ligações TCP ou interagir com fluxos de pacotes TCP ou UDP. Fluxos e os respetivos handshakes são sempre entre a origem e a instância VM. Para resolver melhor os cenários de protocolo TCP, pode efetuar a utilização de SIN contadores de pacotes para compreender quantos ligação de TCP são realizadas tentativas. A métrica reporta o número de pacotes de TCP SIN que foram recebidos.| Média |
| Ligações de realizar o SNAT |  LB público |Balanceador de carga padrão reporta o número de fluxos de saída que são masqueraded para o front-end de endereço de IP público. Portas de realizar o SNAT são um recurso exhaustible. Esta métrica pode dar uma indicação de como descontos elevados a aplicação é depender de realizar o SNAT para saída fluxos teve origem.  Contadores para fluxos de realizar o SNAT de saída com êxito e são reportados e podem ser utilizados para resolver problemas e compreender o estado de funcionamento dos seus fluxos de saída.| Média |
| Contadores de bytes |  LB pública & interno | Os dados processados por front-end os relatórios padrão Balanceador de carga.| Média |
| Contadores de pacote |  LB pública & interno | Balanceador de carga padrão relatórios pacotes processados por front-end.| Média |

### <a name="view-load-balancer-metrics-in-the-portal"></a>Ver as métricas de Balanceador de carga no portal

Portal do Azure expõe as métricas de Balanceador de carga através da página de métricas (pré-visualização), que está disponível na página de recurso de Balanceador de carga para um recurso de Balanceador de carga específico bem como na página de Monitor do Azure. 

Para ver as métricas de recursos com o Balanceador de carga padrão, procurar das métricas (pré-visualização) em qualquer um destas localizações, selecione o (carga balanceador recurso se na página de Monitor) métrica escreva da lista pendente, definir o tipo de agregação adequado e, opcionalmente, configurar a filtragem necessária e o agrupamento e poderá ver a vista histórico para as métricas em condições de fornecido.

![Pré-visualização de métricas para padrão de Balanceador de carga](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Figura - DIP disponibilidade / métrica de estado de sonda de estado de funcionamento do Balanceador de carga*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>Obter métricas multidimensionais programaticamente através de APIs

Orientações de API para obter os valores e definições de métrica de acordo com estão disponível em [Walkthrouh de API de REST monitorização > API de acordo com](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api)

### <a name = "DiagnosticScenarios"></a>Cenários comuns de diagnóstico e vistas recomendadas

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>É o caminho de dados operacional e disponíveis para os meus VIP de Balanceador de carga?

Métrica de disponibilidade de VIP descreve o estado de funcionamento do caminho de dados numa região, para o anfitrião de computação, onde estão localizadas as suas VMs. É um Estado de funcionamento de reflexão da infraestrutura do Azure. Pode utilizar esta métrica para:
- Monitorizar a disponibilidade do seu serviço externa
- Aprofundar e compreender se a plataforma em que o seu serviço é implementado é bom estado de funcionamento ou se o SO convidado ou instância de aplicação estão em bom estado
- Isole se um evento está relacionado com o serviço ou plane de dados subjacente. Não confunda com o estado de sonda de estado de funcionamento ("DIP availability").

Para obter a disponibilidade de VIP para os seus recursos de Balanceador de carga padrão:
- Certifique-se de que o recurso de Balanceador de carga correto está selecionado. 
- Selecione **VIP disponibilidade** do **métrica** pendente. 
- Selecione **médio** para **agregação**. 
- Além disso, adicione o filtro no endereço VIP ou a porta VIP que a dimensão com o endereço IP de front-end necessário ou a porta de front-end e o grupo para a dimensão selecionada.

![VIP pesquisar](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Figura - VIP de Balanceador de carga pesquisar detalhes*

A métrica é gerada por uma medida de Active Directory, em banda. Um serviço de pesquisa na região origina o tráfego para esta medida é ativado, assim que criar uma implementação com público front-end e continua até que remova o front-end. 

>[!NOTE]
>Frontends internos não são suportados neste momento. 

Um pacote de front-end a implementação e a regra de correspondência é gerado periodicamente. -Atravessa a região de origem para o anfitrião onde está localizada a uma VM no conjunto de back-end. A infraestrutura de Balanceador de carga irá efetuar as mesmas operações de balanceamento e a tradução de carga que realiza todo o tráfego restante. Esta pesquisa é na banda no seu de carga com balanceamento de ponto final. Assim que a sonda chega no anfitrião de computação onde está localizada a uma VM bom estado de funcionamento no conjunto de back-end, o anfitrião de computação irá gerar uma resposta para o serviço de pesquisa. A VM não consegue ver este tráfego.
Disponibilidade de VIP falha pelos seguintes motivos:
- A implementação tem sem VMs bom restante no conjunto de back-end. 
- Ocorreu uma falha de infraestrutura que faz com que a disponibilidade de VIP falhar.

Pode utilizar o [métrica de disponibilidade de VIP juntamente com o estado de sonda de estado de funcionamento para fins de diagnóstico](https://aka.ms/lbdiagnostics#vipavailabilityandhealthprobes).

Utilize **médio** como a agregação para a maioria dos cenários.

#### <a name="are-the-backend-instances-for-my-vip-responding-to-probes"></a>As instâncias de back-end para os meus VIP estão a responder para pesquisas?

Métrica de estado de sonda de estado de funcionamento descreve o estado de funcionamento da implementação da aplicação tal como foi configurada por si quando configurar a sonda de estado de funcionamento do Balanceador de carga. Balanceador de carga utiliza o estado da sonda de estado de funcionamento para determinar para onde enviar os fluxos de novo. Sondas de estado de funcionamento provenientes de um endereço de infraestrutura do Azure e são visíveis no SO convidado da VM.

Para obter a disponibilidade do DIP para os seus recursos de Balanceador de carga padrão
- Selecione o **DIP disponibilidade** métrica com **médio** tipo de agregação. 
- Aplicar um filtro no endereço IP de VIP necessário ou porta (ou ambos).

![Disponibilidade do DIP](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Figura - disponibilidade VIP de Balanceador de carga*

Sondas de estado de funcionamento falharem pelos seguintes motivos:
- Se configurar uma sonda do Estado de funcionamento a uma porta não está à escuta ou não responde ou com o protocolo errado. Se o serviço estiver a utilizar as regras de DSR (IP flutuante), tem de certificar-se de que o serviço está a escutar no endereço IP da NIC configuração de IP e não apenas de loopback configurado com o front-end endereço IP.
- Se a sua pesquisa não é permitida pelo grupo de segurança de rede, firewall de SO de convidado de VM ou camada da aplicação filtros.

Utilize **médio** como a agregação para a maioria dos cenários.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Como verificar a minha estatísticas de ligação de saída? 

Métrica de ligações de realizar o SNAT descreve o volume de com êxito e (para [fluxos de saída](https://aka.ms/lboutbound)).

Um volume de ligações com falhas maior que zero indica esgotamento de porta de realizar o SNAT. Tem de investigação mais aprofundada e determinar que poderá estar a causar destas falhas. Manifestos de esgotamento de porta de realizar o SNAT como falhas de estabelecer um [fluxos de saída](https://aka.ms/lboutbound). Consulte o artigo em ligações de saída para compreender os cenários, mecanismos no trabalho e sobre como mitigar / para evitar o esgotamento de porta de realizar o SNAT de design. 

Para obter as estatísticas de ligação de realizar o SNAT,
- Selecione **realizar o SNAT ligações** tipo métrico e **soma** como agregação. 
- grupo por **estado da ligação** com êxito e falha na ligação de realizar o SNAT contagens representado por linhas diferentes. 

![Realizar o SNAT ligação](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Figura - realizar o SNAT contagem de ligação para o Balanceador de carga*


#### <a name="how-do-i-check-inbound--outbound-connection-attempts-for-my-service"></a>Como posso ver as tentativas de ligação de entrada / saída para o meu serviço?

Métrica de pacotes de SIN descreve o volume de pacotes de TCP SIN, que chegou ou que foram enviadas (para [fluxos de saída](https://aka.ms/lboutbound)) associados a um determinado front-end. Esta métrica pode ser utilizada para compreender as tentativas de ligação TCP ao seu serviço.
Utilize Total como a agregação para a maioria dos cenários.

![SIN ligação](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Figura - contagem de SIN Balanceador de carga*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Como verificar a minha consumo de largura de banda de rede? 

Bytes / métrica de contadores de pacote descreve o volume de bytes e pacotes enviadas ou recebidas pelo service numa base por front-end.
Utilize **Total** como a agregação para a maioria dos cenários.

Para obter as estatísticas de contagem de bytes ou pacote
- Selecione o **contagem de Bytes** e / ou **contagem de pacotes** tipo métrico com **médio** como a agregação. 
- Aplique o filtro num IP de front-end específica, a porta de front-end ou o IP back-end ou a porta em questão. 
- ou obter estatísticas gerais para o recurso de Balanceador de carga sem qualquer filtragem.


Algumas vistas de exemplo com configurações diferentes - com base nas métricas

![Contagem de bytes](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Figura - contagem de Byte para o Balanceador de carga*

#### <a name = "vipavailabilityandhealthprobes"></a>Como posso diagnosticar os meus implementação do Balanceador de carga?

A combinação da disponibilidade de VIP & sondas de estado de funcionamento métricas num único gráfico pode permitir que identifique onde pretende procurar o problema e resolver o problema. Pode obter a garantia que Azure está a funcionar corretamente e utilizá-lo para conclusively determinar a configuração ou a aplicação é a causa raiz.

Pode utilizar as métricas de sonda de estado de funcionamento para compreender a forma como o Azure visualiza o estado de funcionamento da implementação de acordo com a configuração que forneceu. Observar sondas de estado de funcionamento é sempre um excelente primeiro passo na monitorização ou determinar uma causa.

Pode demorar um passo adicional e utilize as métricas de disponibilidade do VIP para obter conhecimentos aprofundados sobre como o Azure visualiza o estado de funcionamento a plane de dados subjacente responsável pela sua implementação específica. Ao combinar as métricas, pode isolar onde a falha pode estar conforme ilustrado neste exemplo:

![Diagnóstico de VIP](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Figura - combinar DIP e VIP métricas de disponibilidade*

O gráfico mostra as seguintes informações:
- A infraestrutura de si próprio foi bom estado de funcionamento, a infraestrutura que aloja as suas VMs estava acessível e mais do que uma VM foi colocada em back-end. Esta situação é indicada pelo rastreio azul para disponibilidade de VIP, que mostra a 100%. 
- No entanto, o estado de sonda de estado de funcionamento (disponibilidade DIP) é 0% no início do gráfico conforme indicado pelo rastreio laranja. Área de circled destaques vermelhos onde o estado de funcionamento sondas (disponibilidade DIP) está em bom estado de funcionamento e ponto em que a implementação do cliente foi capaz de aceitar novos fluxos.

O gráfico permitido ao cliente para resolver problemas relacionados com a implementação nos respetivos sem ter de adivinhar ou peça ao suporte se existiam outros problemas a ocorrer. Serviço do cliente não estava disponível, porque as sondas de estado de funcionamento foram falhar devido a uma configuração incorreta ou a uma aplicação que falhou.

### <a name = "Limitations"></a>Limitações

- Disponibilidade de VIP está atualmente disponível apenas para frontends público.

## <a name = "ResourceHealth"></a>Estado de funcionamento de recursos

Estado de funcionamento dos recursos de Balanceador de carga Standard está exposto através de existente **estado de funcionamento do recurso** em **monitorização > Estado de funcionamento do serviço**.

>[!NOTE]
>O estado de funcionamento do recurso para o Balanceador de carga está atualmente disponível para configuração pública de apenas o Balanceador de carga padrão. Recursos de Balanceador de carga internos ou recursos básico SKU do Balanceador de carga expõe o estado de funcionamento do recurso.

Para ver o estado de funcionamento dos seus recursos de Balanceador de carga público padrão
 - Navegue até à **monitorização > Estado de funcionamento do serviço**.

  ![Página objeto da monitorização](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Figura - estado de funcionamento do serviço no Monitor do Azure*

 - Selecione **estado de funcionamento do recurso** e certifique-se de que correto que **ID de subscrição** e **tipo de recurso = Balanceador de carga** está selecionada.

  ![Estado de funcionamento de recursos](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Figura - selecione o recurso para a vista de estado de funcionamento*

 - Clique no recurso de Balanceador de carga da lista para ver o respetivo estado de funcionamento históricos.

    ![Estado de funcionamento do Balanceador de carga](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Figura - vista de estado de funcionamento de recursos do Balanceador de carga*
 
A tabela seguinte lista o estado de funcionamento vários recursos e as respetivas descrições. 

| Estado de funcionamento de recursos | Descrição |
| --- | --- |
| Disponível | O recurso de Balanceador de carga público padrão é bom estado de funcionamento e está disponível |
| Indisponível | O recurso de Balanceador de carga público padrão não está em bom estado. Diagnosticar o estado de funcionamento através do Azure Monitor > métricas. (*Isto também poderá significar que o recurso não é público Balanceador de carga padrão*) |
| Desconhecido | Estado de funcionamento de recursos para o Balanceador de carga público padrão ainda não foi atualizado. (*Isto também poderá significar que o recurso não é público Balanceador de carga padrão*)  |

## <a name="next-steps"></a>Próximos Passos

- Saiba mais sobre [padrão Balanceador de carga](load-balancer-standard-overview.md)
- Saiba mais sobre [conectividade de saída de Balanceador de carga](https://aka.ms/lboutbound)


