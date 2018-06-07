---
title: Pré-requisitos para aceder ao Azure Active Directory reporting API | Microsoft Docs
description: Saiba mais sobre os pré-requisitos para o Azure AD API do relatório de acesso
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/07/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: a38447150dd283ff21a852690cfd48477472f7b8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34588765"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Pré-requisitos para aceder ao Azure Active Directory API do relatório

As [APIs de relatórios Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) proporcionam acesso programático aos dados através de um conjunto de APIs baseadas em REST. Pode chamar estas APIs a partir de várias linguagens e ferramentas de programação.

A Reporting Services utiliza API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) para autorizar o acesso a web APIs.

Para preparar o seu acesso à API do Reporting Services, tem de:

1. Atribuir funções
2. Registar uma aplicação
3. Conceder permissões
4. Recolher as definições de configuração



## <a name="assign-roles"></a>Atribuir funções

Para obter acesso aos dados de relatórios através da API, terá de ter uma das seguintes funções atribuídas:

- Leitor de Segurança

- Administrador de Segurança

- Administrador global




## <a name="register-an-application"></a>Registar uma aplicação

Tem de registar uma aplicação, mesmo que está a aceder a API de relatórios através de um script. Isto dá-lhe um **ID da aplicação**, que é necessário para uma chamada de autorização e permite que o código receber tokens.

Para configurar o seu diretório para o Azure AD API do relatório de acesso, tem de iniciar sessão portal do Azure com uma conta de administrador do Azure que também seja membro do **Administrador Global** função de diretório no seu inquilino do Azure AD.

> [!IMPORTANT]
> As aplicações em execução sob credenciais com privilégios de "admin" como este podem ser muito poderosas, por isso, certifique-se a manter segura a credenciais de ID/segredo da aplicação.
> 


**Para registar uma aplicação do Azure Active Directory:**

1. No [portal do Azure](https://portal.azure.com), no painel de navegação esquerdo, clique em **do Azure Active Directory**.
   
    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. No **do Azure Active Directory** página, clique em **registos de aplicação**.

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. No **registos de aplicação** página, na barra de ferramentas na parte superior, clique em **novo registo de aplicação**.

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. No **criar** página, execute os seguintes passos:

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    a. No **nome** caixa de texto, tipo `Reporting API application`.

    b. Como **tipo de aplicação**, selecione **aplicação Web / API**.

    c. No **URL de início de sessão** caixa de texto, tipo `https://localhost`.

    d. Clique em **Criar**. 


## <a name="grant-permissions"></a>Conceder permissões 

Dependendo da API que pretende aceder, tem de conceder a aplicação as seguintes permissões:  

| API | Permissão |
| --- | --- |
| Windows Azure Active Directory | Ler dados do diretório |
| Microsoft Graph | Ler que todos os dados de registo de auditoria |


![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/36.png)


A secção seguinte apresenta os passos para ambas as APIs. Se não quiser aceder a uma das APIs, pode ignorar os passos relacionados.
 

**Para conceder as permissões de aplicação para utilizar as APIs:**

1. No **registos de aplicação** página, na lista de aplicações, clique em **aplicação API de relatórios**.

2. No **aplicação API de relatórios** página, na barra de ferramentas na parte superior, clique em **definições**. 

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. No **definições** página, clique em **as permissões necessárias**. 

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. No **as permissões necessárias** na página de **API** lista, clique em **Windows Azure Active Directory**. 

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. No **ativar o acesso ao** página, selecione **ler os dados de diretório** e, desmarque **iniciar sessão e ler o perfil de utilizador**. 

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. Na barra de ferramentas na parte superior, clique em **guardar**.

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

7. No **as permissões necessárias** página, na barra de ferramentas na parte superior, clique em **adicionar**.

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/32.png)

8. No **acesso à API adicionar** página, clique em **selecionar um API**.

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/31.png)

9. No **selecionar um API** página, clique em **Microsoft Graph**e, em seguida, clique em **selecione**.

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/33.png)

10. No **ativar o acesso ao** página, selecione **ler todos os dados de registo de auditoria**e, em seguida, clique em **selecione**.  

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/34.png)


11. No **acesso à API adicionar** página, clique em **feito**.  

12. No **as permissões necessárias** página, na barra de ferramentas na parte superior. Clique em **conceder permissões**e, em seguida, clique em **Sim**.

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/17.png)


## <a name="gather-configuration-settings"></a>Recolher as definições de configuração 

Esta secção mostra como obter as seguintes definições do diretório:

- Nome de domínio
- ID de Cliente
- Segredo do cliente

Necessitar destes valores quando configurar chamadas à API do Reporting Services. 

### <a name="get-your-domain-name"></a>Obter o nome de domínio

**Para obter o seu nome de domínio:**

1. No [portal do Azure](https://portal.azure.com), no painel de navegação esquerdo, clique em **do Azure Active Directory**.
   
    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. No **do Azure Active Directory** página, clique em **nomes de domínio personalizado**.

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. Copie o seu nome de domínio na lista de domínios.


### <a name="get-your-applications-client-id"></a>Obter ID de cliente da aplicação

**Para obter o ID de cliente da aplicação:**

1. No [portal do Azure](https://portal.azure.com), no painel de navegação esquerdo, clique em **do Azure Active Directory**.
   
    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. No **registos de aplicação** página, na lista de aplicações, clique em **aplicação API de relatórios**.

3. No **aplicação API de relatórios** página, à **ID da aplicação**, clique em **clique para copiar**.

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a>Obter o segredo do cliente da aplicação
Para obter o segredo do cliente da sua aplicação, terá de criar uma nova chave e guarde o respetivo valor após guardar a nova chave porque não é possível obter este valor já mais tarde.

**Para obter o segredo do cliente da aplicação:**

1. No [portal do Azure](https://portal.azure.com), no painel de navegação esquerdo, clique em **do Azure Active Directory**.
   
    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. No **registos de aplicação** página, na lista de aplicações, clique em **aplicação API de relatórios**.


3. No **aplicação API de relatórios** página, na barra de ferramentas na parte superior, clique em **definições**. 

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. No **definições** na página de **APIR acesso** secção, clique em **chaves**. 

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. No **chaves** página, execute os seguintes passos:

    ![Registar aplicação](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    a. No **Descrição** caixa de texto, tipo `Reporting API`.

    b. Como **expira**, selecione **nos 2 anos**.

    c. Clique em **Guardar**.

    d. Copie o valor da chave.


## <a name="next-steps"></a>Próximos Passos

- [Obter dados com o Azure Active Directory API do relatório com certificados](active-directory-reporting-api-with-certificates.md).

- [Obter uma primeira impressão das APIs de relatórios](active-directory-reporting-api-getting-started-azure-portal.md#explore)

- [Criar a sua própria solução](active-directory-reporting-api-getting-started-azure-portal.md#customize)

