---
title: 'Tutorial: Configurar Asana para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para Asana automaticamente.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: 65501bff4ca6c13fe5951ebc260e3ec75514bc8f
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35947860"
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Tutorial: Configurar Asana para aprovisionamento de utilizadores automática

O objetivo deste tutorial é mostrar-lhe os passos que necessários para efetuar Asana e o Azure Active Directory (Azure AD) para aprovisionar e anular o aprovisionamento contas de utilizador do Azure AD para Asana automaticamente.

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem os seguintes itens:

*   Um inquilino do Azure AD
*   Um inquilino Asana com um [Enterprise](https://www.asana.com/pricing) planear ou melhor ativado 
*   Uma conta de utilizador no Asana com permissões de administrador 

> [!NOTE] 
> Depende do Azure AD aprovisionamento integração o [Asana API](https://app.asana.com/api/1.0/scim/Users), que está disponível para Asana.

## <a name="assign-users-to-asana"></a>Atribuir utilizadores a Asana

AD do Azure utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento da conta de utilizador automáticas, apenas os utilizadores atribuídos a uma aplicação no Azure AD estão sincronizados. 

Antes de configurar e ativar o serviço de aprovisionamento, tem de decidir quais os utilizadores no Azure AD tem acesso à sua aplicação Asana. Em seguida, pode atribuir estes utilizadores à sua aplicação Asana ao seguir as instruções aqui:

[Atribuir um utilizador a uma aplicação empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Sugestões importantes para atribuir utilizadores a Asana

Recomendamos que atribua um único utilizador do Azure AD para Asana para testar a configuração de aprovisionamento. Os utilizadores adicionais podem ser atribuídos mais tarde.

## <a name="configure-user-provisioning-to-asana"></a>Configurar o aprovisionamento de utilizadores para Asana 

Esta secção orienta-o seu Azure AD a ligar à conta de utilizador Asana aprovisionamento API. Também é configurar o serviço de aprovisionamento para criar, atualizar e desativar as contas de utilizador atribuída Asana com base em atribuições de utilizador no Azure AD.

> [!TIP]
> Para ativar baseados em SAML-início de sessão único para Asana, siga as instruções fornecidas a [portal do Azure](https://portal.azure.com). O início de sessão único a pode ser configurado independentemente aprovisionamento automático, embora estas duas funcionalidades complementam entre si.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Para configurar o utilizador automáticas aprovisionamento da conta para Asana no Azure AD

1. No [portal do Azure](https://portal.azure.com), navegue para o **do Azure Active Directory** > **aplicações da empresa** > **todas as aplicações** secção.

2. Se já tiver configurado Asana para o início de sessão único, procure a instância do Asana utilizando o campo de pesquisa. Caso contrário, selecione **adicionar** e procure **Asana** na Galeria de aplicações. Selecione **Asana** nos resultados de pesquisa e adicione-o à sua lista de aplicações.

3. Selecione a sua instância do Asana e, em seguida, selecione o **aprovisionamento** separador.

4. Definir **modo de aprovisionamento** para **automática**.

    ![Aprovisionamento de Asana](./media/asana-provisioning-tutorial/asanaazureprovisioning.png)

5. Sob o **credenciais de administrador** secção, siga estas instruções para gerar o token e introduza-o no **segredo Token**:

    a. Inicie sessão no [Asana](https://app.asana.com) utilizando a sua conta de administrador.

    b. Selecione a fotografia do perfil a partir da barra superior e selecionar as definições atuais do nome da organização.

    c. Vá para o **contas de serviço** separador.

    d. Selecione **adicionar conta de serviço**.

    e. Atualização **nome** e **sobre** e a fotografia do perfil conforme necessário. Copie o token no **Token**e selecione-a no **guardar alterações**.

6. No portal do Azure, selecione **Testar ligação** para se certificar de que o Azure AD pode ligar à sua aplicação Asana. Se a ligação falhar, certifique-se de que a conta de Asana tem permissões de administrador e tente a **Testar ligação** passo novamente.

7. Introduza o endereço de e-mail de uma pessoa ou grupo que pretende receber notificações de erro de aprovisionamento no **correio eletrónico de notificação**. Selecione a caixa de verificação por baixo.

8. Selecione **Guardar**. 

9. Sob o **mapeamentos** secção, selecione **sincronizar utilizadores do Azure Active Directory para Asana**.

10. No **mapeamentos de atributos** secção, reveja os atributos de utilizador a ser sincronizadas a partir do Azure AD para Asana. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no Asana para operações de atualização. Selecione **guardar** para consolidar as alterações. Para obter mais informações, consulte [personalizar mapeamentos de atributos de aprovisionamento de utilizador](../active-directory-saas-customizing-attribute-mappings.md).

11. Para ativar o Azure AD aprovisionamento de serviço para Asana, no **definições** secção, altere **estado de aprovisionamento** para **no**.

12. Selecione **Guardar**. 

Agora que inicia a sincronização inicial para os utilizadores atribuídos a Asana no **utilizadores** secção. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o serviço está em execução. Utilize o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para os registos de atividade de aprovisionamento. Os registos de auditoria descrevem todas as ações efetuadas pelo serviço de aprovisionamento na sua aplicação Asana.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar o início de sessão único](asana-tutorial.md)
