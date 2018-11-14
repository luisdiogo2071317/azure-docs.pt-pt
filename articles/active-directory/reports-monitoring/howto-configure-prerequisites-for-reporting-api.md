---
title: Pré-requisitos para aceder ao Azure Active Directory reporting API | Documentos da Microsoft
description: Saiba mais sobre os pré-requisitos para aceder à API de relatórios do Azure AD
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f72d15707d9f56b9e9b5a5d527d1204007c40afa
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621977"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Pré-requisitos para aceder ao Azure Active Directory reporting API

As [APIs de relatórios Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) proporcionam acesso programático aos dados através de um conjunto de APIs baseadas em REST. Pode chamar estas APIs a partir de várias linguagens e ferramentas de programação.

As utilizações de API de geração de relatórios [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) para autorizar o acesso a web APIs.

Para preparar o acesso para a API de relatórios, terá de:

1. [Atribuir funções](#assign-roles)
2. [Registar uma aplicação](#register-an-application)
3. [Conceder permissões](#grant-permissions)
4. [Recolher as definições de configuração](#gather-configuration-settings)

## <a name="assign-roles"></a>Atribuir funções

Para obter acesso aos dados dos relatórios por meio da API, tem de ter uma das seguintes funções atribuídas:

- Leitor de Segurança

- Administrador de Segurança

- Administrador Global


## <a name="register-an-application"></a>Registar uma aplicação

Tem de registar uma aplicação, mesmo que está acessando a API de relatórios através de um script. Isso lhe dá uma **ID da aplicação**, que é necessário para a autorização chama e permite o seu código para receber tokens.

Para configurar o diretório para aceder à API de relatórios do Azure AD, tem de iniciar sessão para o [portal do Azure](https://portal.azure.com) com uma conta de administrador do Azure que também seja membro da **Administrador Global** função de diretório no seu inquilino do Azure AD.

> [!IMPORTANT]
> Aplicações em execução sob credenciais com privilégios de administrador podem ser muito eficientes, por isso, não se esqueça de manter o aplicativo ID e segredas credenciais numa localização segura.
> 

**Para registar uma aplicação do Azure AD:**

1. Na [portal do Azure](https://portal.azure.com), selecione **Azure Active Directory** no painel de navegação esquerdo.
   
    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Na **do Azure Active Directory** página, selecione **registos das aplicações**.

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Partir do **registos de aplicações** página, selecione **novo registo de aplicação**.

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. Na **criar** página, execute os seguintes passos:

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. Na **Name** caixa de texto, tipo `Reporting API application`.

    b. Como **tipo de aplicação**, selecione **aplicação Web / API**.

    c. Na **URL de início de sessão** caixa de texto, tipo `https://localhost`.

    d. Selecione **Criar**. 


## <a name="grant-permissions"></a>Conceder permissões 

Dependendo da API que pretende aceder, tem de conceder a aplicação as seguintes permissões:  

| API | Permissão |
| --- | --- |
| Windows Azure Active Directory | Ler dados do diretório |
| Microsoft Graph | Ler que todos os dados de registo de auditoria |


![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/36.png)

A seção a seguir lista os passos para ambas as APIs. Se não quiser aceder a uma das APIs, pode ignorar os passos relacionados.

**Para conceder as permissões de aplicação para utilizar as APIs:**

1. Selecione a aplicação a partir da **registos de aplicações** página e selecione **definições**. 

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. Sobre o **configurações** página, selecione **permissões obrigatórias**. 

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. No **permissões obrigatórias** página, além do **API** , clique em **Windows Azure Active Directory**. 

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. Na **ativar o acesso ao** página, selecione **ler dados do diretório** e, desmarcar **iniciar sessão e ler o perfil de utilizador**. 

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Na barra de ferramentas na parte superior, clique em **guardar**.

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/15.png)

6. Sobre o **permissões obrigatórias** página, na barra de ferramentas na parte superior, clique em **Add**.

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/32.png)

7. Sobre o **adicionar acesso à API** página, clique em **selecionar uma API**.

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/31.png)

8. Sobre o **selecionar uma API** página, clique em **Microsoft Graph**e, em seguida, clique em **selecionar**.

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/33.png)

9. Sobre o **ativar o acesso ao** página, selecione **ler todos os dados de registo de auditoria**e, em seguida, clique em **selecionar**.  

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/34.png)

10. Sobre o **adicionar acesso à API** página, clique em **feito**.  

11. Sobre o **permissões obrigatórias** página, na barra de ferramentas na parte superior. Clique em **conceder permissões**e, em seguida, clique em **Sim**.

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/17.png)


## <a name="gather-configuration-settings"></a>Recolher as definições de configuração 

Esta secção mostra como obter as seguintes definições do diretório:

- Nome de domínio
- ID de Cliente
- Segredo do cliente

Vai precisar destes valores quando configurar chamadas para a API de relatórios. 

### <a name="get-your-domain-name"></a>Obter o seu nome de domínio

**Para obter o seu nome de domínio:**

1. Na [portal do Azure](https://portal.azure.com), no painel de navegação esquerdo, selecione **Azure Active Directory**.
   
    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Sobre o **do Azure Active Directory** página, selecione **nomes de domínio personalizado**.

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Copie o seu nome de domínio na lista de domínios.


### <a name="get-your-applications-client-id"></a>Obter ID de cliente da sua aplicação

**Para obter o ID de cliente da sua aplicação:**

1. Na [portal do Azure](https://portal.azure.com), no painel de navegação esquerdo, clique em **Azure Active Directory**.
   
    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Selecione a aplicação a partir da **registos das aplicações** página.

3. A partir da página da aplicação, navegue para **ID da aplicação** e selecione **clique para copiar**.

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Obter segredo do cliente da sua aplicação
Para obter segredo do cliente da sua aplicação, terá de criar uma nova chave e guarde o respetivo valor ao guardar a nova chave, porque não é possível obter este valor deixa de poder mais tarde.

**Para obter segredo do cliente da sua aplicação:**

1. Na [portal do Azure](https://portal.azure.com), no painel de navegação esquerdo, clique em **Azure Active Directory**.
   
    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Selecione a aplicação a partir da **registos das aplicações** página.

3. Na página de aplicativo, na barra de ferramentas na parte superior, selecione **definições**. 

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/05.png)

4. No **definições** página, além do **acesso à API** secção, clique em **chaves**. 

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. Sobre o **chaves** página, execute os seguintes passos:

    ![Registar aplicação](./media/howto-configure-prerequisites-for-reporting-api/14.png)

    a. Na **Descrição** caixa de texto, tipo `Reporting API`.

    b. Como **Expires**, selecione **dentro de 2 anos**.

    c. Clique em **Guardar**.

    d. Copie o valor da chave.


## <a name="next-steps"></a>Passos Seguintes

* [Utilizar o Azure Active Directory reporting API com certificados para obter dados](tutorial-access-api-with-certificates.md)
* [Referência da API de auditoria](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Relatório de atividade de início de sessão de referência da API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
