---
title: Configurar um modelo de dispositivo num aplicativo do Azure IoT Central | Documentos da Microsoft
description: Saiba como configurar um modelo de dispositivo com medidas, configurações, propriedades, regras e um dashboard.
author: viv-liu
ms.author: viviali
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: a076ee5275a4d053613902a1980542590263385c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034282"
---
# <a name="set-up-a-device-template"></a>Configurar um modelo de dispositivo

Um modelo de dispositivo é um esquema que define as características e comportamentos de um tipo de dispositivo que se liga a uma aplicação do Microsoft Azure IoT Central.

Por exemplo, um construtor pode criar um modelo de dispositivo para um fã de ligado de IoT que tem:

- Medida de telemetria de temperatura

- Medida de eventos de erro motor ventoinha

- Ventoinha medição do Estado de funcionamento

- Definição de velocidade de ventoinha

- Propriedade de localização

- Regras que enviar alertas

- Dashboard que lhe dá uma visão geral do dispositivo

Partir deste modelo de dispositivo, pode criar e ligar os dispositivos de ventoinha real com nomes como um operador **ventoinha-1** e **ventoinha-2**. Todos estes fãs tem medidas, configurações, propriedades, regras e um dashboard que os utilizadores da sua aplicação podem monitorizar e gerir.

> [!NOTE]
> Apenas os construtores e os administradores podem criar, editar e eliminar modelos de dispositivos. Qualquer utilizador pode criar dispositivos sobre o **Device Explorer** página a partir de modelos de dispositivos existentes.

## <a name="create-a-device-template"></a>Criar um modelo de dispositivo

1. Vá para o **Application Builder** página.

2. Para criar um modelo em branco, selecione **criar modelo de dispositivo**e, em seguida, selecione **personalizado**.

3. Introduza um nome para o novo modelo de dispositivo e selecione **criar**.

   ![Página de detalhes do dispositivo com "Refrigerador" como o nome do modelo](./media/howto-set-up-template/devicedetailspage.png)

4. Agora, está na **detalhes do dispositivo** página de um novo dispositivo simulado. Um dispositivo simulado é criado automaticamente quando cria um modelo de dispositivo. -Relatórios de dados e pode ser controlado como um dispositivo real.

Agora observar cada uma das guias o **detalhes do dispositivo** página.

## <a name="measurements"></a>Medições

Medidas são os dados provenientes do seu dispositivo. Pode adicionar várias medidas ao seu modelo de dispositivo de acordo com as capacidades do seu dispositivo.

- **Telemetria** medidas são os pontos de dados numéricos que recolhe o seu dispositivo ao longo do tempo. São representadas como um fluxo contínuo. Um exemplo é a temperatura.
- **Evento** medidas são os dados de ponto no tempo que representa algo de significância no dispositivo. Um nível de gravidade representa a importância de um evento. Um exemplo é um erro de motor de ventoinha.
- **Estado** medidas representam o estado do dispositivo ou de seus componentes durante um período de tempo. Por exemplo, um modo de ventoinha pode ser definido como tendo **operacional** e **parado** como dois Estados possíveis.

### <a name="create-a-telemetry-measurement"></a>Criar uma medida de telemetria
Para adicionar uma nova medida de telemetria, selecione **Editar modelo**e, em seguida, clique nas **+ nova medida** botão. Selecione **telemetria** como a medição escreva e introduza os detalhes o **telemetria criar** formulário.

> [!NOTE]
> Quando estiver ligado um dispositivo real, preste atenção ao nome da medida que o dispositivo comunica. O nome deve corresponder exatamente a **nome do campo** entrada para uma medida.

Por exemplo, pode adicionar uma nova medida de telemetria de temperatura:

![Formulário de "Criar telemetria" com detalhes para medidas de temperatura](./media/howto-set-up-template/measurementsform.png)

Depois de selecionar **feito**, o **temperatura** medição aparece na lista de medidas. Um operador pode ver a visualização dos dados de temperatura que o dispositivo está a recolher.

> [!NOTE]
  O tipo de dados da medição de telemetria é duplo.

### <a name="create-an-event-measurement"></a>Criar uma medida de eventos
Para adicionar uma nova medida de evento, selecione **Editar modelo**e, em seguida, clique nas **+ nova medida** botão. Selecione **evento** como a medição escreva e introduza os detalhes o **Create Event** formulário.

Forneça o **nome a apresentar**, **nome do campo**, e **gravidade** detalhes do evento. Pode escolher entre três níveis de gravidade disponíveis: **erro**, **aviso**, e **informações**.  

Por exemplo, pode adicionar um novo **erro de Motor de ventoinha** eventos.

![Formulário de "Criar eventos" com detalhes para um evento de motor de ventoinha](./media/howto-set-up-template/eventmeasurementsform.png)

Depois de selecionar **feito**, o **erro de Motor de ventoinha** medição aparece na lista de medidas. Um operador pode ver a visualização dos dados do evento que o dispositivo está a enviar.

![Gráfico de medição de eventos](./media/howto-set-up-template/eventmeasurementschart.png)

Para ver mais detalhes sobre o evento, selecione o ícone de eventos no gráfico.

![Detalhes do evento de "Erro de Motor de ventoinha"](./media/howto-set-up-template/eventmeasurementsdetail.png)

> [!NOTE]
  O tipo de dados da medição de eventos é a cadeia de caracteres.

### <a name="create-a-state-measurement"></a>Criar uma medida de estado
Para adicionar uma nova medida de estado, selecione **Editar modelo**e, em seguida, clique nas **+ nova medida** botão. Selecione **estado** como a medição escreva e introduza os detalhes o **estado de criar** formulário.

Forneça os detalhes para **nome a apresentar**, **nome do campo**, e **valores** do Estado. Cada valor também pode ter um nome a apresentar que será utilizado quando o valor é apresentado em gráficos e tabelas.

Por exemplo, pode adicionar um novo **ventoinha modo** estado de que tem dois valores possíveis que o dispositivo pode enviar, **operacional** e **parado**.

![Formulário de "Estado de edição" com detalhes para o modo de ventoinha](./media/howto-set-up-template/statemeasurementsform.png)

Depois de selecionar **feito**, o **modo ventoinha** medição do estado é apresentado na lista de medidas. O operador pode ver a visualização dos dados de estado que o dispositivo está a enviar.

![Gráfico de estado de medida](./media/howto-set-up-template/statemeasurementschart.png)

Se o dispositivo envia demasiados pontos de dados num pequeno período de tempo, a medição de estado é apresentado com um elemento visual diferente, como mostrado na captura de ecrã seguinte. Se clicar no gráfico, todos os pontos de dados dentro desse período de tempo são apresentados numa ordem cronológica. Também pode reduzir o intervalo de tempo para ver a medição plotada no gráfico.

> [!NOTE]
  O tipo de dados da medição de estado é a cadeia de caracteres.

## <a name="settings"></a>Definições

Definições de controlarem um dispositivo. Permitem que os operadores da sua aplicação para fornecer entradas para o dispositivo. Pode adicionar várias definições ao seu modelo de dispositivo que são apresentados como mosaicos no **definições** separador para operadores para utilizar. Pode adicionar seis tipos de definições: número, texto, data, alternar, lista de seleção e rótulo de seção.

> [!NOTE]
> Quando estiver ligado um dispositivo real, preste atenção ao nome da definição que o dispositivo comunica. O nome deve corresponder exatamente a **nome do campo** entrada para uma definição.

Elas podem estar em um de três Estados. O dispositivo comunica esses Estados.

- **Sincronizado**: O dispositivo foi alterado para refletir o valor de definição.

- **Pendente**: O dispositivo atualmente está mudando para o valor de definição.

- **Erro**: O dispositivo devolveu um erro.

Por exemplo, pode adicionar uma nova definição de velocidade de ventoinha selecionando **Editar modelo** e inserindo a nova definição:

![Formulário de "Configurar o número" com detalhes para as definições de velocidade](./media/howto-set-up-template/settingsform.png)

Depois de selecionar **salvar**, o **ventoinha velocidade** definição é apresentada como um mosaico e está pronta para ser utilizada para alterar a velocidade de fã do dispositivo.

Depois de criar um mosaico, pode experimentar a nova definição. Primeiro, selecione **feito** na parte do canto superior direito da tela.

![Separador "Definições" com a opção "Modo de Design" para o mosaico](./media/howto-set-up-template/settingstile.png)

## <a name="properties"></a>Propriedades

Propriedades são os metadados do dispositivo que está associada do dispositivo, como a localização do dispositivo e o número de série. Pode adicionar várias propriedades para o modelo de dispositivo que são apresentados como mosaicos no **propriedades** separador. Um operador pode especificar os valores de propriedades quando criam um dispositivo e podem editar estes valores em qualquer altura. Pode adicionar seis tipos de propriedades: número, texto, data, alternar, propriedade do dispositivo e etiqueta.

Existem duas categorias de propriedades:

- **Dispositivo** propriedades que o dispositivo comunica.
- **Aplicação** propriedades que são armazenadas puramente no aplicativo. O dispositivo não reconhece as propriedades da aplicação.

> [!NOTE]
> Para as propriedades dos dispositivos, quando está ligado um dispositivo real, preste atenção ao nome da propriedade que o dispositivo comunica. O nome deve corresponder exatamente a **nome do campo** entrada para a propriedade. Para propriedades da aplicação, o nome do campo pode ser tudo o que quiser, desde que o nome é exclusivo no modelo do dispositivo.

Por exemplo, pode adicionar localização do dispositivo como uma nova propriedade selecionando **Editar modelo** e inserindo a nova propriedade:

![Formulário de "Configurar o texto" na guia "Propriedades"](./media/howto-set-up-template/propertiesform.png)

Depois de selecionar **guardar**, localização do dispositivo é apresentado como um mosaico:

![Mosaico de localização](./media/howto-set-up-template/propertiestile.png)

Depois de criar um mosaico, pode alterar o valor da propriedade. Primeiro, selecione **feito** na parte canto superior direito da tela.

### <a name="create-a-location-property-through-azure-maps"></a>Criar uma propriedade de localização por meio do Azure Maps
Pode dar contexto geográfico aos seus dados de localização no Azure IoT Central e mapear qualquer coordenadas de latitude e longitude de uma morada. Ou pode simplesmente latitude e longitude coordenadas do mapa. Mapas do Azure permite que esta capacidade no Centro de IoT.

Pode adicionar dois tipos de propriedades de localização:
- **Localização como uma propriedade de aplicativo**, que é armazenado puramente no aplicativo. O dispositivo não reconhece as propriedades da aplicação.
- **Localização como uma propriedade de dispositivo**, que informa o dispositivo.

#### <a name="add-location-as-an-application-property"></a>Adicionar localização como uma propriedade de aplicação 
Pode criar uma propriedade de localização como uma propriedade de aplicação com o Azure Maps na sua aplicação IoT Central. Por exemplo, pode adicionar o endereço de instalação de dispositivo. 

1. Sobre o **propriedades** separador, selecione **Editar modelo**.

   ![Guia "Propriedades" com o modo de design ativado](./media/howto-set-up-template/locationcloudproperty1.png)

2. Na biblioteca, selecione **localização**.
3. Configurar **nome a apresentar**, **nome do campo**e (opcionalmente) **valor inicial** para a localização. 

   ![Formulário de "Configurar a localização" com detalhes para a localização](./media/howto-set-up-template/locationcloudproperty2.png)

   Existem dois formatos com suporte para adicionar uma localização:
   - **Localização como um endereço**
   - **Localização como coordenadas** 

4. Selecione **salvar** e **feito**. Agora, um operador pode atualizar o valor de localização sob a forma de campo de localização. 

#### <a name="add-location-as-a-device-property"></a>Adicionar localização como uma propriedade do dispositivo 

Pode criar uma propriedade de localização como uma propriedade de dispositivo que o dispositivo comunica. Por exemplo, se quiser controlar a localização do dispositivo:

1. Sobre o **propriedades** separador, selecione **Editar modelo**.

   ![Guia "Propriedades" com o modo de design ativado](./media/howto-set-up-template/locationdeviceproperty1.png)

2. Selecione **propriedade do dispositivo** da biblioteca.
3. Configurar o nome a apresentar e o nome de campo e selecione **localização** como o tipo de dados. 

   > [!NOTE]
   > O nome do campo tem de corresponder exatamente ao nome da propriedade que o dispositivo comunica. 

   ![Formulário de "Configurar propriedades do dispositivo" com detalhes para a localização](./media/howto-set-up-template/locationdeviceproperty2.png)

Agora que configurou a sua propriedade de localização, pode [adicionar um mapa para visualizar a localização no dashboard do dispositivo](#add-an-azure-maps-location-in-the-dashboard).

## <a name="commands"></a>Comandos

Comandos são utilizados para gerir remotamente um dispositivo. Permitem que os operadores da sua aplicação para executar os comandos instantaneamente no dispositivo. Pode adicionar vários comandos ao seu modelo de dispositivo que são apresentados como mosaicos no **comandos** separador para operadores para utilizar. Como o construtor do dispositivo, tem a flexibilidade de definir comandos, de acordo com os seus requisitos.

Como é que um comando diferente a partir de uma definição? 

* **Definição**: uma definição é uma configuração que pretende aplicar a um dispositivo e quiser que o dispositivo para manter essa configuração até que alterá-lo. Por exemplo, se pretender definir a temperatura de sua freezer e pretende que a configuração, mesmo quando o freezer reinicia. 

* **Comando**: utilizar comandos instantaneamente executar um comando no dispositivo remotamente a partir de IoT Central. Se um dispositivo não está ligado, o comando exceder o tempo limite e falha. Por exemplo, que pretende reiniciar um dispositivo.  

Quando executa um comando, pode ser de uma de três Estados, dependendo se o dispositivo recebeu o comando.

Por exemplo, pode adicionar um novo **eco** comando selecionando **Editar modelo**, em seguida, clicar em **+ novo comando**e inserindo o comando novo:

![Formulário de "Configurar o comando" com detalhes para eco](./media/howto-set-up-template/commandsecho.png)

Depois de selecionar **salvar** e **feito**, o **eco** comando aparece como um mosaico e está pronto para ser usado para retornar o dispositivo.

Depois de criar um mosaico, pode tentar o comando de novo.

## <a name="rules"></a>Regras

Regras de permitem aos operadores monitorizar dispositivos em tempo real. Regras automaticamente invocam ações como enviar um e-mail quando a regra é acionada. Um tipo de regra está disponível hoje:

- **Regra de telemetria**, que é acionado quando a telemetria do dispositivo selecionado ultrapassar um limiar especificado. [Saiba mais sobre regras de telemetria](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

O dashboard é onde um operador pode ir para ver informações sobre um dispositivo. Como um construtor, pode adicionar mosaicos nesta página para o ajudar a compreender como o dispositivo está se comportando de operadores. Pode adicionar vários mosaicos do dashboard ao modelo do dispositivo. Pode adicionar seis tipos de mosaicos do dashboard: imagem, gráfico, gráfico de barras, KPI, definições e propriedades de linha e da etiqueta.

Por exemplo, pode adicionar um **definições e propriedades** mosaico para mostrar uma seleção dos valores atuais das definições e propriedades selecionando **Editar modelo** e o mosaico da biblioteca:

![Formulário de "Configurar os detalhes do dispositivo" com detalhes para as definições e propriedades](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Agora quando um operador visualiza o dashboard, pode ver este mosaico que apresenta as propriedades e configurações do dispositivo:

![Separador "Dashboard" com as definições apresentadas e propriedades para o mosaico](./media/howto-set-up-template/dashboardtile.png)

### <a name="add-an-azure-maps-location-in-the-dashboard"></a>Adicionar uma localização do Azure Maps no dashboard

Se tiver configurado uma propriedade de localização anteriormente em [criar uma propriedade de localização por meio do Azure Maps](#create-a-location-property-through-azure-maps), é possível visualizar a localização, utilizando um mapa no dashboard do dispositivo.

1. Sobre o **Dashboard** separador, selecione **Editar modelo**.

   ![Separador "Dashboard" com o modo de design ativado](./media/howto-set-up-template/locationcloudproperty4map.png)

2. No dashboard do dispositivo, selecione **mapa** da biblioteca. 
3. Forneça um título e escolha a propriedade de localização que configurou anteriormente como parte das suas propriedades do dispositivo.

   ![Formulário de "Configurar o mapeamento de" com detalhes para o título e propriedades](./media/howto-set-up-template/locationcloudproperty5map.png)

4. Selecione **Guardar**. O mosaico do mapa apresenta agora a localização que selecionou. 

   ![Mosaico do mapa com a localização selecionada](./media/howto-set-up-template/locationcloudproperty6map.png) 

Pode redimensionar o mapa para seu tamanho desejado. Agora quando um operador visualiza o dashboard, pode ver todos os mosaicos de dashboard que foram configurados, incluindo um mapa de localização.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como configurar um modelo de dispositivo na sua aplicação do Azure IoT Central, pode:

> [!div class="nextstepaction"]
> [Criar uma nova versão de modelo do dispositivo](howto-version-devicetemplate.md)
