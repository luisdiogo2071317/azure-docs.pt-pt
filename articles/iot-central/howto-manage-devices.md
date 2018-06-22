---
title: Gerir os dispositivos na sua aplicação do Azure IoT Central | Microsoft Docs
description: Como um operador, saiba como gerir dispositivos na sua aplicação do Azure IoT Central.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: b0772e3186c86239c773222a2b2e8d602a46aa52
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300599"
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Gerir dispositivos na sua aplicação do Azure IoT Central

Este artigo descreve como, como um operador, para gerir dispositivos na sua aplicação do Microsoft Azure IoT Central. Como um operador, pode:

- Utilize o **Explorer** página para ver, adicionar e eliminar os dispositivos ligados à sua aplicação do Azure IoT Central.
- Manter um inventário dos seus dispositivos atualizado.
- Manter os seus metadados do dispositivo atualizados alterando os valores armazenados nas propriedades do dispositivo.
- Controlar o comportamento dos seus dispositivos através da atualização de uma definição de um dispositivo específico do **definições** página.

## <a name="view-your-devices"></a>Ver os seus dispositivos

Para ver um dispositivo individual:

1. Escolha **Explorer** no menu de navegação esquerdo. Aqui pode ver uma lista das suas [modelos de dispositivo](howto-set-up-template.md).

1. Escolha um **modelo de dispositivo** no painel da esquerda.

1. No painel da direita, verá uma lista de dispositivos criada a partir desse modelo do dispositivo. Escolha um dispositivo individual para ver o **detalhes do dispositivo** página desse dispositivo:

    [![Página de detalhes do dispositivo](./media/howto-manage-devices/image1.png)](./media/howto-manage-devices/image1.png#lightbox)

## <a name="add-a-device"></a>Adicionar um dispositivo

Para adicionar um dispositivo à sua aplicação do Azure IoT Central:

1. Escolha **Explorer** no menu de navegação esquerdo.

1. Escolha o modelo de dispositivo a partir do qual pretende criar um dispositivo.

1. Escolha + **novo**.

1. Escolha **Real** ou **Simulated**. Um dispositivo real é para um dispositivo físico se ligar à sua aplicação do Azure IoT Central. Um dispositivo simulado tem dados de exemplo gerados pelo Azure IoT Central. Este exemplo utiliza um dispositivo real. Escolha **Real** para navegar para o **detalhes do dispositivo** página para o novo dispositivo.


## <a name="import-devices"></a>Importar dispositivos

Para ligar o elevado número de dispositivos à sua aplicação do Azure IoT Central ofertas em massa importação dispositivos através de um ficheiro CSV. 

Requisitos do ficheiro CSV:
1. O ficheiro CSV deve ter apenas uma coluna que contém os IDs do dispositivo.

1. Ficheiro não deve ter qualquer cabeçalho.


Para registar de em massa dispositivos na sua aplicação:

1. Escolha **Explorer** no menu de navegação esquerdo.

1. No painel esquerdo, selecione o modelo de dispositivo para o qual pretende em massa criar os dispositivos.

 >   [!NOTE] 
    Se não tiver um modelo de dispositivo, mas, em seguida, pode importar dispositivos em **não associadas dispositivos** e registe-as sem qualquer modelo. Depois de terem sido importados dispositivos, pode, em seguida, associá-los com um modelo como um passo subsequente.

1. Clique em **importar**.

    [![Ação de importação](./media/howto-manage-devices/BulkImport1.png)](./media/howto-manage-devices/BulkImport1.png#lightbox)

1. Selecione o ficheiro CSV que tem a lista de IDs de dispositivo para ser importado.

1. Importação de dispositivo é iniciado depois do ficheiro foi carregado. Pode controlar o estado de importação no topo da grelha de dispositivo.

1. Assim que a importação estiver concluída, é apresentada uma mensagem de êxito na grelha de dispositivo.

    [![Importar com êxito](./media/howto-manage-devices/BulkImport3.png)](./media/howto-manage-devices/BulkImport3.png#lightbox)

Se o dispositivo importar falha da operação, verá uma mensagem de erro na grelha de dispositivo. Um ficheiro de registo capturar todos os erros for gerado, podendo ser transferido clicando a mensagem de erro.


**A associação de dispositivos com um modelo**

Se registar dispositivos ao iniciar a importação em **não associadas dispositivos**, em seguida, os dispositivos são criados sem qualquer associação de modelo do dispositivo. Dispositivo tem de estar associado a um modelo para explorar os dados e outros detalhes sobre o dispositivo. Siga estes passos para associar os dispositivos com um modelo:
1. Escolha **Explorer** no menu de navegação esquerdo.
1. No painel esquerdo, escolha **não associadas dispositivos**.
    [![Dispositivos não associados](./media/howto-manage-devices/UnassociatedDevices1.png)](./media/howto-manage-devices/UnassociatedDevices1.png#lightbox)
1. Selecione os dispositivos que pretende associar um modelo.
1. Clique em **associar** opção.
    [![Associar dispositivos](./media/howto-manage-devices/UnassociatedDevices2.png)](./media/howto-manage-devices/UnassociatedDevices2.png#lightbox)
1. Escolha o modelo da lista de modelos disponíveis e clique em **associar** botão.
1. Os dispositivos selecionados serão movidos sob o modelo do respetivo dispositivo.

 >   [!NOTE] 
    Depois de um dispositivo foi associado a um modelo não é possível alterar ou associado a outro modelo.

## <a name="export-devices"></a>Dispositivos de exportação

Para aprovisionar dispositivos liguem à IoT Central, terá da cadeia de ligação do dispositivo que é gerado pelo centro de IoT. Pode utilizar a funcionalidade de exportação para obter as cadeias de ligação e outras propriedades dos dispositivos em massa a partir da sua aplicação. Exportar cria um ficheiro CSV com a identidade de dispositivo, o nome do dispositivo e a cadeia de ligação principal para todos os dispositivos selecionados.

Para efetuar em massa de dispositivos de exportação da sua aplicação:
1. Escolha **Explorer** no menu de navegação esquerdo.

1. No painel esquerdo, escolha o modelo de dispositivo para o qual pretende exportar os dispositivos.

1. Selecione os dispositivos que pretende exportar e, em seguida, clique em de **exportar** ação.

    [![Exportar](./media/howto-manage-devices/Export1.png)](./media/howto-manage-devices/Export1.png#lightbox)

1. Processo de exportação será iniciado e pode controlar o estado na parte superior da grelha. 

1. Uma vez concluída a exportação, é apresentada uma mensagem de êxito, juntamente com uma hiperligação para transferir o ficheiro gerado.

1. Clique em de **mensagem de êxito** para transferir o ficheiro para uma pasta no disco local.

    [![Sucesso de exportação](./media/howto-manage-devices/Export2.png)](./media/howto-manage-devices/Export2.png#lightbox)

1. O ficheiro CSV exportado terá as seguintes informações:
    1. Nome
    1. ID do dispositivo
    1. Cadeia de ligação principal


## <a name="delete-a-device"></a>Eliminar um dispositivo

Para eliminar a um dispositivo simulado ou real da sua aplicação do Azure IoT Central:

1. Escolha **Explorer** no menu de navegação.

1. Escolha o modelo de dispositivo do dispositivo que pretende eliminar.

1. Selecione a caixa junto ao dispositivo para eliminar.

1. Escolha **eliminar**.

## <a name="change-a-device-setting"></a>Alterar uma definição de dispositivo

Definições controlam o comportamento de um dispositivo. Por outras palavras, elas permitem forneça entradas para o seu dispositivo. Pode ver e atualizar as definições do dispositivo no **detalhes do dispositivo** página.

1. Escolha **Explorer** no menu de navegação.

1. Escolha o modelo de dispositivo do dispositivo cujas definições pretende alterar.

1. Escolha o **definições** separador. Aqui pode ver todas as definições de que tem o seu dispositivo e os respetivos valores atuais. Para cada definição, pode ver se o dispositivo ainda está a sincronizar.

1. Modificar as definições para os valores pretendidos. Pode modificar várias definições de uma só vez e atualizá-las todas ao mesmo tempo.

1. Escolha **atualização**. Os valores são enviados para o seu dispositivo. Quando o dispositivo reconhece a alteração da definição, o estado da definição anterior entra em **sincronizado**.

## <a name="change-a-property"></a>Alterar uma propriedade

As propriedades são os metadados associados ao dispositivo, por exemplo, cidade e número de série do dispositivo. Pode ver e atualizar as propriedades no **detalhes do dispositivo** página.

1. Escolha **Explorer** no menu de navegação.

1. Escolha o modelo de dispositivo do dispositivo cujas propriedades pretende alterar.

1. Escolha o **propriedades** separador, onde pode ver todas as propriedades.

1. Modificar as propriedades para os valores pretendidos. Pode modificar várias propriedades de uma só vez e atualizá-las todas ao mesmo tempo.

1. Escolha **atualização**.

> [!NOTE]
> Não é possível alterar o valor de _propriedades do dispositivo_. Propriedades do dispositivo são definidas pelo dispositivo e são só de leitura na aplicação do Azure IoT Central.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como gerir dispositivos na sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Como utilizar os conjuntos de dispositivo](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->
