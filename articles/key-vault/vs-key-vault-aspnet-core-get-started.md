---
title: Introdução à chave de cofre ligado serviço no Visual Studio (projetos de núcleo de ASP.NET) | Microsoft Docs
description: Utilize este tutorial para o ajudar a saber como adicionar suporte de Cofre de chaves para uma aplicação de web do ASP.NET ou ASP.NET Core.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 64d1a404eac955f47308f01edd56b3d008e250a0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787515"
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-core-projects"></a>Introdução à chave de cofre ligado serviço no Visual Studio (projetos de núcleo de ASP.NET)

> [!div class="op_single_selector"]
> - [Introdução](vs-key-vault-aspnet-core-get-started.md)
> - [O que aconteceu](vs-key-vault-aspnet-core-what-happened.md)

Este artigo fornece orientação adicional depois de adicionar o Cofre de chaves para um projeto em ASP.NET Core através de **adicionar serviços ligados** comando no Visual Studio. Se ainda não adicionou o serviço ao seu projeto, pode fazê-em qualquer altura ao seguir as instruções no [Cofre de chaves de adicionar à sua aplicação web utilizando o Visual Studio ligado Services](vs-key-vault-add-connected-service.md).

Consulte [o que aconteceu ao meu projeto em ASP.NET Core?](vs-key-vault-aspnet-core-what-happened.md) para que as alterações efetuadas ao seu projeto ao adicionar o serviço ligado.

## <a name="after-you-connect"></a>Depois de ligar

1. Adicione um segredo no seu Cofre de chaves do Azure. Para obter o local certo no portal, clique na ligação para segredos gerir armazenados neste Cofre de chave. Se fechar a página ou o projeto, pode navegar para-lo no [portal do Azure](https://portal.azure.com) escolhendo **todos os serviços**, em **segurança**, escolha **doCofredechaves**, em seguida, selecione o Cofre de chaves que acabou de criar.

   ![Navegar para o portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. Na secção do Cofre de chaves para a chave do cofre que criou, escolha **segredos**, em seguida, **gerar/importação**.

   ![Gerar/importação um segredo](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Introduza um segredo, tais como "MySecret" e atribua-lhe qualquer valor de cadeia como um teste, em seguida, escolha o **criar** botão.

   ![Criar um segredo](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (opcional) Introduza outro segredo, mas este tempo, coloque-o numa categoria ao nomear- **segredos – MySecret**. Esta sintaxe especifica uma categoria **segredos** que contenha um segredo **MySecret.**
1. No projeto do Visual Studio, agora, pode referenciar estes segredos utilizando as seguintes expressões no código:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

Parabéns, agora ativou a sua aplicação web utilizar o Cofre de chaves para aceder a segredos armazenados em segurança.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos. Isto elimina o Cofre de chaves e os recursos relacionados. Para eliminar o grupo de recursos através do portal:

1. O nome do grupo de recursos na caixa Pesquisar, na parte superior do portal. Quando vir o grupo de recursos utilizado neste Início Rápido nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Na caixa **ESCREVA O NOME DO GRUPO DE RECURSOS:**, escreva o nome do grupo de recursos e selecione **Eliminar**.

# <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como desenvolver com o Cofre de chaves no [Guia do programador do Cofre de chave](key-vault-developers-guide.md)