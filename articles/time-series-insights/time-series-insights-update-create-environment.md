---
title: Tutorial do Azure Time Series Insights (pré-visualização) | Documentos da Microsoft
description: Saiba mais sobre o Azure Time Series Insights (pré-visualização)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/26/2018
ms.openlocfilehash: d4f69533a68e11b3e171963429b141cf0736472d
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53014638"
---
# <a name="azure-time-series-insights-preview-tutorial"></a>Tutorial do Azure Time Series Insights (pré-visualização)

Este tutorial descreve o processo de criação de um ambiente de pré-visualização do Azure Time Series Insights (TSI), preenchido com dados a partir de dispositivos simulados. Neste tutorial, ficará a saber como:

* Crie um ambiente de TSI (pré-visualização).
* Ligar o ambiente do TSI (pré-visualização) para um Hub de eventos.
* Execute uma simulação de farm do vento para transmitir dados para o ambiente de pré-visualização do TSI.
* Execute análise básica nos dados.
* Definir um tipo de modelo de série de tempo e a hierarquia e associá-lo a suas instâncias.

## <a name="create-a-time-series-insights-preview-environment"></a>Criar um ambiente do Time Series Insights (pré-visualização)

Esta secção descreve como criar um ambiente do Azure TSI (pré-visualização) utilizando o [Portal do Azure](https://portal.azure.com/).

1. Inicie sessão no Portal do Azure com a sua conta de subscrição
1. Selecione **+ Criar um recurso** no canto superior esquerdo.
1. Selecione a categoria **Internet das Coisas** e, em seguida, **Time Series Insights**.

   ![um tutorial][1]

1. Na página de ambiente do Time Series Insights, preencha os parâmetros necessários e clique em **seguinte: origem de evento**. Para este tutorial, defina o **ID de série de tempo** como `Id`. Para saber mais sobre **IDs de série de tempo**, leia [IDs de série de tempo](./time-series-insights-update-how-to-id.md).

   ![Tutorial-dois][2]

1. Sobre o **origem do evento** página, preencha os parâmetros necessários e clique em **rever + criar**. Para este tutorial, defina o **Timestamp** campo de propriedade como `Timestamp`.

   ![Tutorial-três][3]

1. Reveja todos os detalhes e clique em **criar** para iniciar o aprovisionamento do seu ambiente.

   ![Tutorial-quatro][4]

1. Receberá uma notificação quando a implementação foi concluída com êxito.

   ![Tutorial-cinco][5]

## <a name="send-events-to-your-tsi-environment"></a>Enviar eventos para o seu ambiente do TSI

Nesta secção, irá utilizar um simulador de dispositivos de windmill para enviar eventos para o seu ambiente do TSI através de um Hub de eventos.

  1. No Portal do Azure, navegue para o recurso do hub de eventos e ligá-lo ao seu ambiente do TSI. Saiba mais [como ligar o seu recurso a um Hub de eventos existente](./time-series-insights-how-to-add-an-event-source-eventhub.md).

  1. Na página de recursos do Hub de eventos, aceda a **políticas de acesso partilhado** e, em seguida **RootManageSharedAccessKey**. Copiar o **ligação chave primária da cadeia de caracteres** apresentado aqui.

      ![Tutorial-seis][6]

  1. Vá para [https://tsiclientsample.azurewebsites.net/windFarmGen.html]( https://tsiclientsample.azurewebsites.net/windFarmGen.html). Esta aplicação web simula windmill dispositivos.
  1. Cole a cadeia de ligação é copiada do passo 3 do **cadeia de ligação do Hub de eventos**

      ![Tutorial-sete][7]

  1. Clique em **clique para iniciar** enviar eventos para o Hub de eventos. Nesta fase, um arquivo chamado `instances.json` serão transferidos para o seu computador. Guarde este ficheiro uma vez que irá precisar deste mais tarde.

  1. Regresse ao seu Hub de eventos. Agora, deverá ver os novos eventos a ser recebidos pelo hub.d

      ![Tutorial-oito][8]

## <a name="analyze-data-in-your-environment"></a>Analisar dados no seu ambiente

Nesta secção, irá efetuar a análise básica em seu tempo explorer de atualização de dados de série com o Time Series Insights.

  1. Navegue para o Explorador de atualização do Time Series Insights clicando no URL da página de recursos no Portal do Azure.

      ![Tutorial-nove][9]

  1. No explorer, clique nas **instâncias Unparented** nós para ver todas as instâncias de série de tempo no ambiente.

      ![Tutorial-dez][10]

  1. Neste tutorial, vamos analisar os dados enviados durante o último dia. Para tal, clique em **vezes rápidas** e selecione o **últimas 24 horas** opção.

      ![Tutorial-11][11]

  1. Selecione **Sensor_0** e escolha **Mostrar valor de média** para visualizar os dados que está a ser enviados a partir desta instância de série de tempo.

      ![Tutorial-doze][12]

  1. Da mesma forma, pode desenhar dados provenientes de outras instâncias de série de tempo para executar a análise básica.

      ![treze para o tutorial][13]

## <a name="define-a-type-and-hierarchy"></a>Definir um tipo e a hierarquia

Nesta secção, irá criar um tipo, a hierarquia e associá-las com as instâncias de série de tempo. Leia mais sobre [modelos de série de tempo](./time-series-insights-update-tsm.md).

  1. No explorer, clique nas **modelo** separador na barra da aplicação.

      ![Tutorial-quatorze][14]

  1. Na secção de tipos, clique em **+ adicionar**. Isso permitirá que criar um novo tipo de modelo de série de tempo.

      ![Tutorial-quinze][15]

  1. No editor de tipo, introduza um **Name**, **Descrição**e criar variáveis para **média**, **Min**, e **Max** os valores, conforme mostrado abaixo. Clique em **criar** para guardar o tipo.

      ![Tutorial-16][16]

      ![Tutorial-seventeen][17]

  1. Na **hierarquias** secção, clique em **+ adicionar**. Isso permitirá que criar uma nova hierarquia de modelo de série de tempo.

     ![Tutorial-dezoito][18]

  1. No editor de hierarquia, introduza um **nome** e adicionar níveis de hierarquia, conforme mostrado abaixo. Clique em **criar** para guardar a hierarquia.

     ![Tutorial-dezenove][19]

     ![Tutorial-vinte][20]

  1. Na **instâncias** secção, selecione uma instância e clique em **editar**. Isso permitirá que associe um tipo e a hierarquia com esta instância.

     ![Tutorial-vinte-um][21]

  1. No editor de instância, escolha o tipo e a hierarquia definidas nos passos 3, 5 acima, conforme mostrado.

     ![Tutorial-vinte-dois][22]

  1. Em alternativa, para fazer isso para todas as instâncias de uma só vez, pode editar o `instances.json` ficheiro transferido anteriormente. Neste ficheiro, substitua todo **typeId** e **hierarchyId** campos com o ID de obteve dos passos 3, 5 acima.

  1. Na **instâncias** secção, clique em **carregar JSON** e carregar o editado `instances.json` ficheiro, conforme mostrado abaixo.

     ![Tutorial-vinte-três][23]

  1. Navegue para o **Analytics** separador e atualize o browser. Agora, deve ver todas as instâncias associadas com o tipo e a hierarquia definido acima.

     ![Tutorial-vinte e quatro][24]

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:  

* Crie um ambiente de TSI (pré-visualização).
* Ligar o ambiente do TSI (pré-visualização) para um Hub de eventos.
* Execute uma simulação de farm do vento para transmitir dados para o ambiente do TSI (pré-visualização).
* Execute análise básica nos dados.
* Definir um tipo de modelo de série de tempo, a hierarquia e associá-lo a suas instâncias.

Agora que sabe como criar seu próprio ambiente de atualização TSI, saiba mais sobre os conceitos fundamentais do TSI.

Leia sobre a configuração de armazenamento do TSI:

> [!div class="nextstepaction"]
> [O armazenamento do Azure TSI (pré-visualização) e de entrada](./time-series-insights-update-storage-ingress.md)

Saiba mais sobre os modelos de série de tempo:

> [!div class="nextstepaction"]
> [Modelação de dados TSI (pré-visualização) do Azure](./time-series-insights-update-tsm.md)

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