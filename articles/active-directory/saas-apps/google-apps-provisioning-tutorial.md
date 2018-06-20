---
title: 'Tutorial: Configurar G Suite para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs'
description: Saiba como aprovisionar e anular o aprovisionamento contas de utilizador do Azure AD para G Suite automaticamente.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 4c685e03e5b7532f50d1eee1590eebedfba2b7c2
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36212909"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Tutorial: Configurar G Suite para o aprovisionamento de utilizador automáticas

O objetivo deste tutorial é mostrar-lhe como aprovisionar e anular o aprovisionamento contas de utilizador do Azure Active Directory (Azure AD) para G Suite automaticamente.

> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](./../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Suite de G, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um G Suite-início de sessão único ativada subscrição
- Uma subscrição do Google Apps ou a subscrição do Google Cloud Platform.

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="assign-users-to-g-suite"></a>Atribuir utilizadores a G Suite

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento da conta de utilizador automáticas, apenas os utilizadores e grupos que tenham sido "atribuídos" a uma aplicação no Azure AD são sincronizados.

Antes de configurar e ativar o serviço de aprovisionamento, terá de decidir quais os utilizadores ou grupos no Azure AD que necessitam de aceder à sua aplicação. Depois de tiver tomado a decisão, pode atribuir estes utilizadores à sua aplicação ao seguir as instruções no [atribuir um utilizador ou grupo a uma aplicação empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

> [!IMPORTANT]
> Recomendamos que um único utilizador do Azure AD ser atribuídos a G Suite para testar a configuração de aprovisionamento. Pode atribuir utilizadores e grupos adicionais mais tarde.

> Quando atribui um utilizador para o conjunto de G, selecione o **utilizador** ou **grupo** função na caixa de diálogo de atribuição. O **predefinido acesso** função não funciona para o aprovisionamento.

## <a name="enable-automated-user-provisioning"></a>Ativar o aprovisionamento automatizado do utilizador

Esta secção orienta-o processo de ligar o seu Azure AD para a API de G Suite de aprovisionamento da conta de utilizador. Também o ajuda a configurar o serviço de aprovisionamento para criar, atualizar e desativar as contas de utilizador atribuído no conjunto de G, com base no utilizador e a atribuição de grupo no Azure AD.

>[!TIP]
>Também pode optar por ativar baseados em SAML-início de sessão único para conjuntos de G, seguindo as instruções de [portal do Azure](https://portal.azure.com). O início de sessão único a pode ser configurado independentemente aprovisionamento automático, apesar destas duas funcionalidades complementar dos entre si.

### <a name="configure-automatic-user-account-provisioning"></a>Configurar o aprovisionamento da conta de utilizador automáticas

> [!NOTE]
> Outra opção viável para automatizar o aprovisionamento de utilizadores para G Suite consiste em utilizar [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en). GADS Aprovisiona as identidades do Active Directory no local para G Suite. Em contrapartida, a solução neste tutorial Aprovisiona os utilizadores do Active Directory do Azure (nuvem) e grupos com capacidade de correio eletrónico para G Suite. 

1. Iniciar sessão para o [consola de administração de aplicações do Google](http://admin.google.com/) com a sua conta de administrador e, em seguida, selecione **segurança**. Se não vir a ligação, poderá estar oculta sob o **mais controlos** menu na parte inferior do ecrã.
   
    ![Selecione a segurança.][10]

2. No **segurança** página, selecione **referência da API**.
   
    ![Selecione a referência da API.][15]

3. Selecione **acesso à API ativar**.
   
    ![Selecione a referência da API.][16]

    > [!IMPORTANT]
    > Para cada utilizador que pretende aprovisionar o conjunto de G, os utilizadores de nome no Azure Active Directory *tem* ser vinculada a um domínio personalizado. Por exemplo, o utilizador nomes esse aspeto bob@contoso.onmicrosoft.com não serão aceites pela G Suite. Por outro lado, bob@contoso.com é aceite. Pode alterar o domínio de um utilizador existente, editando as respetivas propriedades no Azure AD. Iremos tiver incluído instruções sobre como definir um domínio personalizado para o Azure Active Directory e G Suite nos passos seguintes.
      
4. Se ainda não adicionou um nome de domínio personalizado ao seu Azure Active Directory, em seguida, siga os passos seguintes:
  
    a. No [portal do Azure](https://portal.azure.com), no painel de navegação esquerdo, selecione **do Active Directory**. Na lista de diretório, selecione o diretório. 

    b. Selecione **nome de domínio** no painel de navegação esquerdo e, em seguida, selecione **adicionar**.
     
     ![Domínio](./media/google-apps-provisioning-tutorial/domain_1.png)

     ![Adicionar domínio](./media/google-apps-provisioning-tutorial/domain_2.png)

    c. Escreva o seu nome de domínio para o **nome de domínio** campo. Este nome de domínio deve ser o mesmo nome de domínio que pretende utilizar para G Suite. Em seguida, selecione o **Adicionar domínio** botão.
     
     ![Nome de domínio](./media/google-apps-provisioning-tutorial/domain_3.png)

    d. Selecione **seguinte** para ir para a página de verificação. Para verificar que é proprietário deste domínio, edite registos DNS do domínio, de acordo com os valores que são fornecidos nesta página. Pode optar por verificar utilizando **registos MX** ou **registos TXT**, consoante aquilo que selecionar para o **tipo de registo** opção. 
    
    Para obter instruções mais abrangentes sobre a forma de verificar nomes de domínio com o Azure AD, consulte [adicionar o seu próprio nome de domínio para o Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Domínio](./media/google-apps-provisioning-tutorial/domain_4.png)

    e. Repita os passos anteriores para todos os domínios que pretende adicionar ao seu diretório.

    > [!NOTE]
    Para o aprovisionamento de utilizador, o domínio personalizado tem de corresponder ao nome de domínio de origem do Azure AD. Se não corresponderem, poderá conseguir resolver o problema através da implementação de personalização de mapeamento de atributos.


5. Agora que tiver verificado todos os seus domínios com o Azure AD, deve verificá-las novamente com o Google Apps. Para cada domínio que já não está registado com o Google, siga os passos seguintes:
   
    a. No [consola de administração de aplicações do Google](http://admin.google.com/), selecione **domínios**.
     
     ![Seleccione domínios][20]

    b. Selecione **adicionar um domínio ou um alias de domínio**.
     
     ![Adicionar um novo domínio][21]

    c. Selecione **adicionar outro domínio**e, em seguida, escreva o nome de domínio que pretende adicionar.
     
     ![Escreva o nome de domínio][22]

    d. Selecione **continuar e verificar a propriedade de domínio**. Em seguida, siga os passos para verificar que possui o nome de domínio. Para obter instruções abrangentes sobre a forma de verificar o seu domínio com o Google, consulte [verificar a sua propriedade do site com o Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Repita os passos anteriores para quaisquer domínios adicionais que pretende adicionar ao Google Apps.
     
     > [!WARNING]
     > Se alterar o domínio primário para o seu inquilino do conjunto de G, e se já tiver configurado o início de sessão único com o Azure AD, em seguida, terá de repetir o passo #3 em [passo 2: Ativar o início de sessão único](#step-two-enable-single-sign-on).
       
6. No [consola de administração de aplicações do Google](http://admin.google.com/), selecione **funções de administrador**.
   
     ![Selecione as aplicações do Google][26]

7. Determine que conta de administrador que pretende utilizar para gerir o aprovisionamento de utilizadores. Para o **da função Administrador** dessa conta, de editar o **privilégios** para essa função. Certifique-se de ativar todas as **privilégios da API de administrador** para que esta conta pode ser utilizada para o aprovisionamento.
   
     ![Selecione as aplicações do Google][27]
   
    > [!NOTE]
    > Se estiver a configurar o ambiente de produção, é a melhor prática criar uma conta de administrador no G Suite especificamente para este passo. Estas contas tem de ter uma função de administrador associada aos mesmos, que tem os privilégios necessários de API.
     
8. No [portal do Azure](https://portal.azure.com), navegue para o **do Azure Active Directory** > **aplicações da empresa** > **todas as aplicações** secção.

9. Se já tiver configurado G Suite para o início de sessão único, procure a instância do conjunto de G, utilizando o campo de pesquisa. Caso contrário, selecione **adicionar**e, em seguida, procure **G Suite** ou **Google Apps** na Galeria de aplicações. Selecione a sua aplicação na lista de resultados de pesquisa e, em seguida, adicioná-lo à sua lista de aplicações.

10. Selecione a sua instância do conjunto de G e, em seguida, selecione o **aprovisionamento** separador.

11. Definir o **modo de aprovisionamento** para **automática**. 

     ![Aprovisionamento](./media/google-apps-provisioning-tutorial/provisioning.png)

12. Sob o **credenciais de administrador** secção, selecione **autorizar**. É aberta uma caixa de diálogo de autorização de Google numa nova janela do browser.

13. Certifique-se de que pretende dar permissão de Azure Active Directory para efetuar alterações ao seu inquilino de G Suite. Selecione **aceitar**.
    
     ![Confirme as permissões.][28]

14. No portal do Azure, selecione **Testar ligação** para se certificar de que o Azure AD pode ligar à sua aplicação. Se a ligação falhar, certifique-se de que a conta de G Suite tem permissões de administração de equipa. Em seguida, tente o **autorizar** passo novamente.

15. Introduza o endereço de e-mail de uma pessoa ou grupo que deve receber notificações de erro de aprovisionamento no **correio eletrónico de notificação** campo. Em seguida, selecione a caixa de verificação.

16. Selecione **guardar.**

17. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory ao Google Apps**.

18. No **mapeamentos de atributos** secção, reveja os atributos de utilizador são sincronizados a partir do Azure AD para G Suite. Os atributos que são **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no conjunto de G para operações de atualização. Selecione **guardar** para consolidar as alterações.

19. Para ativar o Azure AD aprovisionamento do serviço para o conjunto de G, altere o **estado de aprovisionamento** para **no** no **definições**.

20. Selecione **Guardar**.

Este processo inicia a sincronização inicial de quaisquer utilizadores ou grupos que estão atribuídos ao conjunto de G na secção utilizadores e grupos. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos enquanto o serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para os registos de atividade de aprovisionamento. Estes registos descrevem todas as ações que são efetuadas pelo serviço de aprovisionamento na sua aplicação.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar o início de sessão único](google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/google-apps-provisioning-tutorial/gapps-auth.png
