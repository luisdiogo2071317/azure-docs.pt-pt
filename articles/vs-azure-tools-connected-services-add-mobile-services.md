---
title: Adicionar os serviços móveis com o serviços ligados no Visual Studio | Documentos da Microsoft
description: Adicionar Mobile Services utilizando a caixa de diálogo do Visual Studio adicionar serviços ligados
services: visual-studio-online
documentationcenter: na
author: mlhoop
manager: douge
editor: ''
ms.assetid: 75c3cb93-88e1-476d-a416-f34caa3608e3
ms.service: visual-studio-online
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: mobile
ms.custom: vs-azure
ms.date: 12/16/2015
ms.author: mlearned
ms.openlocfilehash: a1b2524b059601e1f6f999af7a9d9e063f7c0420
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969350"
---
# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Adicionar os serviços móveis com o Visual Studio ligado Services
Com o Visual Studio 2015, pode ligar a serviços móveis do Azure utilizando o **Adicionar serviço ligado** caixa de diálogo. Pode ligar a partir de qualquer aplicação de cliente do c#, qualquer aplicação do JavaScript ou aplicação do Cordova para várias plataformas. Depois de ligar, pode criar e aceder a dados, criar APIs personalizadas e tarefas agendadas ou adicionar suporte para as notificações push.  A operação de serviços ligados adiciona todas as referências apropriadas e código de ligação. Também pode tirar partido do suporte incorporado para a autenticação com uma variedade de esquemas de identidade populares, como o Azure AD, Facebook, Twitter e Accounts da Microsoft.

## <a name="supported-project-types"></a>Tipos de projeto suportados
> [!NOTE]
> No Visual Studio 2015, adicionar Mobile Services do Azure para um projetos Windows Universal (Windows 10) ao utilizar a caixa de diálogo Adicionar serviços ligados não é suportada. Pode adicionar Mobile Services do Azure, instalar os pacotes adequados, utilizando o Gestor de pacotes de NuGet para o seu projeto.
> 
> 

Pode utilizar a caixa de diálogo de serviços ligados para ligar aos serviços móveis do Azure nos seguintes tipos de projeto.

* Projetos do .NET Windows 8.1 Store, telefone e aplicação Universal
* Projetos de JavaScript Windows 8.1 Store, telefone e aplicação Universal
* Projetos criados com o Visual Studio Tools para Apache Cordova

## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Ligar aos serviços móveis do Azure utilizando a caixa de diálogo Adicionar serviços ligados
1. Certifique-se de que tem uma conta do Azure. Se não tiver uma conta do Azure, pode inscrever-se para obter uma [avaliação gratuita](http://go.microsoft.com/fwlink/?LinkId=518146).
2. Abra o **adicionar serviços ligados** caixa de diálogo.
   
   * Para aplicações de .NET, abra o projeto no Visual Studio, abra o menu de contexto para o **referências** nó no Solution Explorer e, em seguida, escolha **Adicionar serviço ligado**
     
        ![Ligar ao serviço móvel do Azure](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)
   * Para projetos de aplicação Apache Cordova, abra o projeto no Visual Studio, abra o menu de contexto para o nó do projeto no Solution Explorer e, em seguida, escolha **Adicionar serviço ligado**.
3. Na **Adicionar serviço ligado** caixa de diálogo caixa, escolha **serviços móveis do Azure**e, em seguida, selecione o **configurar** botão. Poderá ser-lhe pedido para iniciar sessão no Azure, se ainda não o tiver feito.
   
    ![Adicionar um serviço móvel do Azure](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)
4. Na **Mobile Services do Azure** diálogo caixa, escolha um serviço móvel existente se tiver uma. Se precisar de criar um novo serviço móvel do Azure, siga o procedimento abaixo para fazê-lo. Caso contrário, avance para o passo seguinte.
   
    Para criar uma nova conta de serviço móvel:
   
   1. Escolha a * * criar serviço * * ligação na parte inferior da caixa de diálogo.
       ![Adicionar novo serviço ligado móvel](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)
   2. Sobre o **criar o serviço móvel** caixa de diálogo, pode escolher um serviço móvel de back-end de JavaScript, ou um serviço móvel de back-end de .NET do **tempo de execução** na lista pendente. 
      
       ![Criação de serviços móveis](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)
      
       Um serviço de back-end de JavaScript é simples e eficiente. Se criar um serviço móvel de back-end de JavaScript, o código de JavaScript do lado do servidor é armazenado na cloud, mas pode editar scripts de servidor utilizando o Explorador de servidores ou o portal de gestão do Azure. 
      
       Um serviço móvel de back-end do .NET dá-lhe a capacidade máxima e a flexibilidade da Web API e o Entity Framework. Se criar um serviço móvel de back-end de .NET, um projeto é criado para si e adicionado à sua solução. 
   3. Escolha o **região** onde o serviço móvel e, em seguida, introduza um nome de utilizador e palavra-passe para o servidor.
   4. Após introduzir todas as informações necessárias, escolha o **criar** botão para criar o serviço móvel.
   5. O novo serviço móvel deve aparecer na lista de serviços na **Mobile Services do Azure** caixa de diálogo. Escolha o novo serviço móvel na lista e, em seguida, escolha o **adicionar** botão para adicionar o serviço ao seu projeto.
5. Reveja a página de introdução ao obter que aparece e descubra como o seu projeto foi modificado. Sempre que adicionar um serviço ligado, é apresentada uma página de introdução no seu browser. Pode rever os passos seguintes sugeridos e exemplos de código ou mudar para a página o que aconteceu para ver quais referências foram adicionadas ao seu projeto e, como seus arquivos de código e a configuração foram modificados.
6. Com os exemplos de código como guia, começar a escrever código para acessar o seu serviço móvel!

## <a name="how-your-project-is-modified"></a>Como o seu projeto é modificado
Como o Visual Studio modifica seu projeto depende do tipo de projeto. Para C# aplicações de cliente, consulte [o que aconteceu – C# projetos](http://go.microsoft.com/fwlink/p/?LinkId=513119). Para aplicativos de cliente JavaScript, consulte [o que aconteceu – projetos de JavaScript](http://go.microsoft.com/fwlink/p/?LinkId=513120). Para aplicações Cordova, consulte [o que aconteceu – Cordova projetos](http://go.microsoft.com/fwlink/p/?LinkId=513116).

## <a name="next-steps"></a>Passos Seguintes
Faça perguntas e obter ajuda: 

* [Fórum do MSDN: Mobile Services do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)
* [Serviços móveis do Azure no Blog da Equipe do Microsoft Azure](https://azure.microsoft.com/blog/topics/mobile/)
* [Serviços móveis do Azure em azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)
* [Documentação de serviços móveis do Azure em azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)

