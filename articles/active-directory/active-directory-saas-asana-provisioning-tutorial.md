---
title: "Tutorial: Configurar Asana para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs"
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para Asana automaticamente.
services: active-directory
documentationcenter: 
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
ms.openlocfilehash: 4f3bd11a99f43d6405ea285a7a283179d561f92a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Tutorial: Configurar Asana para aprovisionamento de utilizadores automática

O objetivo deste tutorial é mostrar-lhe os passos que necessários para efetuar Asana e o Azure AD para aprovisionar e anular o aprovisionamento contas de utilizador do Azure AD para Asana automaticamente.

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem os seguintes itens:

*   Um inquilino do Azure Active Directory
*   Um inquilino Asana com [Enterprise](https://www.asana.com/pricing) planear ou melhor ativado 
*   Uma conta de utilizador no Asana com permissões de administrador 

> [!NOTE] 
> O Azure AD aprovisionamento integração depende o [Asana API](https://app.asana.com/api/1.0/scim/Users) que está disponível para Asana.

## <a name="assigning-users-to-asana"></a>Atribuir utilizadores a Asana

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento da conta de utilizador automáticas, serão sincronizados apenas os utilizadores que tenham sido "atribuídos" a uma aplicação no Azure AD. 

Antes de configurar e ativar o serviço de aprovisionamento, terá de decidir que os utilizadores no Azure AD representam os utilizadores que necessitam de aceder à sua aplicação Asana. Depois de decidir, pode atribuir estes utilizadores à sua aplicação Asana ao seguir as instruções aqui:

[Atribuir um utilizador a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Sugestões importantes para atribuir utilizadores a Asana

*   Recomenda-se que um único utilizador do Azure AD ser atribuídos a Asana para testar a configuração de aprovisionamento. Os utilizadores adicionais podem ser atribuídos mais tarde.

## <a name="configuring-user-provisioning-to-asana"></a>Configurar o aprovisionamento de utilizadores para Asana 

Esta secção orienta-o seu Azure AD a ligar à conta de utilizador Asana aprovisionamento API e configurar o serviço de aprovisionamento para criar, atualizar e desativar atribuídos a contas de utilizador no Asana com base na atribuição de utilizadores no Azure AD.

> [!TIP]
> Também pode optar por ativar baseados em SAML Single Sign-On para Asana, seguindo as instruções fornecidas no [portal do Azure](https://portal.azure.com). O início de sessão único a pode ser configurado independentemente aprovisionamento automático, apesar destas duas funcionalidades complementar dos entre si.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Para configurar o utilizador automáticas aprovisionamento da conta para Asana no Azure AD:

1)  No [portal do Azure](https://portal.azure.com), navegue para o **do Azure Active Directory > aplicações da empresa > todas as aplicações** secção.

2) Se já tiver configurado Asana para o início de sessão único, procure a instância do Asana utilizando o campo de pesquisa. Caso contrário, selecione **adicionar** e procure **Asana** na Galeria de aplicações. Selecione **Asana** nos resultados de pesquisa e adicione-o à sua lista de aplicações.

3)  Selecione a sua instância do Asana, em seguida, selecione o **aprovisionamento** separador.

4)  Definir o **modo de aprovisionamento** para **automática**.

![Aprovisionamento de Asana](./media/active-directory-saas-asana-provisioning-tutorial/asanaazureprovisioning.png)

5) Na secção credenciais de administrador, siga abaixo instruções para gerar o token e introduza-na **segredo Token** caixa de texto.

* Início de sessão para [Asana](https://app.asana.com) utilizando a conta de administrador
* Clique a fotografia do perfil da barra superior e selecionar as definições atuais do nome da organização
* Navegue até ao separador de contas de serviço
* Clique em Adicionar conta de serviço
* Prestes a atualizar o nome e a fotografia do perfil conforme necessário, copie **Token** e clique em Guardar alterações

6) No portal do Azure, clique em **Testar ligação** para garantir que o Azure AD pode ligar à sua aplicação Asana. Se a ligação falhar, certifique-se a conta de Asana tem as permissões de administrador e tente a **"Ligação de teste"** passo novamente.

7) Introduza o endereço de e-mail de uma pessoa ou grupo que deve receber notificações de erro de aprovisionamento no **correio eletrónico de notificação** campo e marque a caixa de verificação abaixo.

8) Clique em **Guardar**. 

9) Na secção mapeamentos, selecione **sincronizar utilizadores do Azure Active Directory para Asana**.

10) No **mapeamentos de atributos** secção, reveja os atributos de utilizador que serão sincronizados a partir do Azure AD para Asana. Tenha em atenção que os atributos selecionados como **correspondência** propriedades serão utilizadas para corresponder as contas de utilizador no Asana para operações de atualização. Selecione o **guardar** botão para consolidar as alterações. Consulte [personalizar aprovisionamento atributo mapeamentos de utilizador](active-directory-saas-customizing-attribute-mappings.md) para obter mais detalhes

11) Para ativar o Azure AD aprovisionamento do serviço para Asana, altere o **estado de aprovisionamento** para **no** no **definições** secção

12) Clique em **Guardar**. 

Esta ação inicia a sincronização inicial de quaisquer utilizadores atribuídos a Asana na seção de utilizadores. Iniciais demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 20 minutos, desde que o serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para os relatórios de atividade, que descrevem a todas as ações efetuadas pelo serviço de aprovisionamento na sua aplicação Asana de aprovisionamento.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos Adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](active-directory-enterprise-apps-manage-provisioning.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Configurar o início de sessão único](active-directory-saas-asana-tutorial.md)
