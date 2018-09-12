---
title: Definir um novo tipo de dispositivo no Azure IoT Central | Microsoft Docs
description: Este tutorial mostra-lhe como definir, enquanto construtor, um novo tipo de dispositivo na aplicação do Azure IoT Central. O utilizador define a telemetria, o estado, as propriedades e as definições para o seu tipo.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: a2601f55bbc7e99321689afdafcab3135b94bd5b
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702549"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application"></a>Tutorial: Definir um novo tipo de dispositivo na aplicação do Azure IoT Central

Este tutorial mostra-lhe como utilizar, enquanto construtor, um modelo de dispositivo para definir um novo tipo de dispositivo na aplicação do Azure IoT Central. Um modelo do dispositivo define a telemetria, o estado, as propriedades e as definições para o tipo de dispositivo.

Para poder testar a sua aplicação antes de ligar a um dispositivo real, o Azure IoT Central gera um dispositivo simulado a partir do modelo de dispositivo ao criá-lo.

Neste tutorial, vai criar um modelo de dispositivo de **Ar Condicionado Ligado**. Um dispositivo de ar condicionado ligado:

* Envia telemetria, como temperatura e humidade.
* Comunica o estado, como se está ligado ou desligado.
* Tem propriedades como a versão de firmware e o número de série.
* Tem definições como a temperatura de destino e a velocidade da ventoinha.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um novo modelo de dispositivo
> * Adicionar telemetria ao seu dispositivo
> * Ver telemetria simulada
> * Definir medição de eventos
> * Ver eventos simulados
> * Definir medição de estado
> * Ver estado simulado
> * Utilizar propriedades do dispositivo
> * Utilizar definições do dispositivo
> * Utilizar comandos

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de uma aplicação do Azure IoT Central. Se concluiu o início rápido [Criar uma aplicação do Azure IoT Central](quick-deploy-iot-central.md), pode reutilizar a aplicação que criou no início rápido. Caso contrário, execute os seguintes passos para criar uma aplicação do Azure IoT Central vazia:

1. Navegue para a página do [Gestor de Aplicações](https://aka.ms/iotcentral) do Azure IoT Central.

2. Introduza o endereço de e-mail e a palavra-passe que utiliza para aceder à sua subscrição do Azure:

   ![Introduza a sua conta da organização](./media/tutorial-define-device-type/sign-in.png)

3. Para começar a criar uma nova aplicação do Azure IoT Central, escolha **Nova Aplicação**:

    ![Página do Gestor de Aplicações do Azure IoT Central](./media/tutorial-define-device-type/iotcentralhome.png)

4. Para criar uma nova aplicação do Azure IoT Central:

    * Escolha um nome simpático para a aplicação, como **Ares Condicionados Contoso**. O Azure IoT Central gera um prefixo de URL exclusivo para si. Pode alterar este prefixo de URL para algo mais memorável.
    
    * Escolha uma subscrição do Azure e do Azure Active Directory para utilizar. Para obter mais informações sobre as subscrições e diretórios, veja [Criar uma aplicação do Azure IoT Central](howto-create-application.md).
    
    * Utilize um grupo de recursos existente ou crie um novo grupo de recursos com um nome à sua escolha. Por exemplo, **contoso-rg**.
    
    * Escolha a região geograficamente mais próxima de si.
    
    * Escolha o modelo de aplicação da **Aplicação Personalizada**.
    
    * Escolha o plano de pagamento **Aplicação de Avaliação Gratuita de 30 Dias**.
    
    * Escolha **Criar**.

    ![Página da Aplicação do Azure IoT Central](./media/tutorial-define-device-type/iotcentralcreate.png)

Para obter mais informações, veja [Como criar uma aplicação do Azure IoT Central](howto-create-application.md).

## <a name="create-a-new-custom-device-template"></a>Criar um novo modelo de dispositivo personalizado

Como construtor, pode criar e editar os modelos de dispositivos na sua aplicação. Quando cria um modelo de dispositivo, o Azure IoT Central gera um dispositivo simulado a partir do modelo. O dispositivo simulado gera telemetria que lhe permite testar o comportamento da aplicação antes de ligar um dispositivo físico.

Para adicionar um novo modelo de dispositivo à sua aplicação, visite a página **Construtor da Aplicação**. Para tal, selecione o **Construtor da aplicação** no menu de navegação à esquerda.

![Página Application Builder](./media/tutorial-define-device-type/builderhome.png)

## <a name="add-a-device-and-define-telemetry"></a>Adicionar um dispositivo e definir a telemetria

Os passos seguintes mostram como criar um novo modelo de dispositivo de **Ar Condicionado Ligado** para dispositivos que enviam telemetria de temperatura para a sua aplicação:

1. Na página **Construtor da Aplicação**, escolha **Criar Modelo do Dispositivo**:

    ![Página Construtor da Aplicação, Criar Modelo do Dispositivo](./media/tutorial-define-device-type/builderhomedevices.png)

2. Na página **Modelos de Dispositivo**, escolha **Personalizar**. O modelo de dispositivo **Personalizar** permite-lhe definir todas as características e comportamentos do seu ar condicionado ligado:

    ![Dispositivos](./media/tutorial-define-device-type/builderhomedevicescustom.png)

3. Na página **Novo Modelo de Dispositivo**, introduza **Ar Condicionado Ligado** como o nome do seu dispositivo e, em seguida, escolha **Criar**. Também pode carregar uma imagem do seu dispositivo que está visível para os operadores no explorador do dispositivo:

    ![Personalizar Dispositivo](./media/tutorial-define-device-type/createcustomdevice.png)

4. No modelo do dispositivo **Ar Condicionado Ligado**, certifique-se de está na página **Medidas**, onde define a telemetria. Cada modelo de dispositivo que define tem páginas separadas para que possa:

    * Especificar os valores, como telemetria, eventos e estado, enviados pelo dispositivo.
    
    * Definir as definições utilizadas para controlar o dispositivo.
    
    * Definir as propriedades utilizadas para registar informações sobre o dispositivo.
    
    * Definir as regras associadas ao dispositivo.
    
    * Personalizar o dashboard do dispositivo para os operadores.

    ![Medidas do ar condicionado](./media/tutorial-define-device-type/airconmeasurements.png)

    > [!NOTE]
    > Para alterar o nome ou o modelo do dispositivo, clique no texto na parte superior da página.

5. Para adicionar a medição de telemetria de temperatura, escolha **Nova Medida**. Em seguida, escolha **Telemetria** como o tipo de medida:

    ![Medidas do dispositivo de ar condicionado ligado](./media/tutorial-define-device-type/airconmeasurementsnew.png)

6. Cada tipo de telemetria que define para um modelo de dispositivo inclui [opções de configuração](howto-set-up-template.md), como:

    * Opções de apresentação.

    * Detalhes da telemetria.

    * Parâmetros de simulação.

    Para configurar a sua telemetria de **Temperatura**, utilize as informações na tabela seguinte:

    | Definição              | Valor         |
    | -------------------- | -----------   |
    | Nome a Apresentar         | Temperatura   |
    | Nome do Campo           | temperatura   |
    | Unidades                | F             |
    | Mín.                  | 60            |
    | Máx.                  | 110           |
    | Casas decimais       | 0             |

    Também pode escolher uma cor para a apresentação de telemetria. Para guardar a definição de telemetria, escolha **Guardar**:

    ![Configurar a simulação de Temperatura](./media/tutorial-define-device-type/temperaturesimulation.png)

7. Após um curto período, a página **Medidas** mostra um gráfico da telemetria de temperatura a partir do seu dispositivo simulado de ar condicionado ligado. Utilize os controlos para gerir a visibilidade, a agregação ou para editar a definição de telemetria:

    ![Ver simulação de temperatura](./media/tutorial-define-device-type/viewsimulation.png)

8. Também pode personalizar o gráfico com os controlos **Linha**, **Empilhado** e **Editar Intervalo de Tempo**:

    ![Personalizar o gráfico](./media/tutorial-define-device-type/customizechart.png)

## <a name="define-event-measurement"></a>Definir medição de Eventos

Pode utilizar o Evento para definir os dados de ponto no tempo que são enviados pelo dispositivo para indicar algo importante, como um erro ou uma falha de componente. Como os valores de telemetria, o Azure IoT Central pode simular eventos do dispositivo simulado para que possa testar o comportamento da aplicação antes de ligar um dispositivo físico. O utilizador define as medições dos eventos para o tipo de dispositivo na vista **Medidas**.

1. Para adicionar a medição do evento **Erro de Motor da Ventoinha**, escolha **Nova Medição**. Em seguida, escolha **Evento** como o tipo de medição:

    ![Medidas do dispositivo de ar condicionado ligado](./media/tutorial-define-device-type/eventnew.png)

2. Cada tipo de Evento que define para um modelo de dispositivo inclui [opções de configuração](howto-set-up-template.md), como:

   * Nome a Apresentar.

   * Nome do Campo.

   * Gravidade.

    Para configurar o seu evento **Erro do Motor da Ventoinha**, utilize as informações na tabela seguinte:

    | Definição              | Valor             |
    | -------------------- | -----------       |
    | Nome a Apresentar         | Erro de Motor da Ventoinha   |
    | Nome do Campo           | fanmotorerr       |
    | Gravidade             | Erro             |

    Para guardar a definição do evento, escolha **Guardar**:

    ![Configurar medição de Eventos](./media/tutorial-define-device-type/eventconfiguration.png)

3. Após um curto período, a página **Medidas** mostra um gráfico dos eventos gerados de forma aleatória a partir do seu dispositivo simulado de ar condicionado ligado. Utilize os controlos para gerir a visibilidade ou para editar a definição do evento:

    ![Ver simulação do evento](./media/tutorial-define-device-type/eventview.png)

1. Para ver detalhes adicionais sobre o evento, clique no evento no gráfico:

    ![Ver Detalhes do Evento](./media/tutorial-define-device-type/eventviewdetail.png)

## <a name="define-state-measurement"></a>Definir medição de Estado

Pode utilizar o Estado para definir e visualizar o estado do dispositivo ou o respetivo componente durante um período de tempo. Como os valores de telemetria, o Azure IoT Central pode simular o estado do dispositivo simulado para que possa testar o comportamento da aplicação antes de ligar um dispositivo físico. O utilizador define as medições do estado para o tipo de dispositivo na vista **Medidas**.

1. Para adicionar a medição **Modo de Ventoinha**, escolha **Nova Medição**. Em seguida, escolha **Estado** como o tipo de medição:

    ![Medidas do estado do dispositivo de ar condicionado ligado](./media/tutorial-define-device-type/statenew.png)

2. Cada tipo de Estado que define para um modelo de dispositivo inclui [opções de configuração](howto-set-up-template.md), como:

   * Nome a Apresentar.

   * Nome do Campo.

   * Valores com etiquetas de apresentação opcionais.

   * Cor para cada valor.

    Para configurar o seu estado do **Modo da Ventoinha**, utilize as informações na tabela seguinte:

    | Definição              | Valor             |
    | -------------------- | -----------       |
    | Nome a Apresentar         | Modo da Ventoinha          |
    | Nome do Campo           | fanmode           |
    | Valor                | 1                 |
    | Etiqueta de apresentação        | Em funcionamento         |
    | Valor                | 0                 |
    | Etiqueta de apresentação        | Parada           |

    Para guardar a definição da medição do estado, escolha **Guardar**:

    ![Configurar medição de Estado](./media/tutorial-define-device-type/stateconfiguration.png)

3. Após um curto período, a página **Medições** mostra um gráfico dos estados gerados de forma aleatória a partir do seu dispositivo simulado de ar condicionado ligado. Utilize os controlos para gerir a visibilidade ou para editar a definição do estado:

    ![Ver simulação do estado](./media/tutorial-define-device-type/stateview.png)

4. No caso, existem demasiados pontos de dados enviados pelo dispositivo num curto período, a medição de estado é apresentada com um visual diferente, conforme mostrado abaixo. Se clicar no gráfico, então todos os pontos de dados nesse período de tempo são apresentados por ordem cronológica. Também pode reduzir o tempo de intervalo para ver a medição desenhada no gráfico.

    ![Ver Detalhes de estado](./media/tutorial-define-device-type/stateviewdetail.png)

## <a name="settings-properties-and-commands"></a>Definições, propriedades e comandos

As definições, as propriedades, as propriedades do dispositivo e os comandos são valores diferentes definidos num modelo de dispositivo e associados a cada dispositivo individual:

* Utilize _definições_ para enviar dados de configuração para um dispositivo da sua aplicação. Por exemplo, um operador pode utilizar uma definição para alterar o intervalo de telemetria do dispositivo de dois segundos para cinco segundos. Quando um operador altera uma definição, a definição é marcada como pendente na IU até que o dispositivo reconheça que acionou a alteração da definição.

* Utilize _propriedades_ para registar informações sobre o seu dispositivo na sua aplicação. Por exemplo, pode utilizar propriedades para registar o número de série de um dispositivo ou o número de telefone do fabricante do dispositivo. As propriedades são armazenadas na aplicação e não são sincronizadas com o dispositivo. Um operador pode atribuir valores às propriedades.

* Utilize _propriedades do dispositivo_ para ativar um dispositivo para enviar os valores de propriedade para a aplicação. Estas propriedades só podem ser alteradas pelo dispositivo. Para um operador, as propriedades do dispositivo são só de leitura.

* Pode utilizar _comandos_ para gerir remotamente o dispositivo a partir da sua aplicação. Pode executar diretamente comandos no dispositivo a partir da cloud para controlar os dispositivos. Por exemplo, um operador pode executar comandos, como reiniciar, para reiniciar instantaneamente o dispositivo.

## <a name="use-settings"></a>Utilizar as definições

Utilize *definições* para permitir que um operador envie dados de configuração para um dispositivo. Nesta secção, irá adicionar uma definição ao seu modelo de dispositivo **Ar Condicionado Ligado** que permite a um operador definir a temperatura de destino do ar condicionado ligado.

1. Navegue para a página **Definições** para o seu modelo de dispositivo **Ar Condicionado Ligado**:

    ![Preparar para adicionar uma definição](./media/tutorial-define-device-type/deviceaddsetting.png)

    Pode criar definições de diferentes tipos, como texto ou números.

2. Escolha **Número** para adicionar uma definição de número ao seu dispositivo.

3. Para configurar a definição de **Definir Temperatura**, utilize as informações na tabela seguinte:

    | Campo                | Valor           |
    | -------------------- | -----------     |
    | Nome a Apresentar         | Definir Temperatura |
    | Nome do Campo           | setTemperature  |
    | Unidade de medição  | F               |
    | Casas decimais       | 1               |
    | Valor mínimo        | 20              |
    | Valor máximo        | 200             |
    | Valor inicial        | 80              |
    | Descrição          | Defina a temperatura de destino para o ar condicionado |

    Em seguida, escolha **Guardar**:

    ![Configure a definição Definir Temperatura](./media/tutorial-define-device-type/configuresetting.png)

    > [!NOTE]
    > Quando o dispositivo reconhece uma alteração da definição, o estado da definição muda para **sincronizado**.

4. Pode personalizar o esquema da página **Definições** ao mover e redimensionar mosaicos de definições:

    ![Personalizar esquema de definições](./media/tutorial-define-device-type/settingslayout.png)

## <a name="use-properties--device-properties"></a>Propriedades de utilização/propriedades do dispositivo

Utilize *propriedades* para armazenar informações sobre o seu dispositivo na aplicação. Nesta secção, irá adicionar as propriedades da cloud ao modelo de dispositivo **Ar Condicionado Ligado** para armazenar a localização do dispositivo e a data da última assistência. Tenha em atenção que ambas as propriedades do dispositivo são editáveis. Existem também propriedades só de leitura comunicadas pelo dispositivo que não podem ser alteradas, como o número de série do dispositivo e a versão de firmware.
 
1. Navegue para a página **Propriedades** para o seu modelo de dispositivo **Ar Condicionado Ligado**:

    ![Preparar para adicionar uma propriedade](./media/tutorial-define-device-type/deviceaddproperty.png)

    Pode criar propriedades do dispositivo de diferentes tipos, como texto ou números. Para adicionar uma propriedade de localização ao modelo de dispositivo, escolha **Localização**.

2. Para configurar a propriedade da localização, utilize as informações na tabela seguinte:

    | Campo                | Valor                |
    | -------------------- | -------------------- |
    | Nome a Apresentar         | Localização             |
    | Nome do Campo           | localização             |
    | Valor Inicial        | Seattle, WA          |
    | Descrição          | Localização do dispositivo      |

    Deixe os outros campos com os respetivos valores predefinidos.

    ![Configurar as propriedades do dispositivo](./media/tutorial-define-device-type/configureproperties.png)

    Escolha **Guardar**.

3. Para adicionar uma propriedade da data da última assistência ao modelo de dispositivo, escolha **Data**.

4. Para configurar a propriedade da data da última assistência, utilize as informações na tabela seguinte:

    | Campo                | Valor                   |
    | -------------------- | ----------------------- |
    | Nome a Apresentar         | Data da Última Assistência       |
    | Nome do Campo           | serviceDate             |
    | Valor Inicial        | 01/01/2018                |
    | Descrição          | Última assistência           |

    ![Configurar as propriedades do dispositivo](./media/tutorial-define-device-type/configureproperties2.png)

    Escolha **Guardar**.

5. Pode personalizar o esquema da página **Propriedades** ao mover e redimensionar mosaicos de propriedades:

    ![Personalizar esquema de propriedades](./media/tutorial-define-device-type/propertieslayout.png)


## <a name="use-commands"></a>Utilizar comandos

Pode utilizar _comandos_ para ativar um operador para executar comandos diretamente no dispositivo. Nesta secção, vai adicionar um comando ao seu modelo de dispositivo **Ar Condicionado Ligado** que permite a um operador apresentar uma determinada mensagem no ecrã do ar condicionado ligado (isto funciona com o código de exemplo MxChip).

1. Navegue para a página **comandos** para o seu modelo de dispositivo **Ar Condicionado Ligado**:

    ![Preparar para adicionar uma definição](media/tutorial-define-device-type/commandsecho.png)

    Pode criar comandos de tipos diferentes com base nos seus requisitos. 

1. Clique em **Novo Comando** para adicionar um comando ao seu dispositivo.

1. Para configurar o novo comando, utilize as informações na tabela seguinte:

    | Campo                | Valor           |
    | -------------------- | -----------     |
    | Nome a Apresentar         | Comando Echo    |
    | Nome do Campo           | echo            |
    | Tempo Limite Predefinido      | 30              |
    | Tipo a Apresentar         | texto            |
    | Descrição          | Comando do Dispositivo  |  

Pode adicionar mais entradas ao comando ao clicar em **+** para entradas.

2. Escolha **Guardar**.

3. Pode personalizar o esquema da página **Comandos** ao mover e redimensionar mosaicos de comandos:

    ![Personalizar esquema de definições](media/tutorial-define-device-type/commandstileresize.png)

## <a name="view-your-simulated-device"></a>Veja o seu dispositivo simulado

Agora que definiu o seu modelo de dispositivo do **Ar Condicionado Ligado**, pode personalizar o **Dashboard** para incluir as medidas, definições e propriedades que definiu. Em seguida, pode pré-visualizar o dashboard como um operador:

1. Escolha a página **Dashboard** para o seu modelo de dispositivo **Ar Condicionado Ligado**:

    ![Dashboards de ar condicionado ligado](./media/tutorial-define-device-type/aircondashboards.png)

2. Escolha **Gráfico de Linhas** para adicionar o componente ao **Dashboard**:

    ![Componentes do dashboard](./media/tutorial-define-device-type/dashboardcomponents1.png)

3. Configure o componente **Gráfico de Linhas** com as informações na tabela seguinte:

    | Definição      | Valor       |
    | ------------ | ----------- |
    | Título        | Temperatura |
    | Intervalo de Tempo   | Últimos 30 minutos |
    | Medições | temperatura (escolha **Visibilidade** junto a **temperatura**) |

    ![Definições do gráfico de linhas](./media/tutorial-define-device-type/linechartsettings.png)

    Em seguida, escolha **Guardar**.

4. Configure o componente **Gráfico de Eventos** com as informações na tabela seguinte:

    | Definição      | Valor       |
    | ------------ | ----------- |
    | Título        | Eventos |
    | Intervalo de Tempo   | Últimos 30 minutos |
    | Medições | Erro do Motor da Ventoinha (escolha **Visibilidade** junto a **Erro do Motor da Ventoinha**) |

    ![Definições do gráfico de linhas](./media/tutorial-define-device-type/dashboardeventchartsetting.png)

    Em seguida, escolha **Guardar**.

5. Configure o componente **Gráfico de Estado** com as informações na tabela seguinte:

    | Definição      | Valor       |
    | ------------ | ----------- |
    | Título        | Modo da Ventoinha |
    | Intervalo de Tempo   | Últimos 30 minutos |
    | Medições | Modo da Ventoinha (escolha **Visibilidade** junto a **Modo da Ventoinha**) |

    ![Definições do gráfico de linhas](./media/tutorial-define-device-type/dashboardstatechartsetting.png)

    Em seguida, escolha **Guardar**.

6. Para adicionar a opção para definir a temperatura do dashboard, escolha **Definições e Propriedades**:

    ![Componentes do dashboard](./media/tutorial-define-device-type/dashboardcomponents4.png)

7. Configure o componente **Definições e Propriedades** com as informações na tabela seguinte:

    | Definição                 | Valor         |
    | ----------------------- | ------------- |
    | Título                   | Definir temperatura de destino |
    | Definições e Propriedades | Definir Temperatura |

    ![Definições de propriedade do número de série](./media/tutorial-define-device-type/propertysettings3.png)

    Em seguida, escolha **Guardar**.

8. Para adicionar o número de série do dispositivo ao dashboard, escolha **Definições e Propriedades**:

    ![Componentes do dashboard](./media/tutorial-define-device-type/dashboardcomponents3.png)

9. Configure o componente **Definições e Propriedades** com as informações na tabela seguinte:

    | Definição                 | Valor         |
    | ----------------------- | ------------- |
    | Título                   | Número de série |
    | Definições e Propriedades | Número de série |

    ![Definições de propriedade do número de série](./media/tutorial-define-device-type/propertysettings1.png)

    Em seguida, escolha **Guardar**.

10. Para adicionar a versão de firmware do dispositivo ao dashboard, escolha **Definições e Propriedades**:

    ![Componentes do dashboard](./media/tutorial-define-device-type/dashboardcomponents4.png)

11. Configure o componente **Definições e Propriedades** com as informações na tabela seguinte:

    | Definição                 | Valor            |
    | ----------------------- | ---------------- |
    | Título                   | Versão do firmware |
    | Definições e Propriedades | Versão do Firmware |

    ![Definições de propriedade do número de série](./media/tutorial-define-device-type/propertysettings2.png)

    Em seguida, escolha **Guardar**.

12. Para ver o dashboard como operador, desative o **Modo de Estrutura** no canto superior direito da página.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Criar um novo modelo de dispositivo
> * Adicionar telemetria ao seu dispositivo
> * Ver telemetria simulada
> * Definir eventos de dispositivo
> * Ver eventos simulados
> * Definir o seu estado
> * Ver estado simulado
> * Utilizar propriedades do dispositivo
> * Utilizar definições do dispositivo

Agora que definiu um modelo do dispositivo na sua aplicação do Azure IoT Central, eis os passos seguintes sugeridos:

* [Configurar regras e ações para o seu dispositivo](tutorial-configure-rules.md)
* [Personalizar as vistas do operador](tutorial-customize-operator.md)
