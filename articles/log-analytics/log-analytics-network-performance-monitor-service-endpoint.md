---
title: "Solução de Monitor de desempenho no Log Analytics do Azure de rede | Microsoft Docs"
description: "A capacidade de Gestor de ponto final de serviço no Monitor de desempenho de rede permite-lhe monitorizar a conectividade de rede para qualquer ponto final que possui uma porta TCP aberta."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: ba19a4fc24668bff27c961b5b415f840d1132a34
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="service-endpoint-monitor"></a>Monitor de ponto final de serviço

A capacidade de Monitor de ponto final de serviço no [Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) permite-lhe monitorizar a conectividade de rede para qualquer ponto final que possui uma porta TCP aberta. Esses pontos finais incluem sites, aplicações de SaaS, PaaS e bases de dados SQL. 

Pode realizar as seguintes funções com **Monitor de ponto final de serviço**: 

- Monitorizar a conectividade de rede para as suas aplicações e serviços de rede (por exemplo, o Office 365, o Dynamics CRM, linha interna de aplicações empresariais, base de dados SQL, etc.) de várias sucursal de escritórios localizações 
- Testes incorporados para monitorizar a conectividade de rede para pontos finais do Office 365 e Dynamics365 
- Determinar o tempo de resposta, a latência de rede, a perda de pacotes teve ao estabelecer ligação com o ponto final 
- Determinar se o desempenho da aplicação fraco é devido a rede ou devido a algum problema no fim do fornecedor de aplicação 
- Identifique oportunidades de frequente na rede que poderá estar a causar o desempenho de aplicações fraco visualizando a latência contribuída por cada salto num mapa de topologia. 


![Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Configuração 
Para abrir a configuração de Monitor de desempenho de rede, abra o [solução de Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) e clique em de **configurar** botão.

![Configurar o Monitor de desempenho de rede](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-oms-agents-for-the-monitoring"></a>Configure os agentes do OMS para a monitorização.  
Ative as seguintes regras de firewall em nós utilizada para monitorização, de modo a que a solução pode detetar a topologia da sua nós para o ponto final do serviço: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action 
```

### <a name="create-service-endpoint-monitor-tests"></a>Criar testes de Monitor de ponto final de serviço 

Começar a criar os seus testes para monitorizar a conectividade de rede para os pontos finais de serviço 

1. Clique em de **Monitor de ponto final de serviço** separador.
2. Clique em **adicionar teste** e introduza o nome do teste e a descrição. 
3. Selecione o tipo de teste:<br>Selecione **teste Web** se estiver a monitorizar conectividade a um serviço que responde aos pedidos HTTP/S, tais como outlook.office365.com, bing.com.<br>Selecione **teste de rede** se estiver a monitorizar conectividade para um serviço que responde ao pedido TCP, mas não responde a pedidos HTTP/S, como um SQL server, o servidor de FTP, SSH porta etc. 
4. Se não pretender efetuar medidas de rede (latência de rede, perda de pacotes, deteção de topologia), em seguida, desmarque a caixa de texto. Recomendamos que mantenha-lo a opção estiver marcada para aproveitar benefício máximo a capacidade. 
5. Introduza o endereço URL/FQDN/IP de destino para o qual pretende monitorizar a conectividade de rede.  
6. Introduza o número de porta do serviço de destino. 
7. Introduza a frequência que pretende que o teste para ser executada. 
8. Selecione os nós partir do qual pretende monitorizar a conectividade de rede ao serviço. 

    >[!NOTE]
    > Com base no servidor para nós do Windows, a capacidade de utiliza baseada em TCP pedidos para efetuar os valores de rede. Baseada no cliente para nós do Windows, a capacidade de utiliza pedidos baseados em ICMP para efetuar os valores de rede. Em alguns casos, a aplicação de destino bloqueia ICMP com base no pedido a receber devido à qual quando os nós sejam Windows baseados no cliente, a solução é não é possível efetuar a medidas de rede. Por conseguinte, é recomendado que utilizar nós e baseadas no servidor do Windows nestes casos. 

9. Se não pretender criar eventos de estado de funcionamento para os itens que selecionou, em seguida, desmarque **ativar a monitorização de estado de funcionamento destinos abrangidos por este teste**. 
10. Escolha a monitorização de condições. Pode definir limiares personalizados para a geração de eventos de estado de funcionamento, escrevendo os valores de limiar. Sempre que o valor da condição ultrapassar o limiar selecionado para o par sub-rede selecionada rede, é gerado um evento de estado de funcionamento. 
11. Clique em **guardar** para guardar a configuração. 

 ![Configuração de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Instruções 

Mover para a vista de dashboard de monitorização do desempenho de rede e observe o **Monitor de ponto final de serviço** página para obter um resumo do Estado de funcionamento dos testes diferentes que criou.  

![Página de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Clique no mosaico para desagregar e visualizar os detalhes dos testes a **testes** página. Na tabela LHS, pode ver o estado de funcionamento do ponto no tempo e o valor do tempo de resposta do serviço, a latência de rede e a perda de pacotes para todos os testes. Pode utilizar o controlo do gravador de estado de rede para ver o instantâneo de rede em outro hora no passado. Clique num teste na tabela que pretende investigar. Pode ver as tendências históricas dos valores de tempo de resposta de gráficos, latência e perda no painel de RHS. Clique na ligação de teste detalhes para ver o desempenho de cada nó. 

![Testes de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

No **teste nós** vista, pode observar a conectividade de rede de cada nó. Clique no nó ter degradação do desempenho.  Este é o nó de onde a aplicação é observada estar em execução lenta. 

Determinar se o desempenho da aplicação fraco é devido a rede ou devido a algum problema no fim do fornecedor de aplicação ao observar a correlação entre o tempo de resposta da aplicação e a latência de rede- 

**Problema de aplicação:** se há um pico de pedidos com o tempo de resposta, mas a latência de rede é consistente, em seguida, isto sugere que a rede está a funcionar ajustar e o problema é devido a um problema no final da aplicação.  

![Problema de aplicação de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

**Problema de rede:** se um pico de pedidos de tempo de resposta é acompanhado com um pico de pedidos correspondente na latência de rede, em seguida, isto sugere que o aumento do tempo de resposta é devido a um aumento de latência de rede.  

![Problema de rede de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Depois de ter determinado se o problema está devido à rede, pode clicar no **topologia** ligação de visualização para identificar o salto troublesome no mapa de topologia. Por exemplo, na imagem abaixo, pode ver que fora a latência total 105 ms entre o nó e o ponto final da aplicação, 96 ms devido ao salto assinalado a vermelho. Depois de ter identificado o salto troublesome, pode tomar uma ação corretiva.  

![Testes de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnóstico 

Se observar um abnormality, em seguida, siga estes passos:

Se o tempo de resposta do serviço, perda da rede e latência são apresentadas como NA, em seguida, pode ser devido a um ou mais dos seguintes motivos:
- A aplicação está inativo.
- O nó a ser utilizado para a verificar a conectividade de rede para o serviço está inativo.
- O destino introduzido na configuração de teste está incorreto.
- O nó não tem qualquer conectividade de rede.

Se é apresentado um tempo de resposta do serviço válido, mas perda da rede, bem como a latência, é apresentada como NA, em seguida, pode ser devido a um ou mais dos seguintes motivos:
- Se o nó utilizado para a verificar a conectividade de rede para o serviço for máquina cliente do Windows, o serviço de destino está a bloquear pedidos ICMP ou uma firewall de rede está a bloquear pedidos ICMP com a partir do nó de origem.
- A caixa de verificação **efetuar medidas de rede** foi desmarcada na configuração de teste. 

Se o tempo de resposta do serviço NA mas perda da rede, bem como latência é válida, em seguida,-sugere que o serviço de destino não é uma aplicação web. Edite a configuração de teste e escolha o tipo de teste como de teste de rede em vez do teste Web. 

Se a aplicação está em execução lenta, deve determinar se o desempenho da aplicação fraco é devido a rede ou devido a algum problema no fim do fornecedor de aplicação.


## <a name="next-steps"></a>Passos Seguintes
* [Pesquisar registos](log-analytics-log-searches.md) para ver registos de dados de desempenho de rede detalhada.
