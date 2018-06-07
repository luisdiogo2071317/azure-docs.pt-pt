---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 05/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 16bda26a80611b29fdb100736cfc48978e63f75a
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34702431"
---
O modelo do projeto das Funções do Azure no Visual Studio cria um projeto que pode ser publicado numa aplicação de funções no Azure. As aplicações de funções permitem-lhe agrupar funções como unidades lógicas para a gestão, implementação e partilha de recursos.

1. No Visual Studio, selecione **Novo** > **Projeto**, no menu **Ficheiro**.

2. Na caixa de diálogo **Novo Projeto**, selecione **Instalado**, expanda **Visual C#** > **Cloud**, selecione **Funções do Azure**, escreva um **Nome** para o projeto e clique em **OK**. O nome da aplicação de funções deve ser válido como um espaço de nomes C#. Portanto, não use sublinhados, hífenes ou outros carateres não alfanuméricos.

    ![Caixa de diálogo Novo projeto para criar uma função no Visual Studio](./media/functions-vstools-create/functions-vstools-add-new-project.png)

3. Utilize as definições especificadas na tabela a seguir à imagem.

    ![Caixa de diálogo de função nova no Visual Studio](./media/functions-vstools-create/functions-vstools-add-new-function.png) 

    | Definição      | Valor sugerido  | Descrição                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Versão** | Funções do Azure v1 <br />(.NET Framework) | Cria um projeto de função que utiliza o runtime com a versão 1 das Funções do Azure. O runtime com a versão 2, que suporte .NET Core, está atualmente em pré-visualização. Para obter mais informações, veja [How to target Azure Functions runtime version](../articles/azure-functions/functions-versions.md) (Como segmentar a versão do runtime das Funções do Azure).   |
    | **Modelo** | Acionador HTTP | Cria uma função acionada por um pedido HTTP. |
    | **Conta de armazenamento**  | Emulador do Armazenamento | Os acionadores HTTP não utilizam a ligação da conta de Armazenamento. Todos os outros tipos de acionadores requerem uma cadeia de ligação da conta de Armazenamento válida. |
    | **Direitos de acesso** | Anónimo | A função criada pode ser acionada por qualquer cliente sem fornecer uma chave. Esta definição de autorização torna mais fácil testar a função nova. Para obter mais informações sobre chaves e a autorização, veja [Chaves de autorização](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys), no artigo [Enlaces HTTP e webhook](../articles/azure-functions/functions-bindings-http-webhook.md). |
4. Clique em **OK** para criar o projeto de função e a função acionada por HTTP.

