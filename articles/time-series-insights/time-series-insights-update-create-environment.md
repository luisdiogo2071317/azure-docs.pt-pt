---
title: Do Azure Time Series Insights pré-visualização Configurar - configurar um tutorial de ambiente de pré-visualização do Azure Time Series Insights | Documentos da Microsoft
description: Saiba como configurar o ambiente de pré-visualização do Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 9ad957d6378b1279f1ca51939eb4802b0ce7d78f
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322623"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Configurar um ambiente de pré-visualização do Azure Time Series Insights

Este tutorial orienta-o ao longo do processo de criação de um ambiente de pré-visualização do (PAYG) pay as you go do Azure Time Series Insights. Neste tutorial, ficará a saber como:

* Crie um ambiente de pré-visualização do Azure Time Series Insights.
* Ligar o ambiente de pré-visualização do Azure Time Series Insights para um hub de eventos nos Hubs de eventos do Azure.
* Execute uma simulação de farm do vento para transmitir dados para o ambiente de pré-visualização do Azure Time Series Insights.
* Execute análise básica nos dados.
* Definir um tipo de modelo de série de tempo e a hierarquia e associá-lo a suas instâncias.

# <a name="create-a-device-simulation"></a>Criar uma simulação de dispositivos

Nesta secção, irá criar três dispositivos simulados que irão enviar dados para um IoT Hub.

1. Vá para o [home page do Aceleradores de solução do Azure IoT](https://www.azureiotsolutions.com/Accelerators). A home page do Azure IoT solution accelerators mostra vários exemplos criados previamente. Inicie sessão com a sua conta do Azure. Em seguida, selecione **simulação do dispositivo**.

   ![Página do home de Aceleradores de solução do Azure IoT][1]

   Por último, clique em **experimentar agora**.

1. Introduza os parâmetros necessários **criar a simulação do dispositivo** página de solução:

   | Parâmetro | Descrição |
   | --- | --- |
   | Nome da solução |    Um valor exclusivo, utilizado para a criação de um novo grupo de recursos. Os recursos do Azure listados são | criados e atribuídos ao grupo de recursos. |
   | Subscrição | Especificar a mesma subscrição utilizada para a criação de seu ambiente do TSI |
   | Região |   Especifique a mesma região utilizada para a criação de seu TSI. |
   | Implementar Recursos do Azure opcionais    | Deixe o IoT Hub selecionado, como os dispositivos simulados irão utilizá-lo para ligar/fluxo de dados. |

   Depois de inserir os parâmetros necessários, clique em **criar solução**. Aguarde cerca de 10 a 15 minutos para a sua solução a serem implantados.

   ![Criar a solução de simulação de dispositivo][2]

1. No seu **Dashboard de acelerador de solução**, clique nas **iniciar** botão:

   ![Iniciar a solução de simulação do dispositivo][3]

1. Será redirecionado para o **simulação de dispositivo do Microsoft Azure IoT** página. Clique em **+ nova simulação** localizado no canto superior direito do ecrã.

   ![Página de simulação de IoT do Azure][4]

1.  Preencha os parâmetros necessários da seguinte forma:

    ![Parâmetros para preencher][5]

    |||
    | --- | --- |
    | **Nome** | Introduza um nome exclusivo para um simulador |
    | **Descrição** | Introduza uma definição |
    | **Duração de simulação** | Definido como `Run indefinitely` |
    | **Modelo do dispositivo** | **Nome**: Introduza `Chiller` **quantidade**: Introduza `3` |
    | **Hub IoT de destino** | Definido como `Use pre-provisioned IoT Hub` |

    Depois de preencher os parâmetros necessários, clique em **iniciar simulação**.

1. No painel de simulação de dispositivo, consulte a **dispositivos ativos** e **mensagens por segundo**.

    ![Painel de simulação de IoT do Azure][6]

## <a name="list-device-simulation-properties"></a>Propriedades de simulação do dispositivo de lista

Antes de criar um ambiente do Azure Time Series Insights, terá dos nomes do seu IoT Hub, a subscrição e o nome do grupo de recursos.

1. Vá para o **Dashboard de acelerador de solução** e inicie sessão com a mesma conta de subscrição do Azure. Encontre a simulação de dispositivo que criou nos passos anteriores.

1. Clique no simulador de dispositivos e clique em **inicie**. Clique nas **Portal de gestão do Azure** ligação é apresentada no lado direito.

    ![Listagens de simulador][7]

1. Tome nota do IoT Hub, subscrição e os nomes de grupo de recursos.

    ![Portal do Azure][8]

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Criar um ambiente de PAYG de pré-visualização do Time Series Insights

Esta secção descreve como criar um ambiente de pré-visualização do Azure Time Series Insights com o [portal do Azure](https://portal.azure.com/).

1. Inicie sessão no portal do Azure com a sua conta de subscrição.

1. Selecione **criar um recurso**.

1. Selecione o **Internet das coisas** categoria e, em seguida, selecione **Time Series Insights**.

   ![Selecione criar um recurso, em seguida, selecione a Internet das coisas e, em seguida, selecione o Time Series Insights][9]

1. Preencha os campos na página da seguinte forma:

   | | |
   | --- | ---|
   | **Nome do ambiente** | Escolha um nome exclusivo para o ambiente de pré-visualização do Azure Time Series Insights. |
   | **Subscrição** | Introduza a sua subscrição em que pretende criar o ambiente de pré-visualização do Azure Time Series Insights. É melhor prática para utilizar a mesma subscrição que o resto dos seus recursos de IoT criados pelo simulador de dispositivos. |
   | **Grupo de Recursos** | Um grupo de recursos é um contentor de recursos do Azure. Escolha um grupo de recursos existente ou crie um novo, para o recurso de ambiente de pré-visualização do Azure Time Series Insights. É uma prática recomendada usar o mesmo grupo de recursos que o resto dos seus recursos de IoT criados pelo simulador de dispositivos. |
   | **Localização** | Escolha uma região do Datacenter para o seu ambiente de pré-visualização do Azure Time Series Insights. Para evitar os custos de largura de banda adicional e latência, é melhor manter o ambiente de pré-visualização do Azure Time Series Insights na mesma região que outros recursos de IoT. |
   | **Escalão** |  Selecione `PAYG` que significa pay as you go. Este é o SKU do produto de pré-visualização do Azure Time Series Insights. |
   | **ID de propriedade** | Identifica exclusivamente a sua série de tempo. Tenha em atenção que este campo é imutável e não é possível alterar mais tarde. Para este tutorial defina o campo como `iothub-connection-device-id`. Para saber mais sobre o ID de série de tempo, leia [como escolher um ID de série de tempo](./time-series-insights-update-how-to-id.md). |
   | **Nome da conta de armazenamento** | Introduza um nome exclusivo global para uma nova conta de armazenamento a ser criada. |

   Depois de preencher os campos acima, clique em **seguinte: Origem do evento**.

   ![Clique em seguinte: Origem do Evento][10]

1. Na página, preencha os campos da seguinte forma:

   | | |
   | --- | --- |
   | **Criar uma origem de evento?** | Introduza `Yes`|
   | **Nome** | Requer um valor exclusivo, utilizado para o nome da origem de evento.|
   | **Tipo de origem** | Introduza `IoT Hub` |
   | **Selecione um Hub?** | Introduza `Select Existing` |
   | **Subscrição** | Introduza a subscrição que utilizou para o simulador de dispositivos. |
   | **Nome do IoT Hub** | Introduza o nome do hub IoT que criou para o simulador de dispositivos. |
   | **Política de acesso do IoT Hub** | Introduza `iothubowner` |
   | **Grupo de consumidores do IOT Hub** | Precisa de um grupo de consumidores exclusivo para uma pré-visualização do Azure Time Series Insights. |
   | **Timestamp** | Este campo é utilizado para identificar a propriedade timestamp nos seus dados de telemetria de entrada. Para este tutorial, não o preencha o campo. Este simulador utiliza o carimbo de hora de entrada do IoT Hub que assume a predefinição Time Series Insights.|

   Para criar um grupo de consumidores exclusivo:

   1. Clique em **New** junto a **grupo de consumidores do IoT Hub** campo:

      ![Clique em seguinte: Origem do Evento][11]

   1. Dê um nome exclusivo ao grupo de consumidores e clique em **adicionar**:

      ![Clique em Adicionar][12]

   Depois de preencher os campos acima, clique em **rever + criar**.

      ![Rever e criar][13]

1. Reveja todos os campos na página de revisão e clique em **criar**.

   ![Criar][14]

1. Pode ver o estado da implementação.

   ![Conclusão da implantação][15]

1. Deve receber acesso ao seu ambiente de série de tempo, se for o proprietário do inquilino. Para se certificar-se de que tem acesso:

   * Navegue para o seu ambiente de pré-visualização do Azure Time Series Insights criado recentemente. Pode fazê-lo ao procurar o seu grupo de recursos. Em seguida, clique no seu ambiente de time series:

      ![Conclusão da implantação][16]

   * Na página de pré-visualização do Azure Time Series Insights, navegue até **políticas de acesso de dados**.

     ![Políticas de acesso a dados][17]

   * Certifique-se de que as suas credenciais estão listadas.

     ![Verifique as suas credenciais][18]

   Se as suas credenciais não forem apresentadas, terá de dar permissão para aceder ao ambiente. Leia [conceder acesso a dados](./time-series-insights-data-access.md) para saber mais sobre como definir permissões.

## <a name="analyze-data-in-your-environment"></a>Analisar dados no seu ambiente

Nesta seção, efetuar análise básica em seus dados de séries de tempo ao utilizar o [Explorador de pré-visualização do Azure Time Series Insights](./time-series-insights-update-explorer.md).

1. Aceda ao seu Explorador de pré-visualização do Azure Time Series Insights clicando no URL da página de recursos no [portal do Azure](https://portal.azure.com/).

   ![O URL de Explorador do Time Series Insights][19]

1. No explorer, selecione o **instâncias Unparented** nós para ver todos os a pré-visualização do Azure Time Series Insights no ambiente de.

   ![Lista de instâncias unparented][20]

1. Na série de tempo apresentada, clique na primeira instância. Em seguida, clique em **pressão média mostrar**.

   ![Mostrar pressão média][21]

1. Um gráfico de série de tempo deve aparecer no lado direito:

   ![Gráfico de série de tempo][22]

1. Repita **passo 3** com as outras duas séries de tempo. Todas as séries de tempo, em seguida, podem ser visualizadas, conforme mostrado abaixo:

   ![Todos os gráfico de série de tempo][23]

1. Modificar a **intervalo de tempo** para ver as tendências de séries de tempo na última hora. Selecione o **partir** caixa da opção conforme mostrado abaixo:

   ![Selecione a opção de From][24]

1. Alterar o tempo no **partir** caixa da opção para apresentar eventos de última hora:

   ![Selecione a opção de From][25]

1. Em seguida, pode comparar pressão em todos os dispositivos de três na última hora:

   ![Selecione a opção de From][26]

## <a name="define-and-apply-a-model"></a>Definir e aplicar um modelo

Nesta secção, irá aplicar um modelo para estruturar os dados. Para concluir o modelo, irá definir tipos, hierarquias e instâncias. Para saber mais sobre a modelação de dados, aceda a [modelos de série de tempo](./time-series-insights-update-tsm.md).

1. No explorer, selecione o **modelo** separador:

   ![Selecione o separador de modelo][27]

1. Em seguida, clique em **+ adicionar** para adicionar um tipo. No lado direito, abrirá um editor de tipos.

   ![Clique em Adicionar][28]

1. Em seguida, defina três variáveis: Pressão, temperatura e humidade num tipo. Introduza os seguintes campos:

   | | |
   | --- | ---|
   | **Nome** | Introduza `Chiller` |
   | **Descrição** | Introduza `This is a type definition of Chiller` |

   * Agora, defina pressão com três variáveis:

      | | |
      | --- | ---|
      | **Nome** | Introduza `Avg Pressure` |
      | **Valor** | Selecione **pressão (Double)**. Tenha em atenção de que pode demorar alguns minutos para que este campo preencher depois do Azure Time Series Insights começa a receber eventos |
      | **Operação de agregação** | Selecione `AVG` |

      ![Adicione uma variável][29]

      Clique em **+ variável** para adicionar a seguinte variável.

   * Agora, defina a temperatura:

      | | |
      | --- | ---|
      | **Nome** | Introduza `Avg Temperature` |
      | **Valor** | Selecione **temperatura (Double)**. Tenha em atenção de que pode demorar alguns minutos para que este campo preencher depois do Azure Time Series Insights começa a receber eventos |
      | **Operação de agregação** | Selecione `AVG`|

      ![Definir a temperatura][30]

   * Agora, defina humidade:

      | | |
      | --- | ---|
      | **Nome** | Introduza `Max Humidity` |
      | **Valor** | Selecione **humidade (Double)**. Tenha em atenção de que pode demorar alguns minutos para que este campo preencher depois do Azure Time Series Insights começa a receber eventos |
      | **Operação de agregação** | Selecione `MAX`|

      ![Definir a temperatura][31]

   Depois de definir as variáveis, clique em **criar**.

1. Pode ver o seu tipo adicionado:

   ![Ver tipo adicionado][32]

1. A próxima etapa é adicionar uma hierarquia. Na **hierarquias** secção, selecione **+ adicionar** para criar uma nova hierarquia:

   ![Adicionar uma Hierarquia][33]

1. Defina a hierarquia. Introduza os campos da seguinte forma:

   | | |
   | --- | ---|
   | **Nome** | Introduza `Location Hierarchy` |
   | **Nível 1** | Introduza `Country` |
   | **Nível 2** | Introduza `City` |
   | **Nível 3** | Introduza `Building` |

   Depois de preencher os campos acima, clique em **criar**.

   ![Definir uma hierarquia][34]

1. Pode ver a hierarquia criada:

   ![Ver hierarquia][35]

1. Depois de definir a sua hierarquia, clique em **instâncias** à esquerda. Depois das instâncias são apresentadas, clique a primeira instância e selecione **editar**:

   ![Editar uma instância][36]

1. No lado direito, será apresentado um editor de texto. Adicione os seguintes campos:

   | | |
   | --- | --- |
   | **Tipo** | Selecione `Chiller` |
   | **Descrição** | Introduza `Instance for Chiller-01.1` |
   | **Hierarquias** | Ativar `Location Hierarchy` |
   | **País** | Introduza `USA` |
   | **Cidade** | Introduza `Seattle` |
   | **Criando** | Introduza `Space Needle` |

    Depois de preencher os campos acima, clique em **guardar**.

   ![Guardar um chiller][37]

1. Repita o passo anterior para os outros sensores. Utilize os seguintes campos:

   * Para Chiller 01.2:

     | | |
     | --- | --- |
     | **Tipo** | Selecione `Chiller` |
     | **Descrição** | Introduza `Instance for Chiller-01.2` |
     | **Hierarquias** | Ativar `Location Hierarchy` |
     | **País** | Introduza `USA` |
     | **Cidade** | Introduza `Seattle` |
     | **Criando** | Introduza `Pacific Science Center` |

   * Para Chiller 01.3:

     | | |
     | --- | --- |
     | **Tipo** | Selecione `Chiller` |
     | **Descrição** | Introduza `Instance for Chiller-01.1` |
     | **Hierarquias** | Ativar `Location Hierarchy` |
     | **País** | Introduza `USA` |
     | **Cidade** | Introduza `New York` |
     | **Criando** | Introduza `Empire State Building` |

1. Aceda a **Analyze** separador e atualize a página. Expanda todos os níveis de hierarquia para localizar a série de tempo.

   ![Consulte o separador de analisar][38]

1. Para explorar a série de tempo na última hora, altere **vezes rápidas** a última hora:

   ![Explore a última hora][39]

1. Clique em tempos de série sob **Center de ciência do Pacífico** e clique em **Mostrar humidade de Max**.

   ![Mostrar a humidade máxima][40]

1. A série de tempo para **humidade máxima** com um intervalo de tamanho de 1 minuto será aberto. LEFT-click uma região para filtrar um intervalo. Em seguida, clique com botão direito e zoom para analisar os eventos num intervalo de tempo:

   ![Ver, filtrar e zoom][41]

   ![Ver, filtrar e zoom][42]

1. Pode também left-click uma região e, em seguida, clique com botão direito para ver detalhes do evento:

   ![Ver, filtrar e zoom][43]

   ![Ver, filtrar e zoom][44]

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:  

* Criar e utilizar um acelerador de simulação do dispositivo.
* Crie um ambiente do Azure Time Series Insights pré-visualização PAYG.
* Ligar o ambiente de pré-visualização do Azure Time Series Insights para um hub de eventos.
* Execute uma simulação de farm do vento para transmitir dados para o ambiente de pré-visualização do Azure Time Series Insights.
* Execute uma análise básica dos dados.
* Definir um tipo de modelo de série de tempo e a hierarquia e associá-las com suas instâncias.

Agora que sabe como criar seu próprio ambiente de pré-visualização do Azure Time Series Insights, saiba mais sobre os conceitos fundamentais do Azure Time Series Insights.

Leia sobre a configuração de armazenamento do Azure Time Series Insights:

> [!div class="nextstepaction"]
> [Armazenamento de pré-visualização do Time Series Insights do Azure e de entrada](./time-series-insights-update-storage-ingress.md)

Saiba mais sobre os modelos de série de tempo:

> [!div class="nextstepaction"]
> [Modelação de dados do Time Series Insights pré-visualização do Azure](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/device-one-accelerator.png
[2]: media/v2-update-provision/device-two-create.png
[3]: media/v2-update-provision/device-three-launch.png
[4]: media/v2-update-provision/device-four-iot-sim-page.png
[5]: media/v2-update-provision/device-five-params.png
[6]: media/v2-update-provision/device-six-listings.png
[7]: media/v2-update-provision/device-seven-dashboard.png
[8]: media/v2-update-provision/device-eight-portal.png

[9]: media/v2-update-provision/payg-one-azure.png
[10]: media/v2-update-provision/payg-two-create.png
[11]: media/v2-update-provision/payg-three-new.png
[12]: media/v2-update-provision/payg-four-add.png
[13]: media/v2-update-provision/payg-five-event-source.png
[14]: media/v2-update-provision/payg-six-review.png
[15]: media/v2-update-provision/payg-seven-deploy.png
[16]: media/v2-update-provision/payg-eight-environment.png
[17]: media/v2-update-provision/payg-nine-data-access.png
[18]: media/v2-update-provision/payg-ten-verify.png

[19]: media/v2-update-provision/analyze-one-portal.png
[20]: media/v2-update-provision/analyze-two-unparented.png
[21]: media/v2-update-provision/analyze-three-show-pressure.png
[22]: media/v2-update-provision/analyze-four-chart.png
[23]: media/v2-update-provision/analyze-five-chart.png
[24]: media/v2-update-provision/analyze-six-from.png
[25]: media/v2-update-provision/analyze-seven-change-from.png
[26]: media/v2-update-provision/analyze-eight-all.png

[27]: media/v2-update-provision/define-one-model.png
[28]: media/v2-update-provision/define-two-add.png
[29]: media/v2-update-provision/define-three-variable.png
[30]: media/v2-update-provision/define-four-avg.png
[31]: media/v2-update-provision/define-five-humidity.png
[32]: media/v2-update-provision/define-six-type.png
[33]: media/v2-update-provision/define-seven-hierarchy.png
[34]: media/v2-update-provision/define-eight-add-hierarchy.png
[35]: media/v2-update-provision/define-nine-created.png
[36]: media/v2-update-provision/define-ten-edit.png
[37]: media/v2-update-provision/define-eleven-chiller.png
[38]: media/v2-update-provision/define-twelve.png
[39]: media/v2-update-provision/define-thirteen-explore.png
[40]: media/v2-update-provision/define-fourteen-show-max.png
[41]: media/v2-update-provision/define-fifteen-filter.png
[42]: media/v2-update-provision/define-sixteen.png
[43]: media/v2-update-provision/define-seventeen.png
[44]: media/v2-update-provision/define-eighteen.png