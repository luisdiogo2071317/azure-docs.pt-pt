---
title: Criar e executar tarefas na sua aplicação do Azure IoT Central | Documentos da Microsoft
description: Tarefas do Azure IoT Central permitem funcionalidades de gestão de dispositivos em massa, como atualizar uma propriedade do dispositivo, definição ou executar um comando.
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 09/15/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: peterpr
ms.openlocfilehash: ce003eba07950a56fe178a19655a9059416b69b7
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46468782"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Criar e executar uma tarefa no seu aplicativo de Central de IoT do Azure

Pode utilizar o Microsoft Azure IoT Central para gerir os seus dispositivos ligados à escala da utilização de tarefas. A funcionalidade de tarefas permite-lhe executar atualizações em massa para propriedades do dispositivo, as definições e comandos. Este artigo descreve como começar a utilizar as tarefas em seu próprio aplicativo.

## <a name="create-and-run-a-job"></a>Criar e executar uma tarefa

Esta secção mostra-lhe como criar e executar uma tarefa. Cada passo passa por um exemplo que demonstra como executar uma tarefa para os dispositivos de refrigerated de venda automática de máquina que tem de ter a velocidade de ventoinha aumentada.

1. Navegue até às tarefas no painel de navegação.

1. Clique em **+ novo** para começar a criar uma nova tarefa.

    ![Criar nova tarefa](./media/howto-run-a-job/createnewjob.png)

1. Introduza um nome e descrição que o ajudam a identificar a tarefa está a criar.

1. Selecione o conjunto de dispositivo que pretende que o seu trabalho a ser aplicado. Depois de selecionar o dispositivo definida, verá no lado direito, preencha com os dispositivos dentro do conjunto de dispositivo selecionado. Se selecionar um conjunto de dispositivo quebrado, não existem dispositivos irão apresentar e verá uma mensagem que explica que o seu conjunto de dispositivos é quebrado.

1. Em seguida, selecione o tipo de tarefa que vai ser definido (uma definição, propriedade ou comando). Clique em **+** ao lado do tipo de tarefa selecionada e adicionar as suas operações pretendidas.

    ![Configurar tarefa](./media/howto-run-a-job/configurejob.png)

1. No lado direito, escolha os dispositivos que pretende executar a tarefa. Ao clicar na caixa de verificação principal, todos os dispositivos são selecionados no conjunto de dispositivo inteiro. Ao clicar na caixa de verificação junto ao nome, são selecionados todos os dispositivos na página atual.

1. Depois dos dispositivos pretendidos tenham sido selecionados, selecione **executar**. A tarefa aparecerão agora no principal **tarefas** página. Nesta vista, é capaz de ver o seu trabalho em execução e o histórico de qualquer anteriormente executar tarefas. Seu trabalho em execução serão sempre apresentados na parte superior da lista.

    ![Executar tarefa](./media/howto-run-a-job/runjob.png)

    ![Ver Tarefa](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > Poderá ver o histórico das suas tarefas de execução anterior até 30 dias.

1. Para obter uma descrição geral do seu trabalho, clique no nome da tarefa que quer ver da sua lista. Esta descrição geral contém os detalhes da tarefa, dispositivos e os Estados do dispositivo.

    ![Ver o estado do dispositivo](./media/howto-run-a-job/viewdevicestatus.png)

### <a name="stop-a-running-job"></a>Parar uma tarefa em execução

Se desejar parar uma tarefa que está atualmente em execução, clique no nome da tarefa em execução que pretende parar. Escolha o **parar** botão no painel. Verá que o estado da tarefa foi alterado para refletir que a tarefa foi parada.

> [!NOTE]
> Depois de uma tarefa foi parada. não é possível reiniciar. Tem de criar outra tarefa com o operations pretendidos e dispositivos.

## <a name="view-the-job-status"></a>Ver o estado da tarefa

Quando uma tarefa tiver sido criada, o **estado** coluna será atualizada com a mensagem de estado mais recente da tarefa. As mensagens de estado significam o seguinte:

| Mensagem de estado       | Significado de estado                                          |
| -------------------- | ------------------------------------------------------- |
| Concluído            | Esta tarefa foi executada em todos os dispositivos.              |
| Com Falhas               | Esta tarefa tem falha e não totalmente executado nos dispositivos.  |
| Pendente              | Esta tarefa ainda não tenha iniciado em execução em dispositivos.        |
| A executar              | Esta tarefa está atualmente em execução em dispositivos.             |
| Parada              | Esta tarefa foi parada manualmente por um utilizador.           |

A mensagem de estado é seguida por uma visão geral dos dispositivos dentro da tarefa. Estes Estados do dispositivo significam o seguinte:

| Mensagem de estado       | Significado de estado                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Bem-sucedido            | O número de dispositivos que a tarefa foi executada com êxito no.  |
| Com Falhas               | O número de dispositivos que a tarefa falhou a ser executadas a.      |

### <a name="view-the-device-status"></a>Ver o estado do dispositivo

Para ver o estado de cada dispositivo no trabalho, clique no nome da tarefa. Aqui verá os detalhes do trabalho e todos os dispositivos que foram uma parte da tarefa específica. Junto a cada nome de dispositivo, verá uma das seguintes mensagens de estado:

| Mensagem de estado       | Significado de estado                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Concluído            | A tarefa foi executada neste dispositivo.                                     |
| Com Falhas               | A tarefa não conseguiu executar neste dispositivo. A mensagem de erro que acompanha este artigo irá mostrar mais informações.  |
| Pendente              | A tarefa ainda não executada neste dispositivo.                                  |

> [!NOTE]
> Se um dispositivo tiver sido eliminado, não será possível selecionar o dispositivo e esta será apresentada como excluído com o ID de dispositivo.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu a criar tarefas na sua aplicação do Azure IoT Central, seguem-se alguns passos seguintes:

- [Utilizar conjuntos de dispositivos](howto-use-device-sets.md)
- [Gerir os seus dispositivos](howto-manage-devices.md)
- [Versão do seu modelo de dispositivo](howto-version-devicetemplate.md)
