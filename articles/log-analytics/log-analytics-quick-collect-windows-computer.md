---
title: Recolher dados de computadores Windows no local com o Azure Log Analytics | Microsoft Docs
description: "Saiba como implementar o agente do Log Analytics para Windows em execução em computadores fora do Azure e ativar a recolha de dados com o Log Analytics."
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
ms.date: 12/14/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 4462276724628de09fdefb21ff0f3eb61561a09e
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="collect-data-from-windows-computers-hosted-in-your-environment"></a>Recolher dados de computadores Windows alojados no seu ambiente
O [Azure Log Analytics](log-analytics-overview.md) pode recolher dados diretamente de computadores Windows físicos ou virtuais e de outros recursos no seu ambiente para um único repositório, para análise e correlação detalhadas.  Este início rápido mostra-lhe como configurar e recolher dados do seu computador Windows em alguns passos simples.  Para as VMs do Windows do Azure, veja o tópico seguinte [Collect data about Azure Virtual Machines](log-analytics-quick-collect-azurevm.md) (Recolher dados sobre as Máquinas Virtuais do Azure).  

Para compreender os requisitos da rede e do sistema para implementar o agente do Windows, consulte [Recolher dados do seu ambiente com o Log Analytics do Azure](log-analytics-concept-hybrid.md#prerequisites).
 
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

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Depois de fornecer as informações necessárias no painel **Área de Trabalho do OMS**, clique em **OK**.  

Enquanto as informações são confirmadas e a área de trabalho criada, pode acompanhar o progresso em **Notificações**, no menu. 

## <a name="obtain-workspace-id-and-key"></a>Obter o ID e a chave da área de trabalho
Antes de instalar o agente Microsoft Monitoring para Windows, precisa do ID e da chave da área de trabalho do Log Analytics.  O assistente de configuração precisa desta informação para configurar corretamente o agente e assegurar que consegue comunicar com êxito com o Log Analytics.  

1. No portal do Azure, clique em **Mais serviços**, que se encontra no canto inferior esquerdo. Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. Na lista de áreas de trabalho do Log Analytics, selecione *DefaultLAWorkspace*, criada anteriormente.
3. Selecione **Definições avançadas**.<br><br> ![Definições Avançadas do Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecione **Origens Ligadas** e, em seguida, selecione **Servidores Windows**.   
5. O valor à direita de **ID da Área de Trabalho** e **Chave Primária**. Copie e cole ambos no seu editor favorito.   

## <a name="install-the-agent-for-windows"></a>Instalar o agente para Windows
Os passos seguintes instalam e configuram o agente para o Log Analytics no Azure e na cloud do Azure Government mediante a utilização do Microsoft Monitoring Agent no seu computador.  

1. Na página **Servidores Windows**, selecione a versão **Transferir Agente do Windows** adequada para transferir consoante a arquitetura do processador do sistema operativo Windows.
2. Execute a Configuração para instalar o agente no seu computador.
2. Na página **Bem-vindo**, clique em **Seguinte**.
3. Na página **Termos de Licenciamento**, leia a licença e clique em **Aceito**.
4. Na página **Pasta de Destino**, altere ou mantenha a pasta de instalação predefinida e clique em **Seguinte**.
5. Na página **Opções de Configuração do Agente** , escolha ligar o agente ao Azure Log Analytics (OMS) e clique em **Seguinte**.   
6. Na página **Azure Log Analytics**, faça o seguinte:
   1. Cole o **ID da Área de Trabalho** e a **Chave da Área de Trabalho (Chave Primária)** que copiou anteriormente.  Caso o computador deva reportar a uma área de trabalho do Log Analytics na cloud do Azure Government, selecione **Azure US Government**, na lista pendente **Cloud do Azure**.  
   2. Se o computador tiver de comunicar através de um servidor proxy com o serviço do Log Analytics, clique em **Avançadas** e forneça o URL e o número da porta do servidor proxy.  Se o seu servidor proxy precisar de autenticação, escreva o nome de utilizador e a palavra-passe para autenticação no mesmo e clique em **Seguinte**.  
7. Depois de indicar as definições de configuração necessárias, clique em **Seguinte**.<br><br> ![colar o ID da Área de Trabalho e a Chave Primária](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Na página **Pronto para Instalar**, reveja as suas opções e clique em **Instalar**.
9. Na página **Configuração Concluída com Êxito**, clique em **Concluir**.

Quando terminar, o **Microsoft Monitoring Agent** aparece no **Painel de Controlo**. Pode rever a configuração e confirmar que o agente está ligado ao Log Analytics. Quando estiver ligado, no separador **Azure Log Analytics (OMS)**, o agente apresenta uma mensagem que diz: **O Microsoft Monitoring Agent ligou-se com êxito ao serviço Microsoft Operations Management Suite.**<br><br> ![Estado da ligação do MMA ao Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-mma-laworkspace-status.png)

## <a name="collect-event-and-performance-data"></a>Recolher dados de eventos e de desempenho
O Log Analytics pode recolher eventos do registo do Windows e de contadores de desempenho que especificar para análises e relatórios a longo prazo e efetuar ações quando é detetada uma determinada condição.  Siga estes passos para configurar a recolha de eventos a partir do registo de eventos do Windows e de vários contadores de desempenho comuns para começar.  

1. No portal do Azure, clique em **Mais serviços**, que se encontra no canto inferior esquerdo. Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. Selecione **Definições avançadas**.<br><br> ![Definições Avançadas do Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br> 
3. Selecione **Dados** e, em seguida, selecione **Registos de Eventos do Windows**.  
4. Adicione um registo de eventos ao escrever o nome do registo.  Escreva **System** e, em seguida, clique no sinal de adição **+**.  
5. Na tabela, verifique as gravidades **Erro** e **Aviso**.   
6. Clique em **Guardar** no início da página para guardar a configuração.
7. Selecione **Dados de Desempenho do Windows** para ativar a recolha de contadores de desempenho num computador Windows. 
8. Quando configurar os contadores de desempenho do Windows pela primeira vez para uma nova área de trabalho do Log Analytics, é-lhe dada a opção de criar rapidamente vários contadores comuns. São listados com uma caixa de verificação junto a cada um.<br> ![Contadores de desempenho do Windows predefinidos](media/log-analytics-quick-collect-azurevm/windows-perfcounters-default.png).<br> Clique em **Adicionar os contadores de desempenho selecionados**.  Estes são adicionados e predefinidos com um intervalo de amostra de recolha de dez segundo.  
9. Clique em **Guardar** no início da página para guardar a configuração.

## <a name="view-data-collected"></a>Ver os dados recolhidos
Agora que ativou a recolha de dados, vamos executar um exemplo de pesquisa de registo simples para ver alguns dados a partir do computador de destino.  

1. No portal do Azure, na área de trabalho selecionada, clique no mosaico **Pesquisa de Registos**.  
2. No painel Pesquisa de Registos, no tipo de campo de consulta, escreva `Perf` e prima Enter ou clique no botão de pesquisa à direita do campo de consulta.<br><br> ![Exemplo de consulta de pesquisa de registos do Log Analytics](media/log-analytics-quick-collect-linux-computer/log-analytics-portal-queryexample.png)<br><br> Por exemplo, a consulta na imagem seguinte devolveu 735 registos de desempenho.<br><br> ![Resultado da pesquisa de registos do Log Analytics](media/log-analytics-quick-collect-windows-computer/log-analytics-search-perf.png)

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não for necessário, pode remover o agente do computador Windows e eliminar a área de trabalho do Log Analytics.  

Para remover o agente, efetue os passos seguintes.

1. Abra o **Painel de Controlo**.
2. Abra **Programas e Funcionalidades**.
3. Em **Programas e Funcionalidades**, selecione **Microsoft Monitoring Agent** e clique em **Desinstalar**.

Para eliminar a área de trabalho, selecione a área de trabalho do Log Analytics criada anteriormente e clique em **Eliminar** na página de recursos.<br><br> ![Eliminar o recurso do Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-portal-delete-resource.png)

## <a name="next-steps"></a>Passos seguintes
Agora que está a recolher dados operacionais e de desempenho do computador com Linux no local, pode começar facilmente a explorar, analisar e efetuar ações nos dados recolhidos *gratuitamente*.  

Para saber como ver e analisar os dados, avance para o tutorial.   

> [!div class="nextstepaction"]
> [Ver ou analisar dados no Log Analytics](log-analytics-tutorial-viewdata.md)
