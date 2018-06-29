---
title: Solução de Monitor de desempenho no Log Analytics do Azure de rede | Microsoft Docs
description: Utilize a capacidade de Gestor de ponto final de serviço no Monitor de desempenho de rede para monitorizar a conectividade de rede para qualquer ponto final que possui uma porta TCP aberta.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: f6196c1403ded7bb8a72ee5483c2c2056b0e8020
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37030714"
---
# <a name="service-connectivity-monitor"></a>Monitor de Conectividade de Serviço

Pode utilizar a capacidade de Monitor de conectividade de serviço no [Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) para monitorizar a conectividade de rede para qualquer ponto final que possui uma porta TCP aberta. Esses pontos finais incluem sites, aplicações de SaaS, PaaS e bases de dados SQL. 

Pode efetuar as seguintes funções com a monitorização de conectividade do serviço: 

- Monitorize a conectividade de rede para as suas aplicações e serviços de rede de várias localizações ou de sucursais. As aplicações e serviços de rede incluem o Office 365, Dynamics CRM, aplicações de linha de negócio internas e bases de dados SQL.
- Utilize testes incorporados para monitorizar a conectividade de rede para pontos finais do Office 365 e Dynamics 365. 
- Determine o tempo de resposta, a latência da rede e a perda de pacotes experiente ao estabelecer ligação com o ponto final.
- Determine se o desempenho da aplicação fraco é devido a rede ou devido a algum problema no fim do fornecedor de aplicação.
- Identifique oportunidades de frequente na rede que poderá estar a causar o desempenho de aplicações fraco visualizando a latência contribuída por cada salto num mapa de topologia.


![Monitor de Conectividade de Serviço](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Configuração 
Para abrir a configuração de Monitor de desempenho de rede, abra o [solução de Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) e selecione **configurar**.

![Configurar o Monitor de desempenho de rede](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-operations-management-suite-agents-for-monitoring"></a>Configurar os agentes do Operations Management Suite para monitorização
Ative as seguintes regras de firewall em nós utilizada para monitorização, de modo a que a solução pode detetar a topologia da sua nós para o ponto final do serviço: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```

### <a name="create-service-connectivity-monitor-tests"></a>Criar testes de monitorização de conectividade de serviço 

Começar a criar os seus testes para monitorizar a conectividade de rede para os pontos finais de serviço.

1. Selecione o **Monitor de conectividade do serviço** separador.
2. Selecione **adicionar teste**e introduza o nome do teste e a descrição. 
3. Selecione o tipo de teste:<br>

    * Selecione **Web** para monitorizar a conectividade a um serviço que responde aos pedidos HTTP/S, tais como outlook.office365.com ou bing.com.<br>
    * Selecione **rede** para monitorizar a conectividade a um serviço que responde a pedidos TCP, mas não responde a pedidos HTTP/S, como um SQL server, o servidor de FTP ou a porta SSH. 
4. Se não pretender efetuar medidas de rede, tais como latência da rede, perda de pacotes e deteção de topologia, desmarque a **efetuar medidas de rede** caixa de verificação. Mantenha-selecionado para aproveitar benefício máximo a capacidade. 
5. No **destino**, introduza o URL/FQDN/endereço IP para o qual pretende monitorizar a conectividade de rede.
6. No **número de porta**, introduza o número de porta de serviço de destino. 
7. No **frequência de teste**, introduza um valor para a frequência com que pretende que o teste para ser executada. 
8. Selecione os nós partir do qual pretende monitorizar a conectividade de rede ao serviço. 

    >[!NOTE]
    > Com base no servidor para nós do Windows, a capacidade de utiliza baseada em TCP pedidos para efetuar os valores de rede. Baseada no cliente para nós do Windows, a capacidade de utiliza pedidos baseados em ICMP para efetuar os valores de rede. Em alguns casos, a aplicação de destino bloqueia pedidos recebidos com base em ICMP quando os nós sejam Windows baseados no cliente. A solução é não é possível efetuar a medidas de rede. Recomendamos que utilize nós e baseadas no servidor do Windows nestes casos. 

9. Se não pretender criar eventos de estado de funcionamento para os itens seleciona, desmarque **Ativar monitorização do Estado de funcionamento em destinos abrangidos por este teste**. 
10. Escolha a monitorização de condições. Pode definir limiares personalizados para a geração de eventos de estado de funcionamento, introduzindo os valores de limiar. Sempre que o valor da condição ultrapassar o limiar selecionado para a rede selecionada ou par sub-rede, é gerado um evento de estado de funcionamento. 
11. Selecione **guardar** para guardar a configuração. 

    ![Configurações de teste de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Instruções 

Ir para a vista de dashboard de monitorização de desempenho de rede. Para obter um resumo do Estado de funcionamento dos testes diferentes que criou, observe o **Monitor de conectividade do serviço** página. 

![Página de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Selecione o mosaico para ver os detalhes dos testes no **testes** página. Na tabela à esquerda, pode ver o estado de funcionamento do ponto no tempo e o valor do tempo de resposta do serviço, a latência de rede e a perda de pacotes para todos os testes. Utilize o controlo de gravador de estado de rede para ver o instantâneo de rede posteriormente no passado. Selecione o teste na tabela que pretende investigar. Nos gráficos no painel à direita, pode ver as tendências históricas dos valores de tempo de resposta, latência e perda. Selecione o **detalhes de teste** ligação para ver o desempenho de cada nó.

![Testes de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

No **teste nós** vista, pode observar a conectividade de rede de cada nó. Selecione o nó a que tem uma degradação do desempenho. Este é o nó em que a aplicação é observada estar em execução lenta.

Determine se desempenho de aplicações fraco devido a rede ou um problema no fim do fornecedor de aplicação ao observar a correlação entre o tempo de resposta da aplicação e a latência de rede. 

* **Problema de aplicação:** um pico de pedidos do tempo de resposta mas consistência na latência de rede sugere que a rede está a funcionar ajustar e o problema poderá ser devido a um problema no final da aplicação. 

    ![Problema de aplicação de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

* **Problema de rede:** um pico de pedidos de tempo de resposta acompanhado com um pico de pedidos correspondente na latência de rede sugere que o aumento do tempo de resposta poderá dever-se um aumento de latência de rede. 

    ![Problema de rede de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Depois de a determinar se o problema está devido à rede, selecione o **topologia** ligação de visualização para identificar o salto troublesome no mapa de topologia. Um exemplo é mostrado na imagem seguinte. Sem a latência total 105 ms entre o nó e o ponto final da aplicação, 96 ms é devido ao salto assinalado a vermelho. Depois de identificar o salto troublesome, pode tomar uma ação corretiva. 

![Testes de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnóstico 

Se observar um abnormality, siga estes passos:

* Se o tempo de resposta do serviço, perda da rede e latência são apresentadas como NA, um ou mais das seguintes razões poderão ser a causa:

    - A aplicação está inativo.
    - O nó utilizado para a verificar a conectividade de rede para o serviço está inativo.
    - O destino introduzido na configuração de teste está incorreto.
    - O nó não tem qualquer conectividade de rede.

* Se é apresentado um tempo de resposta do serviço válido, mas perda da rede, bem como a latência, é apresentada como NA, um ou mais das seguintes razões poderão ser a causa:

    - Se o nó utilizado para a verificar a conectividade de rede para o serviço é um computador de cliente do Windows, o serviço de destino está a bloquear pedidos ICMP ou uma firewall de rede está a bloquear pedidos ICMP provenientes de nó.
    - O **efetuar medidas de rede** caixa de verificação está em branco na configuração de teste. 

* Se o tempo de resposta do serviço NA mas perda da rede, bem como latência é válida, o serviço de destino poderá não ser uma aplicação web. Edite a configuração de teste e escolha o tipo de teste como **rede** em vez de **Web**. 

* Se a aplicação está em execução lenta, determine se o desempenho da aplicação fraco é devido a rede ou um problema no fim do fornecedor de aplicação.


## <a name="next-steps"></a>Passos Seguintes
[Pesquisar registos](log-analytics-log-searches.md) para ver registos de dados de desempenho de rede detalhada.
