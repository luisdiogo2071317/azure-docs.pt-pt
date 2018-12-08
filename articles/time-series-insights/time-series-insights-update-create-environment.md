---
title: Configurar um tutorial de ambiente de pré-visualização do Azure Time Series Insights | Documentos da Microsoft
description: Saiba como configurar o ambiente de pré-visualização do Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/26/2018
ms.openlocfilehash: 20cec1305f84bd1ff7e01f2e1d38f374aa17bc6f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106688"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Configurar um ambiente de pré-visualização do Azure Time Series Insights

Este tutorial orienta-o ao longo do processo de criação de um ambiente de pré-visualização do Azure Time Series Insights, que é preenchido com dados a partir de dispositivos simulados. Neste tutorial, ficará a saber como:

* Crie um ambiente de pré-visualização do Time Series Insights.
* Ligar o ambiente de pré-visualização do Time Series Insights para um hub de eventos nos Hubs de eventos do Azure.
* Execute uma simulação de farm do vento para transmitir dados para o ambiente de pré-visualização do Time Series Insights.
* Execute análise básica nos dados.
* Definir um tipo de modelo de série de tempo e a hierarquia e associá-lo a suas instâncias.

## <a name="create-a-time-series-insights-preview-environment"></a>Criar um ambiente de pré-visualização do Time Series Insights

Esta secção descreve como criar um ambiente de pré-visualização do Time Series Insights com o [portal do Azure](https://portal.azure.com/).

1. Inicie sessão no portal do Azure com a sua conta de subscrição.

1. Selecione **criar um recurso**.

1. Selecione o **Internet das coisas** categoria e, em seguida, selecione **Time Series Insights**.

  ![Selecione criar um recurso, em seguida, selecione a Internet das coisas e, em seguida, selecione o Time Series Insights][1]

1. Sobre o **Noções básicas** separador, introduza os parâmetros necessários e, em seguida, selecione **seguinte: origem de evento**

  ![O separador de noções básicas de ambiente do Time Series Insights e o próximo: botão de origem do evento][2]

1. Sobre o **origem do evento** separador, introduza os parâmetros necessários e, em seguida, selecione **rever + criar**.

  ![O separador de origem do evento e a revisão + botão Criar][3]

1. Sobre o **resumo** separador, reveja todos os detalhes e, em seguida, selecione **criar** para iniciar o aprovisionamento do seu ambiente.

  ![A guia resumo e no botão Criar][4]

1. Quando a implementação for bem-sucedida, é apresentada uma notificação.

  ![Notificação de implementação concluída com êxito][5]

## <a name="send-events-to-your-time-series-insights-environment"></a>Enviar eventos para o seu ambiente do Time Series Insights

Nesta secção, vai utilizar um simulador de dispositivos de windmill para enviar eventos para o seu ambiente do Time Series Insights através de um hub de eventos.

  1. No portal do Azure, aceda ao seu recurso do hub de eventos e ligá-lo ao seu ambiente do Time Series Insights. Para saber como, veja [ligar o seu recurso para um hub de eventos existente](./time-series-insights-how-to-add-an-event-source-eventhub.md).

  1. Na página de recursos do hub de eventos, aceda a **políticas de acesso partilhado** > **RootManageSharedAccessKey**. Copie o valor para **ligação chave primária da cadeia de caracteres**.

      ![Copie o valor para a cadeia de ligação da chave primária][6]

  1. Vá para [https://tsiclientsample.azurewebsites.net/windFarmGen.html]( https://tsiclientsample.azurewebsites.net/windFarmGen.html). Esta aplicação web no URL simula windmill dispositivos.

  1. Na **cadeia de ligação do Hub de eventos** caixa na página Web, cole a cadeia de ligação que copiou no passo anterior.

      ![Cole a cadeia de ligação da chave primária na caixa de cadeia de ligação do Hub de eventos][7]

  1. Selecione **clique aqui para iniciar** para enviar eventos para o event hub. Um ficheiro denominado *instances.json* é transferido para o seu computador. Guarde este ficheiro para utilizar mais tarde.

  1. Regresse ao seu hub de eventos no portal do Azure. No hub de eventos **descrição geral** página, novos eventos que estão a ser recebidos pelo hub de eventos são apresentados.

     ![Uma página de descrição geral do hub de eventos que mostra as métricas para o hub de eventos][8]

## <a name="analyze-data-in-your-environment"></a>Analisar dados no seu ambiente

Nesta secção, é possível executar análise básica em seu tempo explorer de atualização de dados de série com o Time Series Insights.

  1. Vá para o Explorador de atualização do Time Series Insights clicando no URL da página de recursos no portal do Azure.

      ![O URL de Explorador do Time Series Insights][9]

  1. No explorer, em **hierarquia física**, selecione a **instâncias Unparented** nós para ver todas as instâncias de série de tempo no ambiente.

     ![Lista de instâncias unparented no painel de hierarquia física][10]

  1. Neste tutorial, vamos analisar os dados que lhe foi enviados ao longo do dia anterior. Selecione **vezes rápidas**e, em seguida, selecione **últimas 24 horas**.

     ![Na caixa de lista pendente de horas rápidas, selecione últimas 24 horas][11]

  1. Selecione **Sensor_0**e, em seguida, selecione **Mostrar valor de média** para visualizar os dados que está a ser enviados a partir desta instância do Time Series Insights.

     ![Selecione o valor de média de mostrar para Sensor_0][12]

  1. Da mesma forma, pode desenhar dados provenientes de outras instâncias do Time Series Insights para efetuar a análise básica.

     ![Um desenho de dados do Time Series Insights][13]

## <a name="define-a-type-and-hierarchy"></a>Definir um tipo e a hierarquia 

Nesta secção, criar um tipo e a hierarquia e, em seguida, associar o tipo e a hierarquia com as instâncias do Time Series Insights. Pode ler mais sobre [modelos de série de tempo](./time-series-insights-update-tsm.md).

  1. No explorer, selecione o **modelo** separador.

     ![O separador de modelo no menu do explorer][14]

  1. Na **tipos** secção, selecione **Add** para criar um novo tipo de modelo de série de tempo.

     ![Botão Adicionar na página de tipos][15]

  1. No editor de tipo, introduza os valores para **Name** e **Descrição**. Criar variáveis para **médio**, **Mín**, e **Max** os valores, conforme mostrado nas imagens seguintes. Selecione **criar** para guardar o tipo.

     ![A adicionar um tipo de painel e no botão Criar][16]

     ![Os tipos de exemplo Windmill][17]

  1. Na **hierarquias** secção, selecione **Add** para criar uma nova hierarquia de modelo de série de tempo.

     ![Botão Adicionar na página de hierarquias][18]

  1. No editor de hierarquia, introduza um valor para **nome** e adicionar níveis de hierarquia. Selecione **criar** para guardar a hierarquia.

     ![Adicionar uma hierarquia de painel e no botão Criar][19]

     ![A caixa de hierarquia física][20]

  1. Na **instâncias** secção, selecione uma instância e, em seguida, selecione **editar** para associar um tipo e a hierarquia com esta instância.

     ![Lista de instâncias][21]

  1. No editor de instância, selecione o tipo e a hierarquia que definiu nos passos 3 e 5.

     ![A edição de um painel de instância][22]

  1. Em alternativa, para selecionar o tipo e a hierarquia para todas as instâncias de uma só vez, pode editar a *instances.json* ficheiro transferido anteriormente. Neste ficheiro, substitua todo **typeId** e **hierarchyId** campos com o ID de obteve nos passos 3 e 5.

  1. Na **instâncias** secção, selecione **carregar JSON** e carregar o editado *instances.json* ficheiro.

     ![O botão carregar JSON][23]

  1. Selecione o **Analytics** separador e atualize o browser. Todas as instâncias associadas com o tipo e a hierarquia que definiu devem aparecer.

     ![Um desenho de dados do Time Series Insights][24]

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:  

* Crie um ambiente de pré-visualização do Time Series Insights.
* Ligar o ambiente de pré-visualização do Time Series Insights para um hub de eventos.
* Execute uma simulação de farm do vento para transmitir dados para o ambiente de pré-visualização do Time Series Insights.
* Execute uma análise básica dos dados.
* Definir um tipo de modelo de série de tempo e a hierarquia e associá-las com suas instâncias.

Agora que sabe como criar seu próprio ambiente de atualização do Time Series Insights, saiba mais sobre os conceitos fundamentais do Time Series Insights.

Leia sobre a configuração de armazenamento do Time Series Insights:

> [!div class="nextstepaction"]
> [Armazenamento de pré-visualização do Time Series Insights do Azure e de entrada](./time-series-insights-update-storage-ingress.md)

Saiba mais sobre os modelos de série de tempo:

> [!div class="nextstepaction"]
> [Modelação de dados do Time Series Insights pré-visualização do Azure](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/tutorial-one.png
[2]: media/v2-update-provision/tutorial-two.png
[3]: media/v2-update-provision/tutorial-three.png
[4]: media/v2-update-provision/tutorial-four.png
[5]: media/v2-update-provision/tutorial-five.png
[6]: media/v2-update-provision/tutorial-six.png
[7]: media/v2-update-provision/tutorial-seven.png
[8]: media/v2-update-provision/tutorial-eight.png
[9]: media/v2-update-provision/tutorial-nine.png
[10]: media/v2-update-provision/tutorial-ten.png
[11]: media/v2-update-provision/tutorial-eleven.png
[12]: media/v2-update-provision/tutorial-twelve.png
[13]: media/v2-update-provision/tutorial-thirteen.png
[14]: media/v2-update-provision/tutorial-fourteen.png
[15]: media/v2-update-provision/tutorial-fifteen.png
[16]: media/v2-update-provision/tutorial-sixteen.png
[17]: media/v2-update-provision/tutorial-seventeen.png
[18]: media/v2-update-provision/tutorial-eighteen.png
[19]: media/v2-update-provision/tutorial-nineteen.png
[20]: media/v2-update-provision/tutorial-twenty.png
[21]: media/v2-update-provision/tutorial-twenty-one.png
[22]: media/v2-update-provision/tutorial-twenty-two.png
[23]: media/v2-update-provision/tutorial-twenty-three.png
[24]: media/v2-update-provision/tutorial-twenty-four.png