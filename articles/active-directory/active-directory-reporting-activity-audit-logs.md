---
title: Relatórios de atividade de auditoria no portal do Azure Active Directory | Microsoft Docs
description: Introdução aos relatórios de atividade de auditoria no portal do Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/31/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 97ea32a1e0f8815accff6201251771ab8c088859
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a>Relatório de atividade de auditoria no portal do Azure Active Directory 

Com os relatórios no Azure Active Directory (Azure AD), obtenha todas as informações de que precisa para determinar o estado de funcionamento do seu ambiente.

A arquitetura de relatórios no Azure AD consiste nos seguintes componentes:

- **Atividade** 
    - **Atividades de início de sessão** – Informações sobre a utilização de aplicações geridas e atividades de início de sessão do utilizador
    - **Registos de auditoria** - informações de atividades do sistema sobre gestão de utilizadores e de grupos, as suas aplicações geridas e atividades de diretório.
- **Segurança** 
    - **Inícios de sessão de risco** – Um início de sessão de risco é um indicador de uma tentativa de início de sessão que pode ter sido efetuada por alguém que não é o proprietário legítimo de uma conta de utilizador. Para obter mais detalhes, veja Inícios de sessão de risco.
    - **Utilizadores sinalizados para risco** – Um utilizador de risco é um indicador de uma conta de utilizador que pode ter sido comprometida. Para obter mais detalhes, veja Utilizadores sinalizados para risco.

Este tópico fornece-lhe uma descrição geral das atividades de auditoria.
 
## <a name="who-can-access-the-data"></a>Quem pode aceder aos dados?
* Utilizadores na função de Administrador de Segurança ou de Leitor de Segurança
* Administradores Globais
* Os utilizadores individuais (não administradores) podem ver as suas próprias atividades


## <a name="audit-logs"></a>Registos de auditoria

Os registos de auditoria no Azure Active Directory fornecem registos das atividades de sistema para efeitos de conformidade.  
O primeiro ponto de entrada para todos os dados de auditoria é **Registos de auditoria** na secção **Atividade** do **Azure Active Directory**.

![Registos de auditoria](./media/active-directory-reporting-activity-audit-logs/61.png "Registos de auditoria")

Um registo de auditoria tem uma vista de lista predefinida que mostra:

- a data e hora da ocorrência
- o iniciador / ator (*quem*) de uma atividade 
- a atividade (*o quê*) 
- o destino

![Registos de auditoria](./media/active-directory-reporting-activity-audit-logs/18.png "Registos de auditoria")

Pode personalizar a vista de lista ao clicar em **Colunas** na barra de ferramentas.

![Registos de auditoria](./media/active-directory-reporting-activity-audit-logs/19.png "Registos de auditoria")

Isto permite-lhe apresentar campos adicionais ou remover campos que já são apresentados.

![Registos de auditoria](./media/active-directory-reporting-activity-audit-logs/21.png "Registos de auditoria")


Ao clicar num item na vista de lista, obtém todos os detalhes sobre o mesmo.

![Registos de auditoria](./media/active-directory-reporting-activity-audit-logs/22.png "Registos de auditoria")


## <a name="filtering-audit-logs"></a>Filtrar registos de auditoria

Para limitar os dados comunicados a um nível que funcione para si, pode filtrar os dados de auditoria através dos seguintes campos:

- Intervalo de datas
- Iniciado por (Actor)
- Categoria
- Tipo de recurso de atividade
- Atividade

![Registos de auditoria](./media/active-directory-reporting-activity-audit-logs/23.png "Registos de auditoria")


O filtro **intervalo de datas** permite-lhe definir um período de tempo para os dados devolvidos.  
Os valores possíveis são:

- 1 mês
- 7 dias
- 24 horas
- Personalizado

Quando selecionar um período de tempo personalizado, pode configurar uma hora de início e uma hora de fim.

O filtro **iniciado por** permite-lhe definir o nome de um ator ou o respetivo nome principal universal (UPN).

O filtro **categoria** permite-lhe selecionar um dos seguintes filtros:

- Todos
- Categoria principal
- Diretório principal
- Gestão de palavras-passe personalizada
- Gestão de grupos self-service
- Aprovisionamento da conta - substituição automatizada de palavra-passe
- Utilizadores convidados
- Serviço MIM
- Identity Protection
- B2C

O filtro **tipo de recurso de atividade** permite-lhe selecionar um dos seguintes filtros:

- Todos 
- Grupo
- Diretório
- Utilizador
- Aplicação
- Política
- Dispositivo
- Outros

Quando seleciona **Grupo** como o **tipo de recurso de atividade**, obtém uma categoria de filtro adicional que lhe permite também indicar uma **Origem**:

- Azure AD
- O365


O filtro **atividade** baseia-se na categoria e na seleção de Tipo de recurso de atividade que fizer. Pode selecionar uma atividade específica que queira ver ou selecionar todas. 

Pode obter a lista de todas as Atividades de Auditoria com a Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, em que $tenantdomain é o nome do seu domínio, ou ver o artigo [Audit report events](active-directory-reporting-audit-events.md) (Eventos de relatórios de auditoria).


## <a name="audit-logs-shortcuts"></a>Atalhos dos registos de auditoria

Para além do **Azure Active Directory**, o portal do Azure proporciona-lhe dois pontos de entrada adicionais para dados de auditoria:

- Utilizadores e grupos
- Aplicações Empresariais

### <a name="users-and-groups-audit-logs"></a>Registos de auditoria de utilizadores e grupos

Com os relatórios de auditoria baseados em utilizadores e grupos, poderá obter respostas a perguntas como:

- Que tipos de atualizações os utilizadores aplicaram?

- Quantos utilizadores foram alterados?

- Quantas palavras-passe foram alteradas?

- O que fez um administrador num diretório?

- Quais são os grupos que foram adicionados?

- Existem grupos com as alterações na associação?

- Os proprietários do grupo foram alterados?

- Que licenças foram atribuídas a um grupo ou utilizador?

Se quiser apenas rever dados de auditoria que estejam relacionados com utilizadores e grupos, pode encontrar um vista filtrada em **Registos de auditoria**, na secção **Atividade** de **Utilizadores e Grupos**. Este ponto de entrada tem **Utilizadores e Grupos** como o **Tipo de Recurso de Atividade** pré-selecionado.

![Registos de auditoria](./media/active-directory-reporting-activity-audit-logs/93.png "Registos de auditoria")

### <a name="enterprise-applications-audit-logs"></a>Registos de auditoria de aplicações empresariais

Com os relatórios de auditoria baseados em aplicações, poderá obter respostas a perguntas como:

* Quais são as aplicações que foram adicionadas ou atualizadas?
* Quais são as aplicações que foram removidas?
* Um principal de serviço para uma aplicação foi alterado?
* Os nomes das aplicações foram alterados?
* Quem autorizou uma aplicação?

Se quiser apenas rever dados de auditoria que estejam relacionados com aplicações, pode encontrar um vista filtrada em **Registos de auditoria**, na secção **Atividade** do painel **Aplicações empresariais**. Este ponto de entrada tem **Aplicações Empresariais** como o **Tipo de Recurso de Atividade** pré-selecionado.

![Registos de auditoria](./media/active-directory-reporting-activity-audit-logs/134.png "Registos de auditoria")

Pode filtrar ainda mais esta vista para apenas **grupos** ou **utilizadores**.

![Registos de auditoria](./media/active-directory-reporting-activity-audit-logs/25.png "Registos de auditoria")



## <a name="azure-ad-audit-activity-list"></a>Lista de atividades de auditoria do Azure AD

Esta secção disponibiliza-lhe uma lista de todas as atividades que podem ser registadas. 


|Nome do Serviço|Categoria de Auditoria|Tipo de Recurso de Atividade|Atividade|
|---|---|---|---|
|Aprovisionamento de Contas|Gestão de Aplicações|Aplicação|Administração|
|Aprovisionamento de Contas|Gestão de Aplicações|Aplicação|Operação de diretório|
|Aprovisionamento de Contas|Gestão de Aplicações|Aplicação|Exportar|
|Aprovisionamento de Contas|Gestão de Aplicações|Aplicação|Importar|
|Aprovisionamento de Contas|Gestão de Aplicações|Aplicação|Outros|
|Aprovisionamento de Contas|Gestão de Aplicações|Aplicação|Depósito de processos|
|Aprovisionamento de Contas|Gestão de Aplicações|Aplicação|Ação de regra de sincronização|
|Proxy da aplicação|Gestão de Aplicações|Aplicação|Adicionar aplicação|
|Proxy da aplicação|Recurso|Recurso|Adicionar certificado SSL da aplicação|
|Proxy da aplicação|Recurso|Recurso|Eliminar enlace SSL|
|Proxy da aplicação|Gestão de Aplicações|Aplicação|Eliminar aplicação|
|Proxy da aplicação|Gestão de Diretórios|Diretório|Desativar SSO de Ambiente de Trabalho|
|Proxy da aplicação|Gestão de Diretórios|Diretório|Desativar o SSO de Ambiente de Trabalho num domínio específico|
|Proxy da aplicação|Gestão de Diretórios|Diretório|Desativar proxy de aplicações|
|Proxy da aplicação|Gestão de Diretórios|Diretório|Desativar autenticação pass-through|
|Proxy da aplicação|Gestão de Diretórios|Diretório|Ativar SSO de Ambiente de Trabalho|
|Proxy da aplicação|Gestão de Diretórios|Diretório|Ativar o SSO de Ambiente de Trabalho num domínio específico|
|Proxy da aplicação|Gestão de Diretórios|Diretório|Ativar o proxy de aplicações|
|Proxy da aplicação|Gestão de Diretórios|Diretório|Ativar a autenticação pass-through|
|Proxy da aplicação|Recurso|Recurso|Registar conector|
|Proxy da aplicação|Gestão de Aplicações|Aplicação|Atualizar a aplicação|
|Proxy da aplicação|Gestão de Aplicações|Aplicação|Atualizar o Modo de Início de Sessão Única de aplicação|
|Rollover de Palavra-passe Automatizada|Gestão de Aplicações|Aplicação|Rollover de Palavra-passe Automatizada|
|B2C|Gestão de Aplicações|Aplicação|Adicionar permissões de aplicação V2|
|B2C|Autorização|Autorização|Adicionar permissões de aplicação V2|
|B2C|Chave|Chave|Adicionar uma chave com base no segredo ASCII a um contentor de chaves CPIM|
|B2C|Autorização|Autorização|Adicionar uma chave com base no segredo ASCII a um contentor de chaves CPIM|
|B2C|Chave|Chave|Adicionar uma chave a um contentor de chaves CPIM|
|B2C|Autorização|Autorização|Adicionar uma chave a um contentor de chaves CPIM|
|B2C|Recurso|Recurso|AdminPolicyDatas-RemoveResources|
|B2C|Autorização|Autorização|AdminPolicyDatas-SetResources|
|B2C|Recurso|Recurso|AdminPolicyDatas-SetResources|
|B2C|Recurso|Recurso|AdminUserJourneys-GetResources|
|B2C|Autorização|Autorização|AdminUserJourneys-GetResources|
|B2C|Autorização|Autorização|AdminUserJourneys-RemoveResources|
|B2C|Recurso|Recurso|AdminUserJourneys-RemoveResources|
|B2C|Recurso|Recurso|AdminUserJourneys-SetResources|
|B2C|Autorização|Autorização|AdminUserJourneys-SetResources|
|B2C|Autorização|Autorização|Criar IdentityProvider|
|B2C|Recurso|Recurso|Criar IdentityProvider|
|B2C|Autorização|Autorização|Criar aplicação V1|
|B2C|Gestão de Aplicações|Aplicação|Criar aplicação V1|
|B2C|Gestão de Aplicações|Aplicação|Criar aplicação V2|
|B2C|Autorização|Autorização|Criar aplicação V2|
|B2C|Gestão de Diretórios|Diretório|Criar domínios personalizados no inquilino|
|B2C|Autorização|Autorização|Criar domínios personalizados no inquilino|
|B2C|Autorização|Autorização|Criar um novo AdminUserJourney|
|B2C|Recurso|Recurso|Criar um novo AdminUserJourney|
|B2C|Recurso|Recurso|Criar json de recurso localizado|
|B2C|Autorização|Autorização|Criar json de recurso localizado|
|B2C|Autorização|Autorização|Criar IDP Personalizado novo|
|B2C|Recurso|Recurso|Criar IDP Personalizado novo|
|B2C|Recurso|Recurso|Criar novo IDP|
|B2C|Autorização|Autorização|Criar novo IDP|
|B2C|Autorização|Autorização|Criar ou atualizar um recurso de diretório B2C|
|B2C|Recurso|Recurso|Criar ou atualizar um recurso de diretório B2C|
|B2C|Recurso|Recurso|Criar política|
|B2C|Autorização|Autorização|Criar política|
|B2C|Autorização|Autorização|Criar política trustFramework|
|B2C|Recurso|Recurso|Criar política trustFramework|
|B2C|Autorização|Autorização|Criar política trustFramework com prefixo configurável|
|B2C|Recurso|Recurso|Criar política trustFramework com prefixo configurável|
|B2C|Recurso|Recurso|Criar atributo de utilizador|
|B2C|Autorização|Autorização|Criar atributo de utilizador|
|B2C|Autorização|Autorização|CreateTrustFrameworkPolicy|
|B2C|Recurso|Recurso|CreateTrustFrameworkPolicy|
|B2C|Autorização|Autorização|Cria ou Atualiza um novo AdminUserJourney|
|B2C|Recurso|Recurso|Eliminar IDP|
|B2C|Autorização|Autorização|Eliminar IDP|
|B2C|Recurso|Recurso|Eliminar IdentityProvider|
|B2C|Autorização|Autorização|Eliminar IdentityProvider|
|B2C|Gestão de Aplicações|Aplicação|Eliminar aplicação V1|
|B2C|Autorização|Autorização|Eliminar aplicação V1|
|B2C|Gestão de Aplicações|Aplicação|Eliminar aplicação V2|
|B2C|Autorização|Autorização|Eliminar aplicação V2|
|B2C|Autorização|Autorização|Eliminar concessão de permissões de aplicação V2|
|B2C|Gestão de Aplicações|Aplicação|Eliminar concessão de permissões de aplicação V2|
|B2C|Recurso|Recurso|Eliminar um recurso de diretório B2C|
|B2C|Autorização|Autorização|Eliminar um recurso de diretório B2C|
|B2C|Chave|Chave|Eliminar um contentor de chaves CPIM|
|B2C|Autorização|Autorização|Eliminar um contentor de chaves CPIM|
|B2C|Chave|Chave|Eliminar contentor de chaves|
|B2C|Recurso|Recurso|Eliminar política trustFramework|
|B2C|Autorização|Autorização|Eliminar política trustFramework|
|B2C|Recurso|Recurso|Eliminar atributo de utilizador|
|B2C|Autorização|Autorização|Eliminar atributo de utilizador|
|B2C|Autorização|Autorização|Ativar funcionalidade B2C|
|B2C|Gestão de Diretórios|Diretório|Ativar funcionalidade B2C|
|B2C|Recurso|Recurso|Obter recursos de diretório B2C num grupo de recursos|
|B2C|Recurso|Recurso|Obter recursos de diretório B2C numa subscrição|
|B2C|Autorização|Autorização|Obter recursos de diretório B2C numa subscrição|
|B2C|Autorização|Autorização|Obter IDP Personalizado|
|B2C|Recurso|Recurso|Obter IDP Personalizado|
|B2C|Recurso|Recurso|Obter IDP|
|B2C|Autorização|Autorização|Obter IDP|
|B2C|Autorização|Autorização|Obter aplicações V1 e V2|
|B2C|Gestão de Aplicações|Aplicação|Obter aplicações V1 e V2|
|B2C|Autorização|Autorização|Obter aplicação V1|
|B2C|Gestão de Aplicações|Aplicação|Obter aplicação V1|
|B2C|Autorização|Autorização|Obter aplicações V1|
|B2C|Gestão de Aplicações|Aplicação|Obter aplicações V1|
|B2C|Gestão de Aplicações|Aplicação|Obter aplicação V2|
|B2C|Autorização|Autorização|Obter aplicação V2|
|B2C|Gestão de Aplicações|Aplicação|Obter aplicações V2|
|B2C|Autorização|Autorização|Obter aplicações V2|
|B2C|Recurso|Recurso|Obter um recurso de diretório B2C|
|B2C|Autorização|Autorização|Obter um recurso de diretório B2C|
|B2C|Autorização|Autorização|Obter uma lista de domínios personalizados no inquilino|
|B2C|Gestão de Diretórios|Diretório|Obter uma lista de domínios personalizados no inquilino|
|B2C|Autorização|Autorização|Obter um percurso do utilizador|
|B2C|Recurso|Recurso|Obter um percurso do utilizador|
|B2C|Recurso|Recurso|Obter afirmações de aplicação permitidas para percurso do utilizador|
|B2C|Autorização|Autorização|Obter afirmações de aplicação permitidas para percurso do utilizador|
|B2C|Recurso|Recurso|Obter afirmações de declaração própria permitidas para percurso do utilizador|
|B2C|Autorização|Autorização|Obter afirmações de declaração própria permitidas para percurso do utilizador|
|B2C|Recurso|Recurso|Obter afirmações de declaração própria permitidas da política|
|B2C|Autorização|Autorização|Obter afirmações de declaração própria permitidas da política|
|B2C|Recurso|Recurso|Obter lista de afirmações de saída disponíveis|
|B2C|Autorização|Autorização|Obter lista de afirmações de saída disponíveis|
|B2C|Recurso|Recurso|Obter definições de conteúdo para percurso do utilizador|
|B2C|Autorização|Autorização|Obter definições de conteúdo para percurso do utilizador|
|B2C|Autorização|Autorização|Obter idps para um fluxo de administração específico|
|B2C|Recurso|Recurso|Obter idps para um fluxo de administração específico|
|B2C|Chave|Chave|Obter metadados de chaves ativas do contentor de chaves em JWK|
|B2C|Autorização|Autorização|Obter metadados de chaves ativas do contentor de chaves em JWK|
|B2C|Chave|Chave|Obter metadados do contentor de chaves|
|B2C|Recurso|Recurso|Obter a lista de todos os fluxos de administração|
|B2C|Autorização|Autorização|Obter a lista de todos os fluxos de administração|
|B2C|Autorização|Autorização|Obter a lista de etiquetas de todos os fluxos de administração de todos os utilizadores|
|B2C|Recurso|Recurso|Obter a lista de etiquetas de todos os fluxos de administração de todos os utilizadores|
|B2C|Recurso|Recurso|Obter a lista de inquilinos de um utilizador|
|B2C|Autorização|Autorização|Obter a lista de inquilinos de um utilizador|
|B2C|Recurso|Recurso|Obter afirmações de declaração própria de contas locais|
|B2C|Autorização|Autorização|Obter afirmações de declaração própria de contas locais|
|B2C|Recurso|Recurso|Obter json de recurso localizado|
|B2C|Autorização|Autorização|Obter json de recurso localizado|
|B2C|Autorização|Autorização|Obter operações do fornecedor de recursos Microsoft.AzureActiveDirectory|
|B2C|Recurso|Recurso|Obter operações do fornecedor de recursos Microsoft.AzureActiveDirectory|
|B2C|Recurso|Recurso|Obter políticas|
|B2C|Autorização|Autorização|Obter políticas|
|B2C|Recurso|Recurso|Obter política|
|B2C|Autorização|Autorização|Obter política|
|B2C|Gestão de Diretórios|Diretório|Obter propriedades de recurso de um inquilino|
|B2C|Autorização|Autorização|Obter propriedades de recurso de um inquilino|
|B2C|Recurso|Recurso|Obter a lista de IDPs suportados|
|B2C|Autorização|Autorização|Obter a lista de IDPs suportados|
|B2C|Recurso|Recurso|Obter a lista de IDPs suportados do percurso do utilizador|
|B2C|Autorização|Autorização|Obter a lista de IDPs suportados do percurso do utilizador|
|B2C|Gestão de Diretórios|Diretório|Obter Informações do inquilino|
|B2C|Autorização|Autorização|Obter Informações do inquilino|
|B2C|Gestão de Diretórios|Diretório|Obter funcionalidades do inquilino permitidas|
|B2C|Autorização|Autorização|Obter funcionalidades do inquilino permitidas|
|B2C|Autorização|Autorização|Obter lista de IDPs Personalizados definidos para o inquilino|
|B2C|Recurso|Recurso|Obter lista de IDPs Personalizados definidos para o inquilino|
|B2C|Recurso|Recurso|Obter lista de IDPs definidos para o inquilino|
|B2C|Autorização|Autorização|Obter lista de IDPs definidos para o inquilino|
|B2C|Recurso|Recurso|Obter lista de IDPs locais definidos para o inquilino|
|B2C|Autorização|Autorização|Obter lista de IDPs locais definidos para o inquilino|
|B2C|Recurso|Recurso|Obter os detalhes do inquilino de um utilizador para criação de recurso|
|B2C|Autorização|Autorização|Obter os detalhes do inquilino de um utilizador para criação de recurso|
|B2C|Autorização|Autorização|Obter a lista de inquilinos|
|B2C|Autorização|Autorização|Obter tenantDomains|
|B2C|Gestão de Diretórios|Diretório|Obter tenantDomains|
|B2C|Recurso|Recurso|Obter a cultura predefinida suportada para CPIM|
|B2C|Autorização|Autorização|Obter a cultura predefinida suportada para CPIM|
|B2C|Recurso|Recurso|Obter os detalhes de um fluxo de administração|
|B2C|Autorização|Autorização|Obter os detalhes de um fluxo de administração|
|B2C|Autorização|Autorização|Obter a lista de UserJourneys deste inquilino|
|B2C|Recurso|Recurso|Obter a lista de UserJourneys deste inquilino|
|B2C|Autorização|Autorização|Obter o conjunto de culturas disponíveis suportadas para CPIM|
|B2C|Recurso|Recurso|Obter o conjunto de culturas disponíveis suportadas para CPIM|
|B2C|Autorização|Autorização|Obter política trustFramework|
|B2C|Recurso|Recurso|Obter política trustFramework|
|B2C|Autorização|Autorização|Obter política trustFramework com xml|
|B2C|Recurso|Recurso|Obter política trustFramework com xml|
|B2C|Recurso|Recurso|Obter atributo do utilizador|
|B2C|Autorização|Autorização|Obter atributo do utilizador|
|B2C|Autorização|Autorização|Obter atributos do utilizador|
|B2C|Recurso|Recurso|Obter atributos do utilizador|
|B2C|Autorização|Autorização|Obter a lista de percursos do utilizador|
|B2C|Recurso|Recurso|Obter a lista de percursos do utilizador|
|B2C|Autorização|Autorização|GetIEFPolicies|
|B2C|Recurso|Recurso|GetIEFPolicies|
|B2C|Autorização|Autorização|GetIdentityProviders|
|B2C|Recurso|Recurso|GetIdentityProviders|
|B2C|Recurso|Recurso|GetTrustFrameworkPolicy|
|B2C|Autorização|Autorização|GetTrustFrameworkPolicy|
|B2C|Chave|Chave|Obtém um contentor de chaves CPIM no formato jwk|
|B2C|Autorização|Autorização|Obtém um contentor de chaves CPIM no formato jwk|
|B2C|Chave|Chave|Obtém a lista de contentores de chaves no inquilino|
|B2C|Autorização|Autorização|Obtém a lista de contentores de chaves no inquilino|
|B2C|Autorização|Autorização|Obtém o tipo de inquilino|
|B2C|Gestão de Diretórios|Diretório|Obtém o tipo de inquilino|
|B2C|Autenticação|Autenticação|Emitir um token de acesso para a aplicação|
|B2C|Autenticação|Autenticação|Emitir um código de autorização para a aplicação|
|B2C|Outros|Outros|Emitir um código de autorização para a aplicação|
|B2C|Autenticação|Autenticação|Emitir um token de acesso id_token para a aplicação|
|B2C|Outros|Outros|Emitir um token de acesso id_token para a aplicação|
|B2C|Autorização|Autorização|MigrateTenantMetadata|
|B2C|Recurso|Recurso|MigrateTenantMetadata|
|B2C|Recurso|Recurso|Mover recursos|
|B2C|Autorização|Autorização|Corrigir IdentityProvider|
|B2C|Recurso|Recurso|Corrigir IdentityProvider|
|B2C|Recurso|Recurso|PutTrustFrameworkPolicy|
|B2C|Autorização|Autorização|PutTrustFrameworkPolicy|
|B2C|Autorização|Autorização|PutTrustFrameworkpolicy|
|B2C|Recurso|Recurso|PutTrustFrameworkpolicy|
|B2C|Recurso|Recurso|Remover um percurso do utilizador|
|B2C|Autorização|Autorização|Remover um percurso do utilizador|
|B2C|Autorização|Autorização|Restaurar uma cópia de segurança do contentor de chaves CPIM|
|B2C|Chave|Chave|Restaurar uma cópia de segurança do contentor de chaves CPIM|
|B2C|Gestão de Aplicações|Aplicação|Obter concessões de permissões de aplicação V2|
|B2C|Autorização|Autorização|Obter concessões de permissões de aplicação V2|
|B2C|Gestão de Aplicações|Aplicação|Obter principais de serviço de aplicação V2 no inquilino atual|
|B2C|Autorização|Autorização|Obter principais de serviço de aplicação V2 no inquilino atual|
|B2C|Chave|Chave|Guardar o contentor de chaves|
|B2C|Autorização|Autorização|Atualizar o IDP Personalizado|
|B2C|Recurso|Recurso|Atualizar o IDP Personalizado|
|B2C|Recurso|Recurso|Atualizar IDP|
|B2C|Autorização|Autorização|Atualizar IDP|
|B2C|Recurso|Recurso|Atualizar IDP Local|
|B2C|Autorização|Autorização|Atualizar IDP Local|
|B2C|Gestão de Aplicações|Aplicação|Atualizar aplicação V1|
|B2C|Autorização|Autorização|Atualizar aplicação V1|
|B2C|Gestão de Aplicações|Aplicação|Atualizar aplicação V2|
|B2C|Autorização|Autorização|Atualizar aplicação V2|
|B2C|Gestão de Aplicações|Aplicação|Atualizar concessão de permissões de aplicação V2|
|B2C|Autorização|Autorização|Atualizar concessão de permissões de aplicação V2|
|B2C|Recurso|Recurso|Atualizar um recurso de diretório B2C|
|B2C|Recurso|Recurso|Atualizar política|
|B2C|Autorização|Autorização|Atualizar política|
|B2C|Recurso|Recurso|Atualizar estado da subscrição|
|B2C|Recurso|Recurso|Atualizar atributo de utilizador|
|B2C|Autorização|Autorização|Atualizar atributo de utilizador|
|B2C|Chave|Chave|Carregar uma chave encriptada CPIM|
|B2C|Autorização|Autorização|Carregar uma chave encriptada CPIM|
|B2C|Autorização|Autorização|Autorização de Utilizador: a API está desativada para featureset de inquilino|
|B2C|Autorização|Autorização|Autorização de Utilizador: foi concedido acesso ao utilizador como “Administrador do Inquilino”|
|B2C|Autorização|Autorização|Autorização de Utilizador: foi concedido ao utilizador direitos de acesso “Utilizadores Autenticados”|
|B2C|Autenticação|Autenticação|Validar as credenciais da conta local|
|B2C|Recurso|Recurso|Validar movimentação de recursos|
|B2C|Autenticação|Autenticação|Validar autenticação de utilizador|
|B2C|Gestão de Diretórios|Diretório|Verifique se a funcionalidade B2C está ativada|
|B2C|Autorização|Autorização|Verifique se a funcionalidade B2C está ativada|
|B2C|Autorização|Autorização|Verifique se a funcionalidade está ativada|
|B2C|Gestão de Diretórios|Diretório|Verifique se a funcionalidade está ativada|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Adicionar OAuth2PermissionGrant|
|Diretório do Núcleo|Gestão de Unidade Administrativa|AdministrativeUnit|Add administrative unit|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Adicionar concessão de atribuição de função de aplicação ao utilizador|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Adicionar atribuição de função de aplicação ao grupo|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Adicionar atribuição de função de aplicação ao principal de serviço|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Adicionar aplicação|
|Diretório do Núcleo|Recurso|Recurso|Adicionar dispositivo|
|Diretório do Núcleo|Recurso|Recurso|Adicionar configuração do dispositivo|
|Diretório do Núcleo|Gestão de Funções|Função|Adicionar membro elegível à função|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Adicionar grupo|
|Diretório do Núcleo|Gestão de Unidade Administrativa|AdministrativeUnit|Adicionar membro a unidade administrativa|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Adicionar membro ao grupo|
|Diretório do Núcleo|Gestão de Funções|Função|Adicionar membro à função|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Adicionar proprietário à aplicação|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Adicionar proprietário ao grupo|
|Diretório do Núcleo|Gestão de Políticas|Política|Adicionar proprietário à política|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Adicionar proprietário ao principal de serviço|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Adicionar parceiro à empresa|
|Diretório do Núcleo|Gestão de Políticas|Política|Adicionar política|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Adicionar política ao principal de serviço|
|Diretório do Núcleo|Recurso|Recurso|Adicionar proprietário registado ao dispositivo|
|Diretório do Núcleo|Recurso|Recurso|Adicionar utilizadores registados ao dispositivo|
|Diretório do Núcleo|Gestão de Funções|Função|Adicionar atribuição de função à definição de função|
|Diretório do Núcleo|Gestão de Funções|Função|Adicionar função a partir de modelo|
|Diretório do Núcleo|Gestão de Funções|Função|Adicionar membro com âmbito à função|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Adicionar principal de serviço|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Adicionar credenciais de principal de serviço|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Adicionar domínio não verificado|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Adicionar utilizador|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Adicionar detalhe de aplicação de telemóvel de autenticação forte de utilizadores|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Adicionar domínio verificado|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Alterar licença de utilizador|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Alterar palavra-passe do utilizador|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Autorizar aplicação|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Converter utilizador federado em gerido|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Criar palavra-passe de aplicação para o utilizador|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Criar empresa|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Criar definições da empresa|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Criar definições de grupo|
|Diretório do Núcleo|Gestão de Unidade Administrativa|AdministrativeUnit|Eliminar unidade administrativa|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Eliminar aplicação|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Eliminar palavra-passe de aplicação do utilizador|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Eliminar definições da empresa|
|Diretório do Núcleo|Recurso|Recurso|Eliminar dispositivo|
|Diretório do Núcleo|Recurso|Recurso|Eliminar configuração do dispositivo|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Eliminar grupo|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Eliminar definições do grupo|
|Diretório do Núcleo|Gestão de Políticas|Política|Eliminar política|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Eliminar utilizador|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Despromover parceiro|
|Diretório do Núcleo|Recurso|Recurso|O dispositivo já não está em conformidade|
|Diretório do Núcleo|Recurso|Recurso|O dispositivo já não está a ser gerido|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Diretório eliminado|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Diretório eliminado definitivamente|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Diretório agendado para eliminação|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Desativar conta|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Ativar a Autenticação Forte|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Concluir a aplicação de licença baseada em grupo aos utilizadores|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Eliminação completa da aplicação|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Eliminação completa do grupo|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Eliminação completa do utilizador|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Promover empresa a parceiro|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Purgar propriedades de gestão de direitos|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Remover OAuth2PermissionGrant|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Remover atribuição de função de aplicação do grupo|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Remover atribuição de função de aplicação do principal de serviço|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Remover atribuição de função de aplicação do utilizador|
|Diretório do Núcleo|Gestão de Funções|Função|Remover membro elegível da função|
|Diretório do Núcleo|Gestão de Unidade Administrativa|AdministrativeUnit|Remover membro de unidade administrativa|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Remover membro do grupo|
|Diretório do Núcleo|Gestão de Funções|Função|Remover membro de função|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Remover proprietário da aplicação|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Remover proprietário de grupo|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Remover proprietário do principal de serviço|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Remover parceiro da empresa|
|Diretório do Núcleo|Gestão de Políticas|Política|Remover credenciais de política|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Remover política do principal de serviço|
|Diretório do Núcleo|Recurso|Recurso|Remover proprietário registado do dispositivo|
|Diretório do Núcleo|Recurso|Recurso|Remover utilizadores registados do dispositivo|
|Diretório do Núcleo|Gestão de Funções|Função|Remover atribuição de função da definição de função|
|Diretório do Núcleo|Gestão de Funções|Função|Remover membro com âmbito da função|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Remover principal de serviço|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Remover credenciais de principal de serviço|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Remover domínio não verificado|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Remover detalhe de aplicação de telemóvel de autenticação forte de utilizadores|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Remover domínio verificado|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Repor palavra-passe do utilizador|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Restaurar Grupo|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Restaurar aplicação|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Restaurar utilizador|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Revogar autorização|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir Informações da Empresa|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir funcionalidade Dirsync|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir o sinalizador Dirsyncenabled|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir Parceria|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir limiar de eliminação acidental|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir localização dos dados permitida da empresa|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir funcionalidade multinacional da empresa ativada|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir funcionalidade de diretório no inquilino|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir autenticação de domínio|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Configurar definições de federação em domínio|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Forçar alteração de palavra-passe do utilizador|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Definir licença de grupo|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Definir grupo para ser gerido pelo utilizador|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir política de palavras-passe|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Definir propriedades de gestão de direitos|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Definir gestor de utilizador|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Definir metadados de token de autorização de utilizadores ativado|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Começar a aplicar licença baseada em grupo aos utilizadores|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Acionar recálculo de licença de grupo|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Atualizar StsRefreshTokenValidFrom Timestamp|
|Diretório do Núcleo|Gestão de Unidade Administrativa|AdministrativeUnit|Atualizar unidade administrativa|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Atualizar a aplicação|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Atualizar empresa|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Atualizar definições da empresa|
|Diretório do Núcleo|Recurso|Recurso|Atualizar o dispositivo|
|Diretório do Núcleo|Recurso|Recurso|Atualizar configuração do dispositivo|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Atualizar domínio|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Atualizar segredos externos|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Atualizar segredos externos|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Atualizar grupo|
|Diretório do Núcleo|Gestão de Grupos|Grupo|Atualizar definições de grupo|
|Diretório do Núcleo|Gestão de Políticas|Política|Atualizar política|
|Diretório do Núcleo|Gestão de Funções|Função|Atualizar função|
|Diretório do Núcleo|Gestão de Aplicações|Aplicação|Atualizar principal de serviço|
|Diretório do Núcleo|Gestão de Utilizadores|Utilizador|Atualizar utilizador|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Verificar domínio|
|Diretório do Núcleo|Gestão de Diretórios|Diretório|Verificar domínio verificado por e-mail|
|Identity Protection|Gestão de Utilizadores|Utilizador|O administrador gera uma palavra-passe temporária|
|Identity Protection|Gestão de Utilizadores|Utilizador|Os administradores exigem que o utilizador reponha a respetiva palavra-passe|
|Identity Protection|Outros|Outros|Transferir um tipo de eventos de risco único|
|Identity Protection|Outros|Outros|Transferir administradores e estado de aceitação de resumo semanal|
|Identity Protection|Outros|Outros|Transferir todos os tipos de eventos de risco|
|Identity Protection|Outros|Outros|Transferir eventos sem risco do utilizador|
|Identity Protection|Outros|Outros|Transferir utilizadores sinalizados para risco|
|Identity Protection|Gestão de Diretórios|Diretório|Inclusão|
|Identity Protection|Gestão de Políticas|Política|Definir a política de registo de MFA|
|Identity Protection|Gestão de Políticas|Política|Definir política de risco de início de sessão|
|Identity Protection|Gestão de Políticas|Política|Definir política de risco de utilizador|
|Identity Protection|Gestão de Diretórios|Diretório|Atualizar definições de alerta|
|Identity Protection|Gestão de Diretórios|Diretório|Atualizar definições de resumo semanal|
|Utilizadores Convidados|Gestão de Utilizadores|Utilizador|Atribuir utilizador externo à aplicação|
|Utilizadores Convidados|Outros|Outros|Convites em lote processados|
|Utilizadores Convidados|Outros|Outros|Convites em lote carregados|
|Utilizadores Convidados|Gestão de Utilizadores|Utilizador|E-mail não enviado; subscrição do utilizador anulada|
|Utilizadores Convidados|Gestão de Utilizadores|Utilizador|Convidar utilizador externo|
|Utilizadores Convidados|Gestão de Utilizadores|Utilizador|Resgatar convite de utilizador externo|
|Utilizadores Convidados|Gestão de Utilizadores|Utilizador|Criação de inquilinos viral|
|Utilizadores Convidados|Gestão de Utilizadores|Utilizador|Criação de utilizador viral|
|Microsoft Identity Manager (MIM)|Gestão de Grupos|Grupo|Adicionar Membro|
|Microsoft Identity Manager (MIM)|Gestão de Grupos|Grupo|Criar Grupo|
|Microsoft Identity Manager (MIM)|Gestão de Grupos|Grupo|Eliminar Grupo|
|Microsoft Identity Manager (MIM)|Gestão de Grupos|Grupo|Remover Membro|
|Microsoft Identity Manager (MIM)|Gestão de Grupos|Grupo|Atualizar Grupo|
|Microsoft Identity Manager (MIM)|Gestão de Utilizadores|Utilizador|Registo de Palavras-passe do Utilizador|
|Microsoft Identity Manager (MIM)|Gestão de Utilizadores|Utilizador|Reposição de Palavra-passe do Utilizador|
|Privileged Identity Management|Gestão de Funções|Função|AccessReview_Review|
|Privileged Identity Management|Gestão de Funções|Função|AccessReview_Update|
|Privileged Identity Management|Gestão de Funções|Função|ActivationAborted|
|Privileged Identity Management|Gestão de Funções|Função|ActivationApproved|
|Privileged Identity Management|Gestão de Funções|Função|ActivationCanceled|
|Privileged Identity Management|Gestão de Funções|Função|ActivationRequested|
|Privileged Identity Management|Gestão de Funções|Função|Adicionou|
|Privileged Identity Management|Gestão de Funções|Função|Atribuir|
|Privileged Identity Management|Gestão de Funções|Função|Elevar|
|Privileged Identity Management|Gestão de Funções|Função|Removido|
|Privileged Identity Management|Gestão de Funções|Função|Alterações das definições da função|
|Privileged Identity Management|Gestão de Funções|Função|ScanAlertsNow|
|Privileged Identity Management|Gestão de Funções|Função|Inscrever-se|
|Privileged Identity Management|Gestão de Funções|Função|Anular elevação|
|Privileged Identity Management|Gestão de Funções|Função|UpdateAlertSettings|
|Privileged Identity Management|Gestão de Funções|Função|UpdateCurrentState|
|Gestão de Grupos Personalizada|Gestão de Grupos|Grupo|Aprovar um pedido pendente para aderir a um grupo|
|Gestão de Grupos Personalizada|Gestão de Grupos|Grupo|Cancelar um pedido pendente para aderir a um grupo|
|Gestão de Grupos Personalizada|Gestão de Grupos|Grupo|Criar política de gestão de ciclo de vida|
|Gestão de Grupos Personalizada|Gestão de Grupos|Grupo|Eliminar um pedido pendente para aderir a um grupo|
|Gestão de Grupos Personalizada|Gestão de Grupos|Grupo|Rejeitar um pedido pendente para aderir a um grupo|
|Gestão de Grupos Personalizada|Gestão de Grupos|Grupo|Renovar grupo|
|Gestão de Grupos Personalizada|Gestão de Grupos|Grupo|Pedido para aderir a um grupo|
|Gestão de Grupos Personalizada|Gestão de Grupos|Grupo|Definir propriedades de grupo dinâmicas|
|Gestão de Grupos Personalizada|Gestão de Grupos|Grupo|Atualizar política de gestão de ciclo de vida|
|Gestão de Palavra-passe Personalizada|Gestão de Utilizadores|Utilizador|Bloqueado da reposição de palavras-passe personalizada|
|Gestão de Palavra-passe Personalizada|Gestão de Utilizadores|Utilizador|Alterar palavra-passe (personalizada)|
|Gestão de Palavra-passe Personalizada|Gestão de Diretórios|Diretório|Desativar a repetição de escrita de palavras-passe no diretório|
|Gestão de Palavra-passe Personalizada|Gestão de Diretórios|Diretório|Ativar a repetição de escrita de palavras-passe no diretório|
|Gestão de Palavra-passe Personalizada|Gestão de Utilizadores|Utilizador|Repor palavra-passe (por administrador)|
|Gestão de Palavra-passe Personalizada|Gestão de Utilizadores|Utilizador|Repor palavra-passe (personalizada)|
|Gestão de Palavra-passe Personalizada|Gestão de Utilizadores|Utilizador|Progresso da atividade de fluxo de reposição de palavras-passe personalizada|
|Gestão de Palavra-passe Personalizada|Gestão de Utilizadores|Utilizador|Progresso da atividade de fluxo de reposição de palavras-passe personalizada|
|Gestão de Palavra-passe Personalizada|Gestão de Utilizadores|Utilizador|Desbloquear conta de utilizador (personalizado)|
|Gestão de Palavra-passe Personalizada|Gestão de Utilizadores|Utilizador|Utilizador registado na reposição de palavra-passe personalizada|
|Termos de Utilização|Gestão de Políticas|Política|Editar os Termos de Utilização|
|Termos de Utilização|Gestão de Políticas|Política|Criar os Termos de Utilização|
|Termos de Utilização|Gestão de Políticas|Política|Rejeitar Termos de Utilização|
|Termos de Utilização|Gestão de Políticas|Política|Eliminar Termos de Utilização|
|Termos de Utilização|Gestão de Políticas|Política|Editar Termos de Utilização|
|Termos de Utilização|Gestão de Políticas|Política|Publicar Termos de Utilização|
|Termos de Utilização|Gestão de Políticas|Política|Anular publicação dos Termos de Utilização|




## <a name="next-steps"></a>Passos seguintes

Para obter uma descrição geral dos relatórios, veja [Relatórios do Azure Active Directory](active-directory-reporting-azure-portal.md).

