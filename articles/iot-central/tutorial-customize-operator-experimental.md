---
title: Personalizar as vistas do operador no Azure IoT Central | Microsoft Docs
description: EnqPersonalize as vistas do operador na sua aplicação do Azure IoT Central como construtor.
author: sandeeppujar
ms.author: sandeepu
ms.date: 01/29/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 8636ac26ebf008ef0eb15713f0736650aa00af81
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666405"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view-new-ui-design"></a>Tutorial: Personalizar a vista do operador do Azure IoT Central (design de nova interface do Usuário)

Este tutorial mostra-lhe como personalizar a vista do operador da sua aplicação como construtor. Quando fizer uma alteração à aplicação como construtor, pode pré-visualizar a vista do operador na aplicação do Microsoft Azure IoT Central.

Neste tutorial, irá personalizar a aplicação para apresentar informações relevantes sobre o dispositivo de ar condicionado ligado a um operador. As personalizações permitem ao operador gerir os dispositivos de ar condicionado ligados à aplicação.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar o dashboard do seu dispositivo
> * Configurar o seu esquema de definições do dispositivo
> * Configurar o seu esquema de propriedades do dispositivo
> * Pré-visualizar o dispositivo como operador
> * Configurar a sua home page predefinida
> * Pré-visualizar a home page predefinida como operador

[!INCLUDE [iot-central-experimental-note](../../includes/iot-central-experimental-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Antes de iniciar sessão, deve concluir os dois tutoriais anteriores:

* [Definir um novo tipo de dispositivo na aplicação do Azure IoT Central](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).
* [Configurar regras e ações para o seu dispositivo](tutorial-configure-rules-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="configure-your-device-dashboard"></a>Configurar o dashboard do seu dispositivo

Pode definir as informações que são apresentadas num dashboard do dispositivo como construtor. Na [definir um novo tipo de dispositivo em seu aplicativo](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) tutorial, adicionou um gráfico de linhas e outras informações para o **ligado ar-condicionado** dashboard.

1. Para editar a **ligado ar-condicionado** modelo de dispositivo, escolha **modelos de dispositivos** no menu de navegação à esquerda:

    ![Página de modelos de dispositivo](media/tutorial-customize-operator-experimental/devicetemplates.png)

2. Para personalizar o dashboard do dispositivo, clique nas **ligado ar-condicionado (1.0.0)** dispositivos do modelo de dispositivo que criou no [definir um novo tipo de dispositivo na sua aplicação](tutorial-define-device-type-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json) tutorial.

3. Para editar o dashboard, selecione o **Dashboard** separador.

4. Para adicionar um mosaico do Indicador Chave de Desempenho (KPI) ao dashboard, escolha **KPI**:

    Para definir o KPI, utilize as informações na tabela seguinte:

    | Definição     | Valor |
    | ----------- | ----- |
    | Name        | Temperatura máxima |
    | Intervalo de tempo  | Passado 1 semana |
    | Tipo de Medida | Telemetria |
    | Medida | temperatura |
    | Agregação | Máximo |
    | Visibilidade  | Ativado |

    ![Adicionar KPI](media/tutorial-customize-operator-experimental/addkpi.png)

5. Clique em **Guardar**. Agora pode ver o mosaico de KPI no dashboard:

    ![Mosaico de KPI](media/tutorial-customize-operator-experimental/temperaturekpi.png)

6. Para mover ou redimensionar um mosaico no dashboard, mova o ponteiro do rato sobre o mosaico. Pode arrastar o mosaico para uma localização nova ou redimensioná-la.

## <a name="configure-your-settings-layout"></a>Configurar o seu esquema de definições

Também pode configurar a vista do operador das definições do dispositivo como construtor. Um operador utiliza o separador de definições do dispositivo para configurar um dispositivo. Por exemplo, um operador utiliza o separador de definições para definir a temperatura de destino para o ar condicionado ligado.

1. Para editar o esquema de definições para sua ligado de ar-condicionado, escolha o **definições** separador.

2. Pode mover e redimensionar os mosaicos de definições:

    ![Editar o esquema de definições](media/tutorial-customize-operator-experimental/settingslayout.png)

## <a name="configure-your-properties-layout"></a>Configurar o seu esquema de propriedades

Para além do dashboard e das definições, também pode configurar a vista do operador das propriedades do dispositivo. Um operador utiliza o separador de propriedades do dispositivo para gerir metadados do dispositivo. Por exemplo, um operador utiliza o separador de propriedades para ver um número de série do dispositivo ou atualizar detalhes de contacto para o fabricante.

1. Para editar o layout de propriedades para sua ligado de ar-condicionado, escolha o **propriedades** separador.

2. Pode mover e redimensionar os campos de propriedades:

    ![Editar o esquema de propriedades](media/tutorial-customize-operator-experimental/propertieslayout.png)

## <a name="preview-the-connected-air-conditioner-device-as-an-operator"></a>Pré-visualizar o dispositivo de ar condicionado ligado como um operador

Utilizar o **modelos de dispositivos** página para personalizar os separadores dashboard, definições e propriedades para um operador. Utilizar o **Device Explorer** página para ver e utilizar o modelo de dispositivo.

1. Para ver e utilizar o modelo conectado ar-condicionado como um operador, navegue para o **Device Explorer** página e selecione o dispositivo simulado que IoT Central gerado a partir do seu modelo:

    ![Ver e utilizar o modelo de dispositivo](media/tutorial-customize-operator-experimental/usetemplate.png)

2. Para atualizar a localização deste dispositivo, escolha **propriedades** e edite o valor no mosaico de localização. Em seguida, clique em **guardar**:

    ![Editar um valor de propriedade](media/tutorial-customize-operator-experimental/editproperty.png)

3. Para enviar uma definição para o seu ar condicionado ligado, escolha **Definições**, altere um valor de definição num mosaico e escolha **Atualizar**:

    ![Enviar definição para dispositivo](media/tutorial-customize-operator-experimental/sendsetting.png)

    Quando o dispositivo reconhece o novo valor de definição, a definição é apresentada como **sincronizada** no mosaico.

4. Enquanto operador, pode ver o dashboard do dispositivo, conforme configurado pelo construtor:

    ![Vista do operador do dashboard do dispositivo](media/tutorial-customize-operator-experimental/operatordashboard.png)

## <a name="configure-the-default-home-page"></a>Configurar a sua home page predefinida

Quando um construtor ou operador inicia sessão numa aplicação do Azure IoT Central, verão uma home page. Enquanto construtor, pode configurar o conteúdo desta home page para incluir o conteúdo mais útil e relevante para um operador.

1. Para personalizar a home page padrão, navegue para o **doméstica** página e edite **editar** na parte superior direita da página. Um painel surge do esquerda com uma lista de objetos, pode adicionar para o **home page** página:

    ![Página Application Builder](media/tutorial-customize-operator-experimental/builderhome.png)

2. Para personalizar o **home page** página, adicione mosaicos a partir do **biblioteca**. Escolha **Ligação** e adicione os detalhes do site da sua organização. Em seguida, clique em **guardar**:

    ![Adicionar ligação à home page](media/tutorial-customize-operator-experimental/addlink.png)

    > [!NOTE]
    > Também pode adicionar ligações a páginas na sua aplicação do Azure IoT Central. Por exemplo, pode adicionar uma ligação a um dashboard de dispositivo ou à página de definições.

3. Opcionalmente, escolha **Imagem** e carregue uma imagem a apresentar na sua home page. Uma imagem pode ter um URL para o qual navega ao clicar no mesmo:

    ![Adicionar imagem à home page](media/tutorial-customize-operator-experimental/addimage.png)

    Para obter mais informações, veja [Como preparar e carregar imagens para a aplicação do Azure IoT Central](howto-prepare-images.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

## <a name="preview-the-default-home-page-as-an-operator"></a>Pré-visualizar a home page predefinida como operador

Para pré-visualizar a home page como um operador, clique em **feito** na parte superior direita da página:

![Alternar Modo de Estrutura](media/tutorial-customize-operator-experimental/operatorviewhome.png)

Pode clicar nos mosaicos de ligação e imagem para navegar para os URLs definidos como um construtor.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a personalizar a vista do operador da aplicação.

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Configurar o dashboard do seu dispositivo
> * Configurar o seu esquema de definições do dispositivo
> * Configurar o seu esquema de propriedades do dispositivo
> * Pré-visualizar o dispositivo como operador
> * Configurar a sua home page predefinida
> * Pré-visualizar a home page predefinida como operador

Agora que aprendeu a personalizar a vista do operador da aplicação, os próximos passos sugeridos são:

* [Monitorizar os dispositivos (como administrador)](tutorial-monitor-devices.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
* [Adicionar um novo dispositivo à sua aplicação (como operador e programador de dispositivo)](tutorial-add-device.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
