---
title: Configurar um modelo de dispositivo numa aplicação do Azure IoT Central | Microsoft Docs
description: Saiba como configurar um modelo de dispositivo com medidas, definições, propriedades, regras e um dashboard.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: f71d4c7dc94fedfd598ab87c51366ba9fb1f184a
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063040"
---
# <a name="set-up-a-device-template"></a>Configurar um modelo de dispositivo

Um modelo de dispositivo é um blueprint que define as características e comportamentos de um tipo de dispositivo que estabelece ligação a uma aplicação do Microsoft Azure IoT Central.

Por exemplo, um construtor pode criar um modelo de dispositivo para um ventoinha ligados de IoT que tem:

- Medida de telemetria de temperatura

- Medida de eventos de erro motor ventoinha

- Ventoinha medição do Estado de funcionamento

- Definição de velocidade de ventoinha

- Propriedade de localização

- Regras que enviar alertas

- Dashboard que dá-lhe uma vista global do dispositivo

Este modelo de dispositivo, um operador pode criar e ligar dispositivos ventoinha real com nomes como **ventoinha-1** e **ventoinha 2**. Todas estas ventoinhas tem medidas, definições, propriedades, regras e um dashboard que os utilizadores da sua aplicação podem monitorizar e gerir.

> [!NOTE]
> Apenas construtores e os administradores podem criar, editar e eliminar modelos de dispositivos. Qualquer utilizador pode criar os dispositivos no **Explorador de dispositivo** página a partir de modelos de dispositivos existentes.

## <a name="create-a-device-template"></a>Criar um modelo de dispositivo

1. Vá para o **aplicação Builder** página.

2. Para criar um modelo em branco, selecione **criar modelo de dispositivo**e, em seguida, selecione **personalizada**.

3. Introduza um nome para o novo modelo de dispositivo e selecione **criar**.

   ![Página de detalhes do dispositivo com "Refrigerator" como o nome do modelo](./media/howto-set-up-template/devicedetailspage.png)

4. Agora está no **detalhes do dispositivo** página de um novo dispositivo simulado. Um dispositivo simulado é criado automaticamente quando criar um modelo de dispositivo. Este relatórios de dados e pode ser controlado, tal como um dispositivo real.

Agora observa cada um dos separadores **detalhes do dispositivo** página.

## <a name="measurements"></a>Medições

Medidas são os dados que provém do seu dispositivo. Pode adicionar várias medidas para o modelo de dispositivo para corresponder as capacidades do seu dispositivo.

- **Telemetria** medidas são os pontos de dados numéricos que recolhe o seu dispositivo ao longo do tempo. Se estiver a representado como um fluxo contínuo. Um exemplo é temperatura.
- **Evento** medidas são os dados de ponto no tempo que representa algo de significância no dispositivo. Um nível de gravidade representa a importância de um evento. Um exemplo é um erro de motor ventoinha.
- **Estado** medidas representam o estado do dispositivo ou os respetivos componentes durante um período de tempo. Por exemplo, um modo de ventoinha pode ser definido como tendo **operativo** e **parado** como dois Estados possíveis.

### <a name="create-a-telemetry-measurement"></a>Criar uma medida de telemetria
Para adicionar uma nova medida de telemetria, selecione o **+ nova medida** botão. Selecione **telemetria** como a medição escreva e introduza os detalhes o **criar telemetria** formulário.

> [!NOTE]
> Quando está ligado um dispositivo real, preste atenção ao nome de medida que o dispositivo comunica. O nome tem de corresponder exatamente a **o nome do campo** entrada para uma medida.

Por exemplo, pode adicionar uma medida de telemetria de temperatura novo:

!["Criar telemetria" formulário com detalhes para a medida de temperatura](./media/howto-set-up-template/measurementsform.png)

Depois de selecionar **guardar**, a **temperatura** medida é apresentado na lista de medidas. Um operador pode ver a visualização dos dados de temperatura que o dispositivo está a recolher.

![Gráfico de medida](./media/howto-set-up-template/measurementsgraph.png)

### <a name="create-an-event-measurement"></a>Criar uma medida de eventos
Para adicionar uma nova medida de evento, selecione o **+ nova medida** botão. Selecione **eventos** como a medição escreva e introduza os detalhes o **criar eventos** formulário.

Forneça o **nome a apresentar**, **o nome do campo**, e **gravidade** detalhes para o evento. Pode escolher entre os três níveis de gravidade disponíveis: **erro**, **aviso**, e **informações**.  

Por exemplo, pode adicionar um novo **erro Motor ventoinha** eventos.

!["Criar evento" formulário com detalhes para um evento de motor ventoinha](./media/howto-set-up-template/eventmeasurementsform.png)

Depois de selecionar **guardar**, a **erro Motor ventoinha** medida é apresentado na lista de medidas. Um operador pode ver a visualização de dados de eventos que o dispositivo está a enviar.

![Gráfico de medição de eventos](./media/howto-set-up-template/eventmeasurementschart.png)

Para ver mais detalhes sobre o evento, selecione o ícone de eventos no gráfico:

![Detalhes para o evento de "Ventoinha Motor Error"](./media/howto-set-up-template/eventmeasurementsdetail.png)


### <a name="create-a-state-measurement"></a>Criar uma medida de estado
Para adicionar uma nova medida de estado, selecione o **+ nova medida** botão. Selecione **estado** como a medição escreva e introduza os detalhes o **criar estado** formulário.

Forneça os detalhes para **nome a apresentar**, **o nome do campo**, e **valores** do Estado. Cada valor pode também ter um nome a apresentar que será utilizado quando o valor for apresentada nos gráficos e tabelas.

Por exemplo, pode adicionar um novo **ventoinha modo** Estado que tem dois valores possíveis que o dispositivo pode enviar, **operativo** e **parado**.

![Formato de "Estado de edição" com detalhes para o modo de ventoinha](./media/howto-set-up-template/statemeasurementsform.png)

Depois de selecionar **guardar**, a **ventoinha modo** medição do estado é apresentado na lista de medidas. O operador pode ver a visualização dos dados de estado que o dispositivo está a enviar.

![Gráfico de estado de medida](./media/howto-set-up-template/statemeasurementschart.png)

Se o dispositivo envia demasiados pontos de dados num pequeno período de tempo, a medição de estado é apresentado um visual diferentes, conforme mostrado na captura de ecrã seguinte. Se clicar no gráfico, todos os pontos de dados dentro desse período de tempo são apresentados por uma ordem cronológica. Também pode reduzir o intervalo de tempo para ver a medição desenhada no gráfico.

![Detalhes para a medição do Estado de "Modo de ventoinha estático"](./media/howto-set-up-template/statemeasurementsdetail.png)


## <a name="settings"></a>Definições

As definições de controlar um dispositivo. Permitem que os operadores da sua aplicação para fornecer entradas para o dispositivo. Pode adicionar várias definições ao seu modelo de dispositivo que são apresentados como mosaicos no **definições** separador para operadores a utilizar. Pode adicionar seis tipos de definições: número, texto, data, ativar/desativar, escolha a lista e a etiqueta de secção.

> [!NOTE]
> Quando está ligado um dispositivo real, preste atenção para o nome da definição de que o dispositivo comunica. O nome tem de corresponder exatamente a **o nome do campo** entrada para uma definição.

As definições podem estar em um de três Estados. O dispositivo relata estes Estados.

- **Sincronizado**: O dispositivo foi alterada para refletir o valor da definição.

- **Pendente**: O dispositivo está atualmente a mudar para o valor da definição.

- **Erro**: O dispositivo devolveu um erro.

Por exemplo, pode adicionar uma nova definição de velocidade de ventoinha:

!["Configurar o número de" formulário com detalhes para as definições de velocidade](./media/howto-set-up-template/settingsform.png)

Depois de selecionar **guardar**, a **ventoinha velocidade** definição aparece como um mosaico e está pronta para ser utilizado para alterar a velocidade de ventoinha do dispositivo.

Depois de criar um mosaico, pode experimentar a nova definição. Desactivar em primeiro lugar, modo de estrutura, a parte superior direito do ecrã.

![Separador "Definições" com o comutador de "Modo de estrutura" para o mosaico](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Propriedades

As propriedades são os metadados do dispositivo associado ao dispositivo, tais como a localização do dispositivo e o número de série. Pode adicionar várias propriedades para o modelo de dispositivo que são apresentados como mosaicos no **propriedades** separador. Um operador pode especificar os valores de propriedades quando criarem um dispositivo e podem editar estes valores em qualquer altura. Pode adicionar seis tipos de propriedades: número, texto, data, ativar/desativar, propriedade do dispositivo e etiqueta.

Existem duas categorias de propriedades:

- **Dispositivo** propriedades que o dispositivo comunica.
- **Aplicação** propriedades que são armazenadas puramente na aplicação. O dispositivo não reconhece as propriedades da aplicação.

> [!NOTE]
> Para as propriedades do dispositivo quando está ligado um dispositivo real, preste atenção para o nome da propriedade que o dispositivo comunica. O nome tem de corresponder exatamente a **o nome do campo** entrada para a propriedade. Para propriedades da aplicação, o nome do campo pode ser qualquer coisa que pretende, desde que o nome é exclusivo no modelo de dispositivo.

Por exemplo, pode adicionar localização do dispositivo como uma propriedade novo:

![Formato de "Configurar o texto" no separador "Propriedades"](./media/howto-set-up-template/propertiesform.png)

Depois de selecionar **guardar**, como um mosaico é apresentada a localização do dispositivo:

![Mosaico de localização](./media/howto-set-up-template/propertiestile.png)

Depois de criar um mosaico, pode alterar o valor da propriedade. Desactivar em primeiro lugar, modo de estrutura na parte superior direito do ecrã.

### <a name="create-a-location-property-through-azure-maps"></a>Criar uma propriedade de localização através do Azure Maps
Pode fornecer contexto geográfico aos seus dados de localização no Azure IoT Central e mapear qualquer coordenadas de latitude e longitude de uma morada. Ou pode simplesmente latitude e longitude coordenadas do mapa. Mapas do Azure permite esta capacidade no Centro de IoT.

Pode adicionar dois tipos de propriedades de localização:
- **Localização como uma propriedade de aplicação**, que é armazenado puramente na aplicação. O dispositivo não reconhece as propriedades da aplicação.
- **Localização como uma propriedade de dispositivo**, que o dispositivo comunica.

#### <a name="add-location-as-an-application-property"></a>Adicionar localização como uma propriedade de aplicação 
Pode criar uma propriedade de localização como uma propriedade de aplicação utilizando mapas do Azure na sua aplicação do Centro de IoT. Por exemplo, pode adicionar o endereço de instalação do dispositivo. 

1. No **propriedades** separador, certifique-se de que **modo de estrutura** é **no**.

   ![Separador "Propriedades" com o modo de estrutura ativado](./media/howto-set-up-template/locationcloudproperty1.png)

2. Na biblioteca, selecione **localização**.
3. Configurar **nome a apresentar**, **o nome do campo**e (opcionalmente) **valor inicial** para a localização. 

   !["Configurar a localização" formulário com detalhes para a localização](./media/howto-set-up-template/locationcloudproperty2.png)

   Existem dois formatos suportados para adicionar uma localização:
   - **Localização como um endereço**
   - **Localização como coordenadas** 

4. Selecione **Guardar**. 

   ![Propriedade de localização com o endereço de instalação adicionado](./media/howto-set-up-template/locationcloudproperty3.png)

Agora, um operador pode atualizar o valor de localização sob a forma de campo localização. 

#### <a name="add-location-as-a-device-property"></a>Adicionar localização como uma propriedade do dispositivo 

Pode criar uma propriedade de localização como uma propriedade de dispositivo que o dispositivo comunica. Por exemplo, se pretender controlar a localização do dispositivo:

1. No **propriedades** separador, certifique-se de que **modo de estrutura** é **no**.

   ![Separador "Propriedades" com o modo de estrutura ativado](./media/howto-set-up-template/locationdeviceproperty1.png)

2. Selecione **propriedade do dispositivo** da biblioteca.
3. Configurar o nome a apresentar e o nome de campo e selecione **localização** como o tipo de dados. 

   > [!NOTE]
   > O nome do campo tem de corresponder exatamente ao nome da propriedade que o dispositivo comunica. 

   !["Configurar propriedades do dispositivo" formulário com detalhes para a localização](./media/howto-set-up-template/locationdeviceproperty2.png)

Agora que configurou a sua propriedade de localização, pode [adicionar um mapa para visualizar a localização no dashboard do dispositivo](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Comandos

Os comandos são utilizados para gerir remotamente um dispositivo. Permitem que os operadores da sua aplicação de forma instantânea executar comandos no dispositivo. Pode adicionar vários comandos para o modelo de dispositivo que são apresentados como mosaicos no **comandos** separador para operadores a utilizar. Como o construtor do dispositivo, tem a flexibilidade para definir os comandos de acordo com os requisitos.

Como é que um comando diferente da definição de uma? 

* **Definição**: uma definição é uma configuração que pretende aplicar a um dispositivo e pretende que o dispositivo para manter essa configuração até que alterá-lo. Por exemplo, pretende definir a temperatura do seu freezer e pretende que definir, mesmo quando o freezer for reiniciado. 

* **Comando**: Utilize comandos instantaneamente executá um comando no dispositivo remotamente a partir do Centro de IoT. Se um dispositivo não está ligado, o comando exceder o tempo limite e falha. Por exemplo, pretende reiniciar um dispositivo.  

Quando executar um comando, pode ser um de três Estados, dependendo se o dispositivo recebeu o comando. 

Por exemplo, pode adicionar um novo **eco** comando:

!["Configurar o comando" formulário com detalhes para eco](./media/howto-set-up-template/commandsecho.png)

Depois de selecionar **guardar**, a **eco** comandos aparece como um mosaico e está pronto para ser utilizado para o dispositivo de eco.

Depois de criar um mosaico, pode experimentar o comando de novo.

## <a name="rules"></a>Regras

Regras de ativar os operadores monitorizar dispositivos em tempo real. Regras de invocam automaticamente ações como enviar um e-mail quando a regra é acionada. Está atualmente disponível um tipo de regra:

- **Regra de telemetria**, que é acionado quando a telemetria do dispositivo selecionado atravesse um limiar especificado. [Saiba mais sobre as regras de telemetria](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

O dashboard é onde um operador pode ir para ver informações sobre um dispositivo. Como um construtor, pode adicionar mosaicos nesta página para ajudar a compreender a forma como o dispositivo está a comportar os operadores. Pode adicionar vários mosaicos do dashboard ao seu modelo do dispositivo. Pode adicionar seis tipos de mosaicos do dashboard: a imagem, gráfico, gráfico de barras, KPI, definições e propriedades de linha e etiqueta.

Por exemplo, pode adicionar um **definições e propriedades** mosaico para mostrar uma seleção dos valores atuais das propriedades e definições:

!["Configurar os detalhes do dispositivo" formulário com detalhes para as definições e propriedades](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Agora quando um operador vistas do dashboard, pode ver este mosaico que mostra as propriedades e definições do dispositivo:

![Separador "Dashboard" com as definições apresentadas e propriedades para o mosaico](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Adicione uma localização de mapas do Azure no dashboard

Se tiver configurado uma propriedade de localização anterior na [criar uma propriedade de localização através do Azure Maps](#create-a-location-property-through-azure-maps), pode visualizar a localização, utilizando um mapa no seu dashboard do dispositivo.

1. No **Dashboard** separador, certifique-se de que **modo de estrutura** é **no**.

   ![Separador "Dashboard" com o modo de estrutura ativado](./media/howto-set-up-template/locationcloudproperty4map.png)

2. No dashboard do dispositivo, selecione **mapa** da biblioteca. 
3. Dê um título e escolha a propriedade de localização que tenha configurado como parte das propriedades do dispositivo.

   !["Configurar o mapa" formulário com detalhes para o título e propriedades](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Selecione **Guardar**. O mosaico de mapa apresenta agora a localização que selecionou. 

   ![Mosaico de mapa com a localização selecionada](./media/howto-set-up-template/locationcloudproperty6map.png) 

Pode redimensionar o mapa para o seu tamanho pretendido.

Agora quando um operador vistas do dashboard, pode ver todos os mosaicos do dashboard que tiver configurado, incluindo um mapa de localização.

![Mosaicos no dashboard](./media/howto-set-up-template/locationcloudproperty7map.png) 

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como configurar um modelo de dispositivo na sua aplicação do Azure IoT Central, pode:

> [!div class="nextstepaction"]
> [Criar uma nova versão de modelo do dispositivo](howto-version-devicetemplate.md)
