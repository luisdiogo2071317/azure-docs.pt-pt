---
title: Define o dispositivo de utilização na sua aplicação do Azure IoT Central | Microsoft Docs
description: Como um operador, como utilizar o dispositivo define na sua aplicação do Azure IoT Central.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: d27e687acf62e4a1e96f1bb49607618dff45512b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628509"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>Utilizar conjuntos de dispositivo na sua aplicação do Azure IoT Central

Este artigo descreve como, como um operador, para utilizar o dispositivo conjuntos na sua aplicação do Microsoft Azure IoT Central.

Um conjunto de dispositivos é uma lista de dispositivos que estão agrupados porque corresponderem algumas critérios especificados. Dispositivo define ajuda a gerir, visualizar e analisar dispositivos de escala ao agrupar dispositivos para grupos mais pequenos e lógicos. Por exemplo, pode criar uma lista de todos os dispositivos de ar condicionado para edifícios em Seattle para ativar o técnico de Seattle localizar todos os dispositivos para que ela é responsável. Este artigo mostra como criar e configurar conjuntos de dispositivo.

## <a name="create-a-device-set"></a>Criar um conjunto de dispositivos

Para criar um conjunto de dispositivos:

1. Escolha **conjuntos de dispositivo** no menu de navegação esquerdo.

1. Clique em **+ Novo**.

    ![Novo conjunto de dispositivos](media/howto-use-device-sets/image1.png)

1. Dê um nome que seja exclusivo em toda a aplicação completa do conjunto de dispositivos. Também pode adicionar uma descrição. Um conjunto de dispositivo só pode conter os dispositivos a partir de um modelo de único dispositivo. Escolha o modelo de dispositivo a utilizar para este conjunto.

1. Crie a consulta para identificar os dispositivos para o dispositivo definida ao selecionar uma propriedade, um operador de comparação e um valor. Pode adicionar várias consultas e dispositivos que cumprem **todos os** os critérios são colocados no conjunto de dispositivos. O conjunto de dispositivo que criou está acessível a qualquer pessoa que tenha acesso à aplicação, pelo que qualquer pessoa pode ver, modificar ou eliminar o conjunto de dispositivos.

    ![Consulta de conjunto de dispositivos](media/howto-use-device-sets/image2.png)

    > [!NOTE]
    > O conjunto de dispositivos é uma consulta dinâmica. Sempre que ver a lista de dispositivos, pode haver dispositivos diferentes na lista. A lista depende quais os dispositivos atualmente cumprem os critérios da consulta.

1. Escolha **Guardar**.

## <a name="configure-the-dashboard-for-your-device-set"></a>Configurar o Dashboard para o conjunto de dispositivos

Depois de criar o conjunto de dispositivos, pode configurar o respetivo **Dashboard**. O **Dashboard** é home page, onde pode colocar imagens e ligações. Também pode adicionar grelhas que listam os dispositivos no conjunto de dispositivos.

1. Escolha **conjuntos de dispositivo** no menu de navegação esquerdo.

1. Escolha o **Dashboard** separador.

1. Ativar **modo de desenho**.

    ![Modo de estrutura no](media/howto-use-device-sets/image3.png)

1. Para obter informações sobre como adicionar uma imagem, consulte [preparar e carregar imagens para a aplicação do Azure IoT Central](howto-prepare-images.md).

1. Adicione um mosaico de ligação:
    1. Escolha **ligação** no painel direito.

        ![Escolha a ligação](media/howto-use-device-sets/image6.png)

    1. Permitir a ligação um **título**.
    1. Escolha um URL para seja aberta quando a ligação é clicada.
    1. Forneça a ligação uma descrição que mostra abaixo o **título**.
    1. Escolha **Guardar**.

        ![Guardar a ligação](media/howto-use-device-sets/image7.png)

    1. Pode mover e redimensiona o mosaico de ligação no **Dashboard**.

1. Adicione uma grelha. Uma grelha é uma tabela de dispositivos num dispositivo com as colunas que escolher.
    1. Escolha **grelha** no painel direito.

        ![Escolha a grelha](media/howto-use-device-sets/image8.png)

    1. Atribua a grelha um **título**.
    1. Selecione as colunas a apresentar escolhendo o botão de definições. No painel que aparece, selecione a coluna que pretende apresentar e escolher a seta para a direita para selecioná-lo.
    1. Escolha **OK**.
    1. Escolha **Guardar**.

        ![Guardar grelha](media/howto-use-device-sets/image9.png)

    1. Arrastar e largar grelha para colocá-lo no **Dashboard**.

    > [!NOTE]
    > Pode adicionar várias imagens, ligações e grelhas.

1. Desativar **modo de estrutura**.

    ![Modo de desenho desativado](media/howto-use-device-sets/image10.png)

## <a name="configure-the-list-for-your-device-set"></a>Configurar a lista para o conjunto de dispositivos

Depois de criar o conjunto de dispositivos, pode configurar o **lista**. O **lista** mostra todos os dispositivos no dispositivo definidos numa tabela com as colunas que escolher.

1. Escolha **conjuntos de dispositivo** no menu de navegação esquerdo.

1. Escolha o **lista** separador.

1. Escolha **opções de coluna**.

    ![Opções de coluna](media/howto-use-device-sets/image11.png)

1. Escolha as colunas a apresentar ao selecionar a coluna que pretende mostrar e escolher a seta para a direita para selecioná-lo.

    ![Escolher colunas](media/howto-use-device-sets/image12.png)

1. Escolha **OK**.

## <a name="analytics"></a>Análise

A análise em conjuntos de dispositivo é o mesmo que o separador de análise principal no menu de navegação esquerdo. Pode saber mais sobre a análise no artigo no [como criar análise](howto-create-analytics.md).

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como utilizar os conjuntos de dispositivo na sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Como criar regras de telemetria](howto-create-telemetry-rules.md)
