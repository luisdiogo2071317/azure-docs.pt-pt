---
title: Adicionar o armazenamento do Azure com os serviços ligados no Visual Studio | Documentos da Microsoft
description: Adicionar o armazenamento do Azure à sua aplicação utilizando a caixa de diálogo do Visual Studio adicionar serviços ligados
services: visual-studio-online
author: ghogen
manager: douge
assetId: 521ec044-ad4b-4828-8864-01decde2e758
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/26/2017
ms.author: ghogen
ms.openlocfilehash: 73b9a1725ada07835db518b57140a2e4d43e143a
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057012"
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Adicionar o armazenamento do Azure com o Visual Studio ligado Services
Com o Visual Studio, pode ligar-se qualquer um dos seguintes para o armazenamento do Azure utilizando o **adicionar serviços ligados** caixa de diálogo:

- Serviço de nuvem do c#
- Serviço móvel de back-end de .NET
- Web site ASP.NET ou serviço
- Serviço ASP.NET Core
- Serviço de WebJob do Azure 

A funcionalidade de serviço ligado adiciona todas as referências necessárias e código de ligação ao seu projeto e modifica os ficheiros de configuração de forma adequada. 

Após a conclusão, o **adicionar serviços ligados** caixa de diálogo automaticamente apresenta documentação com detalhes sobre os passos necessários para começar a trabalhar com o armazenamento de BLOBs, filas e tabelas.

## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Ligar ao armazenamento do Azure com a caixa de diálogo de serviços ligados
1. Abra o projeto no Visual Studio

1. Na **Explorador de soluções**, clique com botão direito a **serviços ligados** nó e, no menu de contexto e selecione **Adicionar serviço ligado**.
   
    ![Adicionar o Azure serviço ligado](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. Na **serviços ligados** página, selecione **armazenamento na Cloud com o armazenamento do Azure**.
   
    ![Adicionar o armazenamento do Azure](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. Na **armazenamento do Azure** caixa de diálogo, selecione uma conta de armazenamento existente e selecione **Add**.
   
    Se precisar de criar uma conta de armazenamento, avance para o passo seguinte. Caso contrário, avance para o passo 6.
    
    ![Adicionar conta de armazenamento existente ao projeto](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. Para criar uma conta de armazenamento: 
   
   1. Selecione **criar uma nova conta de armazenamento** na parte inferior da caixa de diálogo.

   1. Preencha os **criar conta de armazenamento** caixa de diálogo e selecione **criar**.
      
       ![Nova conta de armazenamento do Azure](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. Quando o **armazenamento do Azure** é apresentada a caixa de diálogo, a nova conta de armazenamento é apresentada na lista. Selecione a nova conta de armazenamento na lista e selecione **adicionar**.

1. O armazenamento serviço ligado é apresentado sob o **referências de serviço** nó do seu projeto.
   
## <a name="how-your-project-is-modified"></a>Como o seu projeto é modificado
Quando terminar a caixa de diálogo, o Visual Studio adiciona as referências e modifica a determinados ficheiros de configuração. As alterações específicas dependem do tipo de projeto: 

- Projeto do ASP.NET - [o que aconteceu – projetos do ASP.NET](http://go.microsoft.com/fwlink/p/?LinkId=513126)
- Projeto do ASP.NET Core - [o que aconteceu – projetos do ASP.NET 5](http://go.microsoft.com/fwlink/p/?LinkId=513124) 
- Projeto de serviço cloud (funções da web e funções de trabalho) - [o que aconteceu – projetos de serviço em nuvem](http://go.microsoft.com/fwlink/p/?LinkId=516965)
- Projeto de WebJob - [o que aconteceu - projetos de trabalho Web](visual-studio/vs-storage-webjobs-what-happened.md)

## <a name="next-steps"></a>Passos Seguintes
- [Fórum do MSDN: O armazenamento do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [Blog da Equipe do armazenamento do Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/)
- [Documentação do Armazenamento do Microsoft Azure](https://docs.microsoft.com/azure/storage/)
