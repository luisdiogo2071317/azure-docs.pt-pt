---
title: 'Guia de Início Rápido: Adicionar utilizadores convidados no portal do Azure'
description: Utilize este guia de início rápido para saber como os administradores do Azure Active Directory podem adicionar utilizadores convidados B2B no portal do Azure e ler as instruções do fluxo de trabalho de convite B2B.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: ed49eed6dbfce9a71f13770e3ddcaec6557cf875
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45986550"
---
# <a name="quickstart-add-guest-users-to-your-directory-in-the-azure-portal"></a>Guia de Início Rápido: Adicionar utilizadores convidados ao seu diretório no portal do Azure

Pode convidar qualquer pessoa a colaborar com a sua organização ao adicioná-la ao seu diretório como um utilizador convidado. Em seguida, pode enviar um e-mail de convite que contenha uma ligação de resgate ou enviar uma ligação direta para uma aplicação que pretende partilhar. Os utilizadores convidados podem iniciar sessão com as respetivas identidades das redes sociais, da escola ou do trabalho.

Neste guia de início rápido, irá adicionar um novo utilizador convidado ao Azure Active Directory, enviar um convite e ver como funciona o processo de resgate do convite do utilizador convidado.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir o cenário deste tutorial, precisa de:

 - Uma função que lhe permita criar utilizadores no seu diretório de inquilinos, como a função de Administrador Global ou qualquer uma das funções de diretório de administrador limitadas.
 - Uma conta de e-mail válida que possa adicionar ao seu diretório de inquilinos e que possa utilizar para receber o e-mail de convite para teste.

## <a name="add-a-new-guest-user-in-azure-ad"></a>Adicionar um novo utilizador convidado no Azure Active Directory

1. Inicie sessão no [portal do Azure](https://portal.azure.com/) como administrador do Azure Active Directory.
2. No painel esquerdo, selecione **Azure Active Directory**.
3.  Em **Gerir**, selecione **Utilizadores**.

    ![Selecione Azure Active Directory](media/quickstart-add-users-portal/quickstart-users-portal-user.png)

4.  Selecione **Novo utilizador convidado**.

    ![Selecione Azure Active Directory](media/quickstart-add-users-portal/quickstart-users-portal-user-3.png)

5.  Em **Nome de utilizador**, introduza o endereço de e-mail do utilizador externo. Em **Incluir uma mensagem pessoal com o convite**, escreva uma mensagem de boas-vindas. 

    ![Selecione Azure Active Directory](media/quickstart-add-users-portal/quickstart-users-portal-user-4.png)

6. Selecione **Convidar** para enviar automaticamente o convite ao utilizador convidado. É apresentada uma notificação com a mensagem **Utilizador convidado com sucesso** no canto superior direito. 
7.  Após enviar o convite, a conta do utilizador será automaticamente adicionada ao diretório como convidada.

## <a name="assign-an-app-to-the-guest-user"></a>Atribuir uma aplicação ao utilizador convidado
Adicione a aplicação Salesforce ao seu inquilino de teste e atribua o utilizador convidado de teste à aplicação.
1.  Inicie sessão no portal do Azure como administrador do Azure Active Directory.
2.  No painel esquerdo, selecione **Aplicações empresariais**.
3.  Selecione **Nova aplicação**.
4. Em **Adicionar a partir da galeria**, pesquise a aplicação **Salesforce** e, em seguida, selecione-a.

    ![Selecione Azure Active Directory](media/quickstart-add-users-portal/quickstart-users-portal-select-salesforce.png)
5. Selecione **Adicionar**.
6. Em **Gerir**, selecione **Início de sessão único** e em **Modo de Início de Sessão Único**, selecione **Início de Sessão Baseado em Palavra-passe** e clique em **Guardar**.
7. Em **Gerir**, selecione **Utilizadores e grupos** > **Adicionar utilizador** > **Utilizadores e grupos**.
8. Utilize a caixa de pesquisa para pesquisar o utilizador de teste (caso seja necessário) e selecione o utilizador de teste na lista. Em seguida, clique em **Selecionar**.
9. Selecione **Atribuir**. 

## <a name="accept-the-invitation"></a>Aceitar o convite
Agora inicie sessão como utilizador convidado para ver o convite.
1.  Inicie sessão na conta de e-mail do seu utilizador convidado de teste.
2.  Na sua caixa de entrada, procure o e-mail "Foi convidado".

    ![E-mail de convite B2B](media/quickstart-add-users-portal/quickstart-users-portal-email-small.png)

3.  No corpo do e-mail, selecione **Começar Agora**. É apresentada uma página **Analisar permissões** no browser. 

    ![Página de aceitação do convite B2B](media/quickstart-add-users-portal/quickstart-users-portal-accept.png)

4. Selecione **Aceitar**. É apresentado o Painel de Acesso, que enumera as aplicações a que o utilizador convidado pode aceder.

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não forem necessários, elimine o utilizador convidado de teste e a aplicação de teste.
1.  Inicie sessão no portal do Azure como administrador do Azure Active Directory.
2.  No painel esquerdo, selecione **Azure Active Directory**.
3.  Em **Gerir**, selecione **Aplicações empresariais**.
4.  Abra a aplicação **Salesforce** e, em seguida, selecione **Eliminar**.
5.  No painel esquerdo, selecione **Azure Active Directory**.
6.  Em **Gerir**, selecione **Utilizadores**.
7.  Selecione o utilizador de teste e, em seguida, selecione **Eliminar utilizador**.

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, criou um utilizador convidado no portal do Azure e enviou um convite para partilhar aplicações. Em seguida, viu o processo de resgate na perspetiva do utilizador convidado e verificou que a aplicação era apresentada no Painel de Acesso do utilizador convidado. Para saber mais sobre como adicionar utilizadores convidados para colaboração, veja [Adicionar utilizadores de colaboração B2B do Azure Active Directory no portal do Azure](add-users-administrator.md).