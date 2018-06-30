---
title: Configurar o Monitor de desempenho de rede para circuitos do ExpressRoute do Azure | Microsoft Docs
description: Configure monitorização (NPM) para circuitos do ExpressRoute do Azure de rede baseado na nuvem. Isto inclui a monitorização através de peering privado do ExpressRoute e peering da Microsoft.
documentationcenter: na
services: expressroute
author: cherylmc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2018
ms.author: cherylmc
ms.openlocfilehash: 47f219b7319e4d2bbadf03954f7bd7f6f39da3b4
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128984"
---
# <a name="configure-network-performance-monitor-for-expressroute"></a>Configurar o Monitor de Desempenho de Rede para o ExpressRoute

Monitor de desempenho de rede (NPM) é uma solução que monitoriza a conectividade entre as implementações de nuvem do Azure e localizações no local (sucursais, etc.) de monitorização de rede baseado na nuvem. NPM faz parte da análise de registos. NPM oferece uma extensão para o ExpressRoute permite-lhe monitorizar o desempenho de rede através de circuitos ExpressRoute que estão configurados para utilizar o peering privado ou peering da Microsoft. Quando configurar NPM para o ExpressRoute, pode detetar problemas de rede para identificar e eliminar. Este serviço também está disponível para a nuvem do Azure Government.

Pode:

* Monitorizar a perda e latência em várias VNets e definir alertas

* Monitorizar todos os caminhos (incluindo caminhos redundantes) na rede

* Resolver problemas de rede transitórios e ponto no tempo que são difíceis de replicar

* Ajudar a determinar um segmento específico na rede que é responsável por degradação do desempenho

* Obter o débito por rede virtual (se tiver agentes instalados em cada VNet)

* Consulte o estado do sistema ExpressRoute de um ponto anterior no tempo

## <a name="workflow"></a>Workflow

Se encontram instalados agentes de monitorização em vários servidores, no local e no Azure. Os agentes de comunicarem entre si, mas não enviam dados, enviam pacotes de handshake TCP. A comunicação entre os agentes permite ao Azure mapear a topologia de rede e o caminho que o tráfego pode demorar algum.

1. Crie uma área de trabalho do NPM. Este é o mesmo que uma área de trabalho do OMS.
2. Instalar e configurar agentes de software: 
    * Instale agentes de monitorização de servidores no local e as VMs do Azure (para o peering privado).
    * Configure as definições nos servidores do agente de monitorização para permitir que os agentes de monitorização comunicar. (As portas de firewall aberta, etc.)
3. Configurar regras para permitir que o agente de monitorização instalado em VMs do Azure para comunicar com no local (NSG) do grupo de segurança de rede os agentes de monitorização.
4. Configurar a monitorização: detetar automaticamente e gerir quais as redes são visíveis no NPM.

Se já estiver a utilizar o Monitor de desempenho de rede para monitorizar os outros objetos ou os serviços e já tiver área de trabalho de uma das regiões suportadas, pode ignorar o passo 1 e o passo 2 e iniciar a configuração com o passo 3.

## <a name="configure"></a>Passo 1: Criar uma área de trabalho

Crie uma área de trabalho na subscrição com a ligação de VNets para circuit(s) o ExpressRoute.

1. No [portal do Azure](https://portal.azure.com), selecione a subscrição que tenha as VNETs em modo de peering para o circuito do ExpressRoute. Em seguida, pesquise a lista de serviços no **Marketplace** 'Monitor de desempenho de rede'. No retorno, clique para abrir o **Monitor de desempenho de rede** página.

   >[!NOTE]
   >Pode criar uma área de trabalho nova ou utilize uma área de trabalho existente. Se pretender utilizar uma área de trabalho existente, tem de se certificar de que a área de trabalho foi migrada para o novo idioma de consulta. [Obter mais informações...](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search-upgrade)
   >

   ![portal](.\media\how-to-npm\3.png)<br><br>
2. Na parte inferior do principal **Monitor de desempenho de rede** página, clique em **criar** para abrir **Monitor de desempenho de rede - criar nova solução** página. Clique em **área de trabalho do OMS - Selecione uma área de trabalho** para abrir a página de áreas de trabalho. Clique em **+ criar nova área de trabalho** para abrir a página da área de trabalho.
3. No **área de trabalho OMS** página, selecione **criar novo**, em seguida, configure as seguintes definições:

  * Área de trabalho OMS - escreva um nome para a sua área de trabalho.
  * Subscrição - se tiver várias subscrições, selecione aquela que pretende associar à área de trabalho de novo.
  * Grupo de recursos - criar um grupo de recursos ou utilize uma já existente.
  * Localização - nesta localização é utilizada para especificar a localização da conta de armazenamento que é utilizada para os registos de ligação do agente.
  * Escalão de preço - selecione o escalão de preço.
  
    >[!NOTE]
    >O circuito de ExpressRoute pode estar em qualquer parte do mundo. Não tem de estar na mesma região que a área de trabalho.
    >
  
    ![área de trabalho](.\media\how-to-npm\4.png)<br><br>
4. Clique em **OK** para guardar e implementar o modelo de definições. Assim que valida o modelo, clique em **criar** para implementar a área de trabalho.
5. Depois da área de trabalho ter sido implementada, navegue para o **NetworkMonitoring(name)** recursos que criou. Valide as definições, em seguida, clique em **solução requer configuração adicional**.

   ![configuração adicional](.\media\how-to-npm\5.png)

## <a name="agents"></a>Passo 2: Instalar e configurar agentes

### <a name="download"></a>2.1: Transfira o ficheiro de configuração do agente

1. Vá para o **definições comuns** separador do **configuração de Monitor de desempenho de rede** página para o seu recurso. Clique no agente que corresponde ao processador do servidor a **instalar agentes de OMS** secção e transfira o ficheiro de configuração.
2. Em seguida, copie o **ID da área de trabalho** e **chave primária** para bloco de notas.
3. Do **configurar agentes OMS para a monitorização utilizando o protocolo TCP** secção, transfira o Script do Powershell. O script do PowerShell ajuda-o a abrir a porta de firewall relevantes para as transações de TCP.

  ![Script do PowerShell](.\media\how-to-npm\7.png)

### <a name="installagent"></a>2.2: instalar um agente de monitorização em cada servidor de monitorização (em cada VNET que pretende monitorizar)

Recomendamos que instale, pelo menos, dois agentes em cada lado da ligação ExpressRoute para redundância (por exemplo, no local, as VNETs do Azure). O agente tem de ser instalado num servidor do Windows (2008 SP1 ou posterior). Não é suportada a monitorização circuitos do ExpressRoute com o sistema operativo de ambiente de trabalho do Windows e com SO Linux. Utilize os seguintes passos para instalar agentes:
   
  >[!NOTE]
  >Agentes enviado por SCOM (inclui [MMA](https://technet.microsoft.com/library/dn465154(v=sc.12).aspx)) poderá não conseguir detetar consistentemente a respetiva localização se estes estão alojados no Azure. Recomendamos que não utilize estes agentes as VNETs do Azure para monitorizar o ExpressRoute.
  >

1. Executar **configuração** para instalar o agente em cada servidor que pretende utilizar para a monitorização do ExpressRoute. O servidor que utiliza para a monitorização pode ser uma VM ou no local e tem de ter acesso à Internet. Terá de instalar pelo menos um agente no local e um agente em cada segmento de rede que pretende monitorizar no Azure.
2. Na página **Bem-vindo**, clique em **Seguinte**.
3. No **termos de licenciamento** página, leia a licença e, em seguida, clique em **concordo**.
4. No **pasta de destino** página, altere ou mantenha a pasta de instalação predefinida e, em seguida, clique em **seguinte**.
5. No **opções de configuração do agente** página, pode optar por ligar o agente ao Log Analytics do Azure ou o Operations Manager. Em alternativa, pode deixar as escolhas em branco se pretender configurar o agente mais tarde. Depois de efetuar o selection(s), clique em **seguinte**.

  * Se tiver optado por ligar para **Log Analytics do Azure**, cole o **ID da área de trabalho** e **chave da área de trabalho** (chave primária) que copiou no bloco de notas na secção anterior. Clique depois em **Seguinte**.

    ![ID e a chave](.\media\how-to-npm\8.png)
  * Se tiver optado por ligar para **do Operations Manager**, no **configuração do grupo de gestão** , escreva o **Management Group Name**, **servidor de gestão** e o **porta do servidor de gestão**. Clique depois em **Seguinte**.

    ![Operations Manager](.\media\how-to-npm\9.png)
  * No **conta de ação do agente** página, escolha o o **Sistema Local** conta, ou **domínio ou conta de computador Local**. Clique depois em **Seguinte**.

    ![Conta](.\media\how-to-npm\10.png)
6. No **pronto para instalar** página, reveja as suas opções e, em seguida, clique em **instalar**.
7. Na página **Configuração Concluída com Êxito**, clique em **Concluir**.
8. Quando terminar, o Microsoft Monitoring Agent é apresentado no painel de controlo. Pode rever a configuração não existe e certifique-se de que o agente está ligado a análise de registos do Azure (OMS). Quando estiver ligado, o agente é apresentada uma mensagem a indicar: **o Microsoft Monitoring Agent foi ligado com êxito para o serviço do Microsoft Operations Management Suite**.

9. Repita este procedimento para cada VNET que precisa de ser monitorizados.

### <a name="proxy"></a>2.3: Configurar definições de proxy (opcionais)

Se estiver a utilizar um proxy web para aceder à Internet, utilize os seguintes passos para configurar definições de proxy para o Microsoft Monitoring Agent. Execute estes passos para cada servidor. Se tiver vários servidores que necessita configurar, poderá considerar mais fácil utilizar um script para automatizar este processo. Se Sim, consulte [para configurar definições de proxy para o Microsoft Monitoring Agent utilizando um script](../log-analytics/log-analytics-windows-agent.md).

Para configurar definições de proxy para o Microsoft Monitoring Agent através do painel de controlo:

1. Abra o **painel de controlo**.
2. Abra o **Agente de Monitorização da Microsoft**.
3. Clique no separador **Definições de Proxy**.
4. Selecione **utilizar um servidor proxy** e escreva o URL e a porta número, se necessitar de um. Se o servidor proxy requer autenticação, escreva o nome de utilizador e a palavra-passe para aceder ao servidor proxy.

  ![proxy](.\media\how-to-npm\11.png)

### <a name="verifyagent"></a>2.4: verificar a conectividade de agente

Pode facilmente verificar se os agentes estão a comunicar.

1. Num servidor com o agente de monitorização, abra o **painel de controlo**.
2. Abra o **Microsoft Monitoring Agent**.
3. Clique em de **Log Analytics do Azure** separador.
4. No **estado** coluna, deverá ver que o agente ligado com êxito à análise de registos.

  ![status](.\media\how-to-npm\12.png)

### <a name="firewall"></a>2.5: abrir as portas de firewall nos servidores de agente de monitorização

Para utilizar o protocolo TCP, é necessário abrir as portas de firewall para se certificar de que os agentes de monitorização podem comunicar.

Pode executar um script do PowerShell para criar as chaves de registo que são necessários para o Monitor de desempenho de rede. Este script cria também as regras de Firewall do Windows para permitir a monitorização de agentes para criar ligações TCP entre si. As chaves de registo criadas pelo script Especifique se pretende registar os registos de depuração e o caminho para o ficheiro de registos. Também define a porta TCP de agente utilizada para comunicação. Os valores para estas chaves são configurados automaticamente pelo script. Não deve alterar manualmente estas chaves.

Porta 8084 está aberta por predefinição. Pode utilizar uma porta personalizada, fornecendo o parâmetro 'portNumber' para o script. No entanto, se o fizer, tem de especificar a mesma porta para todos os servidores em que executa o script.

>[!NOTE]
>O script do PowerShell 'EnableRules' configura regras de Firewall do Windows apenas no servidor onde o script é executado. Se tiver uma firewall de rede, deve certificar-se de que permite o tráfego destinado a porta TCP que está a ser utilizada pelo Monitor de desempenho de rede.
>
>

Nos servidores de agente, abra uma janela do PowerShell com privilégios administrativos. Execute o [EnableRules](https://aka.ms/npmpowershellscript) script do PowerShell (que transferiu anteriormente). Não utilize parâmetros.

![PowerShell_Script](.\media\how-to-npm\script.png)

## <a name="opennsg"></a>Passo 3: Configurar regras de grupo de segurança de rede

Para monitorizar os servidores de agente que se encontrem no Azure, tem de configurar regras para permitir tráfego TCP uma porta utilizada pelo NPM para transações sintéticas (NSG) do grupo de segurança de rede. A porta predefinida é 8084. Isto permite que um agente de monitorização instalado na VM do Azure para comunicar com um local agente de monitorização.

Para mais informações sobre o NSG, consulte [grupos de segurança de rede](../virtual-network/virtual-networks-create-nsg-arm-portal.md).

>[!NOTE]
>Certifique-se de que instalou os agentes (o agente de servidor no local e o agente do servidor do Azure) e ter a executar o script do PowerShell antes de continuar com este passo.
>

## <a name="setupmonitor"></a>Passo 4: Detetar ligações de peering

1. Navegue para o mosaico de descrição geral do Monitor de desempenho de rede, acedendo ao **todos os recursos** página, em seguida, clique na na lista de permissões NPM área de trabalho.

  ![área de trabalho de npm](.\media\how-to-npm\npm.png)
2. Clique em de **Monitor de desempenho de rede** mosaico de descrição geral para trazer o dashboard. O dashboard contém uma página do ExpressRoute, que mostra que o ExpressRoute é um 'Estado não configurado'. Clique em **funcionalidade configuração** para abrir a página de configuração de Monitor de desempenho de rede.

  ![configuração de funcionalidade](.\media\how-to-npm\npm2.png)
3. Na página de configuração, navegue até ao separador 'ExpressRoute Peerings', localizado no painel do lado esquerdo. Em seguida, clique em **detetar agora**.

  ![detetar](.\media\how-to-npm\13.png)
4. Quando tiver concluído a deteção, verá uma lista que contém os seguintes itens:
  * Todas as ligações de peering da Microsoft no circuit(s) ExpressRoute que estão associados esta subscrição.
  * Todas as ligações de peering privadas que se ligam às VNets associado a esta subscrição.
            
## <a name="configmonitor"></a>Passo 5: Configurar monitores

Nesta secção, configure os monitores. Siga os passos para o tipo de peering que pretende monitorizar: **peering privado**, ou **peering da Microsoft**.

### <a name="private-peering"></a>Peering privado

Para o peering privado, quando a deteção estiver concluída, será apresentado será regras para exclusivo **circuito nome** e **nome da VNet**. Inicialmente, estas regras estão desativadas.

![regras](.\media\how-to-npm\14.png)

1. Verifique o **monitorizar este peering** caixa de verificação.
2. Selecione a caixa de verificação **Ativar monitorização do Estado de funcionamento para este peering**.
3. Escolha as condições de monitorização. Pode definir limiares personalizados para gerar eventos de estado de funcionamento, escrevendo os valores de limiar. Sempre que o valor da condição ultrapassar o limiar selecionado para o par sub-rede selecionada rede, é gerado um evento de estado de funcionamento.
4. Clique em agentes ON PREM **adicionar agentes** botão para adicionar os servidores no local a partir do qual pretende monitorizar a ligação de peering privada. Certifique-se de que escolhe apenas os agentes que têm conectividade para o ponto final de serviço Microsoft que especificou na secção para o passo 2. Os agentes no local tem de ser capazes de alcançar o ponto final utilizando a ligação do ExpressRoute.
5. Guarde as definições.
6. Depois de ativar as regras e selecionar os valores e os agentes que pretende monitorizar, há um Aguarde aproximadamente 30-60 minutos para os valores começar a preencher e **ExpressRoute monitorização** mosaicos fique disponível.

### <a name="microsoft-peering"></a>Peering da Microsoft

Para peering da Microsoft, clique o connection(s) de peering da Microsoft que pretende monitorizar e configurar as definições.

1. Verifique o **monitorizar este peering** caixa de verificação. 
2. (Opcional) Pode alterar o ponto de final de serviço da Microsoft de destino. Por predefinição, o NPM opta por um ponto final de serviço de Microsoft como destino. NPM monitoriza a conectividade dos seus servidores no local para este ponto final de destino através do ExpressRoute. 
    * Para alterar este ponto final de destino, clique o **(editar)** ligação em **destino:** e selecione outro ponto final do destino de serviço Microsoft da lista de URLs.
      ![editar destino](.\media\how-to-npm\edit_target.png)<br>

    * Pode utilizar um URL ou o endereço IP personalizado. Esta opção é especialmente relevante se estiver a utilizar o peering para estabelecer uma ligação aos serviços do Azure PaaS, como o Storage do Azure, bases de dados do SQL Server e Web sites que são oferecidos em endereços IP públicos da Microsoft. Para tal, clique na ligação **(utilizar o URL personalizado ou endereço IP em vez disso)** na parte inferior da lista de URL, em seguida, introduza o ponto final público do seu serviço de Azure PaaS que está ligado através do peering da Microsoft do ExpressRoute.
    ![URL personalizado](.\media\how-to-npm\custom_url.png)<br>

    * Se estiver a utilizar estas definições opcionais, certifique-se de que apenas o Microsoft ponto final de serviço está selecionado aqui. O ponto final tem de ser ligada ao ExpressRoute e pode ser acedida por agentes no local.
3. Selecione a caixa de verificação **Ativar monitorização do Estado de funcionamento para este peering**.
4. Escolha as condições de monitorização. Pode definir limiares personalizados para gerar eventos de estado de funcionamento, escrevendo os valores de limiar. Sempre que o valor da condição ultrapassar o limiar selecionado para o par sub-rede selecionada rede, é gerado um evento de estado de funcionamento.
5. Clique em agentes ON PREM **adicionar agentes** botão para adicionar os servidores no local a partir do qual pretende monitorizar a ligação de peering da Microsoft. Certifique-se de que escolhe apenas agentes que tenham conectividade com os pontos finais de serviço Microsoft que especificou na secção para o passo 2. Os agentes no local tem de ser capazes de alcançar o ponto final utilizando a ligação do ExpressRoute.
6. Guarde as definições.
7. Depois de ativar as regras e selecionar os valores e os agentes que pretende monitorizar, há um Aguarde aproximadamente 30-60 minutos para os valores começar a preencher e **ExpressRoute monitorização** mosaicos fique disponível.

## <a name="explore"></a>Passo 6: Ver mosaicos de monitorização

Depois de ver os mosaicos de monitorização, os circuitos ExpressRoute e recursos de ligação estão a ser monitorizados pelo NPM. Pode clicar num mosaico de Peering da Microsoft para desagregar o estado de funcionamento de ligações de Peering da Microsoft.

![monitorização de mosaicos](.\media\how-to-npm\15.png)

### <a name="dashboard"></a>Página de Monitor de desempenho de rede

A página NPM contém uma página para o ExpressRoute que mostra uma descrição geral do Estado de funcionamento dos circuitos ExpressRoute e peerings.

![Dashboard](.\media\how-to-npm\dashboard.png)

### <a name="circuits"></a>Lista de circuitos

Para ver uma lista de todos os monitorizado circuitos do ExpressRoute, clique em de **circuitos ExpressRoute** mosaico. Pode selecionar um circuito e ver o estado de funcionamento, gráficos de tendência de perda de pacotes, utilização de largura de banda e latência. Os gráficos são interativos. Pode selecionar uma janela de tempo personalizadas para representar os gráficos. Pode arrastar o rato através de uma área no gráfico para ampliar e ver os pontos de dados de detalhado.

![circuit_list](.\media\how-to-npm\circuits.png)

#### <a name="trend"></a>Tendência de perda, débito e latência

Os gráficos de largura de banda, a latência e perda são interativos. Pode ampliar em qualquer secção nestes gráficos, utilizar controlos de rato. Também pode ver as largura de banda, a latência e perda de dados para outros intervalos clicando **data/hora**, localizado abaixo do botão de ações no canto superior esquerdo.

![tendência](.\media\how-to-npm\16.png)

### <a name="peerings"></a>Lista de Peerings

Para ver a lista de todas as ligações a redes virtuais através de peering privado, clique em de **privada Peerings** mosaico no dashboard. Aqui, pode selecionar um virtual ligação de rede e ver o estado de funcionamento, gráficos de tendência de perda de pacotes, utilização de largura de banda e latência.

![lista de circuito](.\media\how-to-npm\peerings.png)

### <a name="nodes"></a>Vista de nós

Para ver a lista de todas as ligações entre os nós no local e pontos finais do serviço de Azure VMs/Microsoft para a ligação de peering de ExpressRoute escolhido, clique em **ver ligações de nó**. Pode ver o estado de funcionamento de cada hiperligação, bem como a tendência de perda e latência associados aos mesmos.

![Vista de nós](.\media\how-to-npm\nodes.png)

### <a name="topology"></a>Topologia de circuito

Para visualizar a topologia de circuito, clique o **topologia** mosaico. Isto leva-o para a vista de topologia de selecionado circuito ou peering. O diagrama de topologia fornece a latência de cada segmento na rede. Cada salto de camada 3 é representado por um nó do diagrama. Clicar num salto de revela mais detalhes sobre o salto.

Pode aumentar o nível de visibilidade para incluir saltos no local, movendo a barra do controlo de deslize abaixo **filtros**. Mover a barra do controlo de deslize para a esquerda ou direita, aumenta/diminuições o número de saltos no gráfico de topologia. A latência em cada segmento está visível, que permite isolamento mais rápido de segmentos de latência elevada na sua rede.

![filtros](.\media\how-to-npm\topology.png)

#### <a name="detailed-topology-view-of-a-circuit"></a>Vista detalhada da topologia de um circuito

Esta vista mostra as ligações VNet.
![topologia de detalhado](.\media\how-to-npm\17.png)