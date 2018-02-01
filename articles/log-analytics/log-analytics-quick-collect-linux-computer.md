---
title: Recolher dados de computadores com Linux no local com o Azure Log Analytics | Microsoft Docs
description: Saiba como implementar o agente do Log Analytics para Linux e ativar a recolha de dados a partir desse SO com o Log Analytics.
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/23/2018
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 839fc3a326dca8b60c6750231b06d2369c3de2fc
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="collect-data-from-linux-computers-hosted-in-your-environment"></a>Recolher dados de computadores com Linux alojados no seu ambiente
O [Azure Log Analytics](log-analytics-overview.md) pode recolher dados diretamente de computadores com Linux físicos ou virtuais e de outros recursos no seu ambiente para um único repositório para análise e correlação detalhadas.  Este início rápido mostra como configurar e recolher dados do seu computador com Linux em alguns passos simples.  Para as VMs do Linux do Azure, veja o tópico seguinte [Recolher dados sobre Máquinas Virtuais do Azure](log-analytics-quick-collect-azurevm.md).  

Para compreender os requisitos da rede e do sistema para implementar o agente do Linux, consulte [Recolher dados do seu ambiente com o Log Analytics do Azure](log-analytics-concept-hybrid.md#prerequisites).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Criar uma área de trabalho
1. No portal do Azure, clique em **Mais serviços**, que se encontra no canto inferior esquerdo. Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.<br><br> ![Portal do Azure](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Clique em **Criar** e, em seguida, selecione as opções para os seguintes itens:

  * Indique um nome para a nova **Área de Trabalho do OMS**, como *DefaultLAWorkspace*. 
  * Selecione uma **Subscrição** à qual ligar ao escolher na lista pendente se a predefinição selecionada não é adequada.
  * Para **Grupo de Recursos**, selecione um grupo de recursos existente que contenha uma ou mais máquinas virtuais do Azure.  
  * Selecione a **Localização** para onde as VMs devem ser implementadas.  Para obter mais informações, veja em que [regiões está disponível o Log Analytics](https://azure.microsoft.com/regions/services/).
  * Pode escolher de entre três **escalões de preço** diferentes no Log Analytics, mas para este início rápido, vai selecionar o escalão **gratuito**.  Para obter informações adicionais sobre os escalões específicos, veja [Detalhes de Preços do Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](./media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Depois de fornecer as informações necessárias no painel **Área de Trabalho do OMS**, clique em **OK**.  

Enquanto as informações são confirmadas e a área de trabalho criada, pode acompanhar o progresso em **Notificações**, no menu. 

## <a name="obtain-workspace-id-and-key"></a>Obter o ID e a chave da área de trabalho
Antes de instalar o agente do OMS para Linux, precisa do ID e da chave da área de trabalho do Log Analytics.  Esta informação é necessária para o script de wrapper do agente configurar corretamente o agente e assegurar que consegue comunicar com êxito com o Log Analytics.  

1. No portal do Azure, clique em **Mais serviços**, que se encontra no canto inferior esquerdo. Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. Na lista de áreas de trabalho do Log Analytics, selecione *DefaultLAWorkspace*, criada anteriormente.
3. Selecione **Definições avançadas**.<br><br> ![Definições Avançadas do Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecione **Origens Ligadas** e, em seguida, selecione **Servidores Linux**.   
5. O valor à direita de **ID da Área de Trabalho** e **Chave Primária**. Copie e cole ambos no seu editor favorito.   

## <a name="install-the-agent-for-linux"></a>Instalar o agente para Linux
Os passos seguintes configuram o agente do Log Analytics no Azure e na cloud do Azure Government.  

>[!NOTE]
>O agente do OMS para Linux não pode ser configurado para reportar a mais do que uma área de trabalho do Log Analytics.  

1. Para configurar o computador com Linux para ligar ao Log Analytics, execute o seguinte comando ao fornecer o ID da área de trabalho e a chave primária que copiou anteriormente.  Este comando transfere o agente, valida a respetiva soma de verificação e instala-o. 
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Para configurar o computador com Linux para ligar ao Log Analytics na cloud do Azure Government, execute o seguinte comando ao fornecer o ID da área de trabalho e a chave primária que copiou anteriormente.  Este comando transfere o agente, valida a respetiva soma de verificação e instala-o. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

## <a name="configure-agent-to-communicate-with-a-proxy-server"></a>Configurar o agente para comunicar com um servidor proxy

Execute os passos seguintes se os computadores com Linux precisarem de comunicar através de um servidor proxy com o Log Analytics.  O valor de configuração do proxy tem a seguinte sintaxe `[protocol://][user:password@]proxyhost[:port]`.  A propriedade *proxyhost* aceita um nome de domínio completamente qualificado ou o endereço IP do servidor proxy.    

1. Edite o ficheiro `/etc/opt/microsoft/omsagent/proxy.conf` ao executar os comandos seguintes e altere os valores para as definições específicas.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Reinicie o agente ao executar o seguinte comando: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="collect-event-and-performance-data"></a>Recolher dados de eventos e de desempenho
O Log Analytics pode recolher eventos do Linux Syslog e contadores de desempenho que especificar para análises e relatórios a longo prazo e efetuar ações quando é detetada uma determinada condição.  Siga estes passos para configurar a recolha de eventos a partir do Linux Syslog e vários contadores de desempenho comuns para começar.  

1. Selecione **Syslog**.  
2. Adicione um registo de eventos ao escrever o nome do registo.  Escreva **Syslog** e, em seguida, clique no sinal de adição **+**.  
3. Na tabela, desmarque as gravidades **Informações**, **Aviso** e **Depurar**. 
4. Clique em **Guardar** no início da página para guardar a configuração.
5. Selecione **Dados de Desempenho do Linux** para ativar a recolha de contadores de desempenho num computador Windows. 
6. Quando configurar os contadores de desempenho do Linux pela primeira vez para uma nova área de trabalho do Log Analytics, é-lhe dada a opção de criar rapidamente vários contadores comuns. São listados com uma caixa de verificação junto a cada um.<br><br> ![Contadores de desempenho do Windows predefinidos](media/log-analytics-quick-collect-azurevm/linux-perfcounters-default.png).<br><br> Clique em **Adicionar os contadores de desempenho selecionados**.  Estes são adicionados e predefinidos com um intervalo de amostra de recolha de dez segundo.  
7. Clique em **Guardar** no início da página para guardar a configuração.

## <a name="view-data-collected"></a>Ver os dados recolhidos
Agora que ativou a recolha de dados, vamos executar um exemplo de pesquisa de registo simples para ver alguns dados a partir do computador de destino.  

1. No portal do Azure, navegue até Log Analytics e selecione a área de trabalho criada anteriormente.
2. Clique no mosaico **Pesquisa de Registos**, no painel Pesquisa de Registos, no tipo de campo de consulta `Perf` e, em seguida, prima Enter ou clique no botão de pesquisa à direita do campo de consulta.<br><br> ![Exemplo de consulta de pesquisa de registos do Log Analytics](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Por exemplo, a consulta na imagem seguinte devolveu 735 registos de desempenho.<br><br> ![Resultado da pesquisa de registos do Log Analytics](media/log-analytics-quick-collect-linux-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não for necessário, pode remover o agente do computador com Linux e eliminar a área de trabalho do Log Analytics.  

Para remover o agente, efetue os passos seguintes.

1. Transfira o agente Linux [universal script](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) para o computador.
2. Execute o ficheiro .sh do pacote com o argumento *--purge* no computador, o que remove completamente o agente e a respetiva configuração.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

Para eliminar a área de trabalho, selecione a área de trabalho do Log Analytics criada anteriormente e clique em **Eliminar** na página de recursos.<br><br> ![Eliminar o recurso do Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Passos seguintes
Agora que está a recolher dados operacionais e de desempenho do computador com Linux no local, pode começar facilmente a explorar, analisar e efetuar ações nos dados recolhidos *gratuitamente*.  

Para saber como ver e analisar os dados, avance para o tutorial.   

> [!div class="nextstepaction"]
> [Ver ou analisar dados no Log Analytics](log-analytics-tutorial-viewdata.md)
