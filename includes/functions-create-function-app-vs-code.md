---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2018
ms.author: glenga
ms.openlocfilehash: 79dbee33928fbc7560d0ea27be3af25cc510e996
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52638661"
---
## <a name="create-an-azure-functions-project"></a>Criar um projeto das Funções do Azure

O modelo do projeto das Funções do Azure no Visual Studio Code cria um projeto que pode ser publicado numa aplicação de funções no Azure. As aplicações de funções permitem-lhe agrupar funções como unidades lógicas para a gestão, implementação e partilha de recursos.

1. No Visual Studio Code, selecione o logótipo do Azure para apresentar a área **Azure: Funções** e, em seguida, selecione o ícone Criar Novo Projeto.

    ![Criar um projeto de aplicação de função](./media/functions-create-function-app-vs-code/create-function-app-project.png)

1. Escolha uma localização para a área de trabalho do projeto e escolha **Selecionar**.

    > [!NOTE]
    > Este artigo foi criado para ser concluído fora de uma área de trabalho. Neste caso, não selecione uma pasta de projeto que faz parte de uma área de trabalho.

1. Selecione a linguagem para o seu projeto de aplicação de funções. Neste artigo, é utilizado o JavaScript.
    ![Escolher linguagem do projeto](./media/functions-create-function-app-vs-code/create-function-app-project-language.png)

1. Quando lhe for pedido, escolha **Adicionar à área de trabalho**.

O Visual Studio Code cria o projeto da aplicação de funções numa nova área de trabalho. Este projeto contém os ficheiros de configuração [host.json](../articles/azure-functions/functions-host-json.md) e [local.settings.json](../articles/azure-functions/functions-run-local.md#local-settings-file), bem como os ficheiros de projetos específico de idiomas. Também obtém um novo repositório de Git na pasta de projeto.