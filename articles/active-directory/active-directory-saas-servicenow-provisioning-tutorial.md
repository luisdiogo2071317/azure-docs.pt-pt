---
title: 'Tutorial: Configurar o ServiceNow para aprovisionamento de utilizadores automática no Azure Active Directory | Microsoft Docs'
description: Saiba como aprovisionar e anular o aprovisionamento contas de utilizador do Azure AD para ServiceNow automaticamente.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: d3e64cc232aa01a5619a55290f9b07421165a6d5
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698735"
---
# <a name="tutorial-configure-servicenow-for-automatic-user-provisioning-with-azure-active-directory"></a>Tutorial: Configurar o ServiceNow para aprovisionamento de utilizadores automática no Azure Active Directory

O objetivo deste tutorial é mostrar-lhe os passos que necessários para efetuar do ServiceNow e AD do Azure para aprovisionar e anular o aprovisionamento contas de utilizador do Azure AD para ServiceNow automaticamente.

> [!NOTE]
> Este tutorial descreve um conector desenvolvido com o serviço de fornecimento de utilizador do Azure AD. Para obter detalhes importantes sobre o que faz este serviço, como funciona e perguntas mais frequentes, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações de SaaS no Azure Active Directory](./active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ServiceNow, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Para o ServiceNow, uma instância ou o inquilino do ServiceNow, versão Calgary ou superior
- Para expressas ServiceNow, uma instância do ServiceNow rápida, versão Helsinki ou superior

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).


## <a name="assigning-users-to-servicenow"></a>Atribuir utilizadores a ServiceNow

Azure Active Directory utiliza um conceito chamado "atribuições de" para determinar quais os utilizadores devem receber acesso às aplicações selecionadas. No contexto de aprovisionamento da conta de utilizador automáticas, apenas os utilizadores e grupos que tenham sido "atribuídos" a uma aplicação no Azure AD é sincronizado.

Antes de configurar e ativar o serviço de aprovisionamento, tem de decidir quais os utilizadores e/ou grupos no Azure AD representam os utilizadores que necessitam de aceder à sua aplicação do ServiceNow. Depois de decidir, pode atribuir estes utilizadores à sua aplicação do ServiceNow ao seguir as instruções aqui: [atribuir um utilizador ou grupo a uma aplicação empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)


> [!IMPORTANT]
>*   Recomenda-se que um único utilizador do Azure AD é atribuído a ServiceNow para testar a configuração de aprovisionamento. Os utilizadores adicionais e/ou grupos podem ser atribuídos mais tarde.
>*   Ao atribuir um utilizador a ServiceNow, tem de selecionar uma função de utilizador válido. A função de "Acesso predefinida" não funciona para o aprovisionamento.

## <a name="enable-automated-user-provisioning"></a>Ativar o aprovisionamento automatizado do utilizador

Esta secção orienta-o de ligar o seu Azure AD a conta de utilizador do ServiceNow aprovisionamento API e configurar o serviço de aprovisionamento para criar, atualizar e desativar as contas de utilizador atribuído do ServiceNow com base na atribuição de utilizadores e grupos no Azure AD.

> [!TIP]
>Também pode optar por ativar baseados em SAML Single Sign-On para ServiceNow, seguindo as instruções fornecidas no [portal do Azure](https://portal.azure.com). O início de sessão único a pode ser configurado independentemente aprovisionamento automático, apesar destas duas funcionalidades complementar dos entre si.

### <a name="configure-automatic-user-account-provisioning"></a>Configurar o aprovisionamento da conta de utilizador automáticas

1. No [portal do Azure](https://portal.azure.com), navegue para o **do Azure Active Directory > aplicações da empresa > todas as aplicações** secção.

2. Se já tiver configurado o ServiceNow para o início de sessão único, procure a instância do ServiceNow utilizando o campo de pesquisa. Caso contrário, selecione **adicionar** e procure **ServiceNow** na Galeria de aplicações. Selecione o ServiceNow dos resultados de pesquisa e adicioná-lo à sua lista de aplicações.

3. Selecione a sua instância do ServiceNow, em seguida, selecione o **aprovisionamento** separador.

4. Definir o **aprovisionamento** modo para **automática**. 

    ![a aprovisionar](./media/active-directory-saas-servicenow-provisioning-tutorial/provisioning.png)

5. Na secção credenciais de administrador, execute os seguintes passos:
   
    a. No **nome da instância do ServiceNow** caixa de texto, escreva o nome de instância do ServiceNow.

    b. No **nome de utilizador de Admin do ServiceNow** caixa de texto, escreva o nome de utilizador do administrador.

    c. No **palavra-passe de administrador de ServiceNow** caixa de texto, palavra-passe do administrador.

6. No portal do Azure, clique em **Testar ligação** para garantir que o Azure AD pode ligar à sua aplicação do ServiceNow. Se a ligação falhar, certifique-se a sua conta ServiceNow tem permissões de administrador de equipa e tente a **"Credenciais de administrador"** passo novamente.

7. Introduza o endereço de e-mail de uma pessoa ou grupo que deve receber notificações de erro de aprovisionamento no **correio eletrónico de notificação** campo e marque a caixa de verificação.

8. Clique em **guardar.**

9. Na secção mapeamentos, selecione **sincronizar utilizadores do Azure Active Directory para ServiceNow.**

10. No **mapeamentos de atributos** secção, reveja os atributos de utilizador que são sincronizados a partir do Azure AD e ServiceNow. Os atributos selecionados como **correspondência** propriedades são utilizadas para corresponder as contas de utilizador no ServiceNow para operações de atualização. Selecione o botão Guardar para consolidar as alterações.

11. Para ativar o Azure AD aprovisionamento do serviço de ServiceNow, altere o **estado de aprovisionamento** para **no** na secção de definições

12. Clique em **guardar.**

Inicia a sincronização inicial de todos os utilizadores e/ou grupos atribuídos a ServiceNow na secção utilizadores e grupos. A sincronização inicial demora mais para efetuar a sincronizações subsequentes, o que ocorrer aproximadamente a cada 40 minutos, desde que o serviço está em execução. Pode utilizar o **detalhes de sincronização** secção para monitorizar o progresso e siga as ligações para aprovisionamento de registos de atividade, que descrevem a todas as ações efetuadas pelo serviço de aprovisionamento na sua aplicação do ServiceNow.

Para obter mais informações sobre como ler o Azure AD, os registos de aprovisionamento, consulte [relatórios sobre o aprovisionamento da conta de utilizador automáticas](active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerir o aprovisionamento da conta de utilizador para aplicações da empresa](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)
* [Configurar o início de sessão único](active-directory-saas-servicenow-tutorial.md)


