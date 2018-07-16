---
title: Como utilizar as palavras-passe de aplicação no Azure MFA? | Microsoft Docs
description: Esta página ajudará os usuários a compreender quais são as palavras-passe de aplicação e o que são utilizadas com relação ao MFA do Azure.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: lizross
ms.custom: end-user
ms.openlocfilehash: 290458e95aaed0cc85d83539d9d870c334df45df
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059432"
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>O que são palavras-passe de aplicação no Azure multi-factor Authentication?
Determinadas aplicações não baseadas no browser, tais como o cliente de e-mail nativo da Apple que utiliza o Exchange Active Sync, atualmente não suportam autenticação multifator. Autenticação multifator está ativada por utilizador. Isso significa que se um utilizador tiver sido ativado para autenticação multifator e que estão a tentar utilizar aplicações não baseadas no browser, será possível fazê-lo. Uma palavra-passe de aplicação permite tal utilização. Se impor o multi-factor Authentication através de políticas de acesso condicional e não através de MFA por utilizador, não é possível criar palavras-passe de aplicação. Aplicações que utilizam políticas de acesso condicional para controlar o acesso não é necessário palavras-passe de aplicação.

Assim que tiver uma palavra-passe de aplicação, é usar isso em vez da palavra-passe original com estas aplicações não baseadas no browser. Isto acontece porque ao se registrar para verificação de dois passos, está dizendo ao Microsoft para não deixar que qualquer pessoa que inicie sessão com a palavra-passe se eles também não é possível efetuar a verificação de segundo. O cliente de e-mail nativa da Apple no seu telemóvel não pode iniciar sessão como utilizador porque não há verificação de dois passos. A solução para isso é criar uma mais segura palavra-passe de aplicação não utilizar todos os dias, mas apenas para aqueles aplicativos que não é possível suportar a verificação de dois passos. Utilize a palavra-passe de aplicação para que aplicações podem ignorar a multi-factor authentication e continuar a funcionar.

> [!NOTE]
> Novos protocolos de autenticação de suporte de clientes do Office 2013 (incluindo Outlook) e pode ser utilizado com verificação de dois passos.  Isso significa que uma vez ativada, as palavras-passe de aplicação não são necessárias para utilização com clientes do Office 2013.  Para obter mais informações, consulte [Office 2013 autenticação moderna pré-visualização pública anunciada](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-to-use-app-passwords"></a>Como utilizar as palavras-passe de aplicação
Seguem-se algumas coisas a serem lembrados sobre como utilizar as palavras-passe de aplicação.

* Não crie suas próprias palavras-passe de aplicação. Em vez disso, estas são geradas automaticamente. Uma vez que apenas tem de introduzir a palavra-passe de aplicação uma vez por aplicação, é mais seguro utilizar uma palavra-passe mais complexa, gerada automaticamente, em vez de fazer um que pode se lembrar.
* Atualmente, existe um limite de 40 palavras-passe por utilizador. Se tentar criá-lo Depois de ter atingido o limite, será solicitado que elimine uma das suas palavras-passe de aplicação existentes antes de criar um novo.
* Deve usar uma palavra-passe aplicação por dispositivo, não por aplicação. Por exemplo, pode criar uma palavra-passe de aplicação para o seu computador portátil e utilizar essa palavra-passe de aplicação para todas as suas aplicações desse portátil. Em seguida, crie uma segunda palavra-passe de aplicação a utilizar para todas as suas aplicações no ambiente de trabalho.
* Tem uma palavra-passe de aplicação à primeira que registar para verificação de dois passos.  Se precisar de outras opções, pode criá-los.



## <a name="creating-and-deleting-app-passwords"></a>Criar e excluir as palavras-passe de aplicação
Durante sua inicial início de sessão, tem uma palavra-passe de aplicação que pode utilizar.  Além disso, também pode criar e eliminar as palavras-passe de aplicação mais tarde.  Como fazer isso depende de como utilizar o multi-factor authentication. Responda às seguintes perguntas para determinar onde deve ir ao gerir palavras-passe de aplicação:

1. Usa a verificação da sua conta Microsoft pessoal? Se Sim, deve consultar o [palavras-passe de aplicação e verificação de dois passos](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artigo para obter ajuda. Se não, continue a duas perguntas das perguntas.

2. OK, pelo que utilizar a verificação de dois passos para a sua conta escolar ou profissional. Utilizá-la para iniciar sessão em aplicações do Office 365? Se Sim, deve consultar [criar uma palavra-passe de aplicação para o Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) para obter ajuda. Se não, continue a pergunta três.

3. Usa a verificação com o Microsoft Azure? Se Sim, avance para o [gerir palavras-passe de aplicação no portal do Azure](#manage-app-passwords-in-the-Azure-portal) seção deste artigo. Se não, continue a pergunta de quatro.

4. Não tem a certeza onde pode utilizar a verificação de dois passos? Avance para o [gerir palavras-passe de aplicação com o portal MyApps](#manage-app-passwords-with-the-myapps-portal) seção deste artigo.


## <a name="manage-app-passwords-in-the-azure-portal"></a>Gerir palavras-passe de aplicação no portal do Azure
Se utilizar a verificação de dois passos com o Azure, em que pretende criar palavras-passe de aplicação através do portal do Azure.



## <a name="manage-app-passwords-with-the-myapps-portal"></a>Faça a gestão de palavras-passe de aplicação com o portal MyApps.
Se não tiver a certeza de como utilizar a autenticação multifator, em seguida, pode sempre criar e eliminar as palavras-passe de aplicação através do portal myapps.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Para criar uma palavra-passe de aplicação com o portal MyApps
1. Inicie sessão no [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Clique no nome da parte superior direita e selecione **perfil**.
3. Selecione **verificação adicional de segurança**.
   ![Selecione a verificação de segurança adicional - captura de ecrã](./media/multi-factor-authentication-end-user-app-passwords/myapps1.png)

4. Selecione **palavras-passe de aplicação**.
   ![Selecione palavras-passe de aplicação - captura de ecrã](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Clique em **Criar**.
6. Introduza um nome para a palavra-passe de aplicação e clique em **seguinte**.
7. Copie a palavra-passe de aplicação para a área de transferência e cole-o na sua aplicação.
   ![Criar uma palavra-passe de aplicação](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Para eliminar uma palavra-passe de aplicação com o portal MyApps
1. Inicie sessão no [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Na parte superior, selecione o perfil.
3. Selecione **verificação adicional de segurança**.

   ![Selecione a verificação de segurança adicional - captura de ecrã](./media/multi-factor-authentication-end-user-app-passwords/myapps1.png)

4. Selecione **palavras-passe de aplicação**.

   ![Selecione palavras-passe de aplicação - captura de ecrã](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Junto a palavra-passe de aplicação que pretende eliminar, clique em **eliminar**.

   ![Eliminar uma palavra-passe de aplicação](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Confirme que pretende eliminar essa palavra-passe clicando **Sim**.
7. Assim que a palavra-passe de aplicação for eliminada, pode clicar em **fechar**.

## <a name="next-steps"></a>Passos Seguintes

- [Gerir as definições da verificação de dois passos](multi-factor-authentication-end-user-manage-settings.md)

- Experimente o [aplicação Microsoft Authenticator](microsoft-authenticator-app-how-to.md) para verificar os inícios de sessão com as notificações de aplicações, em vez de textos ou chamadas a receber.
