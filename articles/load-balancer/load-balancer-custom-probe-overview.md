---
title: Utilizar as pesquisas personalizadas do Balanceador de carga para monitorizar o estado de funcionamento | Documentos da Microsoft
description: Saiba como utilizar sondas de estado de funcionamento para monitorizar instâncias por trás do Balanceador de carga
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2018
ms.author: kumud
ms.openlocfilehash: b73028935fd60945a948c1c4e1848424b615d92e
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363688"
---
# <a name="load-balancer-health-probes"></a>As sondas de estado de funcionamento do Balanceador de carga

O Azure Load Balancer utiliza sondas de estado de funcionamento para determinar a quais instâncias de conjunto de back-end irão receber novos fluxos. Pode utilizar sondas de estado de funcionamento para detetar a falha de um aplicativo numa instância de back-end. Também pode gerar uma resposta personalizada para uma sonda de estado de funcionamento e utilizar a sonda de estado de funcionamento para controlo de fluxo e sinalizar ao balanceador de carga se pretende continuar a enviar novos fluxos ou parar o envio de novos fluxos para uma instância de back-end. Isso pode ser utilizado para gerir a carga ou períodos de indisponibilidade planeados.

Quando uma sonda de estado de funcionamento falha, o Balanceador de carga para a enviar novos fluxos para a respetiva instância de mau estado de funcionamento. O comportamento de fluxos de novos e existentes depende se um fluxo é TCP ou UDP como bem como que SKU do Balanceador de carga que está a utilizar.  Revisão [sonda inativo comportamento para obter detalhes](#probedown).

> [!IMPORTANT]
> As sondas de estado de funcionamento do Balanceador de carga provêm do endereço IP 168.63.129.16 e não tem de ser bloqueadas para sondas marcar sua instância.  Revisão [endereço IP de origem de sonda](#probesource) para obter detalhes.

## <a name="health-probe-types"></a>Tipos de sonda de estado de funcionamento

Sondas de estado de funcionamento podem observar qualquer porta numa instância de back-end, incluindo a porta em que o serviço real é fornecido. A sonda de estado de funcionamento suporta serviços de escuta TCP ou pontos de extremidade HTTP. 

Para o balanceamento de carga UDP, deve gerar um sinal de sonda de estado de funcionamento personalizados para a instância de back-end através de sonda de estado de funcionamento de um TCP ou HTTP.

Ao usar [regras de balanceamento de carga de portas HA](load-balancer-ha-ports-overview.md) com [Balanceador de carga Standard](load-balancer-standard-overview.md), todas as portas têm balanceada de carga e uma resposta de sonda de estado de funcionamento único deve refletir o estado de toda a instância.  

Deve ser não NAT ou de um Estado de funcionamento da pesquisa por meio da instância que recebe a sonda de estado de funcionamento para outra instância na sua VNet, como isso pode levar a falhas em cascata no seu cenário de proxy.

Se desejar testar uma falha de sonda de estado de funcionamento ou marcar para baixo de uma instância individual, pode utilizar um grupo de segurança para a sonda de estado de funcionamento de bloqueio explícita (destino ou [origem](#probesource)).

### <a name="tcp-probe"></a>Sonda TCP

Sondas TCP iniciam uma ligação ao efetuar um handshake TCP aberto de três vias com a porta definido.  Isto é, em seguida, seguido de um handshake TCP fechar de quatro vias.

O intervalo de sonda mínimo é de 5 segundos e o número mínimo de respostas de mau estado de funcionamento é 2.  A duração total não pode exceder os 120 segundos.

Uma sonda TCP falha quando:
* O serviço de escuta TCP na instância não responde durante o período de tempo limite.  Uma sonda é marcada para baixo com base no número de solicitações de sonda com falha, o que foram configuradas para ir sem resposta antes de os marcar a sonda.
* A sonda recebe uma reposição da instância TCP.

### <a name="http-probe"></a>Sonda HTTP

Sondas HTTP estabelecer uma ligação de TCP e emitir um HTTP GET com o caminho especificado. Sondas HTTP suportam caminhos relativos para o HTTP GET. A sonda de estado de funcionamento está marcado como cópia de segurança quando a instância responde com um Estado HTTP 200 dentro do período de tempo limite.  Tentativa de sondas de estado de funcionamento HTTP para verificar a cada 15 segundos da porta de sonda de estado de funcionamento configurado por predefinição. O intervalo de sonda mínimo é de 5 segundos. A duração total não pode exceder os 120 segundos. 


Sondas HTTP também podem ser útil se pretender implementar sua própria lógica para remover instâncias de rotação do Balanceador de carga. Por exemplo, pode decidir remover uma instância se ele for superior a 90% da CPU e devolver um Estado de HTTP não 200. 

Se utilizar os serviços Cloud e ter funções da web que utilizam w3wp.exe, alcança automáticas, monitorização do seu Web site. Falhas no código do seu site devolver um Estado que não 200 para a sonda de Balanceador de carga.  A sonda HTTP substitui a sonda de agente de convidado de predefinida. 

Uma sonda HTTP falha quando:
* Ponto final da sonda HTTP devolve um código de resposta HTTP que não 200 (por exemplo, 403, 404 ou 500). Esta ação marca a sonda de estado de funcionamento para baixo imediatamente. 
* Ponto final da sonda HTTP não responder durante a um período de tempo limite do segundo 31. Dependendo do valor de tempo limite que está definido, várias solicitações de sondagem podem passar sem resposta antes da sonda é marcada como não está em execução (ou seja, antes de SuccessFailCount sondas são enviadas).
* Ponto final da sonda HTTP fecha a ligação através de uma reposição TCP.

### <a name="guest-agent-probe-classic-only"></a>Pesquisa do agente convidado (apenas clássica)

Funções de serviço cloud (funções de trabalho e funções da web) usar um agente de convidado para a sonda de monitorização por predefinição.   Isso deve ser considerado uma opção de último recurso.  Sempre deve definir uma sonda de estado de funcionamento explicitamente com um TCP ou uma sonda HTTP. Uma sonda de agente convidado não é tão eficaz quanto sondas explicitamente definidas na maioria dos cenários de aplicação.  

Uma sonda de agente convidado é uma verificação do agente convidado dentro da VM. Em seguida, escuta e responde com uma resposta HTTP 200 OK, apenas quando a instância está no estado pronto. (Outros Estados são ocupado, reciclagem ou a parar.)

Para obter mais informações, consulte [configurar o ficheiro de definição de serviço (. csdef) para sondas de estado de funcionamento](https://msdn.microsoft.com/library/azure/ee758710.aspx) ou [comece por criar um balanceador de carga público para os serviços cloud](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Se o agente convidado não responder com HTTP 200 OK, o Balanceador de carga marca a instância como não responsivo. Em seguida, pára a enviar de fluxos para essa instância. O Balanceador de carga continua a verificar a instância. 

Se o agente convidado responde com um HTTP 200, o Balanceador de carga envia novos fluxos a essa instância novamente.

Quando utiliza uma função da web, normalmente, executa o código de site no w3wp.exe, que não é monitorizado pelo Azure agente de recursos de infraestrutura ou de convidado. Falhas no w3wp.exe (por exemplo, as respostas HTTP 500) não são relatadas para o agente convidado. Conseqüentemente, o Balanceador de carga não Use essa instância da rotação.

## <a name="probe-health"></a>Sonda de estado de funcionamento

Sondas de estado de funcionamento TCP e HTTP são consideradas íntegros e marcar a instância de função em bom estado quando:

* A sonda de estado de funcionamento é efetuada com êxito pela primeira vez que a VM é inicializado.
* O número para SuccessFailCount (descrita anteriormente) define o valor de sondas com êxito, que são necessárias para marcar a instância de função em bom estado. Se uma instância de função tiver sido removida, o número de sondas com êxito, sucessivas tem de ser igual ou exceder o valor de SuccessFailCount para marcar a instância de função em execução.

> [!NOTE]
> Se o estado de funcionamento de uma instância de função varia, o Balanceador de carga tem de aguardar já para a funcionalidade guarda a instância de função em bom estado. Este tempo de espera extra protege o utilizador e a infraestrutura e é uma política intencional.

## <a name="probe-count-and-timeout"></a>Contagem de sonda e o tempo limite

Comportamento de sonda depende:

* O número de sondas com êxito, que permitem que uma instância seja marcado como períodos.
* O número de sondas de falha que fazer com que uma instância seja marcado como inativo.

Os valores de tempo limite e a frequência definidos no SuccessFailCount determinam se uma instância foi confirmada para estar em execução ou não está em execução. No portal do Azure, o tempo limite é definido para duas vezes o valor da frequência.

Uma regra de balanceamento de carga definiu uma sonda de estado de funcionamento único o conjunto de back-end respectivos.

## <a name="probedown"></a>Sonda de comportamento

### <a name="tcp-connections"></a>Ligações TCP

Novas ligações de TCP serão concluída com êxito à instância de back-end que está em bom estado e tem um aplicativo capaz de aceitar um novo fluxo e o SO convidado.

Se a sonda de estado de funcionamento de uma instância de back-end falha, continuam a conexões TCP estabelecidas para esta instância de back-end.

Se todas as sondas para todas as instâncias de um conjunto de back-end falharem, não existem fluxos novo serão enviados para o conjunto de back-end. Balanceador de carga Standard permitirá estabelecidos fluxos TCP para continuar.  Balanceador de carga básico irá terminar todos os fluxos TCP existente para o conjunto de back-end.
 
Uma vez que o fluxo é sempre entre o cliente e o convidado da VM, um conjunto com todas as sondas para baixo fará com que um front-end não responder a tentativas abertas de ligação de TCP, porque não existe nenhuma instância de bom estado de funcionamento de back-end para receber o fluxo.

### <a name="udp-datagrams"></a>Datagramas UDP

Os datagramas UDP serão entregue a instâncias de back-end em bom estado.

UDP é sem ligações e não existe nenhum Estado de fluxo controlado por UDP. Se falhar a sonda de estado de funcionamento de qualquer instância de back-end, os fluxos UDP existentes podem mover para outra instância de bom estado de funcionamento do conjunto de back-end.

Se todas as sondas para todas as instâncias de um conjunto de back-end falharem, os fluxos UDP existentes irão terminar para básico e Standard balanceadores de carga.


## <a name="probesource"></a>Endereço IP de origem de sonda

Todas as sondas de estado de funcionamento do Balanceador de carga provêm do endereço IP 168.63.129.16 como a origem.  Quando levam seus próprios endereços IP para rede Virtual do Azure, é garantido que este endereço IP de origem de sonda de estado de funcionamento para que seja exclusivo como globalmente está reservado para a Microsoft.  Este endereço é o mesmo em todas as regiões e não se altera. Ele não deve ser considerado um risco de segurança porque apenas a plataforma do Azure interna pode obter um pacote a partir deste endereço IP. 

Para a sonda de estado de funcionamento do Balanceador de carga marcar a sua instância, **tem** permitir que este endereço IP em qualquer serviço [grupos de segurança](../virtual-network/security-overview.md) e políticas de local firewall.

Se não permitir este endereço IP nas suas políticas de firewall, a sonda de estado de funcionamento falhará, pois é não é possível alcançar a sua instância.  Por sua vez, o Balanceador de carga marcará-se para baixo da sua instância devido à falha de sonda de estado de funcionamento.  Isso pode causar a falha do seu serviço com balanceamento de carga. 

Não também deve configurar o seu VNet com o Microsoft pertence o intervalo de endereços IP que contém 168.63.129.16.  Isto irá entrar em conflito com o endereço IP da sonda de estado de funcionamento.

Se tiver várias interfaces na sua VM, terá de assegurar a que responder à sonda na interface de que utilizador o recebeu.  Isso pode exigir exclusivamente NAT'ing este endereço na VM numa base por interface de origem.

## <a name="monitoring"></a>Monitorização

Todos os [Balanceador de carga Standard](load-balancer-standard-overview.md) expõe o estado de sonda de estado de funcionamento como métricas multidimensionais por instância através do Azure Monitor.

Balanceador de carga básico expõe o estado de sonda de estado de funcionamento por conjunto de back-end através do Log Analytics.  Só está disponível para balanceadores de carga básico público e não está disponível para balanceadores de carga básico interno.  Pode usar [do log analytics](load-balancer-monitor-log.md) para verificar o estado de funcionamento de sonda de Balanceador de carga público e a contagem de sonda. O registo pode ser utilizado com o Power BI ou informações operacionais do Azure para fornecer estatísticas sobre o estado de funcionamento do Balanceador de carga.


## <a name="limitations"></a>Limitações

-  Sonda de estado de funcionamento HTTP não suporta a TLS (HTTPS).  Utilize uma sonda TCP para a porta 443.

## <a name="next-steps"></a>Passos Seguintes

- [Introdução à criação de um balanceador de carga público no Resource Manager com o PowerShell](load-balancer-get-started-internet-arm-ps.md)
- [API de REST para sondas de estado de funcionamento](https://docs.microsoft.com/en-us/rest/api/load-balancer/loadbalancerprobes/get)

