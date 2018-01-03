---
title: "Resolução de problemas de desempenho de rede Virtual do Azure | Microsoft Docs"
description: "Esta página fornece um método padronizado para testar o desempenho de ligação de rede do Azure."
services: expressroute
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2017
ms.author: jonor
ms.openlocfilehash: 56f011632a2aa3ef0632efd5ace472c0fc79a329
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="troubleshooting-network-performance"></a>Resolução de problemas de desempenho da rede
## <a name="overview"></a>Descrição geral
O Azure oferece mais rápidas e mais estáveis formas de ligar a partir da sua rede no local ao Azure. Métodos como VPN Site a Site e ExpressRoute com êxito são utilizados pelos clientes grande e em pequenos para executar as respetivas empresas no Azure. Mas, o que acontece quando o desempenho não cumpre as expectativas ou a experiência anterior? Este documento pode ajudá-lo a normalizar a forma de que testar e linha de base de um ambiente específico.

Este documento mostra como pode facilmente e consistentemente testar a latência da rede e largura de banda entre dois anfitriões. Este documento também fornece algumas conselhos sobre formas observar a rede do Azure e ajudar a isolar os pontos de problema. O script do PowerShell e ferramentas abordadas requerem dois anfitriões na rede (em cada extremidade da ligação que está a ser testada). Um anfitrião tem de ser um Windows Server ou o ambiente de trabalho, outros podem ser Windows ou Linux. 

>[!NOTE]
>A abordagem de resolução de problemas, as ferramentas e os métodos utilizados são preferências pessoais. Este documento descreve a abordagem de ferramentas e que devo efetuar frequentemente. A abordagem provavelmente serão diferentes, não há nada errado com diferentes abordagens para resolver o problema. No entanto, se não tiver uma abordagem estabelecida, este documento pode começar no caminho para criar as suas próprias métodos, ferramentas e as preferências de resolução de problemas de rede.
>
>

## <a name="network-components"></a>Componentes de rede
Antes de profundamente para resolução de problemas, vamos discutir sobre alguns termos e componentes comuns. Este debate garante que estiver a pensar sobre cada componente na cadeia de ponto-a-ponto que ativa a conectividade no Azure.
[![1]][1]

O nível mais elevado, posso descrevem três domínios de encaminhamento de rede principais;

- a rede do Azure (nuvem azul à direita)
- da Internet ou de uma WAN (nuvem verde no Centro de)
- a rede empresarial (nuvem peach no lado esquerdo)

Vamos observar o diagrama da direita para a esquerda, abordam brevemente cada componente:
 - **Máquina virtual** - o servidor pode ter vários NICs, certifique-se de que os encaminhamentos estáticos, as rotas predefinidas e definições do sistema operativo estão a enviar e receber tráfego a forma errada-lo. Além disso, cada SKU de VM tem uma restrição de largura de banda. Se estiver a utilizar um SKU de VM mais pequeno, o tráfego é limitado pela largura de banda disponível para o NIC. Normalmente, utilizar um DS5v2 para teste (e, em seguida, eliminar, uma vez concluído com os testes poupar dinheiro) para garantir que a largura de banda suficiente na VM.
 - **NIC** -Certifique-se de que sabe o IP privado atribuído para o NIC em questão.
 - **NIC NSG** - existe poderá ser específicos NSGs aplicados ao nível do NIC, certifique-se de que o conjunto de regras do NSG é adequado para o tráfego que está a tentar passar. Por exemplo, certifique-se as portas 5201 para iPerf, 3389 para RDP ou 22 para SSH estão abertas para permitir tráfego de teste para passar.
 - **Sub-rede da VNet** -a NIC está atribuído a uma sub-rede específica, certifique-se de que sabe que uma e as regras associadas essa sub-rede.
 - **Sub-rede NSG** - tal como a NIC, os NSGs podem ser aplicadas em, bem como a sub-rede. Certifique-se de que o conjunto de regras do NSG é adequado para o tráfego que está a tentar passar. (para o tráfego de entrada para o NIC a sub-rede que NSG aplica-se pela primeira vez, em seguida, o NSG NIC, por outro lado para o tráfego de saída da VM o NSG NIC aplica-se primeiro e o NSG sub-rede entra play).
 - **Sub-rede UDR** -rotas definidas pelo utilizador pode direcionar o tráfego para um salto intermédio (como uma firewall ou Balanceador de carga). Certifique-se de que sabe que o se houver um UDR no local para o tráfego e, se, por isso, onde vão e que esse salto seguinte irá fazer para o tráfego. (por exemplo, uma firewall foi passar algum tráfego e negar o restante tráfego entre os dois anfitriões mesmos).
 - **A sub-rede de gateway / NSG / UDR** -tal como a sub-rede VM, a sub-rede do gateway pode ter NSGs e UDRs. Certifique-se de que sabe se forem existe e que os efeitos terem o tráfego.
 - **Gateway de VNet (ExpressRoute)** -depois de peering (ExpressRoute) ou a VPN estiver ativada, não sabemos de muitas definições que podem afetar como ou, se as rotas de tráfego. Se tiver vários circuitos ExpressRoute ou túneis VPN ligados ao mesmo Gateway de VNet, deve ter em consideração as definições de peso de ligação como preferência de ligação esta definição afeta e afeta o caminho que demora o tráfego.
 - **Encaminhar o filtro** (não mostrado) - um filtro de rota só se aplica ao Peering da Microsoft no ExpressRoute, mas é essencial para verificar se não está a ver as rotas que espera no Peering da Microsoft. 

Neste momento, está na parte da ligação WAN. Este domínio de encaminhamento pode ser o fornecedor de serviços, a sua WAN empresarial ou a Internet. Muitos saltos, tecnologias e as empresas envolvidos com estas ligações podem tornar um pouco difícil resolução de problemas. Muitas vezes, trabalha para a regra de saída do Azure e a redes empresariais primeiro antes de jumping para esta coleção de empresas e saltos.

No diagrama anterior, na extremidade esquerda é sua rede empresarial. Dependendo do tamanho da sua empresa, este domínio de encaminhamento pode ser alguns dispositivos de rede entre si e a WAN ou várias camadas de dispositivos numa rede campus/enterprise.

Tendo em conta as complexidades destes ambientes de rede de alto nível de diferentes três, frequentemente é o ideal para começar a nas margens e tente mostram onde é um bom desempenho, e onde degrada. Esta abordagem pode ajudar a identificar o domínio de encaminhamento de problema das três e, em seguida, concentre-se a resolução de problemas nesse ambiente específico.

## <a name="tools"></a>Ferramentas
A maioria dos problemas de rede podem ser analisados e isolada utilizando ferramentas básicas de como o ping e traceroute. É raro que terá de ir mais profundo uma análise do pacote como Wireshark. Para ajudar a resolver problemas, o Toolkit de conectividade do Azure (AzureCT) foi desenvolvido para colocar algumas destas ferramentas num pacote fácil. Para fins de teste de desempenho, posso, como utilizar iPerf e PSPing. iPerf é uma ferramenta frequentemente utilizada e executa a maioria dos sistemas operativos. iPerf é boa para testes os desempenhos básico e é bastante fáceis de utilizar. PSPing é uma ferramenta de ping desenvolvida pela SysInternals. PSPing é uma forma fácil de efetuar os pings ICMP e TCP no comando também fáceis de utilizar um. Ambas estas ferramentas são simples e são "instaladas" simplesmente por coping os ficheiros para um diretório no anfitrião.

Tiver encapsulada todas estas ferramentas e métodos para um módulo do PowerShell (AzureCT) que pode instalar e utilizar.

### <a name="azurect---the-azure-connectivity-toolkit"></a>AzureCT - o Toolkit de conectividade do Azure
O módulo do AzureCT PowerShell tem dois componentes [testes de disponibilidade] [ Availability Doc] e [teste de desempenho][Performance Doc]. Este documento só estiver preocupado com o teste de desempenho, por isso permite focarem-se nos dois comandos de desempenho de ligação neste módulo do PowerShell.

Existem três passos básicos para utilizar este toolkit para testar o desempenho. 1) instale o módulo do PowerShell, 2) instale o suporte de aplicações iPerf e PSPing 3) execute o teste de desempenho.

1. Instalar o módulo do PowerShell

    ```powershell
    (new-object Net.WebClient).DownloadString("https://aka.ms/AzureCT") | Invoke-Expression
    
    ```

    Este comando descarrega o módulo do PowerShell e instala-o localmente.

2. Instalar as aplicações de suporte
    ```powershell
    Install-LinkPerformance
    ```
    Este comando AzureCT instala iPerf e PSPing num diretório novo "C:\ACTTools", também abre as portas de Firewall do Windows para permitir o protocolo ICMP e porta 5201 (iPerf) tráfego.

3. Execute o teste de desempenho

    Em primeiro lugar, no anfitrião remoto tem de instalar e executar iPerf no modo de servidor. Certifique-se também de 22 (SSH para Linux) e a permitir o tráfego na porta 5201 para iPerf ou o anfitrião remoto está a escutar o 3389 (RDP para Windows). Se o anfitrião remoto é o windows, instale o AzureCT e execute o comando de instalação LinkPerformance configurar iPerf e as regras de firewall necessárias para iniciar iPerf no modo de servidor com êxito. 
    
    Assim que o computador remoto estiver pronto, abra o PowerShell no computador local e iniciar o teste:
    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 10
    ```

    Este comando executa uma série de carga em simultâneo e testes de latência para ajudar a calcular a capacidade de largura de banda e latência de ligação de rede.

4. Reveja o resultado dos testes

    O formato de saída do PowerShell semelhante a:

    [![4]][4]

    Os resultados detalhados do todos os testes de PSPing e iPerf são nos ficheiros de texto individuais no diretório de ferramentas AzureCT em "C:\ACTTools."

## <a name="troubleshooting"></a>Resolução de problemas
Se o teste de desempenho não está a dar resultados esperados, perceber a razão pela deve ser um processo passo a passo transferência progressiva. Uma abordagem systematic, geralmente, tendo em conta o número de componentes no caminho, fornece um caminho mais rápido para a resolução do jumping em torno e perdas de tempo potencialmente fazer o mesmo teste de várias vezes.

>[!NOTE]
>O cenário aqui é um problema de desempenho, não é um problema de conectividade. Os passos seria diferentes se tráfego não foi passar em todos os.
>
>

Em primeiro lugar, os pressupostos de desafio. É a expetativa razoável? Por exemplo, se tiver um circuito de ExpressRoute 1 Gbps e 100 ms da latência é unreasonable esperar o completa 1 Gbps de tráfego fornecido as características de desempenho do TCP através de ligações de latência elevada. Consulte o [referencia secção](#references) para obter mais pressupostos de desempenho no.

Em seguida, recomendamos que começa com os limites entre domínios de encaminhamento e tente isolar o problema para um único domínio de encaminhamento principais; a rede empresarial, WAN ou a rede do Azure. As pessoas, muitas vezes, blame "caixa negra" no caminho, ao blaming caixa negra é fácil fazer, significativamente pode atrasar resolução especialmente se o problema, na verdade, uma área que tem a capacidade de efetuar alterações. Certifique-se de que efetua a devida diligence antes de manipulação desativado para o fornecedor de serviços ou o ISP.

Depois de ter identificado o domínio de encaminhamento principais que aparece conter o problema, deve criar um diagrama da área em questão. Num whiteboard, o bloco de notas ou o Visio como um diagrama proporciona um concreto "mapa luta" para permitir uma abordagem methodical para isolar mais o problema. Pode planear pontos de teste e atualizar o mapa como limpar áreas ou dig mais aprofundada, como o avança ser teste.

Agora que tem um diagrama, inicie dividir a rede em segmentos e restringir o problema. Saber em que funciona e onde não. Manter a mover os pontos de teste para isolar para baixo até o componente inválido.

Além disso, não se esqueça de ver outras camadas do modelo OSI. É fácil concentrar-se em da rede e de camadas de 1 a 3 (camadas físico, dados e rede), mas os problemas podem também ser cópias de segurança na camada 7 camada da aplicação. Manter uma consideração aberta e certifique-se pressupostos.

## <a name="advanced-expressroute-troubleshooting"></a>ExpressRoute avançadas de resolução de problemas
Se não tiver a certeza em que é realmente o limite da nuvem, a isolar os componentes do Azure pode ser um desafio. Quando é utilizado o ExpressRoute, o limite é um componente de rede denominado do Microsoft Enterprise Edge (MSEE). **Ao utilizar o ExpressRoute**, o MSEE é o primeiro ponto de contacto na rede da Microsoft e o último salto mantendo a rede da Microsoft. Quando cria um objeto de ligação entre o gateway de VNet e o circuito do ExpressRoute, na verdade, está a efetuar uma ligação do MSEE. Reconhecer do MSEE, tal como o salto primeiro ou último (consoante a direção que vai) é fundamental para isolar problemas de rede do Azure para qualquer provar que é o problema no Azure ou ainda mais downstream na WAN ou à rede empresarial. 

[![2]][2]

>[!NOTE]
> Tenha em atenção que o MSEE não se encontra na nuvem do Azure. O ExpressRoute é realmente na periferia da rede da Microsoft não realmente no Azure. Assim que o se estiver ligado com o ExpressRoute para uma MSEE, se estiver ligado à rede da Microsoft, a partir daí, em seguida, pode aceder a qualquer um dos serviços cloud, como o Office 365 (com o Peering da Microsoft) ou do Azure (com o Peering da Microsoft e/ou privada).
>
>

Se estiverem ligadas duas VNets (VNets A e B no diagrama) para o **mesmo** circuito do ExpressRoute, pode realizar uma série de testes para isolar o problema no Azure (ou provar a não se encontra no Azure)
 
### <a name="test-plan"></a>Plano de teste
1. Execute o teste Get-LinkPerformance entre VM1 e VM2. Este teste fornece informações aprofundadas sobre se o problema é local ou não. Se este teste produz latência aceitável e resultados de largura de banda, pode marcar a rede de VNet local como boa.
2. Partindo do princípio de local VNet o tráfego é boa, execute o teste Get-LinkPerformance entre VM1 e VM3. Este teste exercises a ligação através da rede da Microsoft para baixo até o MSEE e no Azure. Se este teste produz latência aceitável e resultados de largura de banda, pode marcar a rede do Azure como boa.
3. Se o Azure é ruled, pode executar uma sequência de testes semelhante na sua rede empresarial. Se o que também testes bem, está na altura de trabalhar com o fornecedor de serviços ou ISP para diagnosticar a ligação WAN. Exemplo: Executar este teste entre dois escritórios ou entre o suporte técnico e um servidor de centro de dados. Consoante o que estiver a testar, encontrar pontos finais (servidores, computadores, etc.) que podem exercer nesse caminho.

>[!IMPORTANT]
> É fundamental que para cada teste marcar a hora do dia a que execução do teste e registe os resultados numa localização comum (posso como OneNote ou Excel). Cada execução do teste deve ter saída idêntica para que possa compare os dados resultantes entre execuções do teste e não tem "holes" nos dados. A consistência entre vários testes é o principal motivo que devo utilizar o AzureCT para resolução de problemas. Não é a magia nos cenários de carga exato posso executar, mas em vez disso, o *magic* é o facto de que é apresentada uma *saída de teste e os dados consistente* de teste de todas as. O tempo de gravação e a presença de dados consistentes sempre único são especialmente útil se encontrar mais tarde se o problema está esporádicas. Ser diligent com a recolha de dados adiantado e irá evitar horas de retesting os cenários mesmos (posso aprendidas desta forma de disco rígida muitos anos, há).
>
>

## <a name="the-problem-is-isolated-now-what"></a>O problema está isolado, que agora?
Mais pode isolar o problema mais fácil é para corrigir, no entanto, muitas vezes, atingir o ponto em que não é possível voltar mais aprofundada ou adicional com a resolução de problemas. Exemplo: consulte a ligação entre o fornecedor de serviços demorar saltos através da Europa, mas o caminho esperado é todos na Ásia. Este ponto é quando deve entrar para obter ajuda. Quem que faz é dependente isolada o problema para o domínio de encaminhamento, ou mesmo melhor se conseguir restringi-lo para um componente específico.

Para problemas de rede empresarial, o departamento de TI interno ou o fornecedor de serviços de suporte de rede (que pode ser o fabricante de hardware) poderá ajudar a configuração do dispositivo ou a reparação de hardware.

A WAN, partilhar os resultados do teste com o fornecedor de serviços ou ISP pode ajudar a obtê-los iniciado e evitar que abrangem algumas das zero mesmo que ter sido testado já. No entanto, a não ser offended se pretenderem verificar os resultados próprios. "Confiar mas certifique-se" é uma boa lema quando a resolução de problemas com base nos resultados comunicados outras pessoas.

Com o Azure, depois de isolar o problema como muito mais detalhadamente como poderá, está na altura de rever o [documentação de rede de Azure] [ Network Docs] e, em seguida, se ainda necessária [abrir um pedido de suporte][Ticket Link].

## <a name="references"></a>Referências
### <a name="latencybandwidth-expectations"></a>Expectativas de latência/largura de banda
>[!TIP]
> Latência geográfica (quilómetros ou kilometers) entre os pontos finais que está a testar por até que ponto é o componente maior de latência. Enquanto existir latência de equipamento (componentes físicos e virtuais, número de saltos, etc.) envolvidas, geografia terminada para ser o componente maior de latência global ao lidar com ligações WAN. Também é importante ter em atenção que a distância é a distância do fibra executar não o valor ou a distância do mapa da estrada. Este distância é incredibly difícil obter com qualquer precisão. Como resultado, posso geralmente utilizar uma calculadora de distância cidade na internet e saber o que este método é uma medida grossly incorreta, mas é suficiente para definir uma expetativa geral.
>
>

Posso já tem um programa de configuração do ExpressRoute em Seattle, Washington, em a EUA. A tabela seguinte mostra a latência e largura de banda posso vimos testes em várias localizações do Azure. Tiver estimar a distância geográfica entre cada final do teste.

Configuração de teste:
 - Um servidor físico com o Windows Server 2016 com um 10 Gbps NIC, ligados a um circuito ExpressRoute.
 - Um circuito do Premium ExpressRoute 10Gbps na localização identificada com o Peering privado ativado.
 - Uma VNet do Azure com um gateway de UltraPerformance na região especificada.
 - Uma VM DS5v2 a executar o Windows Server 2016 na VNet. A VM foi sem domínio associado, criados a partir da predefinição imagem do Azure (sem otimização ou personalização) com AzureCT instalado.
 - Todos os testes estava a utilizar o comando AzureCT Get-LinkPerformance com um teste de carga de 5 minutos para cada um dos seis de teste executam. Por exemplo:

    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 300
    ```
 - O fluxo de dados de cada teste tinha a carga que fluem do local no servidor físico (iPerf do cliente em Seattle) até que a VM do Azure (servidor iPerf na região do Azure listada).
 - Os dados da coluna "Latência" são do teste de carga não (um teste de latência TCP sem iPerf em execução).
 - Os dados da coluna "Máxima da largura de banda" são do teste de carga de fluxo TCP 16 com um tamanho de janela de 1 Mb.

[![3]][3]

### <a name="latencybandwidth-results"></a>Resultados de latência/largura de banda
>[!IMPORTANT]
> Estes números destinam-se apenas a referência geral. Vários fatores afetam a latência e enquanto estes valores são geralmente consistentes ao longo do tempo, condições dentro do Azure ou a rede de fornecedores de serviços podem enviar tráfego através de caminhos diferentes em qualquer altura, deste modo, latência e largura de banda podem ser afetados. Geralmente, os efeitos destas alterações não resultarem numa diferenças significativas.
>
>

| | | | | | |
|-|-|-|-|-|-|
|ExpressRoute<br/>Localização|Azure<br/>Região|Estimado<br/>Distância (km)|Latência|1 sessão<br/>Largura de Banda|Máximo<br/>Largura de Banda|
| Seattle | EUA Oeste 2        |    191 km |   5 ms | 262.0 Mbits/s |  3.74 Gbits/seg | 21
| Seattle | EUA Oeste          |  1,094 km |  18 ms |  82.3 Mbits/s |  3.70 Gbits/seg | 20
| Seattle | EUA Central       |  2,357 km |  40 ms |  38.8 Mbits/s |  2.55 Gbits/seg | 17
| Seattle | EUA Centro-Sul |  2,877 km |  51 ms |  30.6 Mbits/s |  2.49 Gbits/seg | 19
| Seattle | EUA Centro-Norte |  2,792 km |  55 ms |  27.7 Mbits/s |  2.19 Gbits/seg | 18
| Seattle | EUA Leste 2        |  3,769 km |  73 ms |  21.3 Mbits/s |  1.79 Gbits/seg | 16
| Seattle | EUA Leste          |  3,699 km |  74 ms |  21.1 Mbits/s |  1.78 Gbits/seg | 15
| Seattle | Leste do Japão       |  7,705 km | 106 ms |  14.6 Mbits/s |  1.22 Gbits/seg | 28
| Seattle | Reino Unido Sul         |  7,708 km | 146 ms |  10.6 Mbits/s |   896 Mbits/s | 24
| Seattle | Europa Ocidental      |  7,834 km | 153 ms |  10.2 Mbits/s |   761 Mbits/s | 23
| Seattle | Leste da Austrália   | 12,484 km | 165 ms |   9.4 Mbits/s |   794 Mbits/s | 26
| Seattle | Sudeste Asiático   | 12,989 km | 170 ms |   9.2 Mbits/s |   756 Mbits/s | 25
| Seattle | Sul do Brasil *   | 10,930 km | 189 ms |   8.2 Mbits/s |   699 Mbits/s | 22
| Seattle | Sul da Índia      | 12,918 km | 202 ms |   7.7 Mbits/s |   634 Mbits/s | 27

\*A latência para Brasil é um bom exemplo onde a distância valor significativamente difere de acordo com a executar a distância de fibra. Posso seria de esperar que a latência seria no neighborhood de 160 ms, mas é, na verdade, 189 ms. Esta diferença contra os meus expectativa pode indicar um problema de rede num local, mas provavelmente que execute a fibra não ir para Brasil numa linha direitas e tem um km 1.000 adicional ou para levar para aproveitar Brasil Seattle.

## <a name="next-steps"></a>Passos Seguintes
1. Transferir o Toolkit de conectividade do Azure a partir do GitHub em [http://aka.ms/AzCT][ACT]
2. Siga as instruções para [ligação de teste de desempenho][Performance Doc]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-network-performance/network-components.png "componentes de rede do azure"
[2]: ./media/expressroute-troubleshooting-network-performance/expressroute-troubleshooting.png "resolução de problemas do ExpressRoute"
[3]: ./media/expressroute-troubleshooting-network-performance/test-diagram.png "ambiente de teste de desempenho"
[4]: ./media/expressroute-troubleshooting-network-performance/powershell-output.png "saída do PowerShell"

<!--Link References-->
[Performance Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/PerformanceTesting.md
[Availability Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/AvailabilityTesting.md
[Network Docs]: https://docs.microsoft.com/azure/index#pivot=services&panel=network
[Ticket Link]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview
[ACT]: http://aka.ms/AzCT











