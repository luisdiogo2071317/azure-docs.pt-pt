---
title: Configurar um modelo de dispositivo numa aplicação do Azure IoT Central | Microsoft Docs
description: Saiba como configurar um modelo de dispositivo com medidas, definições, propriedades, regras e dashboard.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 0bef8722ee51c916652e6964305f324341052341
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36754667"
---
# <a name="set-up-a-device-template"></a>Configurar um modelo de dispositivo

Um modelo de dispositivo é um blueprint que define as características e comportamentos de um tipo de dispositivo que estabelece ligação a uma aplicação do Microsoft Azure IoT Central.

Por exemplo, um construtor pode criar um modelo de dispositivo para um ventoinha ligado de IoT que tem:

- Medida de telemetria de temperatura

- Medida de eventos de erro motor ventoinha

- Ventoinha medição do Estado de funcionamento

- Definição de velocidade de ventoinha

- Propriedade de localização

- Regras que enviar alertas

- Dashboard que dá-lhe uma todas as cópias de segurança vista sobre o dispositivo

Este modelo de dispositivo, um operador pode criar e ligar dispositivos ventoinha real com nomes como **ventoinha-1** e **ventoinha 2**. Todas estas ventoinhas tem medidas, definições e propriedades, regras e um dashboard que os utilizadores da sua aplicação podem monitorizar e gerir.

> [!NOTE]
> Apenas construtores e os administradores podem criar, editar e eliminar modelos de dispositivos. Qualquer utilizador pode criar dispositivos de **Explorador de dispositivo** página a partir de modelos de dispositivos existentes.

## <a name="create-a-new-device-template"></a>Criar um novo modelo de dispositivo

1. Navegue para o **aplicação Builder** página.

2. Para criar um modelo em branco, escolha **criar modelo de dispositivo**e, em seguida, escolha **personalizada**.

3. Introduza um nome para o novo modelo de dispositivo e escolher **criar**.

    ![Página de detalhes do dispositivo](./media/howto-set-up-template/devicedetailspage.png)

4. Agora está no **detalhes do dispositivo** página de um novo dispositivo simulado. Um dispositivo simulado é criado automaticamente quando criar um novo modelo de dispositivo. Este relatórios de dados e pode ser controlado, tal como um dispositivo real.

Agora observa cada um dos separadores **detalhes do dispositivo** página.

## <a name="measurements"></a>Medições

Medidas são dos dados provenientes do seu dispositivo. Pode adicionar várias medidas para o modelo de dispositivo para corresponder as capacidades do seu dispositivo. Atualmente, telemetria e eventos são os tipos de valores suportados.

- **Telemetria** medidas são os pontos de dados numéricos dispositivo recolhe ao longo do tempo e são representados uma sequência contínua. Por exemplo, temperatura.
- **Evento** medidas são os dados de ponto no tempo que representam algo de significância no dispositivo. Os eventos ter gravidade associada aos mesmos, que representa a importância do evento. Por exemplo, ventoinha motor erro
- **Estado** medidas representam o estado do dispositivo ou os respetivos componentes durante um período de tempo. Por exemplo, ventoinha modo que pode ser definido como tendo a funcionar e parado como dois Estados possíveis.

### <a name="create-a-telemetry-measurement"></a>Criar uma medida de telemetria
Para adicionar uma nova medida de telemetria, clique em **+ nova medida** botão abre-se um formulário com opções para selecionar o tipo de medida. Selecione **telemetria** e introduza os detalhes o **criar telemetria** formulário.

> [!NOTE]
> Quando um dispositivo real está ligada, pague atenção para o nome da medida o dispositivo comunica. O nome tem de corresponder exatamente a **o nome do campo** de uma medida.

Por exemplo, pode adicionar uma medida de telemetria de temperatura novo:

![Formulário de medidas](./media/howto-set-up-template/measurementsform.png)

Após escolher **guardar**, a **temperatura** medida é apresentada na lista de medidas e um operador pode ver a visualização dos dados de temperatura o dispositivo está a recolher.

![Gráfico de medidas](./media/howto-set-up-template/measurementsgraph.png)

### <a name="create-an-event-measurement"></a>Criar uma medida de eventos
Para adicionar uma nova medida de eventos, clique em **+ nova medida** botão abre-se um formulário com opções para selecionar o tipo de medida. Selecione **eventos** e introduza os detalhes o **criar eventos** formulário.

Este formulário fornecer o **nome a apresentar**, **o nome do campo**e o **gravidade** do evento. Pode escolher entre os disponíveis três níveis de gravidade - **erro**, **aviso**, e **informações**.  

Por exemplo, pode adicionar um novo evento de 'Ventoinha Motor Error'.

![Formato de valores de eventos](./media/howto-set-up-template/eventmeasurementsform.png)

Após escolher **guardar**, a **erro Motor ventoinha** medida é apresentada na lista de medidas e um operador pode ver a visualização dos dados de eventos, o dispositivo está a enviar.

![Gráfico de medidas de eventos](./media/howto-set-up-template/eventmeasurementschart.png)

Para ver detalhes adicionais sobre o evento, clique no ícone de eventos no gráfico:

![Detalhe do evento medidas](./media/howto-set-up-template/eventmeasurementsdetail.png)


### <a name="create-a-state-measurement"></a>Criar uma medida de estado
Para adicionar uma nova medida de estado, clique em **+ nova medida** botão abre-se um formulário com opções para selecionar o tipo de medida. Selecione **estado** e introduza os detalhes o **criar estado** formulário.

Este formulário fornecer o **nome a apresentar**, **o nome do campo**e os possíveis **valores** do Estado. Cada **valor** também pode ter um nome a apresentar que será utilizado ao mostrar o valor nos gráficos e tabelas.

Por exemplo, pode adicionar um novo estado de 'Ventoinha Mode', que tem dois valores possíveis que o dispositivo pode enviar, **operativo** e **parado**.

![Formato de valores de estado](./media/howto-set-up-template/statemeasurementsform.png)

Após escolher **guardar**, a **ventoinha modo** medição do estado é apresentado na lista de medidas e o operador pode ver a visualização dos dados de estado, o dispositivo está a enviar.

![Gráfico de medidas de estado](./media/howto-set-up-template/statemeasurementschart.png)

No caso, existem demasiados pontos de dados enviados pelo dispositivo num curto período, a medição de estado é apresentada com um visual diferente, conforme mostrado abaixo. Se clicar no gráfico, então todos os pontos de dados nesse período de tempo são apresentados por ordem cronológica. Também pode reduzir o intervalo de tempo para ver a medição desenhada no gráfico.

![Detalhe de estado medidas](./media/howto-set-up-template/statemeasurementsdetail.png)


## <a name="settings"></a>Definições

As definições de controlar um dispositivo. Permitem que os operadores da sua aplicação para fornecer entradas para o dispositivo. Pode adicionar várias definições ao seu modelo de dispositivo que são apresentados como mosaicos no **definições** separador para operadores a utilizar. Existem seis tipos de definições que pode adicionar: número, texto, data, ativar/desativar, escolha a lista e a etiqueta de secção.

> [!NOTE]
> Quando está ligado um dispositivo real, preste atenção para o nome da definição de relatórios do dispositivo. O nome tem de corresponder exatamente a **o nome do campo** de uma definição.

As definições podem estar em um de três Estados. Estes Estados são reportados pelo dispositivo.

- **Sincronizado**: o dispositivo foi alterada para refletir o valor da definição.

- **Pendente**: o dispositivo está atualmente a mudar para o valor da definição.

- **Erro**: o dispositivo devolveu um erro.

Por exemplo, pode adicionar uma nova definição de velocidade de ventoinha:

![Formulário de definições](./media/howto-set-up-template/settingsform.png)

Depois de escolher **guardar**, a **ventoinha velocidade** definição aparece como um mosaico e está pronta para ser utilizado para alterar a velocidade de ventoinha do dispositivo.

Depois de criar um novo mosaico, pode experimentar a nova definição. Desactivar em primeiro lugar, o modo de estrutura na parte superior direita do ecrã.

![Mosaico de definições](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Propriedades

As propriedades são os metadados associados ao dispositivo, tais como a localização do dispositivo e o número de série do dispositivo. Pode adicionar várias propriedades para o modelo de dispositivo, que são apresentados como mosaicos no **propriedades** separador. Um operador pode especificar os valores de propriedades quando é criar um novo dispositivo e podem editar estes valores em qualquer altura. Existem seis tipos de propriedades, pode adicionar: número, texto, data, ativar/desativar, propriedade do dispositivo e etiqueta.

Existem dois tipos de propriedades:

- **Propriedades do dispositivo** propriedades são reportadas pelo dispositivo.
- **Propriedades da aplicação** são propriedades armazenadas puramente na aplicação. O dispositivo não tem conhecimento de propriedades da aplicação.

> [!NOTE]
> Para as propriedades do dispositivo, quando um dispositivo real está ligada, pague atenção para o nome da propriedade o dispositivo comunica. O nome tem de corresponder exatamente a **o nome do campo** da propriedade. Para propriedades da aplicação, o nome do campo pode ser qualquer coisa que pretende, desde que o nome é exclusivo no modelo de dispositivo.

Por exemplo, pode adicionar localização do dispositivo como uma propriedade novo:

![Formulário de propriedades](./media/howto-set-up-template/propertiesform.png)

Depois de escolher **guardar**, como um mosaico é apresentada a localização do dispositivo:

![Mosaico propriedades](./media/howto-set-up-template/propertiestile.png)

> [!NOTE]
> Depois de criar um novo mosaico, pode alterar o valor da propriedade. Desactivar em primeiro lugar, o modo de estrutura na parte superior direita do ecrã.

### <a name="create-a-location-property-powered-by-azure-maps"></a>Criar uma propriedade de localização com tecnologia do Maps do Azure
Pode fornecer contexto geográfico aos seus dados de localização no Azure IoT Central e mapear qualquer latitude e longitude as coordenadas de uma morada ou simplesmente coordenadas de latitude e longitude. Esta capacidade que no Azure IoT Central utiliza a tecnologia de mapas de Azure.

Existem dois tipos de propriedades de localização, que pode adicionar:
- **Localização como uma propriedade de aplicação** que serão armazenados puramente na aplicação. O dispositivo não tem conhecimento de propriedades da aplicação.
- **Localização como uma propriedade de dispositivo** que será reportada pelo dispositivo.

#### <a name="adding-location-as-an-application-property"></a>A adição de localização como uma propriedade de aplicação 

Pode criar uma localização de mapas de propriedades como uma propriedade de aplicação do Azure a utilizar na sua aplicação do Azure IoT Central. Por exemplo, pode adicionar o endereço de instalação do dispositivo. 

1. Navegue até ao separador de propriedade do dispositivo; Certifique-se de que modo de estrutura está ativado.

   ![Propriedade de localização](./media/howto-set-up-template/locationcloudproperty1.png)

2. No separador de propriedade, clique em localização.
3. Configure opcionalmente o nome a apresentar, nome de campo e o valor inicial da localização. 

   ![Formulário de propriedade de localização](./media/howto-set-up-template/locationcloudproperty2.png)

   Existem dois formatos suportados para adicionar uma localização:
   - **Localização como um endereço**
   - **Localização como coordenadas** 

4.  Clique em **Guardar**. 

    ![Campo de propriedade de localização](./media/howto-set-up-template/locationcloudproperty3.png)

Agora, um operador pode atualizar o valor de localização sob a forma de campo localização. 

#### <a name="adding-location-as-a-device-property"></a>A adição de localização como uma propriedade do dispositivo 

Pode criar uma propriedade de localização como uma propriedade de dispositivo que é reportada pelo dispositivo.
Por exemplo, pretende controlar a localização do dispositivo.

1.  Navegue até ao separador de propriedade do dispositivo; Certifique-se de que modo de estrutura está ativado.

2.  Clique em dispositivos propriedade da biblioteca.

    ![Campo de propriedade de localização](./media/howto-set-up-template/locationdeviceproperty1.png)

3.  Configurar o nome a apresentar, nome do campo e escolha "localização" como um tipo de dados. 

    > [!NOTE]
    > O nome do campo deve corresponder exatamente ao nome da propriedade do dispositivo relatórios. 

    ![Campo de propriedade de localização](./media/howto-set-up-template/locationdeviceproperty2.png)

    ![Vista de operador de propriedade de localização](./media/howto-set-up-template/locationdeviceproperty2.png)

Agora que configurou a propriedade de localização, poderá adicionar um mapa para visualizar a localização no Dashboard do dispositivo. Veja como [adicionar localização do mapa do Azure no Dashboard](howto-set-up-template.md#add-location-azure-map-in-dashboard).

## <a name="commands"></a>Comandos

Os comandos são utilizados para gerir remotamente um dispositivo. Permitem que os operadores da sua aplicação instantaneamente executar comandos no dispositivo. Pode adicionar vários comandos para o modelo de dispositivo que são apresentados como mosaicos no **comandos** separador para operadores a utilizar. Como o construtor do dispositivo tem a flexibilidade para definir o comando de acordo com os seus requisitos.

Como é uma **comando** diferente de um **definição**? 

* Definição: Definição é uma configuração que pretende aplicar a um dispositivo e pretende que o dispositivo para manter essa configuração até que alterá-lo. Por exemplo, pretende definir a temperatura do seu freezer e pretender essa definição, mesmo quando o freezer reiniciado. 

* Comando: Utilizar comandos instantaneamente executar um comando no dispositivo remotamente do IoTCentral, se um dispositivo não estiver ligado, em seguida, o limite de tempo do comando e falha. Por exemplo, pretende reiniciar/reiniciar um dispositivo.  

Comandos quando executado podem estar em um de três Estados, dependendo se o dispositivo recebeu o comando. 

Por exemplo, pode adicionar um novo comando eco:

![Formulário de comandos](./media/howto-set-up-template/commandsecho.png)

Depois de escolher **guardar**, a **eco** comandos aparece como um mosaico e está pronto para ser utilizado para o dispositivo de eco.

Depois de criar um novo mosaico, pode experimentar o comando de novo.

## <a name="rules"></a>Regras

Regras de ativar os operadores monitorizar dispositivos em tempo real. Regras automaticamente invocam **ações** como enviar um e-mail quando a regra é acionado. Há um tipo de regra disponível atualmente:

- **Regra de telemetria:** uma regra de telemetria é acionado quando a telemetria do dispositivo selecionado atravesse um limiar especificado. Saiba mais sobre [regras de telemetria](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

O dashboard é onde um operador pode ir para ver informações sobre um dispositivo. Como um construtor, pode adicionar mosaicos para esta página que o ajudam a compreender a forma como o dispositivo está a comportar os operadores. Pode adicionar vários mosaicos do dashboard ao seu modelo do dispositivo. Existem seis tipos de mosaicos do dashboard pode adicionar: a imagem, gráfico, gráfico de barras, KPI, definições e propriedades de linha e etiqueta.

Por exemplo, pode adicionar um **definições e propriedades** mosaico para mostrar uma seleção dos valores atuais das propriedades e definições:

![Formulário de detalhes de dispositivos do dashboard](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Agora quando um operador vistas do dashboard, pode ver este mosaico que mostra as propriedades e definições do dispositivo:

![Mosaico do dashboard](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-location-azure-map-in-dashboard"></a>Adicionar localização do mapa do Azure no Dashboard

Se tiver configurado uma propriedade de localização como os passos [criar uma propriedade de localização com tecnologia do Azure Maps](howto-set-up-template.md), será possível visualizar a localização, utilizando um mapa diretamente no seu dashboard do dispositivo.

1.  Navegue até ao separador Dashboard do dispositivo; Certifique-se de que modo de estrutura está ativado.

2.  No Dashboard do dispositivo, selecione o mapa da biblioteca. 

    ![Selecione de mapa do Azure de localização de dashboard](./media/howto-set-up-template/locationcloudproperty4map.png)

3.  Dê um título e escolha a propriedade de localização que configurou anteriormente como parte da sua propriedade do dispositivo.

    ![Configurar o Azure mapa de localização de dashboard](./media/howto-set-up-template/locationcloudproperty5map.png)

4.  Guardar e irá ver o mapa de mosaico apresenta a localização que selecionou. 

    ![Localização de dashboard visualizar do mapa do Azure](./media/howto-set-up-template/locationcloudproperty6map.png) 

    Será possível redimensionar o mapa para o seu tamanho pretendido.

    Agora quando um operador vistas do dashboard, pode ver estes todos os os Dashboard mosaicos que configurou, incluindo uma mapa de localização!

    ![Dashboard de mapa do Azure de localização de dashboard](./media/howto-set-up-template/locationcloudproperty7map.png) 

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu a configurar um modelo de dispositivo na sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Criar uma nova versão de modelo do dispositivo](howto-version-devicetemplate.md)
