---
title: "Configurar o início de sessão automático-aceleração para uma aplicação através de uma política de deteção de Realm Inicial | Microsoft Docs"
description: "Explica que um inquilino do Azure AD é e como gerir o Azure através do Azure Active Directory."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 11/09/2017
ms.author: billmath
ms.openlocfilehash: e2e6e5c40dc4a9f67f94c45f8394512db3f777f5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="configure-sign-in-auto-acceleration-for-an-application-by-using-a-home-realm-discovery-policy"></a>Configurar o início de sessão automático-aceleração para uma aplicação utilizando uma política de deteção de Realm Inicial

O documento seguinte fornece uma introdução para deteção de Realm Inicial e aceleração automática.

## <a name="home-realm-discovery"></a>Deteção de Realm Inicial
Deteção de Realm doméstico (HRD) é o processo que permite que o Azure Active Directory (Azure AD) para determinar, no momento de início de sessão, em que um utilizador tem de autenticar.  Quando um utilizador inicia sessão para um inquilino do Azure AD para aceder a um recurso ou para a página de início de sessão comuns do Azure AD, escreva um nome de utilizador (UPN). Azure AD utiliza esse para detetar em que o utilizador tem de iniciar sessão. 

O utilizador poderá ter de ser direcionado para uma das seguintes localizações sejam autenticados:

- O inquilino doméstico do utilizador (podem ser o mesmo inquilino como o recurso que o utilizador está a tentar aceder). 

- Conta Microsoft.  O utilizador é um convidado no inquilino do recurso.

- Outro fornecedor de identidade que está federada com o inquilino do Azure AD.

-  Um fornecedor de identidade no local, tais como serviços de Federação do Active Directory (AD FS).

## <a name="auto-acceleration"></a>Aceleração automática 
Algumas organizações configurar o seu inquilino do Azure Active Directory a federar com outro IdP, tais como o AD FS para autenticação de utilizador.  

Nestes casos, quando um utilizador iniciar sessão numa aplicação, são primeiro primeiro apresentados com uma página de início de sessão do Azure AD. Depois de que escreveu o UPN, é direcionado para a página de início de sessão do IdP. Em determinadas circunstâncias, os administradores poderão querer direcionar os utilizadores para a página de início de sessão quando está a iniciar sessão em aplicações específicas. 

Isto significa que os utilizadores, podem ignorar a página inicial do Azure Active Directory. Este processo é referido como "início de sessão automático-aceleração."

Nos casos em que o inquilino é federado para outro IdP para início de sessão, aceleração automática faz com que o utilizador iniciar sessão mais está mais simples.  Pode configurar a aceleração de automático para aplicações individuais.

>[!NOTE]
>Se configurar uma aplicação para a aceleração de automática, os utilizadores convidados não podem iniciar sessão. Se um utilizador não efetuar diretamente para um IdP federado para autenticação, não há nenhuma forma para os mesmos voltar para a página de início de sessão no Azure Active Directory. Utilizadores de convidado, o que poderão ter de ser direcionado para outros inquilinos ou uma IdP externo, como uma conta Microsoft, não é possível iniciar sessão para essa aplicação porque que está a ignorar a etapa de deteção de Realm Inicial.  

Existem duas formas de controlar a aceleração de automático para um federado IdP:   

- Utilize uma sugestão de domínio nos pedidos de autenticação para uma aplicação. 
- Configure uma política de deteção de Realm Inicial para ativar a aceleração de automática.

## <a name="domain-hints"></a>Sugestões de domínio 
Sugestões de domínio são as diretivas de que estão incluídas no pedido de autenticação de uma aplicação. Podem ser utilizadas para acelerar o utilizador para os respetivos federada IdP-página sessão. Ou podem ser utilizados por uma aplicação multi-inquilino para acelerar o utilizador diretamente para o com marca corporativa página sessão do Azure AD para o respetivo inquilino.  

Por exemplo, a aplicação "largeapp.com" poderá permitir que os seus clientes aceder a aplicação com um URL personalizado "contoso.largeapp.com." A aplicação também pode incluir uma sugestão de domínio contoso.com do pedido de autenticação. 

Sintaxe de sugestão de domínio varia consoante o protocolo que é utilizado e, normalmente, está configurado na aplicação.

**WS-Federation**: whr=contoso.com na cadeia de consulta.

**SAML**: ou um SAML pedido de autenticação que contém uma sugestão de domínio ou um whr=contoso.com de cadeia de consulta.

**Abrir ID Connect**: um domain_hint=contoso.com de cadeia de consulta. 

Se uma sugestão de domínio está incluída no pedido de autenticação da aplicação e o inquilino está Federado com esse domínio, tenta do Azure AD para redirecionamento de início de sessão para IdP que está configurado para esse domínio. 

Se a sugestão de domínio não fazer referência a um domínio verificado federado, é ignorada e normal deteção de Realm inicial é invocada.

Para obter mais informações sobre a aceleração de automática utilizando as sugestões de domínio que são suportadas pelo Azure Active Directory, consulte o [Enterprise Mobility + blogue de segurança](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/11/using-azure-ad-to-land-users-on-their-custom-login-page-from-within-your-app/).

>[!NOTE]
>No caso de uma sugestão de domínio é um pedido de autenticação, a presença substitui qualquer política HRD que está definida para a aplicação.

## <a name="home-realm-discovery-policy"></a>Política de deteção de Realm Inicial
Algumas aplicações não fornecem uma forma de configurar o pedido de autenticação que emitir. Nestes casos, não é possível utilizar sugestões de domínio para controlar a aceleração de automática. Aceleração automática pode ser configurada através da política para conseguir o mesmo comportamento.  

### <a name="set-hrd-policy"></a>Conjunto HRD política
Existem três passos necessários para a definição de início de sessão automático-aceleração de uma aplicação:


1. Criar uma política HRD para aceleração automática.

2. Localizar o principal de serviço ao qual pretende anexar a política.

3. Anexar a política para o princípio de serviço. As políticas que poderão ter sido criadas de um inquilino, mas não têm qualquer efeito até que estão ligados a uma entidade. 

Uma política HRD pode ser anexada a um principal de serviço e apenas uma política HRD pode estar ativa numa determinada entidade num dado momento.  

Pode utilizar os cmdlets do Microsoft Active Directory Graph API do Azure diretamente ou o Azure Active Directory PowerShell para configurar a aceleração de automática através da política de HRD.

A Graph API que manipula política é descrita no [operações na política](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) artigo no MSDN.

Segue-se um exemplo de definição de política HRD:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu"
    }
   }
```

O tipo de política é "HomeRealmDiscoveryPolicy."

Se **AccelerateToFederatedDomain** é false, a política não tem qualquer efeito.

**PreferredDomain** deve indicar um domínio ao qual pretende acelerar. Pode ser omitido se o inquilino tem apenas um domínio federado.  Se for omitido e existe mais do que um verificar o domínio federado, a política não tem efeito.

Se **PreferredDomain** for especificado, tem de corresponder um domínio verificado, federado para o inquilino. Todos os utilizadores da aplicação tem de conseguir iniciar sessão no domínio.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Prioridade e a avaliação das políticas HRD
Políticas HRD podem ser criadas e, em seguida, atribuídas a organizações específicas e principais de serviço. Isto significa que é possível que várias políticas aplicar a uma aplicação específica. A política HRD que entra em vigor segue estas regras:


- Se uma sugestão de domínio estiver presente no pedido de autenticação, qualquer política HRD é ignorada. É utilizado o comportamento que é especificado pela sugestão de domínio.

- Caso contrário, se uma política é atribuída explicitamente para o principal de serviço, é imposto. 

- Se não houver nenhuma sugestão de domínio e nenhuma política é atribuída explicitamente para o principal de serviço, uma política que é atribuída explicitamente na organização do principal do serviço principal é imposta. 

- Se não houver nenhuma sugestão de domínio e nenhuma política foi atribuída o principal de serviço ou a organização, é utilizado o comportamento HRD predefinido.

## <a name="tutorial-for-setting-sign-in-auto-acceleration-on-an-application-by-using-an-hrd-policy"></a>Tutorial para definir o início de sessão automático-aceleração de uma aplicação utilizando uma política HRD
Iremos utilizar cmdlets do Azure AD PowerShell para percorrer alguns cenários, incluindo:


- Configuração de aceleração de automática para uma aplicação para um inquilino com um único domínio federado.

- Configuração de aceleração de automática para uma aplicação para um dos vários domínios que são verificados para o seu inquilino.

- Listar as aplicações para o qual está configurada uma política.

### <a name="prerequisites"></a>Pré-requisitos
Nos exemplos a seguir, criar, atualizar, ligar e eliminar políticas no principais de serviço de aplicação no Azure AD.

1.  Para começar, transfira a pré-visualização de cmdlet do Azure AD PowerShell mais recente. 

2.  Depois de transferir os cmdlets do Azure AD PowerShell, execute o comando de ligar para iniciar sessão Azure AD com a sua conta de administrador:

    ``` powershell
    Connect-AzureAD -Confirm
    ```
3.  Execute o seguinte comando para ver todas as políticas da sua organização:

    ``` powershell
    Get-AzureADPolicy
    ```

Se não forem devolvidas, significa que não tem políticas criadas no seu inquilino.

### <a name="example-set-auto-acceleration-for-an-application"></a>Exemplo: Definir a aceleração de automática para uma aplicação 
Neste exemplo, criar uma política que auto-acelera utilizadores para um ecrã de início de sessão do AD FS quando está a iniciar sessão a uma aplicação. Os utilizadores podem iniciar sessão para o AD FS sem ter de introduzir um nome de utilizador na página de início de sessão do AD do Azure pela primeira vez. 

#### <a name="step-1-create-an-hrd-policy"></a>Passo 1: Criar uma política HRD
``` powershell
New-AzureADPoly -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Se tiver um único domínio federado, que autentica utilizadores para aplicações, terá de criar apenas uma política HRD.  

Para ver a nova política e obter o respetivo **ObjectID**, execute o seguinte comando:

``` powershell
Get-AzureADPolicy
```


Para ativar a aceleração de automaticamente depois de ter uma política HRD, pode atribuí-la para vários princípios de serviço de aplicações.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>Passo 2: Localizar o principal de serviço ao qual pretende atribuir a política  
É necessário o **ObjectID** de principais de serviço ao qual pretende atribuir a política. Existem várias formas de localizar o **ObjectID** de principais de serviço.    

Pode utilizar o portal ou pode consultar [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Pode também aceder ao [gráfico Explorer ferramenta](https://graphexplorer.cloudapp.net/) e inicie sessão na sua conta do Azure AD para ver os principais de serviço da sua organização. Porque estão a utilizar o PowerShell, pode utilizar o cmdlet get-AzureADServicePrincipal para listar os princípios de serviço e os respetivos IDs.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Passo 3: Atribuir a política para o principal de serviço  
Depois de ter o **ObjectID** do principal de serviço da aplicação para o qual pretende configurar automaticamente aceleração, execute o seguinte comando. Este comando associa a política HRD que criou no passo 1 com o principal de serviço que localizou no passo 2.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

Pode repetir este comando para cada principal de serviço ao qual pretende adicionar a política.

#### <a name="step-4-check-which-application-service-principals-your-auto-acceleration-policy-is-assigned-to"></a>Passo 4: Verificar os principais de serviço de aplicações a política de aceleração automática está atribuída a
Para verificar as aplicações que têm de política de aceleração automática configurada, utilize o **Get-AzureADPolicyAppliedObject** cmdlet. Transmita-a **ObjectID** da política que pretende verificar.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Passo 5: Terminar!
Experimente a aplicação para verificar se a nova política está a funcionar.

### <a name="example-list-the-applications-for-which-an-auto-acceleration-policy-is-configured"></a>Exemplo: Listar as aplicações para o qual está configurada uma política de aceleração automática

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>Passo 1: Lista de todas as políticas que foram criadas na sua organização 

``` powershell
Get-AzureADPolicy
```

Tenha em atenção o **ObjectID** da política que pretende para listar atribuições para.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>Passo 2: Lista de principais de serviço ao qual a política é atribuída  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-remove-an-auto-acceleration-policy-for-an-application"></a>Exemplo: Remover uma política de aceleração automática para uma aplicação
#### <a name="step-1-get-the-objectid"></a>Passo 1: Obtenha o ObjectID
Utilize o exemplo anterior para obter o **ObjectID** da política e de que o principal de serviço de aplicações que pretende removê-lo. 

#### <a name="step-2-remove-the-policy-assignment-from-the-application-service-principal"></a>Passo 2: Remover a atribuição de política de principal de serviço de aplicações  

``` powershell
Remove-AzureADApplicationPolicy -ObjectId <ObjectId of the Service Principal>  -PolicyId <ObjectId of the policy>
```

#### <a name="step-3-check-removal-by-listing-the-service-principals-to-which-the-policy-is-assigned"></a>Passo 3: Verificar a remoção, listar os principais de serviço ao qual a política é atribuída 

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
## <a name="next-steps"></a>Passos seguintes
- Para obter mais informações sobre como funciona a autenticação no Azure AD, consulte [cenários de autenticação para o Azure AD](develop/active-directory-authentication-scenarios.md).
- Para obter mais informações sobre o utilizador-início de sessão único, consulte [acesso a aplicações e início de sessão no Azure Active Directory](active-directory-enterprise-apps-manage-sso.md).
- Visite o [guia para programadores do Active Directory](develop/active-directory-developers-guide.md) para uma descrição geral de todos os conteúdos relacionados com o programador.
