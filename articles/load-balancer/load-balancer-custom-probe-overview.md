---
title: Utilizar as pesquisas personalizadas do Balanceador de carga para monitorizar o estado de funcionamento | Documentos da Microsoft
description: Saiba como utilizar as pesquisas personalizadas para o Balanceador de carga do Azure para monitorizar instâncias por trás do Balanceador de carga
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
ms.date: 07/13/2018
ms.author: kumud
ms.openlocfilehash: 741b32f394ca2ce447826f7207f7fd9cbede9c51
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070880"
---
# <a name="understand-load-balancer-probes"></a>Compreender as sondas do Balanceador de carga

O Azure Load Balancer utiliza sondas de estado de funcionamento para determinar que instância de conjunto de back-end deve receber novos fluxos.   Pode utilizar sondas de estado de funcionamento para detetar a falha de um aplicativo numa instância de back-end.  Também pode utilizar a resposta de sonda de estado de funcionamento do seu aplicativo para sinalizar para o Balanceador de carga se pretende continuar a enviar novos fluxos ou parar o envio de novos fluxos a uma instância de back-end para gerir a carga ou períodos de indisponibilidade planeados.

Sondas de estado de funcionamento definem se novos fluxos são estabelecidos para instâncias de back-end em bom estado. Quando uma sonda de estado de funcionamento falha, o Balanceador de carga para a enviar novos fluxos para a respetiva instância de mau estado de funcionamento.  Conexões TCP estabelecidas continuam após falha de sonda de estado de funcionamento.  Fluxos UDP existentes vão passar a partir da instância de mau estado de funcionamento para outra instância do conjunto de back-end.

Se todas as sondas para um conjunto de back-end falharem, balanceadores de carga básico irá terminar todos os fluxos TCP existente para o conjunto de back-end, ao passo que o Balanceador de carga Standard irão permitir estabelecidos fluxos TCP para continuar; Não existem fluxos novos serão enviados para o conjunto de back-end.  Todos os fluxos UDP existentes irão terminar para básico e Balanceadores de carga Standard quando a ativação do conjunto de todas as sondas para um back-end.

Funções de serviço cloud (funções de trabalho e funções da web) utilizam um agente de convidado para a monitorização de sonda. Tem de configurar o TCP ou HTTP sondas de estado de funcionamento personalizado ao utilizar serviços em nuvem com VMs de IaaS por trás do Balanceador de carga.

## <a name="understand-probe-count-and-timeout"></a>Compreender a contagem de sonda e o tempo limite

Comportamento de sonda depende:

* O número de sondas com êxito, que permitem que uma instância como como períodos.
* O número de sondas de falha que fazer com que uma instância como como Inativas.

Os valores de tempo limite e a frequência definidos no SuccessFailCount determinam se uma instância foi confirmada para estar em execução ou não está em execução. No portal do Azure, o tempo limite é definido para duas vezes o valor da frequência.

Configuração da pesquisa de todas as instâncias com balanceamento de carga para um ponto final (ou seja, um conjunto com balanceamento de carga) tem de ser o mesmo. Não pode ter uma configuração de pesquisa diferente para cada instância de função ou uma VM no mesmo serviço alojado para uma combinação de ponto de extremidade específico. Por exemplo, cada instância tem de ter as portas locais idênticas e tempos limite.

> [!IMPORTANT]
> Uma sonda de Balanceador de carga utiliza o endereço IP 168.63.129.16. Este endereço IP público facilita a comunicação aos recursos da plataforma interna para o traga-your-own-IP cenário a rede Virtual do Azure. O endereço IP público 168.63.129.16 virtual é utilizado em todas as regiões, e não altera. Recomendamos que permite que este endereço IP em todas as políticas de local firewall. Ele não deve ser considerado um risco de segurança porque apenas a plataforma do Azure interna pode obter uma mensagem a partir desse endereço. Se não permitir este endereço IP nas suas políticas de firewall, ocorre um comportamento inesperado numa variedade de cenários. O comportamento inclui a configuração do mesmo intervalo de endereços IP de 168.63.129.16 e endereços IP de duplicar.

## <a name="learn-about-the-types-of-probes"></a>Saiba mais sobre os tipos de sondas

### <a name="guest-agent-probe"></a>Pesquisa do agente convidado

Uma sonda de agente convidado só está disponível para serviços Cloud do Azure. Balanceador de carga utiliza o agente convidado dentro da VM. Em seguida, escuta e responde com uma resposta HTTP 200 OK, apenas quando a instância está no estado pronto. (Outros Estados são ocupado, reciclagem ou a parar.)

Para obter mais informações, consulte [configurar o ficheiro de definição de serviço (. csdef) para sondas de estado de funcionamento](https://msdn.microsoft.com/library/azure/ee758710.aspx) ou [comece por criar um balanceador de carga público para os serviços cloud](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>O que faz uma pesquisa de agente convidado marcar uma instância como o mau estado de funcionamento?

Se o agente convidado não responder com HTTP 200 OK, o Balanceador de carga marca a instância como não responsivo. Em seguida, pára a enviar tráfego para essa instância. O Balanceador de carga continua a enviar um ping a instância. Se o agente convidado responde com um HTTP 200, o Balanceador de carga envia o tráfego para essa instância novamente.

Quando utiliza uma função da web, normalmente, executa o código de site no w3wp.exe, que não é monitorizado pelo Azure agente de recursos de infraestrutura ou de convidado. Falhas no w3wp.exe (por exemplo, as respostas HTTP 500) não são relatadas para o agente convidado. Conseqüentemente, o Balanceador de carga não Use essa instância da rotação.

### <a name="http-custom-probe"></a>Sonda personalizada de HTTP

Sonda HTTP personalizada substitui a sonda de agente de convidado de predefinida. Pode criar sua própria lógica personalizada para determinar o estado de funcionamento da instância de função. O load balancer sonda o ponto final a cada 15 segundos, por predefinição. A instância é considerada na rotação do Balanceador de carga se ele responde com um HTTP 200 dentro do período de tempo limite. O período de tempo limite é 31 segundos por padrão.

Uma sonda personalizada HTTP pode ser útil se pretender implementar sua própria lógica para remover instâncias de rotação do Balanceador de carga. Por exemplo, pode decidir remover uma instância se for superior a 90% da CPU e devolve um Estado que não 200. Se tiver funções da web que utilizam w3wp.exe, também obtém automática de monitorização do seu Web site. Falhas no código do seu site devolver um Estado que não 200 para a sonda de Balanceador de carga.

> [!NOTE]
> A sonda personalizada do HTTP suporta caminhos relativos e apenas os protocolos HTTP. HTTPS não é suportada.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>O que torna uma sonda personalizada de HTTP marcar uma instância como o mau estado de funcionamento?

* A aplicação de HTTP devolve um código de resposta HTTP que não 200 (por exemplo, 403, 404 ou 500). Esta confirmação positiva alertá-lo para tomar a instância da aplicação fora de serviço agora mesmo.
* O servidor HTTP não responde depois do período de tempo limite. Dependendo do valor de tempo limite que está definido, várias solicitações de sondagem podem passar sem resposta antes da sonda é marcada como não está em execução (ou seja, antes de SuccessFailCount sondas são enviadas).
* O servidor fecha a ligação através de uma reposição TCP.

### <a name="tcp-custom-probe"></a>Sonda personalizada de TCP

As pesquisas personalizadas de TCP iniciam uma ligação ao efetuar um handshake de três vias com a porta definido.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>O que torna uma sonda personalizada de TCP marcar uma instância como o mau estado de funcionamento?

* O servidor TCP não responde depois do período de tempo limite. Quando a sonda está marcada como não está em execução depende do número de pedidos de sonda com falha que foram configuradas para ir sem resposta antes de os marcar a sonda como não está em execução.
* A sonda recebe um TCP repor a partir da instância de função.

Para obter mais informações sobre como configurar uma sonda de estado de funcionamento HTTP ou uma sonda TCP, consulte [introdução à criação de um balanceador de carga público no Resource Manager com o PowerShell](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-the-load-balancer-rotation"></a>Adicionar instâncias em bom estado de volta para a rotação do Balanceador de carga

Sondas TCP e HTTP são consideradas íntegros e marcar a instância de função em bom estado quando:

* O Balanceador de carga obtém uma sonda positiva arranca a VM de pela primeira vez.
* O número para SuccessFailCount (descrita anteriormente) define o valor de sondas com êxito, que são necessárias para marcar a instância de função em bom estado. Se uma instância de função tiver sido removida, o número de sondas com êxito, sucessivas tem de ser igual ou exceder o valor de SuccessFailCount para marcar a instância de função em execução.

> [!NOTE]
> Se o estado de funcionamento de uma instância de função varia, o Balanceador de carga tem de aguardar já para a funcionalidade guarda a instância de função em bom estado. Este tempo de espera extra protege o utilizador e a infraestrutura e é uma política intencional.

## <a name="use-log-analytics-for-a-load-balancer"></a>Utilizar o log analytics para um balanceador de carga

Pode usar [do log analytics](load-balancer-monitor-log.md) para verificar o estado de funcionamento de sonda de Balanceador de carga e sonda de contagem. O registo pode ser utilizado com o Power BI ou informações operacionais do Azure para fornecer estatísticas sobre o estado de funcionamento do Balanceador de carga.
