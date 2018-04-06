---
title: Guia do administrador para o SSO do diretório de Active Directory do Azure Plug-in | Microsoft Docs
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Jira/Confluence.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: jeedes
ms.openlocfilehash: d34ff6021816c73fb064a3ce73b7fcf3ae22dbd1
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="admin-guide-for-the-azure-active-directory-sso-plug-in"></a>Guia do administrador para o SSO do diretório de Active Directory do Azure Plug-in

## <a name="overview"></a>Descrição geral

O Azure Active Directory (Azure AD)-início de sessão único (SSO) Plug-in permite que os clientes do Microsoft Azure AD utilizar a respetiva conta escolar ou profissional para iniciar sessão no Atlassian Jira e baseadas em servidor Confluence produtos. Implementa baseados em SAML 2.0 SSO.

## <a name="how-it-works"></a>Como funciona

Quando os utilizadores querem iniciar sessão aplicação Atlassian Jira ou Confluence, verá o **início de sessão com o Azure AD** botão na página de início de sessão. Quando selecionarem, se estiver a necessário para iniciar sessão utilizando a Azure AD organização-página sessão (ou seja, a conta escolar ou profissional).

Depois dos utilizadores estarem autenticados, deverá conseguir iniciar sessão para a aplicação. Se já são autenticadas com o ID e a palavra-passe para a respetiva conta escolar ou profissional, em seguida, estes diretamente a iniciar sessão na aplicação. 

Início de sessão a funciona em Jira e Confluence. Se os utilizadores tem sessão iniciados para a aplicação de Jira e Confluence é aberto na mesma janela de browser, não têm de fornecer as credenciais para a outra aplicação. 

Os utilizadores também podem obter o produto Atlassian através de aplicações My sob a conta escolar ou profissional. Deve ser terminada sem a ser-lhe pedidas credenciais.

> [!NOTE]
> Aprovisionamento de utilizador não é efetuado através de plug-in.

## <a name="audience"></a>Audiência

Administradores Jira e Confluence podem utilizar o plug-in para ativar a SSO ao utilizar o Azure AD.

## <a name="assumptions"></a>Pressupostos

* As instâncias de Jira e Confluence são ativadas por HTTPS.
* Os utilizadores já são criados no Jira ou Confluence.
* Os utilizadores têm funções atribuídas no Jira ou Confluence.
* Administradores tem acesso a informações necessárias para configurar o plug-in.
* Jira ou Confluence está disponível fora da rede da empresa.
* O plug-in funciona com apenas a versão no local do Jira e Confluence.

## <a name="prerequisites"></a>Pré-requisitos

Antes de instalar o plug-in, tenha em atenção as seguintes informações:

* Jira e Confluence estão instalados numa versão do Windows de 64 bits.
* Versões Jira e Confluence são ativadas por HTTPS.
* Jira e Confluence estão disponíveis na internet.
* Credenciais de administrador são para Jira e Confluence.
* Credenciais de administrador são no local para o Azure AD.
* WebSudo está desativado na Jira e Confluence.

## <a name="supported-versions-of-jira-and-confluence"></a>Versões suportadas do Jira e Confluence

O plug-in suporta as seguintes versões do Jira e Confluence:

* Jira núcleos e do Software: 6.0 para 7.2.0
* Suporte técnico do serviço de Jira: 3.0 para 3.2
* Confluence: 5.0 à versão 5.10

## <a name="installation"></a>Instalação

Para instalar o plug-in, siga estes passos:

1. Iniciar sessão na sua instância Jira ou Confluence como um administrador.
    
2. Aceda à consola de administração do Jira/Confluence e selecione **suplementos**.
    
3. No Marketplace Atlassian, procure **Plug-in do Microsoft SAML SSO**.
 
   A versão adequada do plug-in é apresentado nos resultados da pesquisa.
 
5. Selecione o plug-in e instala-o do Gestor de plug-in Universal (UPM).
 
Depois do plug-in estiver instalado, é apresentado no **utilizador instalado suplementos** secção **gerir suplementos**.
    
## <a name="plug-in-configuration"></a>Configuração de plug-in

Antes de começar a utilizar o plug-in, tem de configurá-lo. Selecione o plug-in, selecione o **configurar** botão e forneça os detalhes de configuração.

A imagem seguinte mostra o ecrã de configuração no Jira e Confluence:
    
![Ecrã de configuração de plug-in](./media/ms-confluence-jira-plugin-adminguide/jira.png)

*   **URL de metadados**: O URL para obter os metadados de Federação do Azure AD.
 
*   **Identificadores**: O URL que o Azure AD utiliza para validar a origem do pedido. Mapeia para o **identificador** elemento no Azure AD. O plug-in automaticamente deriva este URL como https://*< domínio: porta >*/.
 
*   **URL de resposta**: O URL de resposta no seu fornecedor de identidade (IdP), que inicia o SAML início de sessão. Mapeia para o **URL de resposta** elemento no Azure AD. O plug-in automaticamente deriva este URL como https://*< domínio: porta >*/plugins/servlet/saml/auth.
 
*   **URL de início de sessão**: O URL de início de sessão na sua IdP que inicia o SAML início de sessão. Mapeia para o **sessão** elemento no Azure AD. O plug-in automaticamente deriva este URL como https://*< domínio: porta >*/plugins/servlet/saml/auth.
 
*   **ID de entidade IdP**: O ID de entidade que utiliza o IdP. Esta caixa é preenchida quando o URL de metadados está resolvido.
 
*   **URL de início de sessão**: O URL de início de sessão da sua IdP. Esta caixa é preenchida do Azure AD quando o URL de metadados está resolvido.
 
*   **URL de fim de sessão**: O URL de fim de sessão da sua IdP. Esta caixa é preenchida do Azure AD quando o URL de metadados está resolvido.
 
*   **Certificado x. 509**: certificado de x. 509 do IdP. Esta caixa é preenchida do Azure AD quando o URL de metadados está resolvido.
 
*   **Nome do botão de início de sessão**: O nome do botão de início de sessão que a organização quer que os utilizadores vejam na página de início de sessão.
 
*   **Localizações de ID de utilizador de SAML**: A localização onde o ID de utilizador Jira ou Confluence é esperado na resposta SAML. Pode ser na **NameID** ou um nome de atributo personalizado.
 
*   **Nome de atributo**: O nome do atributo onde é esperado o ID de utilizador.
 
*   **Ativar a deteção de Realm inicial**: A seleção, certifique-se a empresa estiver a utilizar serviços de Federação do Active Directory (AD FS) - baseados em sessão - no.
 
*   **Nome de domínio**: O nome de domínio se início de sessão do AD FS com base.
 
*   **Ativar o único Signout**: Certifique-se de que pretende terminar a sessão do Azure AD quando um utilizador termina a sessão de Jira ou Confluence a seleção.

## <a name="troubleshooting"></a>Resolução de problemas

* **A obter vários erros de certificado**: iniciar sessão Azure AD e remova os vários certificados que estão disponíveis em relação a aplicação. Certifique-se de que apenas um certificado está presente.

* **Um certificado está prestes a expirar no Azure AD**: suplementos asseguramos rollover automático do certificado. Quando um certificado está prestes a expirar, um novo certificado deve ser marcado como ativo e certificados não utilizados devem ser eliminados. Quando um utilizador tenta iniciar sessão para Jira neste cenário, os plug-in fetches e guarda o novo certificado.

* **Pretende desativar WebSudo (desativar a sessão de administrador segura)**:
    
  * Para Jira, proteja administrador sessões (ou seja, confirmação de palavra-passe antes de aceder a funções de administração) estão ativadas por predefinição. Se pretende remover esta capacidade na sua instância Jira, especificar a seguinte linha no seu ficheiro jira Properties: `ira.websudo.is.disabled = true`
    
  * Para Confluence, siga os passos no [site de suporte de Confluence](https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html).

* **Os campos que são deveria ser preenchido pelo URL de metadados não estão a obter povoados**:
    
  * Verifique se o URL está correto. Verifique se tem a mapear o ID de inquilino e de aplicação correto
    
  * Introduza o URL no browser e veja se receber o XML dos metadados de Federação.

* **Erro interno do servidor**: Reveja os registos no diretório de registo da instalação. Se estiver a obter o erro quando o utilizador está a tentar iniciar sessão utilizando o SSO do Azure AD, pode partilhar os registos com a equipa de suporte.

* **Há um erro de "ID de utilizador não foi encontrado" quando o utilizador tentar iniciar sessão**: crie o ID de utilizador no Jira ou Confluence.

* **Há um erro "Aplicação não encontrada" no Azure AD**: ver se o URL adequado está mapeado para a aplicação no Azure AD.

* **É necessário suporte**: entrar o [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). A equipa responde no 48 de 24 horas de expediente.
    
  Também pode emitir um pedido de suporte com a Microsoft através do canal de portal do Azure.