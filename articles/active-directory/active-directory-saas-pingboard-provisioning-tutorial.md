---
title: "Tutorial: Configurar Pingboard para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs"
description: Saiba como configurar o Azure Active Directory para aprovisionar e anular o aprovisionamento contas de utilizador para Pingboard automaticamente.
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
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: f74e890cf716cfd4bc7b41fcc4aa244969cafde5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Tutorial: Configurar Pingboard para aprovisionamento de utilizadores automática

O objetivo deste tutorial é mostrar-lhe os passos que necessários para efetuar para permitir o aprovisionamento e anular o aprovisionamento de contas de utilizador do Azure Active Directory para Pingboard automática.

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial assume que já tem os seguintes itens:

*   Um inquilino do Azure Active Directory
*   Um inquilino Pingboard [conta profissional](https://pingboard.com/pricing) 
*   Uma conta de utilizador no Pingboard com permissões de administrador 

> [!NOTE] 
> O Azure Active Directory aprovisionamento integração baseia-se na Pingboard API (`https://your_domain.pingboard.com/scim/v2`) que está disponível para a sua conta.

## <a name="assigning-users-to-pingboard"></a>Atribuir utilizadores a Pingboard

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento da conta de utilizador automáticas, apenas os utilizadores que tenham sido "atribuídos" a uma aplicação no Azure Active Directory estão sincronizados. 

Antes de configurar e ativar o serviço de aprovisionamento, tem de decidir que os utilizadores no Azure Active Directory representam os utilizadores que necessitam de aceder à sua aplicação Pingboard. Depois de decidir, pode atribuir estes utilizadores à sua aplicação Pingboard ao seguir as instruções aqui:

[Atribuir um utilizador a uma aplicação empresarial](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Sugestões importantes para atribuir utilizadores a Pingboard

*   É recomendado que atribua um único utilizador do Azure Active Directory para Pingboard para testar a configuração de aprovisionamento. Os utilizadores adicionais podem ser atribuídos mais tarde.

## <a name="configuring-user-provisioning-to-pingboard"></a>Configurar o aprovisionamento de utilizadores para Pingboard 

Esta secção orienta-o de ligar o Azure Active Directory para a conta de utilizador de Pingboard aprovisionamento API e configurar o serviço de aprovisionamento para criar, atualizar e desativar as contas de utilizador atribuída Pingboard com base na atribuição de utilizadores no Azure Do Active Directory.

> [!TIP]
> Também pode optar por ativar baseados em SAML Single Sign-On para Pingboard, seguindo as instruções fornecidas no [portal do Azure](https://portal.azure.com). O início de sessão único a pode ser configurado independentemente aprovisionamento automático, apesar destas duas funcionalidades complementar dos entre si.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-active-directory"></a>Para configurar o aprovisionamento da conta de utilizador automáticas para Pingboard no Azure Active Directory:

1)  No [portal do Azure](https://portal.azure.com), navegue para o **do Azure Active Directory > aplicações da empresa > todas as aplicações** secção.

2) Se já tiver configurado Pingboard para o início de sessão único, procure a instância do Pingboard utilizando o campo de pesquisa. Caso contrário, selecione **adicionar** e procure **Pingboard** na Galeria de aplicações. Selecione **Pingboard** nos resultados de pesquisa e adicione-o à sua lista de aplicações.

3)  Selecione a sua instância do Pingboard, em seguida, selecione o **aprovisionamento** separador.

4)  Definir o **modo de aprovisionamento** para **automática**.

![Aprovisionamento de Pingboard](./media/active-directory-saas-pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
5) Na secção credenciais de administrador, execute os seguintes passos:

* No **URL de inquilino** caixa de texto, introduza `https://your_domain.pingboard.com/scim/v2` e substitua o seu domínio real your_domain
* Inicie sessão no [Pingboard](https://pingboard.com/) utilizando a conta de administrador.
* Clique em suplementos > integrações > do Azure Active Directory.
* Clique no separador de configurar e selecione **ativar o aprovisionamento de utilizadores do Azure**.
* Copiar o **tokens de portador do OAuth** campo e introduzir na **segredo Token** caixa de texto.

6) No portal do Azure, clique em **Testar ligação** para garantir que o Azure Active Directory podem ligar-se à sua aplicação Pingboard. Se a ligação falhar, certifique-se a conta de Pingboard tem as permissões de administrador e tente a **"Ligação de teste"** passo novamente.

7) Introduza o endereço de e-mail de uma pessoa ou grupo que deve receber notificações de erro de aprovisionamento no **correio eletrónico de notificação** campo e marque a caixa de verificação seguinte.

8) Clique em **Guardar**. 

9) Na secção mapeamentos, selecione **sincronizar utilizadores do Azure Active Directory para Pingboard**.

10) No **mapeamentos de atributos** secção, reveja os atributos de utilizador a ser sincronizado do Azure Active Directory para Pingboard. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no Pingboard para operações de atualização. Selecione o **guardar** botão para consolidar as alterações. Para obter mais informações, consulte [personalizar aprovisionamento atributo mapeamentos de utilizador](active-directory-saas-customizing-attribute-mappings.md).

11) Para ativar o Azure Active Directory aprovisionamento do serviço para Pingboard, altere o **estado de aprovisionamento** para **no** no **definições** secção

12) Clique em **guardar** para iniciar a sincronização inicial de utilizadores atribuídos a Pingboard.

Iniciais demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 20 minutos, desde que o serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para os relatórios de atividade, que descrevem a todas as ações efetuadas pelo serviço de aprovisionamento na sua aplicação Pingboard de aprovisionamento.

Para obter mais informações sobre como ler o Azure Active Directory registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos Adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](active-directory-enterprise-apps-manage-provisioning.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Configurar o início de sessão único](active-directory-saas-pingboard-tutorial.md)
