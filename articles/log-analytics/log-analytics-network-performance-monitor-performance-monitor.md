---
title: Funcionalidade de Monitor de desempenho na solução de Monitor de desempenho de rede no Log Analytics do Azure | Microsoft Docs
description: A capacidade de Monitor de desempenho no Monitor de desempenho de rede ajuda-o a monitorizar a conectividade de rede em vários pontos na sua rede. Pode monitorizar implementações de nuvem e localizações no local, vários centros de dados e sucursais e aplicações com múltiplas camadas fundamentais ou micro-serviços.
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
ms.openlocfilehash: 65497548d0b8066627be25520c28d39491918d09
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
ms.locfileid: "30241450"
---
# <a name="network-performance-monitor-solution-performance-monitoring"></a>Solução de Monitor de desempenho de rede: a monitorização de desempenho

A capacidade de Monitor de desempenho no [Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) ajuda-o a monitoriza conectividade de rede em vários pontos na sua rede. Pode monitorizar implementações de nuvem e localizações no local, vários centros de dados e sucursais e aplicações com múltiplas camadas fundamentais ou micro-serviços. Com a monitorização de desempenho, pode detetar problemas de rede antes dos utilizadores queixarem. Principais vantagens são que, pode: 

- Monitorizar a perda e latência entre várias sub-redes e definir alertas.
- Monitorize todos os caminhos (incluindo caminhos redundantes) na rede.
- Resolva problemas de rede transitórios e ponto no tempo, que são difíceis de replicar.
- Determine o segmento específico na rede, o que é responsável por degradação do desempenho.
- Monitorize o estado de funcionamento da rede, sem a necessidade de SNMP.


![Monitor de Desempenho da Rede](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Configuração
Para abrir a configuração de Monitor de desempenho de rede, abra o [solução de Monitor de desempenho de rede](log-analytics-network-performance-monitor.md)e selecione **configurar**.

![Configurar o Monitor de desempenho de rede](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Criar novas redes

Uma rede no Monitor de desempenho de rede é um contentor para sub-redes lógico. Ajuda a organizar a monitorização da infraestrutura da rede, consoante as suas necessidades. Pode criar uma rede com um nome amigável e adicionar sub-redes ao mesmo, de acordo com a lógica de negócio. Por exemplo, pode criar uma rede com o nome de Londres e adicionar todas as sub-redes no seu centro de dados de Londres. Ou pode criar uma rede com o nome *ContosoFrontEnd* e adicionar a esta rede todas as sub-redes com o nome Contoso servem o front-end da sua aplicação. A solução cria automaticamente uma rede predefinida, que contém todas as sub-redes detetadas no seu ambiente. 

Sempre que cria uma rede, adicione uma sub-rede para o mesmo. Em seguida, essa sub-rede é removida da rede predefinida. Se eliminar uma rede, todas as respetivas sub-redes automaticamente reencaminhadas para a rede predefinida. A rede predefinida atua como um contentor para todas as sub-redes que não estão contidos em qualquer rede definida pelo utilizador. Não é possível editar ou eliminar a rede predefinida. Permanece sempre no sistema. Pode criar tantas redes personalizadas conforme necessário. Na maioria dos casos, as sub-redes existentes na sua organização são dispostas numa rede de mais do que um. Crie uma ou mais redes para as sub-redes para a lógica de negócio de grupo.

Para criar uma nova rede:


1. Selecione o **redes** separador.
1. Selecione **adicionar rede**e, em seguida, introduza o nome de rede e a descrição. 
2. Selecione uma ou mais sub-redes e, em seguida, selecione **adicionar**. 
3. Selecione **guardar** para guardar a configuração. 


### <a name="create-monitoring-rules"></a>Criar regras de monitorização 

Monitor de desempenho gera eventos de estado de funcionamento quando o limiar de desempenho de ligações de rede entre dois sub-redes ou entre duas redes é quebrado. O sistema pode saber automaticamente estes limiares. Também pode fornecer limiares personalizados. O sistema cria automaticamente uma regra predefinida, o que gera um evento de estado de funcionamento sempre perda ou latência entre qualquer par de rede ou sub-rede liga falhas limiar aprendidas de sistema. Este processo ajuda-o a solução de monitorizar a infraestrutura de rede até que ainda não criou quaisquer regras de monitorização explicitamente. Se a regra predefinida está ativada, todos os nós enviam transações sintéticas para todos os outros nós que ativado para a monitorização. A regra predefinida é útil com pequenas redes. Um exemplo é um cenário em que tem um pequeno número de servidores a executar um microsserviço e deve certificar-se de que todos os servidores têm conectividade entre si.

>[!NOTE]
> Recomendamos que desative a regra predefinida e criar regras personalizadas de monitorização, especialmente com redes de grandes dimensões onde utiliza um grande número de nós para monitorização. Personalizar as regras de monitorização pode reduzir o tráfego gerado pela solução e ajudam a organizar os a monitorização da rede.

Crie regras de monitorização, de acordo com a lógica de negócio. Um exemplo é se pretende monitorizar o desempenho de conectividade de rede de dois sites do office para a sede. Todas as sub-redes no office site1 na rede O1 de grupo. Em seguida, agrupe todas as sub-redes no office site2 na rede O2. Por fim, grupo todas as sub-redes na sede na rede H. Crie duas regras de monitorização - um entre O1 e H e outros entre O2 e H. 

Para criar regras de monitorização personalizadas:

1. Selecione **Adicionar regra** no **Monitor** separador e introduza o nome da regra e a descrição.
2. Selecione o par de ligações de sub-rede ou de rede para a monitorização a partir da lista. 
3. Selecione a rede que contém as sub-redes que pretender a partir da rede na lista pendente. Em seguida, selecione as sub-redes de correspondente sub-rede na lista pendente. Se pretender monitorizar todas as sub-redes uma ligação de rede, selecione **todas as sub-redes**. Da mesma forma, selecione os outros sub-redes que pretende. Para excluir a monitorização para ligações de sub-rede específico de seleções que fez, selecione **adicionar exceção**. 
4. Escolha entre ICMP e TCP protocolos a execução das transações sintéticas. 
5. Se não pretender criar eventos de estado de funcionamento para os itens selecionados, desmarque **Ativar monitorização do Estado de funcionamento nas ligações abrangido por esta regra**. 
6. Escolha a monitorização de condições. Para configurar limiares personalizados de geração de eventos de estado de funcionamento, introduza os valores de limiar. Sempre que o valor da condição excede o limiar selecionado para a rede selecionada ou par sub-rede, é gerado um evento de estado de funcionamento. 
7. Selecione **guardar** para guardar a configuração. 

Depois de guardar uma regra de monitorização, pode integrar dessa regra com a gestão de alertas, selecionando **criar alertas**. Uma regra de alerta é criada automaticamente com a consulta de pesquisa. Outros parâmetros necessários estão preenchidos automaticamente. Utilizar uma regra de alerta, pode receber alertas com correio eletrónico base, além dos alertas no Monitor de desempenho de rede existentes. Alertas também podem acionar toma ações com runbooks ou, podem integrar com soluções de gestão de serviço existente utilizando webhooks. Selecione **gerir alerta** para editar as definições de alerta. 

Agora pode criar regras de Monitor de desempenho mais ou mover para o dashboard de solução para utilizar a capacidade.

### <a name="choose-the-protocol"></a>Escolher o protocolo

Monitor de desempenho de rede utiliza transações sintéticas para calcular as métricas de desempenho de rede, como a latência de perda e a ligação do pacote. Para compreender melhor este conceito, considere um agente de monitorização de desempenho de rede ligado a uma extremidade de uma ligação de rede. Este agente de monitorização de desempenho de rede envia pacotes de sonda para um segundo agente de Monitor de desempenho de rede ligado a outro final da rede. O segundo agente responde com pacotes de resposta. Este processo repete-se algumas vezes. Ao medir o número de respostas e o tempo decorrido para receber cada resposta, o agente de monitorização de desempenho de rede primeiro avalia ligação latência e perda de pacotes. 

O formato, o tamanho e a sequência destes pacotes é determinado pelo protocolo que escolheu ao criar regras de monitorização. Com base no protocolo de pacotes, os dispositivos de rede intermédio, tais como routers e comutadores, podem processar estes pacotes de forma diferente. Por conseguinte, à sua escolha de protocolo afeta a precisão dos resultados. A opção de protocolo também determina se tem de efetuar quaisquer passos manuais depois de implementar a solução de Monitor de desempenho de rede. 

Monitor de desempenho de rede oferece a opção entre os protocolos ICMP e TCP para a executing transações sintéticas. Se escolher o ICMP quando criar uma regra de transação sintética, os agentes de Monitor de desempenho de rede utilizam mensagens de eco ICMP para calcular a latência de rede e a perda de pacotes. ECO de ICMP utiliza a mesma mensagem que é enviada pelo utilitário de ping convencionais. Quando utilizar TCP como protocolo, o Monitor de desempenho de rede os agentes envia TCP SIN pacotes através da rede. Este passo é seguido um conclusão de handshake TCP e a ligação é removida utilizando RST pacotes. 

Antes de escolher um protocolo, considere as seguintes informações: 

* **Deteção de várias rotas de rede.** TCP é mais exata quando detetar várias rotas, e necessita de menos agentes em cada sub-rede. Por exemplo, um ou dois agentes que utilizar TCP podem detetar todos os caminhos redundantes entre sub-redes. Tem várias agentes que utilizam o ICMP para alcançar os resultados semelhantes. Utilizar o ICMP, se tiver um número de rotas entre duas sub-redes, precisa de mais do que 5N agentes na sub-rede de uma origem ou de destino.

* **Exatidão dos resultados.** Routers e comutadores tendem a atribuir prioridade mais baixa para pacotes de eco ICMP em comparação comparadas pacotes TCP. Em determinadas situações, quando os dispositivos de rede são bastante carregados, refletem os dados obtidos por TCP mais detalhadamente a perda e latência teve por aplicações. Isto ocorre porque a maioria do tráfego de aplicação flui através de TCP. Nestes casos, o ICMP fornece resultados menos exato em comparação comparados o TCP. 

* **Configuração da firewall.** Protocolo TCP requer que os pacotes TCP são enviados para uma porta de destino. A porta predefinida utilizada por agentes de Monitor de desempenho de rede é 8084. Pode alterar a porta quando configurar os agentes. Certifique-se de que as firewalls de rede ou as regras de grupo (NSG) de segurança de rede (no Azure) permitem o tráfego na porta. Terá também de certificar-se de que a firewall local nos computadores onde se encontram instalados agentes está configurada para permitir tráfego esta porta. Pode utilizar scripts do PowerShell para configurar regras de firewall nos seus computadores com o Windows, mas terá de configurar manualmente a firewall da rede. Em contrapartida, o ICMP não funciona ao utilizar uma porta. Na maioria dos cenários de empresa, o tráfego ICMP é permitido através de firewalls para permitir a utilização de ferramentas de diagnóstico de rede, como o utilitário de ping. Se pode enviar ping a uma máquina a partir de outra, pode utilizar o protocolo ICMP sem ter de configurar manualmente as firewalls.

>[!NOTE] 
> Alguns firewalls poderão bloquear o ICMP, que pode levar a retransmissão que resulta num grande número de eventos no seu sistema de gestão de informações e o evento de segurança. Certifique-se de que o protocolo que escolheu não está bloqueado por uma firewall da rede ou o NSG. Caso contrário, o Monitor de desempenho de rede não é possível monitorizar o segmento de rede. Recomendamos que utilize TCP para monitorização. Utilize o ICMP em cenários onde não é possível utilizar o TCP, tais como: 
>
> - Utilize nós do Windows baseados no cliente, porque sockets em bruto de TCP não são permitidos em clientes do Windows.
> - A firewall da rede ou o NSG bloqueia TCP.
> - Não sabe como mudar o protocolo.

Se optar por utilizar o ICMP durante a implementação, pode mudar para TCP em qualquer altura, editando as predefinições de monitorização de regra.

1. Aceda a **desempenho de rede** > **Monitor** > **configurar**   >  **Monitor**. Em seguida, selecione **regra predefinida**. 
2. Desloque-se para o **protocolo** secção e selecione o protocolo que pretende utilizar. 
3. Selecione **guardar** para aplicar a definição. 

Mesmo que a regra predefinida utiliza um protocolo específico, pode criar novas regras com um protocolo diferente. Pode ainda criar uma mistura de regras em que algumas regras utilizam ICMP e outros utilizam TCP. 

## <a name="walkthrough"></a>Instruções 

Ver agora uma investigação simple para a causa raiz de um evento de estado de funcionamento.

No dashboard da solução, um evento de estado de funcionamento mostra que uma ligação de rede está danificada. Para investigar o problema, selecione o **ligações de rede monitorizados** mosaico.

A página de desagregação mostra que o **DMZ2 DMZ1** ligação de rede está danificada. Selecione **ver ligações de sub-rede** para esta ligação de rede. 


A página de desagregação mostra todas as ligações de sub-rede no **DMZ2 DMZ1** ligação de rede. Para ambas as ligações de sub-rede, a latência ultrapassou o limiar, o que torna a ligação de rede em mau estado de funcionamento. Também pode ver as tendências de latência de ambas as ligações de sub-rede. Utilize a seleção do tempo controlar no gráfico focar-se no intervalo de tempo necessário. Pode ver a hora do dia quando latência atingiu o respetivo horário de pico. Procure os registos mais tarde durante este período de tempo investigar o problema. Selecione **ver ligações de nó** para desagregar ainda mais. 
 
 ![Página de ligações de sub-rede](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Semelhante à página anterior, a página de desagregar para a ligação de determinada sub-rede lista respetivas ligações de nó que constituem. Pode efetuar ações semelhantes aqui, tal como fez no passo anterior. Selecione **topologia de vista** para visualizar a topologia entre dois nós. 
 
 ![Página de ligações de nó](media/log-analytics-network-performance-monitor/node-links.png) 

Todos os caminhos de entre os dois nós selecionados são desenhados no mapa de topologia. Pode visualizar a topologia de salto por salto de rotas entre dois nós no mapa de topologia. Dá-lhe uma fotografia limpar rotas quantos existam entre dois nós e o que caminhos demorar os pacotes de dados. Congestionamentos de desempenho de rede são mostrados a vermelho. Para localizar uma ligação de rede defeituoso ou um dispositivo de rede defeituosa, observe os elementos vermelhos no mapa de topologia. 

 ![Dashboard de topologia com o mapa de topologia](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

Pode rever o número de saltos em cada caminho no, a latência e perda de **ação** painel. Utilize a barra de deslocamento para ver os detalhes dos caminhos de mau estado de funcionamento. Utilize os filtros para selecionar os caminhos com o salto mau estado de funcionamento para que a topologia para apenas os caminhos selecionados é representada. Para aplicar zoom dentro ou fora do mapa de topologia, utilize a roda do rato. 

Na imagem seguinte, a causa raiz das áreas de problema para a secção específica da rede aparecer na caminhos vermelhos e saltos. Selecione um nó de mapa de topologia para revelar as propriedades do nó, que inclui o endereço IP e FQDN. Selecionar um salto mostra o endereço IP do salto. 
 
![Mapa de topologia com propriedades de nó selecionado](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Passos Seguintes
[Pesquisar registos](log-analytics-log-searches.md) para ver registos de dados de desempenho de rede detalhada.
