---
title: Configurar o início de sessão automático-aceleração para uma aplicação através de uma política de deteção de Realm Inicial | Microsoft Docs
description: Explica que um inquilino do Azure AD é e como gerir o Azure através do Azure Active Directory.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: it-pro
ms.date: 06/08/2018
ms.author: barbkess
ms.openlocfilehash: 86b7d8aa9d1c0d770f7b145377f49c5dc2c694a2
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35303917"
---
# <a name="configure-azure-active-directory-sign-in-behavior-for-an-application-by-using-a-home-realm-discovery-policy"></a>Configurar o início de sessão do Azure Active Directory no comportamento para uma aplicação utilizando uma política de deteção de Realm Inicial

O documento seguinte fornece uma introdução ao configurar o comportamento de autenticação do Azure Active Directory para utilizadores federados.   Abrange a configuração de aceleração automática e a autenticação restrições para os utilizadores no domínio federado.

## <a name="home-realm-discovery"></a>Deteção de Realm Inicial
Deteção de Realm doméstico (HRD) é o processo que permite que o Azure Active Directory (Azure AD) para determinar em que um utilizador tem de autenticar no momento de início de sessão.  Quando um utilizador inicia sessão para um inquilino do Azure AD para aceder a um recurso ou para a página de início de sessão comuns do Azure AD, escreva um nome de utilizador (UPN). Azure AD utiliza esse para detetar em que o utilizador tem de iniciar sessão. 

O utilizador poderá ter de ser direcionado para uma das seguintes localizações sejam autenticados:

- O inquilino doméstico do utilizador (podem ser o mesmo inquilino como o recurso que o utilizador está a tentar aceder). 

- Conta Microsoft.  O utilizador é um convidado no inquilino do recurso.

-  Um fornecedor de identidade no local, tais como serviços de Federação do Active Directory (AD FS).

- Outro fornecedor de identidade que está federada com o inquilino do Azure AD.

## <a name="auto-acceleration"></a>Aceleração automática 
Algumas organizações configurar domínios no seu inquilino do Azure Active Directory a federar com outro IdP, tais como o AD FS para autenticação de utilizador.  

Quando um utilizador inicia sessão numa aplicação, são apresentados primeiro com uma página de início de sessão do Azure AD. Depois de que escreveu o UPN, se estiverem num domínio federado é direcionado para a página de início de sessão do IdP que servem esse domínio. Em determinadas circunstâncias, os administradores poderão querer direcionar os utilizadores para a página de início de sessão quando está a iniciar sessão em aplicações específicas. 

Como resultado, os utilizadores podem ignorar a página inicial do Azure Active Directory. Este processo é referido como "início de sessão automático-aceleração."

Nos casos em que o inquilino é federado para outro IdP para início de sessão, aceleração automática faz com que o utilizador iniciar sessão mais está mais simples.  Pode configurar a aceleração de automático para aplicações individuais.

>[!NOTE]
>Se configurar uma aplicação para a aceleração de automática, os utilizadores convidados não podem iniciar sessão. Se um utilizador não efetuar diretamente para um IdP federado para autenticação, não há nenhuma forma para os mesmos voltar para a página de início de sessão no Azure Active Directory. Utilizadores de convidado, o que poderão ter de ser direcionado para outros inquilinos ou uma IdP externo, como uma conta Microsoft, não é possível iniciar sessão para essa aplicação porque que está a ignorar a etapa de deteção de Realm Inicial.  

Existem duas formas de controlar a aceleração de automático para um federado IdP:   

- Utilize uma sugestão de domínio nos pedidos de autenticação para uma aplicação. 
- Configure uma política de deteção de Realm Inicial para ativar a aceleração de automática.

### <a name="domain-hints"></a>Sugestões de domínio    
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
>No caso de uma sugestão de domínio é um pedido de autenticação, a presença substitui aceleração automática que está definida para a aplicação na política HRD.

### <a name="home-realm-discovery-policy-for-auto-acceleration"></a>Política de deteção de Realm Inicial para a aceleração de automática
Algumas aplicações não fornecem uma forma de configurar o pedido de autenticação que emitir. Nestes casos, não é possível utilizar sugestões de domínio para controlar a aceleração de automática. Aceleração automática pode ser configurada através da política para conseguir o mesmo comportamento.  

## <a name="enable-direct-authentication-for-legacy-applications"></a>Ativar a autenticação direta para aplicações legadas
É recomendado para as aplicações utilizem o início de sessão interativo e bibliotecas do AAD para autenticar os utilizadores. As bibliotecas asseguramos dos fluxos de utilizador federado.  As aplicações legadas, por vezes, não são escritas para compreender a Federação. Se não efetuar a deteção de realm inicial e não interage com o ponto final federado correto para autenticar um utilizador. Caso pretenda, pode utilizar a política de HRD para ativar aplicações legadas específicas que submetem credenciais de nome de utilizador/palavra-passe para a autenticação diretamente no Azure Active Directory. Sincronização de Hash de palavra-passe tem de estar ativada. 

> [!IMPORTANT]
> Ative a autenticação direta apenas se tiver de sincronização de Hash de palavra-passe ativada e sabe que pode autenticar-se esta aplicação, sem quaisquer políticas implementadas pelo seu IdP no local. Se desativar a sincronização de Hash de palavra-passe, ou desativar a sincronização de diretórios com o AD Connect por qualquer motivo, deve remover esta política para evitar a possibilidade de autenticação direta, utilizando um hash de palavra-passe obsoletos.

## <a name="set-hrd-policy"></a>Conjunto HRD política
Existem três passos para a política HRD de definição de uma aplicação para federado início de sessão automático-aceleração ou diretas aplicações baseado na nuvem:

1. Crie uma política HRD.

2. Localize o principal de serviço ao qual pretende anexar a política.

3. Anexe a política para o principal de serviço. 

Políticas apenas em vigor para uma aplicação específica quando estão ligados a um principal de serviço. 

Apenas uma política HRD pode estar ativa um principal de serviço num dado momento.  

Pode utilizar os cmdlets do Microsoft Active Directory Graph API do Azure diretamente ou o Azure Active Directory PowerShell para criar e gerir a política HRD.

A Graph API que manipula política é descrita no [operações na política](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) artigo no MSDN.

Segue-se um exemplo de definição de política HRD:
    
 ```
   {  
    "HomeRealmDiscoveryPolicy":
    {  
    "AccelerateToFederatedDomain":true,
    "PreferredDomain":"federated.example.edu",
    "AllowCloudPasswordValidation":true
    }
   }
```

O tipo de política é "HomeRealmDiscoveryPolicy."

**AccelerateToFederatedDomain** é opcional. Se **AccelerateToFederatedDomain** é false, a política não tem efeito em aceleração automática. Se **AccelerateToFederatedDomain** é verdadeiro e existe apenas um é verificado e domínio federado do inquilino, em seguida, os utilizadores será executado direitas para IdP federado para início de sessão. Se for VERDADEIRO e existir mais do que um domínio verificado no inquilino, **PreferredDomain** tem de ser especificado.

**PreferredDomain** é opcional. **PreferredDomain** deve indicar um domínio ao qual pretende acelerar. Pode ser omitido se o inquilino tem apenas um domínio federado.  Se for omitido e existe mais do que um verificar o domínio federado, a política não tem efeito.

 Se **PreferredDomain** for especificado, tem de corresponder um domínio verificado, federado para o inquilino. Todos os utilizadores da aplicação tem de conseguir iniciar sessão no domínio.

**AllowCloudPasswordValidation** é opcional. Se **AllowCloudPasswordValidation** for VERDADEIRO, a aplicação tem permissão para autenticar um utilizador federado através da apresentação de credenciais de nome de utilizador/palavra-passe diretamente para o ponto de final token do Azure Active Directory. Isto só funciona se a sincronização de Hash de palavra-passe está ativada.

### <a name="priority-and-evaluation-of-hrd-policies"></a>Prioridade e a avaliação das políticas HRD
Políticas HRD podem ser criadas e, em seguida, atribuídas a organizações específicas e principais de serviço. Isto significa que é possível que várias políticas aplicar a uma aplicação específica. A política HRD que entra em vigor segue estas regras:


- Se uma sugestão de domínio estiver presente no pedido de autenticação, qualquer política HRD é ignorada para a aceleração de automática. É utilizado o comportamento que é especificado pela sugestão de domínio.

- Caso contrário, se uma política é atribuída explicitamente para o principal de serviço, é imposto. 

- Se não houver nenhuma sugestão de domínio e nenhuma política é atribuída explicitamente para o principal de serviço, uma política que é atribuída explicitamente na organização do principal do serviço principal é imposta. 

- Se não houver nenhuma sugestão de domínio e nenhuma política foi atribuída o principal de serviço ou a organização, é utilizado o comportamento HRD predefinido.

## <a name="tutorial-for-setting-hrd-policy-on-an-application"></a>Tutorial de definição de política HRD uma aplicação 
Iremos utilizar cmdlets do Azure AD PowerShell para percorrer alguns cenários, incluindo:


- Configurar política HRD para fazer a aceleração de automática para uma aplicação de um inquilino com um único domínio federado.

- Configurar política HRD para fazer a aceleração de automática para uma aplicação para um dos vários domínios que são verificados para o seu inquilino.

- Configurar política HRD para ativar uma aplicação legada para direcionar a autenticação de nome de utilizador/palavra-passe para o Azure Active Directory para um utilizador federado.

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

### <a name="example-set-hrd-policy-for-an-application"></a>Exemplo: Definir política HRD para uma aplicação 

Neste exemplo, criar uma política que quando é atribuído a uma aplicação ou: 
- Auto-acelera utilizadores para um ecrã de início de sessão do AD FS ao que estão a iniciar sessão a uma aplicação quando há um único domínio no seu inquilino. 
- Acelera o automaticamente aos utilizadores do AD FS início de sessão ecrã existe é mais de um domínio federado no seu inquilino.
- Permite que o nome de utilizador/palavra-passe não interativa de início de sessão diretamente ao Azure Active Directory para utilizadores federados para as aplicações que a política é atribuída a.

#### <a name="step-1-create-an-hrd-policy"></a>Passo 1: Criar uma política HRD

A seguinte política auto-acelera utilizadores para um ecrã de início de sessão do AD FS ao que estão a iniciar sessão a uma aplicação quando há um único domínio no seu inquilino.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true}}") -DisplayName BasicAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```
Os utilizadores de auto-acelera da seguinte política para do AD FS início de sessão ecrã há mais de um domínio federado no seu inquilino. Se tiver mais de um domínio federado que autentica utilizadores para aplicações, tem de especificar o domínio para acelerar o automático.

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AccelerateToFederatedDomain`":true, "PreferredDomain":"federated.example.edu"}}") -DisplayName MultiDomainAutoAccelerationPolicy -Type HomeRealmDiscoveryPolicy
```

Para criar uma política para ativar a autenticação de nome de utilizador/palavra-passe para os utilizadores federados diretamente com o Azure Active Directory para aplicações específicas, execute o seguinte comando:

``` powershell
New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AllowCloudPasswordValidation`":true}}") -DisplayName EnableDirectAuthPolicy -Type HomeRealmDiscoveryPolicy
```


Para ver a nova política e obter o respetivo **ObjectID**, execute o seguinte comando:

``` powershell
Get-AzureADPolicy
```


Para aplicar a política HRD depois de ter criado-lo, pode atribuí-la para vários principais de serviço de aplicações.

#### <a name="step-2-locate-the-service-principal-to-which-to-assign-the-policy"></a>Passo 2: Localizar o principal de serviço ao qual pretende atribuir a política  
É necessário o **ObjectID** de principais de serviço ao qual pretende atribuir a política. Existem várias formas de localizar o **ObjectID** de principais de serviço.    

Pode utilizar o portal ou pode consultar [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Pode também aceder ao [gráfico Explorer ferramenta](https://graphexplorer.cloudapp.net/) e inicie sessão na sua conta do Azure AD para ver os principais de serviço da sua organização. Porque estão a utilizar o PowerShell, pode utilizar o cmdlet get-AzureADServicePrincipal para listar os principais de serviço e os respetivos IDs.

#### <a name="step-3-assign-the-policy-to-your-service-principal"></a>Passo 3: Atribuir a política para o principal de serviço  
Depois de ter o **ObjectID** do principal de serviço da aplicação para o qual pretende configurar automaticamente aceleração, execute o seguinte comando. Este comando associa a política HRD que criou no passo 1 com o principal de serviço que localizou no passo 2.

``` powershell
Add-AzureADServicePrincipalPolicy -Id <ObjectID of the Service Principal> -RefObjectId <ObjectId of the Policy>
```

Pode repetir este comando para cada principal de serviço ao qual pretende adicionar a política.

No caso em que uma aplicação já tem uma política de HomeRealmDiscovery atribuída, não poderá adicionar um segundo.  Nesse caso, altere a definição da política de deteção de Realm Inicial que é atribuída à aplicação para adicionar parâmetros adicionais.

#### <a name="step-4-check-which-application-service-principals-your-hrd-policy-is-assigned-to"></a>Passo 4: Verificar quais principais de serviço de aplicações a política HRD está atribuída a
Para verificar as aplicações que tenham política HRD configurada, utilize o **Get-AzureADPolicyAppliedObject** cmdlet. Transmita-a **ObjectID** da política que pretende verificar.

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```
#### <a name="step-5-youre-done"></a>Passo 5: Terminar!
Experimente a aplicação para verificar se a nova política está a funcionar.

### <a name="example-list-the-applications-for-which-hrd-policy-is-configured"></a>Exemplo: Listar as aplicações para que HRD está configurada uma política

#### <a name="step-1-list-all-policies-that-were-created-in-your-organization"></a>Passo 1: Lista de todas as políticas que foram criadas na sua organização 

``` powershell
Get-AzureADPolicy
```

Tenha em atenção o **ObjectID** da política que pretende para listar atribuições para.

#### <a name="step-2-list-the-service-principals-to-which-the-policy-is-assigned"></a>Passo 2: Lista de principais de serviço ao qual a política é atribuída  

``` powershell
Get-AzureADPolicyAppliedObject -ObjectId <ObjectId of the Policy>
```

### <a name="example-remove-an-hrd-policy-for-an-application"></a>Exemplo: Remover uma política HRD para uma aplicação
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
## <a name="next-steps"></a>Passos Seguintes
- Para obter mais informações sobre como funciona a autenticação no Azure AD, consulte [cenários de autenticação para o Azure AD](../develop/active-directory-authentication-scenarios.md).
- Para obter mais informações sobre o utilizador-início de sessão único, consulte [acesso a aplicações e início de sessão no Azure Active Directory](configure-single-sign-on-portal.md).
- Visite o [guia para programadores do Active Directory](../develop/active-directory-developers-guide.md) para uma descrição geral de todos os conteúdos relacionados com o programador.
