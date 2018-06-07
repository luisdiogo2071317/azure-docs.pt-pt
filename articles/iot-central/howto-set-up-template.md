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
ms.openlocfilehash: c88b27edab71527b4b3eca71c00af2930c22f8cd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629240"
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
Apenas construtores e os administradores podem criar, editar e eliminar modelos de dispositivos. Qualquer utilizador pode criar dispositivos de **Explorador de dispositivo** página a partir de modelos de dispositivos existentes.

## <a name="create-a-new-device-template"></a>Criar um novo modelo de dispositivo

1. Navegue para o **aplicação Builder** página.

1. Para criar um modelo em branco, escolha **criar modelo de dispositivo**e, em seguida, escolha **personalizada**.

1. Introduza um nome para o novo modelo de dispositivo e escolher **criar**.

    ![Página de detalhes do dispositivo](./media/howto-set-up-template/devicedetailspage.png)

1. Agora está no **detalhes do dispositivo** página de um novo dispositivo simulado. Um dispositivo simulado é criado automaticamente quando criar um novo modelo de dispositivo. Este relatórios de dados e pode ser controlado, tal como um dispositivo real.

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

> [!NOTE]
> Depois de criar um novo mosaico, pode experimentar a nova definição. Desactivar em primeiro lugar, o modo de estrutura na parte superior direita do ecrã:

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

## <a name="rules"></a>Regras

Regras de ativar os operadores monitorizar dispositivos em tempo real. Regras automaticamente invocam **ações** como enviar um e-mail quando a regra é acionado. Há um tipo de regra disponível atualmente:

- **Regra de telemetria:** uma regra de telemetria é acionado quando a telemetria do dispositivo selecionado atravesse um limiar especificado. Saiba mais sobre [regras de telemetria](howto-create-telemetry-rules.md).

## <a name="dashboard"></a>Dashboard

O dashboard é onde um operador pode ir para ver informações sobre um dispositivo. Como um construtor, pode adicionar mosaicos para esta página que o ajudam a compreender a forma como o dispositivo está a comportar os operadores. Pode adicionar vários mosaicos do dashboard ao seu modelo do dispositivo. Existem seis tipos de mosaicos do dashboard pode adicionar: a imagem, gráfico, gráfico de barras, KPI, definições e propriedades de linha e etiqueta.

Por exemplo, pode adicionar um **definições e propriedades** mosaico para mostrar uma seleção dos valores atuais das propriedades e definições:

![Formulário de detalhes de dispositivos do dashboard](./media/howto-set-up-template/dashboardsettingsandpropertiesform.png)

Agora quando um operador vistas do dashboard, pode ver este mosaico que mostra as propriedades e definições do dispositivo:

![Mosaico do dashboard](./media/howto-set-up-template/dashboardtile.png)

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu a configurar um modelo de dispositivo na sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Criar uma nova versão de modelo do dispositivo](howto-version-devicetemplate.md)
