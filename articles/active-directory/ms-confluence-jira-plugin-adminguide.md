---
title: "Microsoft Azure Active Directory único início de sessão Plug-in administração guia | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e do Microsoft Azure Active Directory-início de sessão único para JIRA."
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
ms.openlocfilehash: af949d1db8af37a534a16364f9f0763479c436e4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="microsoft-azure-active-directory-single-sign-on-plugin-admin-guide"></a>Microsoft Azure Active Directory único início de sessão Plug-in administração manual

## <a name="table-of-contents"></a>Índice

1. **[OVERVIEW](#overview)**
2. **[COMO FUNCIONA](#how-it-works)**
3. **[AUDIENCE](#audience)**
4. **[ASSUMPTIONS](#assumptions)**
5. **[PREREQUISITES](#prerequisites)**
6. **[VERSÕES SUPORTADAS DO JIRA E CONFLUENCE](#supported-versions-of-jira-and-confluence)**
7. **[INSTALLATION](#installation)**
8. **[CONFIGURAÇÃO DE PLUG-IN](#plugin-configuration)**
9. **[EXPLICAÇÃO DE CAMPO PARA O ECRÃ DE CONFIGURAÇÃO DO SUPLEMENTO:](#field-explanation-for-add---on-configuration-screen:)**
10. **[TROUBLESHOOTING](#troubleshooting)**

## <a name="overview"></a>Descrição geral

Estes suplementos ativar os clientes do Microsoft Azure AD utilizar o respetivo nome de utilizador da organização e a palavra-passe para início de sessão para o Atlassian Jira e Confluence servidor baseado em produtos. Implementa SAML 2.0 com base SSO.

## <a name="how-it-works"></a>Como funciona

Quando os utilizadores querem iniciar sessão na aplicação Atlassian Jira ou Confluence, verá o **início de sessão com o Azure AD** botão na página de início de sessão. Quando clicam na mesma, são necessárias para iniciar sessão com a página de início de sessão do Azure AD organização.

Assim que os utilizadores estarem autenticados, deverá conseguir iniciar sessão na aplicação. Se já são autenticadas com o ID de organização e a palavra-passe, em seguida, iniciam diretamente na aplicação. Além disso, tenha em atenção de que início de sessão desse funciona em JIRA e Confluence. Se os utilizadores são registados numa aplicação JIRA e Confluence também está aberta na mesma janela de browser, têm de iniciar sessão uma vez e não tem de fornecer as credenciais novamente para outra aplicação. Os utilizadores também podem obter o produto Atlassian através de myapps sob a conta do Azure e devem ter sessão iniciadas sem a ser pedidas as credenciais.

> [!NOTE]
> Aprovisionamento de utilizador não é efetuado através deste suplemento.

## <a name="audience"></a>Audiência

Administradores JIRA e Confluence que estiver a planear utilizar este plug-in para ativar a SSO em utilizar o Azure AD.

## <a name="assumptions"></a>Pressupostos

* A instância de JIRA/Confluence é ativadas por HTTPS
* Os utilizadores já são criados no JIRA/Confluence
* Os utilizadores têm a função atribuída no JIRA/Confluence
* Administradores tem acesso a informações necessárias para configurar o plug-in
* JIRA/Confluence estão disponíveis fora da rede da empresa
* Adicionar no funciona com apenas a versão no local de JIRA e confluence

## <a name="prerequisites"></a>Pré-requisitos

Tenha em atenção os seguintes pré-requisitos antes de avançar continuar com a instalação do suplemento:

* JIRA/Confluence estão instalados numa versão do Windows de 64 bits
* Versões JIRA/Confluence são ativadas por HTTPS
* Tenha em atenção a versão suportada para o plug-in na secção "Versões suportadas" abaixo.
* JIRA/Confluence está disponível na internet.
* Credenciais de administrador para JIRA/Confluence
* Credenciais de administrador para o Azure AD
* WebSudo deve ser desativado na JIRA e confluence

## <a name="supported-versions-of-jira-and-confluence"></a>Versões suportadas do JIRA e Confluence

A partir de agora, são suportadas as seguintes versões do JIRA e Confluence:

* JIRA núcleos e do Software: 6.0 para 7.2.0
* JIRA serviço para o suporte técnico: 3.0 para 3.2
* Confluence: 5.0 à versão 5.10

## <a name="installation"></a>Instalação

Administrador deve seguir os passos indicados abaixo para instalar o plug-in:

1. Inicie sessão na sua instância JIRA/Confluence como administrador
    
2. Aceda a administração de JIRA/Confluence e clique em suplementos.
    
3. De Atlassian Marketplace, procure **Microsoft SAML SSO Plug-in**
 
4. Versão adequada do suplemento é apresentada na pesquisa
 
5. Selecione o plug-in e UPM instala o mesmo.
 
6. Assim que o plug-in estiver instalado, é apresentado na secção de suplementos utilizador instalado da secção de suplemento gerir
 
7. Tem de configurar o plug-in antes de começar a utilizá-la.
 
8. Clique no plug-in e ver um botão de configurar.
 
9. Clique nele para fornecer entradas de configuração
    
## <a name="plugin-configuration"></a>Configuração de plug-in

Imagem seguinte mostra o ecrã de configuração do suplemento JIRA e Confluence
    
![configuração do suplemento](./media/ms-confluence-jira-plugin-adminguide/jira.png)

### <a name="field-explanation-for-add-on-configuration-screen"></a>Explicação de campo para o ecrã de configuração do suplemento:

1.   URL de metadados: URL para obter os metadados de Federação a partir do Azure AD
 
2.   Identificador: Utilizados pelo Azure AD para validar a origem do pedido. Isto é mapeado para o elemento identificador no Azure AD. Este é automaticamente derivada pelo plug-in como https://<domain:port>/
 
3.   URL de resposta: URL de resposta de utilização na sua IdP para iniciar o início de sessão SAML. Isto é mapeado para o elemento de URL de resposta no Azure AD. Este é automaticamente derivada pelo plug-in como https://<domain:port>/plugins/servlet/saml/auth
 
4.   URL de início de sessão: Utilização no URL de sessão na sua IdP para iniciar o início de sessão SAML. Isto é mapeado para o elemento de início de sessão no Azure AD. Este é automaticamente derivada pelo plug-in como https://<domain:port>/plugins/servlet/saml/auth
 
5.   ID de entidade IdP: O ID de entidade que utiliza o IdP. Este é preenchido quando o URL de metadados for resolvido.
 
6.   URL de início de sessão: O URL de início de sessão da sua IdP. Este é preenchido do Azure AD quando o URL de metadados está resolvido.
 
7.   URL para terminar sessão: O URL de fim de sessão da sua IdP. Este é preenchido do Azure AD quando o URL de metadados está resolvido.
 
8.   Certificado x. 509: Certificado de x. 509 o IdP. Este é preenchido do Azure AD quando o URL de metadados está resolvido.
 
9.   Nome do botão de início de sessão: Nome do botão de início de sessão a organização pretende ver. Este texto é apresentado aos utilizadores no botão de início de sessão no ecrã de início de sessão.
 
10.   Localizações do ID de utilizador SAML: Onde o id de utilizador é esperado na resposta SAML. Este pode ser NameID ou um nome de atributo personalizado. Este ID tem de ser o id de utilizador JIRA/Confluence.
 
11.   O nome do atributo: nome do atributo em que pode ser esperado da Id de utilizador.
 
12.   Deteção de Realm inicial de ativar: Verificar este sinalizador, se a empresa utilizando o início de sessão baseado no AD FS.
 
13.   Nome de domínio: Forneça o nome de domínio aqui em caso do início de sessão baseado no AD FS
 
14.   Ativar início de sessão único limite: verificar este office se de que pretende terminar a sessão do Azure AD quando um utilizador terminar a sessão de JIRA/Confluence.

### <a name="troubleshooting"></a>Resolução de problemas

* Se estiver a receber vários erros de certificados
    
    * Início de sessão para o Azure AD e remova os certificados de múltiplas disponíveis contra a aplicação. Certifique-se de que existe apenas um certificado presente.

* Certificado está prestes a expirar no Azure AD.
    
    * Suplementos asseguramos rollover automático do certificado. Quando um certificado está prestes a expirar, o novo certificado deve ser marcado como ativo e deve ser eliminado o certificado não utilizados. Quando um utilizador tenta iniciar sessão para JIRA neste cenário, o suplemento obtém o novo certificado e guarde no plug-in.

* Como desativar WebSudo (desativar a sessão de administrador segura)
    
    * JIRA: Proteja administrador sessões (ou seja, confirmação de palavra-passe antes de aceder a funções de administração) estão ativadas por predefinição. Se pretender desativar esta na sua instância JIRA, pode desativar esta funcionalidade, especificando a seguinte linha no ficheiro de jira Properties: "ira.websudo.is.disabled = true"
    
    * Confluence: Siga os passos fornecidos indicado no seguinte URL https://confluence.atlassian.com/doc/configuring-secure-administrator-sessions-218269595.html

* Os campos que são deveria ser preenchido pelo URL de metadados não são obter preenchidos
    
    * Verifique se o URL está correto. Verifique se tem de mapear o inquilino correto e o id de aplicação.
    
    * Atingiu o URL do browser e veja se estão a receber o XML dos metadados de Federação.

* Erro interno do servidor:
    
    * Percorra os registos presentes no diretório registos da instalação. Se estiver a receber o erro quando o utilizador está a tentar iniciar sessão utilizando o SSO do Azure AD, pode partilhar os registos com as informações de suporte fornecidas abaixo neste documento.

* ID de utilizador não foi encontrado erro quando o utilizador tenta iniciar sessão
    
    * Utilizador não é criado na JIRA/Confluence, por isso, crie o mesmo.

* Aplicação não foi encontrada erro no Azure AD
    
    * Ver se o URL adequado está mapeado para a aplicação no Azure AD.

* Detalhes do suporte: entrar-nos em: [equipa de integração do Azure AD SSO](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Iremos responder dentro de 24 48 horas de expediente.
    
    * Também pode emitir um pedido de suporte com a Microsoft através do canal de portal do Azure.