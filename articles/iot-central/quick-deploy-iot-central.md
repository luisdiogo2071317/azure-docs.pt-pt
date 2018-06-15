---
title: Implementar uma aplicação do Azure IoT Central | Microsoft Docs
description: Crie uma nova aplicação do Azure IoT Central para gerir dispositivos de venda refrigerados. Veja os dados telemétricos gerados a partir dos seus dispositivos simulados.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/15/2018
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 9a9b9e66995a2945ef95056f2fcbcd59814d08ff
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34630498"
---
# <a name="create-an-azure-iot-central-application"></a>Criar uma aplicação do Azure IoT Central

Como _construtor_, utilize a IU do Azure IoT Central para definir a sua aplicação do Microsoft Azure IoT Central. Este manual de início rápido mostra-lhe como:

- Crie uma aplicação do Azure IoT Central que contenha um exemplo do _modelo de dispositivo_ e _dispositivos_ simulados.
- Veja as funcionalidades do modelo de dispositivo da **Máquina de Venda Refrigerada** na sua aplicação.
- Veja a telemetria e a análise a partir dos seus dispositivos do **Refrigerador**.

Neste início rápido, verá um dispositivo do **Refrigerador** simulado a partir de um modelo de dispositivo. O dispositivo simulado:

* Envia telemetria, como a temperatura e a pressão, para a sua aplicação.
* Comunica os valores de propriedade do dispositivo, como um alerta de movimento para a aplicação.
* Tem definições de dispositivo, como a velocidade da ventoinha, que pode definir na aplicação.

Quando cria um dispositivo simulado a partir de um modelo de dispositivo numa aplicação do Azure IoT Central, o dispositivo simulado permite-lhe testar a aplicação antes de ligar a um dispositivo real.

## <a name="create-the-application"></a>Criar a aplicação

Para concluir este início rápido, tem de criar uma aplicação do Azure IoT Central a partir do modelo da aplicação de **Exemplo do Contoso**.

Navegue para a página do [Gestor de Aplicações](https://aka.ms/iotcentral) do Azure IoT Central. Em seguida, introduza o endereço de e-mail e a palavra-passe que utiliza para aceder à sua subscrição do Azure:

![Introduza a sua conta da organização](media/quick-deploy-iot-central/sign-in.png)

Para começar a criar uma nova aplicação do Azure IoT Central, escolha **Nova Aplicação**:

![Página do Gestor de Aplicações do Azure IoT Central](media/quick-deploy-iot-central/iotcentralhome.png)

Para criar uma nova aplicação do Azure IoT Central:

1. Escolha o plano de pagamento **Aplicação de Avaliação Gratuita**.
1. Escolha um nome simpático para a aplicação, como **Contoso IoT**. O Azure IoT Central gera um prefixo de URL exclusivo para si. Pode alterar este prefixo de URL para algo mais memorável.
1. Escolha o modelo da aplicação de **Exemplo do Contoso** .
1. Em seguida, escolha **Criar**.

![Página da Aplicação do Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate.png)

## <a name="navigate-to-the-application"></a>Navegue para a aplicação

Quando a aplicação estiver pronta, a **Homepage** da sua aplicação é apresentada. O _Modo de Design_ no canto superior direito pode ser ativado para editar a Homepage. O URL da aplicação é o URL que especificou no passo anterior:

![Página Application Builder](media/quick-deploy-iot-central/apphome.png)

Utilize o _menu de navegação à esquerda_ para aceder às diferentes áreas da sua nova aplicação do Azure IoT Central:

![Menu de navegação à esquerda](media/quick-deploy-iot-central/navbar.png)

Para ver os modelos de dispositivos e os dispositivos na sua aplicação, escolha **Explorador de Dispositivos** no menu de navegação à esquerda. O exemplo de aplicação inclui o modelo de dispositivo da **Máquina de Venda Refrigerada**. Já existem três dispositivos simulados criados através deste modelo de dispositivo:

![Explorador de dispositivos](media/quick-deploy-iot-central/deviceexplorer.png)

## <a name="view-the-device-template-and-devices"></a>Ver o modelo de dispositivo e dispositivos

Utilize os seguintes passos para ver um dispositivo de refrigerador que foi criado a partir do modelo de dispositivo da **Máquina de Venda Refrigerada**. Um modelo de dispositivo define:

* _Medidas_, como a telemetria de temperatura, enviada a partir de um dispositivo.
* _Definições_, como a velocidade da ventoinha, que lhe permite controlar o dispositivo.
* _Propriedades_, como o número de série, que armazenam informações sobre o dispositivo.
* [Regras](howto-create-telemetry-rules.md) que lhe permitem automatizar ações com base no comportamento do dispositivo.
* Um _dashboard_ personalizável que apresenta informações sobre o dispositivo.

Pode criar os dispositivos simulados e reais a partir de um modelo de dispositivo.

### <a name="measurements"></a>Medições

A página **Medidas** para o dispositivo do **Refrigerador 1** é apresentada. Pode ver a lista de medidas enviadas do dispositivo simulado. A página também apresenta um gráfico personalizável das medidas visíveis:

![Página de Medições](media/quick-deploy-iot-central/measurements.png)

Pode alternar a visibilidade dos elementos individuais e personalizar o gráfico. O gráfico atual mostra a telemetria de um dispositivo simulado. Pode adicionar novas medidas ao modelo de dispositivo se tiver as permissões adequadas.

> [!NOTE]
> Poderá ter de aguardar durante um curto período antes dos dados simulados serem apresentados no gráfico.

### <a name="settings"></a>Definições

Escolha **Definições**. Na página **Definições**, pode controlar o dispositivo. Por exemplo, pode atualizar a velocidade da ventoinha no refrigerador:

![Definições](media/quick-deploy-iot-central/settings.png)

É apresentada uma definição como **sincronizada** quando um dispositivo reconhece a alteração.

### <a name="properties"></a>Propriedades

Escolha **Propriedades**. Na página **Propriedades**, pode:

* Manter informações sobre o seu dispositivo, como o nome de cliente.
* Ver os valores de propriedade comunicados pelo dispositivo, por exemplo, como um alerta de movimento.

![Propriedades](media/quick-deploy-iot-central/properties.png)

### <a name="dashboard"></a>Dashboard

Escolha **Dashboard**. O dashboard é uma vista personalizável de informações sobre o seu dispositivo, como medidas, KPIs e propriedades:

![Dashboard](media/quick-deploy-iot-central/dashboard.png)

## <a name="view-analytics"></a>Ver análise

A secção anterior mostrou como ver informações sobre um dispositivo individual. Pode utilizar [conjuntos de dispositivos](howto-use-device-sets.md) e [análise](howto-create-analytics.md) para ver informações consolidadas a partir de vários dispositivos.

Um conjunto de dispositivos utiliza uma consulta para selecionar dinamicamente um conjunto de dispositivos que correspondem aos critérios. Por exemplo, o conjunto de dispositivos **Máquinas em Seattle** seleciona dispositivos do refrigerador cuja localização é Seattle. Para ver o conjunto de dispositivos **Máquinas em Seattle**, escolha **Conjuntos de Dispositivos** no menu de navegação à esquerda e, em seguida, escolha **Máquinas em Seattle**:

![Conjunto de dispositivos Máquinas em Seattle](media/quick-deploy-iot-central/deviceset.png)

Pode ver os dados de análise para os dispositivos num conjunto de dispositivos na página **Análise**:

![Análise das máquinas em Seattle](media/quick-deploy-iot-central/analytics.png)

## <a name="next-steps"></a>Passos seguintes

Neste início rápido criou uma aplicação do Azure IoT Central pré-preenchida que contém um modelo de dispositivo e dispositivos simulados da **Máquina de Venda Refrigerada**. Veja [Definir um novo modelo de dispositivo na sua aplicação](tutorial-define-device-type.md) para saber mais, enquanto construtor, sobre como definir os seus próprios modelos de dispositivos.
