---
title: "Funcionalidade de Monitor de desempenho na solução de Monitor de desempenho de rede no Log Analytics do Azure | Microsoft Docs"
description: "A capacidade de Monitor de desempenho no Monitor de desempenho de rede ajuda-o a monitorizar a conectividade de rede em vários pontos na sua rede, tais como implementações de nuvem e de localizações no local, múltiplos centros de dados e de sucursais, crítico de missão multicamadas aplicações/microserviços."
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
ms.openlocfilehash: a90ab3bc857b704d9d94daf96d17611844abb1a6
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="network-performance-monitor-solution---performance-monitoring"></a>Rede de solução de Monitor de desempenho - monitorização de desempenho

A capacidade de Monitor de desempenho no [Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) ajuda a monitorizar a conectividade de rede em vários pontos na sua rede, tais como implementações de nuvem e localizações no local, múltiplos centros de dados e sucursais, missão críticos multicamadas aplicações/microserviços. Com a monitorização de desempenho, pode detetar problemas de rede antes dos utilizadores queixarem. Vantagens chaves são: 

- Monitorizar a perda e latência em várias sub-redes e conjunto de alertas 
- Monitorizar todos os caminhos (incluindo caminhos redundantes) na rede 
- Resolver problemas de rede transitórios & ponto no tempo, que são difíceis de replicar 
- Determinar o segmento específico na rede, o que é responsável por degradação do desempenho 
- Monitorizar o estado de funcionamento da rede, sem a necessidade de SNMP 


![Monitor de Desempenho da Rede](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Configuração
Para abrir a configuração de Monitor de desempenho de rede, abra o [solução de Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) e clique em de **configurar** botão.

![Configurar o Monitor de desempenho de rede](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Criar novas redes

Uma rede no NPM é um contentor para sub-redes lógico. Ajuda a organizar a monitorização da infraestrutura da rede, consoante as suas necessidades de monitorização. Pode criar uma rede com um nome amigável e adicionar sub-redes ao mesmo, de acordo com a lógica de negócio. Por exemplo, pode criar uma rede com o nome de Londres e adicionar todas as sub-redes no seu centro de dados de Londres, ou com o nome de uma rede *ContosoFrontEnd* e adicione todas as sub-redes que serve o front-end da sua aplicação com o nome Contoso a esta rede. A solução cria automaticamente uma rede predefinida que contém todas as sub-redes detetadas no seu ambiente. Sempre que cria uma rede, adicionar uma sub-rede para a mesma e essa sub-rede é removida da rede predefinida. Se eliminar uma rede, todas as respetivas sub-redes automaticamente reencaminhadas para a rede predefinida. Assim, a rede predefinida age como um contentor para todas as sub-redes que não estão contidos em qualquer rede definida pelo utilizador. Não é possível editar ou eliminar a rede predefinida. Permanece sempre no sistema. No entanto, pode criar tantas redes personalizadas conforme necessário. Na maioria dos casos, as sub-redes existentes na sua organização são dispostas em mais de uma rede e deve criar uma ou mais redes para as sub-redes para a lógica de negócio de grupo.

Para criar uma nova rede:


1. Clique em de **separador redes**.
1. Clique em **adicionar rede** e, em seguida, escreva o nome de rede e a descrição. 
2. Selecione uma ou mais sub-redes e, em seguida, clique em **adicionar**. 
3. Clique em **guardar** para guardar a configuração. 


### <a name="create-monitoring-rules"></a>Criar regras de monitorização 

Monitor de desempenho gera eventos de estado de funcionamento quando o limiar de desempenho de ligações de rede entre dois sub-redes ou entre duas redes é quebrado. Estes limiares podem ser adquiridos automaticamente pelo sistema, ou pode fornecer limiares personalizados. O sistema cria automaticamente uma regra predefinida, o que gera um evento de estado de funcionamento sempre perda ou latência entre qualquer par de rede/sub-rede liga falhas limiar aprendidas de sistema. Isto ajuda a solução de monitorizar a infraestrutura de rede até que ainda não criou quaisquer regras de monitorização explicitamente. Se o **regra predefinida** está ativada, todos os nós enviam transações sintéticas para todos os outros nós que tiver ativado para monitorização. A regra predefinida é útil com redes pequenos, por exemplo, num cenário em que tem um pequeno número de servidores a executar um microsserviço e deve certificar-se de que todos os servidores têm conectividade entre si. 

>[!NOTE]
> Recomenda-se vivamente que desative o **regra predefinida** e crie regras de monitorização personalizadas, especialmente com redes de grandes dimensões em que estiver a utilizar um grande número de nós para monitorização. Isto reduz o tráfego gerado pela solução e ajudam a organizar os a monitorização da rede. 

Crie regras de monitorização, de acordo com a lógica de negócio. Por exemplo, se pretender monitorizar o desempenho de conectividade de rede de dois sites do office para headquarter, em seguida, grupo de todas as sub-redes no office site1 na rede O1, todas as sub-redes no office site2 na rede O2 e todas as sub-redes na headquarter na rede H. Criar 2 monitorização regras-um entre O1 e H e outros entre O2 e H. 

Para criar regras de monitorização personalizadas:

1. Clique em **Adicionar regra** no **Monitor** separador e introduza o nome da regra e a descrição. 
2. Selecione o par de ligações de sub-rede ou de rede para a monitorização a partir da lista. 
3. Primeiro selecione a rede na qual o primeira sub-rede/s de interesse está incluído na lista pendente rede e, em seguida, selecione o sub-rede/s na lista pendente sub-rede correspondente. Selecione **todas as sub-redes** se pretender monitorizar todas as sub-redes uma ligação de rede. Da mesma forma, selecione os outros sub-rede/s de interesse. E, pode clicar em **adicionar exceção** para excluir a monitorização para ligações de sub-rede específica da seleção efetuadas. 
4. Escolha entre o ICMP e TCP em protocolos de execução das transações sintéticas. 
5. Se não pretender criar eventos de estado de funcionamento para os itens que selecionou, em seguida, desmarque **ativar a monitorização de estado de funcionamento nas ligações abrangidas por esta regra**. 
6. Escolha a monitorização de condições. Pode definir limiares personalizados para a geração de eventos de estado de funcionamento, escrevendo os valores de limiar. Sempre que o valor da condição ultrapassar o limiar selecionado para o par sub-rede selecionada rede, é gerado um evento de estado de funcionamento. 
7. Clique em **guardar** para guardar a configuração. 

Depois de guardar uma regra de monitorização, pode integrar dessa regra com a gestão de alertas, clicando em **criar alertas**. Uma regra de alerta é criada automaticamente com a consulta de pesquisa e outros necessários parâmetros preenchidos na automaticamente. Utilizar uma regra de alerta, pode receber alertas baseados em e-mail, para além dos alertas existentes dentro do NPM. Alertas também podem acionar toma ações com runbooks ou podem integrar com soluções de gestão de serviço existente com webhooks. Pode clicar em **gerir alerta** para editar as definições de alerta. 

Agora pode criar regras de Monitor de desempenho mais ou mover para o dashboard de solução para começar a utilizar a capacidade 

### <a name="choose-the-protocol"></a>Escolher o protocolo

Monitor de desempenho de rede (NPM) utiliza transações sintéticas para calcular as métricas de desempenho de rede, como a latência de perda e a ligação do pacote. Para melhor compreender isto, considere um agente NPM ligado a uma extremidade de uma ligação de rede. Este agente NPM envia pacotes de sonda para um agente NPM segundo ligado a outro final da rede. O segundo agente responde com pacotes de resposta. Este processo é repetido algumas vezes. Ao medir o número de respostas e o tempo decorrido para receber cada resposta, o agente NPM primeiro avalia ligação latência e perda de pacotes. 

O formato, o tamanho e a sequência destes pacotes é determinado pelo protocolo que escolheu ao criar regras de monitorização. Com base no protocolo de pacotes, o intermédio dispositivos de rede (routers, comutadores, etc.) pode processar estes pacotes de forma diferente. Por conseguinte, à sua escolha de protocolo afeta a precisão dos resultados. Além disso, a opção de protocolo também determina se tem de efetuar quaisquer passos manuais depois de implementar a solução NPM. 

NPM oferece a opção entre os protocolos ICMP e TCP para a executing transações sintéticas. Se escolher o ICMP quando criar uma regra de transação sintética, os agentes NPM utilizam mensagens de eco ICMP para calcular a latência de rede e a perda de pacotes. ECO de ICMP utiliza a mesma mensagem que é enviada pelo utilitário de Ping convencional. Quando utilizar TCP como protocolo, agentes NPM enviam pacotes de TCP SIN através da rede. Isto é seguido de um handshake TCP conclusão e, em seguida, remover a ligação utilizando pacotes RST. 

Antes de escolher um protocolo a utilizar, considere as seguintes informações: 

**Detetar várias rotas de rede.**  TCP é mais exata quando detetar várias rotas e necessita de menos agentes em cada sub-rede. Por exemplo, um ou dois agentes através de TCP podem detetar todos os caminhos redundantes entre sub-redes. No entanto, terá de vários agentes utilizando o ICMP para alcançar os resultados semelhantes. Utilizando o ICMP, se tiver um número de rotas entre duas sub-redes tem mais do que 5N agentes na sub-rede de uma origem ou de destino. 

**Exatidão dos resultados.** Routers e comutadores tendem a atribuir prioridade mais baixa para pacotes de eco ICMP em comparação comparadas pacotes TCP. Em determinadas situações, quando os dispositivos de rede são bastante carregados, refletem os dados obtidos por TCP mais detalhadamente a perda e latência teve por aplicações. Isto ocorre porque a maioria do tráfego de aplicação flui através de TCP. Nestes casos, o ICMP fornece resultados menos exatos em comparação comparados o TCP. 

**Configuração da firewall.** Protocolo TCP requer que os pacotes TCP são enviados para uma porta de destino. A porta predefinida utilizada por agentes NPM for 8084, no entanto, pode alterar isto quando configurar os agentes. Por isso, tem de garantir que as firewalls de rede ou as regras do NSG (no Azure) estão a permitir tráfego na porta. Terá também de certificar-se de que a firewall local nos computadores onde se encontram instalados agentes está configurada para permitir tráfego esta porta. Pode utilizar scripts do PowerShell para configurar regras de firewall nos seus computadores com Windows, no entanto, terá de configurar manualmente a firewall da rede. Em contrapartida, o ICMP não funciona através da porta. Na maioria dos cenários de empresa, o tráfego ICMP é permitido através de firewalls para permitir a utilização de ferramentas de diagnóstico de rede, como o utilitário de Ping. Por isso, se pode enviar Ping a uma máquina a partir de outra, em seguida, pode utilizar o protocolo ICMP sem ter de configurar manualmente as firewalls. 

>[!NOTE] 
> Alguns firewalls podem bloquear ICMP, que pode levar a retransmissão resultando num grande número de eventos no seu sistema de gestão de informações e o evento de segurança. Certifique-se de que o protocolo que escolheu não está bloqueado por um firewall de rede/NSG, caso contrário NPM não é possível monitorizar o segmento de rede. Por este motivo, recomendamos que utilize TCP para monitorização. Deve utilizar ICMP em cenários onde não são capazes de utilizar o TCP, tais como: 
>
> - Estiver a utilizar nós baseada no cliente do Windows, uma vez que sockets em bruto de TCP não são permitidos no cliente Windows
> - Os blocos de firewall/NSG de rede TCP
> - Como mudar o protocolo 

Se optar por utilizar o ICMP durante a implementação, pode mudar para TCP em qualquer altura, editando as predefinições de monitorização de regra:

1. Navegue para **desempenho de rede** > **Monitor** > **configurar**   >  **Monitor** e, em seguida, clique em **regra predefinida**. 
2. Desloque-se para o **protocolo** secção e selecione o protocolo que pretende utilizar. 
3. Clique em **guardar** para aplicar a definição. 

Mesmo que a regra predefinida está a utilizar um protocolo específico, pode criar novas regras com um protocolo diferente. Pode ainda criar uma mistura de regras de onde algumas das regras de utilizam o ICMP e outro utiliza TCP. 

## <a name="walkthrough"></a>Instruções 

Vamos ver agora que já leu sobre o Monitor de desempenho, uma investigação simple para a causa raiz de um evento de estado de funcionamento.  

No dashboard da solução, repare que há um evento de estado de funcionamento – uma ligação de rede está danificada. Decidir a investigar o problema e clique em **ligações de rede monitorizados** mosaico.

Na página de pesquisa, verifique se o **DMZ2 DMZ1** ligação de rede está danificada. Clique em de **ver ligações de sub-rede** ligação para esta ligação de rede. 


A página de desagregação mostra todas as ligações de sub-rede no **DMZ2 DMZ1** ligação de rede. Tenha em atenção que, para ambas as ligações de sub-rede, a latência ultrapassou o limiar de efetuar a ligação de rede em mau estado de funcionamento. Também pode ver as tendências de latência de ambas as ligações de sub-rede. Pode utilizar a seleção do tempo controlo no gráfico focar-se no intervalo de tempo necessário. Pode ver a hora do dia quando latência atingiu o respetivo horário de pico. Pode procurar posteriormente os registos para este período de tempo investigar o problema. Clique em **ver ligações de nó** para desagregar ainda mais. 
 
 ![Ligações de sub-rede](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Semelhante à página anterior, a página de desagregar para a ligação de determinada sub-rede lista pendente a ligações de nó que constituem. Pode efetuar ações semelhantes aqui, tal como fez no passo anterior. Clique em **topologia de vista** para visualizar a topologia entre dois nós. 
 
 ![Ligações de nó](media/log-analytics-network-performance-monitor/node-links.png) 

Todos os caminhos de entre os dois nós selecionados são desenhados no mapa de topologia. Pode visualizar a topologia de salto por salto de rotas entre dois nós no mapa de topologia. Dá-lhe uma fotografia limpar rotas quantos existam entre dois nós e o que os caminhos de pacotes de dados estão a utilizar. Congestionamentos de desempenho de rede são marcados na cor vermelho. Pode localizar uma ligação de rede defeituoso ou um dispositivo de rede defeituoso observando elementos coloridos vermelhos no mapa de topologia. 

 ![Dashboard de topologia](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

O número de saltos em cada caminho, a latência e perda podem ser revistas no **ação** painel. Utilize a barra de deslocamento para ver os detalhes de nesses caminhos mau estado de funcionamento. Utilize os filtros para selecionar os caminhos com o salto mau estado de funcionamento para que a topologia para apenas os caminhos selecionados é representada. Pode utilizar a roda do rato para aplicar zoom dentro ou fora do mapa de topologia. 

Na abaixo da imagem, claramente pode ver a causa raiz das áreas de problema para a secção específica da rede ao observar os caminhos e saltos na cor vermelho. Clicar num nó no mapa de topologia de revela as propriedades do nó, incluindo o FQDN e o endereço IP. Clicar num salto mostra o endereço IP do salto. 
 
![Dashboard de topologia](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Passos Seguintes
* [Pesquisar registos](log-analytics-log-searches.md) para ver registos de dados de desempenho de rede detalhada.
