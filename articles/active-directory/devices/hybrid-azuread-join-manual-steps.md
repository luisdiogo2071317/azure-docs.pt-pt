---
title: Configurar dispositivos híbridos associados ao Azure Active Directory manualmente | Microsoft Docs
description: Saiba como configurar dispositivos híbridos associados ao Azure Active Directory manualmente.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/01/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: c85d3ce6ab3e84d454ddbc2550f430b87705c192
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622181"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Tutorial: Configurar dispositivos híbridos associados ao Azure Active Directory manualmente. 

Com a gestão de dispositivos no Azure Active Directory (Azure AD), pode assegurar que os seus utilizadores acedem aos seus recursos a partir de dispositivos que cumpram as suas normas de segurança e conformidade. Para obter mais detalhes, veja [Introduction to device management in Azure Active Directory](overview.md) (Introdução à gestão de dispositivos no Azure Active Directory).


> [!TIP]
> Se utilizar o Azure AD Connect for uma opção para si, veja [Select your scenario](hybrid-azuread-join-plan.md#select-your-scenario) (Selecionar o seu cenário). A utilização do Azure AD Connect permite-lhe simplificar a configuração da associação híbrida ao Azure AD significativamente.



Se tiver um ambiente do Active Directory no local e quiser associar os seus dispositivos associados ao domínio ao Azure AD, pode fazê-lo ao configurar os dispositivos híbridos associados ao Azure AD. Neste tutorial, vai aprender a configurar manualmente a associação híbrida ao Azure AD para os seus dispositivos.

> [!div class="checklist"]
> * Pré-requisitos
> * Passos de configuração
> * Configurar o ponto de ligação do serviço
> * Configurar a emissão de afirmações
> * Permitir dispositivos de nível inferior do Windows
> * Verificar dispositivos associados
> * Resolver problemas relacionados com a implementação
 




## <a name="prerequisites"></a>Pré-requisitos

Este tutorial parte do princípio de que está familiarizado com:
    
-  [Introduction to device management in Azure Active Directory](../device-management-introduction.md) (Introdução à gestão de dispositivos no Azure Active Directory)
    
-  [Como planear a sua implementação associada híbrida do Azure Active Directory](hybrid-azuread-join-plan.md)

-  [Como controlar a associação híbrida do Azure AD dos seus dispositivos](hybrid-azuread-join-control.md)


Antes de começar a permitir dispositivos híbridos associados ao Azure AD na sua organização, tem de confirmar que:

- Está a executar uma versão atualizada do Azure AD Connect.

- O Azure AD Connect sincronizou os objetos de computador dos dispositivos que pretende que sejam associados de forma híbrida ao Azure AD. Se os objetos de computador pertencerem a unidades organizacionais (UO) específicas, essas UOs também têm de ser configuradas para sincronização no Azure AD Connect.

  

Azure AD Connect:

- Mantém a associação entre a conta do computador no seu Active Directory (AD) no local e o objeto de dispositivo no Azure AD. 
- Permite outras funcionalidades relacionadas com o dispositivo, como o Windows Hello for Business.

Confirme que os seguintes URLs estão acessíveis nos computadores na rede da sua organização, para registá-los no Azure AD:

- https://enterpriseregistration.windows.net

- https://login.microsoftonline.com Permitir
- https://device.login.microsoftonline.com

- O STS da sua organização (domínios federados)

Se ainda não o tiver feito, o STS da organização (para domínios federados) deve ser incluído nas definições de intranet local do utilizador.

Se a sua organização estiver a considerar utilizar o SSO integrado, os seguintes URLs têm de ser alcançáveis nos computadores da sua organização e também adicionados à zona de intranet local do utilizador:

- https://autologon.microsoftazuread-sso.com

- Além disso, a definição seguinte deve ser ativada na zona de intranet do utilizador: “Permitir atualizações da barra de estado através de script”.

Se a organização utilizar a configuração gerida (não federada) com o Azure AD no local e não utilizar o AD FS para federar com o Azure AD, a associação híbrida ao Azure AD no Windows 10 utiliza os objetos do computador no AD para se sincronizar com o Azure AD. Certifique-se de que as Unidades Organizacionais (UOs) que contêm os objetos de computador que têm de ser associados de forma híbrida ao Azure AD estão ativadas para sincronização na configuração de sincronização do Azure AD Connect.

Relativamente aos dispositivos Windows 10 na versão 1703 ou anterior, caso a organização precise de acesso à Internet através de um proxy de saída, tem de implementar o Web Proxy Auto-Discovery (WPAD) para permitir que os computadores Windows 10 se registem no Azure AD. 

A partir do Windows 10 1803, mesmo que uma tentativa de associação híbrida do Azure AD por um dispositivo num domínio federado com o AD FS falhe, se o Azure AD Connect estiver configurado para sincronizar os objetos do computador/dispositivo com o Azure AD, então o dispositivo irá tentar concluir a associação híbrida do Azure AD através do computador/dispositivo sincronizado.

## <a name="configuration-steps"></a>Passos de configuração

Pode configurar os dispositivos híbridos associados ao Azure AD para vários tipos de plataformas de dispositivos Windows. Este tópico inclui os passos necessários para todos os cenários de configuração comuns.  

Utilize a tabela abaixo para obter uma descrição geral dos passos necessários para o seu cenário:  



| Passos                                      | Dispositivos Windows atuais e sincronização de hash de palavra-passe | Dispositivos Windows atuais e federação | Dispositivos Windows de nível inferior |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Configurar o ponto de ligação do serviço | ![Marcar][1]                            | ![Marcar][1]                    | ![Marcar][1]        |
| Configurar a emissão de afirmações           |                                        | ![Marcar][1]                    | ![Marcar][1]        |
| Permitir dispositivos não Windows 10      |                                        |                                | ![Marcar][1]        |
| Verificar dispositivos associados          | ![Marcar][1]                            | ![Marcar][1]                    | ![Marcar][1]        |



## <a name="configure-service-connection-point"></a>Configurar o ponto de ligação do serviço

Os seus dispositivos utilizam o objeto de ponto de ligação do serviço (SCP) durante o registo para detetar informações do inquilino do Azure AD. No Active Directory no local (AD), o objeto SCP dos dispositivos híbridos associados ao Azure AD tem de existir na partição de contexto de nomenclatura da configuração da floresta do computador. Só existe um contexto de nomenclatura de configuração por floresta. Numa configuração multifloresta do Active Directory, o ponto de ligação do serviço tem de existir em todas as florestas que contêm computadores associados a um domínio.

Pode utilizar o cmdlet [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) para obter o contexto de nomenclatura da configuração da sua floresta.  

Para uma floresta com o nome de domínio do Active Directory *fabrikam.com*, o contexto de nomenclatura da configuração é:

`CN=Configuration,DC=fabrikam,DC=com`

Na sua floresta, o objeto SCP do registo automático dos dispositivos associados a um domínio está localizado em:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Dependendo da forma como implementou o Azure AD Connect, o objeto SCP poderá já estar configurado.
Pode utilizar o seguinte script do Windows PowerShell para confirmar se o objeto já existe e obter os valores de deteção: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

A saída **$scp.Keywords** mostra as informações do inquilino do Azure AD, como, por exemplo:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Se o ponto de ligação do serviço não existir, pode executar o cmdlet `Initialize-ADSyncDomainJoinedComputerSync` no servidor do Azure AD Connect para criá-lo. É necessária a credencial de administrador empresarial para executar este cmdlet.  
O cmdlet:

- Cria o ponto de ligação do serviço na floresta do Active Directory à qual o Azure AD Connect está ligado. 
- Requer que seja especificado o parâmetro `AdConnectorAccount`. Esta é a conta configurada como a conta de conector do Active Directory no Azure AD Connect. 


O script abaixo mostra um exemplo da utilização do cmdlet. Neste script, `$aadAdminCred = Get-Credential` requer que indique um nome de utilizador. Tem de indicar o nome de utilizador no formato de nome principal de utilizador (UPN) (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

O cmdlet `Initialize-ADSyncDomainJoinedComputerSync`:

- Utiliza o módulo Active Directory do PowerShell e as Ferramentas do AD DS, que se baseiam nos serviços Web do Active Directory que estão a ser executados num controlador de domínio. Os Serviços Web do Active Directory são suportados em controladores de domínio em execução no Windows Server 2008 R2 e posterior.
- Só são suportados pela **versão 1.1.166.0 do módulo MSOnline do PowerShell**. Para transferir este módulo, utilize esta [ligação](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/).   
- Se as ferramentas do AD DS não estiverem instaladas, `Initialize-ADSyncDomainJoinedComputerSync` falhará.  As ferramentas do AD DS podem ser instaladas através do Gestor de Servidor, em Features - Remote Server Administration Tools - Role Administration Tools (Funcionalidades - Ferramentas de Administração Remota do Servidor - Ferramentas de Administração de Funções).

Em controladores de domínio que executem o Windows Server 2008 ou versões anteriores, utilize o script abaixo para criar o ponto de ligação do serviço.

Numa configuração multifloresta, deve utilizar o seguinte script para criar o ponto de ligação do serviço em cada floresta onde existam computadores:
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC
    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()

No script anterior,

- `$verifiedDomain = "contoso.com"` é um marcador de posição que tem de ser substituído por um dos seus nomes de domínio verificados no Azure AD. Antes de poder utilizá-lo, tem de ser o proprietário do domínio.

Para obter mais detalhes sobre os nomes de domínio verificados, veja [Adicionar um nome de domínio personalizado ao Azure Active Directory](../active-directory-domains-add-azure-portal.md).  
Para obter uma lista dos domínios verificados da sua empresa, pode utilizar o cmdlet [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0). 

![Get-AzureADDomain](./media/hybrid-azuread-join-manual-steps/01.png)

## <a name="setup-issuance-of-claims"></a>Configurar a emissão de afirmações

Numa configuração federada do Azure AD, os dispositivos utilizam o Active Directory Federation Services (AD FS) ou um serviço de federação de terceiros para se autenticarem no Azure AD. Os dispositivos autenticam-se para obterem um token de acesso para se registarem no Registo de Dispositivos do Azure Active Directory (Azure DRS).

Os dispositivos Windows atuais autenticam-se por meio da Autenticação Integrada do Windows num ponto final WS-Trust ativo (versões 1.3 ou 2005) alojado pelo serviço de federação no local.

> [!NOTE]
> Se for utilizado o AD FS, tem de ser ativado **adfs/services/trust/13/windowstransport** ou **adfs/services/trust/2005/windowstransport**. Se estiver a utilizar o Proxy de Autenticação Web, confirme também que este ponto final é publicado através do proxy. Pode ver que pontos finais estão ativados na consola de gestão do AD FS, em **Service > Endpoints** (Serviço > Pontos Finais).
>
>Se o AD FS não for o seu serviço de federação no local, siga as instruções do seu fornecedor para confirmar que o mesmo suporta os pontos finais WS-Trust 1.3 ou 2005 e que são publicados através de Metadata Exchange File (MEX).

Para concluir o registo dos dispositivos, as afirmações seguintes têm de existir no token que o Azure DRS recebeu. O Azure DRS vai criar um objeto de dispositivo no Azure AD com algumas destas informações, que são depois utilizadas pelo Azure AD Connect para associar o objeto de dispositivo acabado de criar à conta do computador no local.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Se tiver vários nomes de domínio verificados, tem de indicar a seguinte afirmação para os computadores:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Se já estiver a emitir uma afirmação ImmutableID (por exemplo, um ID de início de sessão alternativo), tem de indicar uma afirmação correspondente para os computadores:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

Nas secções abaixo, vai encontrar informações sobre:
 
- Os valores que cada afirmação deve ter
- O aspeto que uma definição vai ter no AD FS

A definição ajuda-o a verificar se os valores estão presentes ou se precisa de os criar.

> [!NOTE]
> Se não utilizar o AD FS para o seu servidor de federação no local, siga as instruções do seu fornecedor para criar a configuração adequada para emitir estas afirmações.

### <a name="issue-account-type-claim"></a>Emitir afirmação de tipo de conta

**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - esta afirmação tem de conter o valor **DJ**, que identifica o dispositivo como um computador associado a um domínio. No AD FS, pode adicionar uma regra de transformação de emissão semelhante à seguinte:

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>Emitir o objectGUID da conta de computador no local

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - esta afirmação tem de conter o valor **objectGUID** da conta de computador no local. No AD FS, pode adicionar uma regra de transformação de emissão semelhante à seguinte:

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>Emitir o objectSID da conta de computador no local

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - esta afirmação tem de conter o valor **objectSid** da conta de computador no local. No AD FS, pode adicionar uma regra de transformação de emissão semelhante à seguinte:

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Emitir issuerID para o computador caso existam vários nomes de domínio verificados no Azure AD

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - esta afirmação tem de conter o Uniform Resource Identifier (URI) dos nomes de domínio verificados que estejam ligados ao serviço de federação no local (AD FS ou serviço de terceiros) que emite o token. No AD FS, pode adicionar regras de transformação de emissão semelhantes às seguintes, por essa ordem específica, a seguir às referidas acima. Tenha em conta que é necessária uma regra para emitir explicitamente a regra para os utilizadores. Nas regras abaixo, é adicionada uma primeira regra que identifica o utilizador vs. a autenticação do computador.

    @RuleName = "Issue account type with the value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


Na afirmação acima,

- `<verified-domain-name>` é um marcador de posição que tem de ser substituído por um dos seus nomes de domínio verificados no Azure AD. Por exemplo, valor = "http://contoso.com/adfs/services/trust/"



Para obter mais detalhes sobre os nomes de domínio verificados, veja [Adicionar um nome de domínio personalizado ao Azure Active Directory](../active-directory-domains-add-azure-portal.md).  

Para obter uma lista dos domínios verificados da sua empresa, pode utilizar o cmdlet [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0). 

![Get-MsolDomain](./media/hybrid-azuread-join-manual-steps/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Emitir ImmutableID para o computador se existir um para os utilizadores (por exemplo, o ID de início de sessão alternativo está definido)

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - esta afirmação tem de conter um valor válido para computadores. No AD FS, pode criar uma regra de transformação de emissão da seguinte forma:

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>Script de programa auxiliar para criar as regras de transformação de emissão do AD FS

O seguinte script ajuda-o a criar as regras de transformação de emissão descritas anteriormente.

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a>Observações 

- Este script anexa as regras às já existentes. Não o execute duas vezes, porque o conjunto de regras será adicionado a dobrar. Antes de voltar a executar o script, confirme que não existem regras correspondentes para estas afirmações (com as condições correspondentes).

- Se tiver vários nomes de domínio verificados (conforme mostrado no portal do Azure AD ou através do cmdlet Get-MsolDomains), defina o valor de **$multipleVerifiedDomainNames** no script como **$true**. Confirme também que remove todas as afirmações issuerid existentes que possam ter sido criadas pelo Azure AD Connect ou por outros meios. Eis um exemplo desta regra:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Se já tiver emitido uma afirmação **ImmutableID** para as contas de utilizador, defina o valor de **$immutableIDAlreadyIssuedforUsers** no script como **$true**.

## <a name="enable-windows-down-level-devices"></a>Permitir dispositivos de nível inferior do Windows

Se alguns dos seus dispositivos associados a um domínio são dispositivos de nível inferior do Windows, terá de:

- Definir uma política no Azure AD para permitir que os utilizadores registem dispositivos.
 
- Configurar o serviço de federação no local para emitir afirmações para suportar a **Autenticação Integrada do Windows (IWA)** para o registo de dispositivos.
 
- Adicionar o ponto final da autenticação de dispositivos do Azure AD às zonas de intranet local, para evitar pedidos de certificado durante a autenticação dos dispositivos.

- Controlar dispositivos de nível inferior do Windows 


### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Definir uma política no Azure AD para permitir que os utilizadores registem dispositivos

Para registar dispositivos de nível inferior do Windows, tem de se certificar de que a definição para permitir que os utilizadores registem dispositivos no Azure AD está configurada. No portal do Azure, pode encontrar esta definição em:

`Azure Active Directory > Users and groups > Device settings`
    
A seguinte política tem de ser definida como **Todos**: **os utilizadores podem registar os seus dispositivos com o Azure AD**

![Registar dispositivos](./media/hybrid-azuread-join-manual-steps/23.png)


### <a name="configure-on-premises-federation-service"></a>Configurar o serviço de federação no local 

O serviço de Federação no local tem de suportar a emitir a **authenticationmethod** e **wiaormultiauthn** afirmações quando recebe um pedido de autenticação à parte da entidade confiadora do Azure AD que contém um resource_params parâmetro com um valor codificado como mostrado abaixo:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Quando é recebido um pedido destes, o serviço de federação nTLM tem de autenticar o utilizador com a Autenticação Integrada do Windows e, depois, tem de emitir as duas afirmações abaixo:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

No AD FS, tem de adicionar uma regra de transformação de emissão que seja transmitida pelo método de autenticação.  

**Para adicionar esta regra:**

1. Na consola de gestão do AD FS, aceda a `AD FS > Trust Relationships > Relying Party Trusts`.
2. Clique com o botão direito do rato no objeto de confiança da entidade confiadora da Plataforma de Identidade do Microsoft Office 365 e selecione **Edit Claim Rules** (Editar Regras de Emissão).
3. No separador **Issuance Transform Rules** (Regras de Transformação de Emissão), selecione **Add Rule** (Adicionar Regra).
4. Na lista de modelos **Claim rule** (Regra de afirmação), selecione **Send Claims Using a Custom Rule** (Enviar Afirmações com uma Regra Personalizada).
5. Selecione **Seguinte**.
6. Na caixa **Claim rule name** (Nome da regra de afirmação), escreva **Auth Method Claim Rule** (Regra de Afirmação de Método de Autenticação).
7. Na caixa **Claim rule** (Regra de afirmação), escreva a seguinte regra:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. No servidor de Federação, escreva o comando do PowerShell abaixo depois de substituir **\<RPObjectName\>** com o nome de objeto de terceiros entidade confiadora para seu objeto de confiança de terceiros da entidade confiadora do AD do Azure. Normalmente, este objeto é denominado **Plataforma de Identidade do Microsoft Objeto 365**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a>Adicionar o ponto final da autenticação de dispositivos do Azure AD às zonas de Intranet local

Para evitar pedidos de certificado quando os utilizadores de dispositivos registados se autenticam no Azure AD, pode enviar uma política para os dispositivos associados ao domínio para adicionar o seguinte URL à zona de Intranet local no Internet Explorer:

`https://device.login.microsoftonline.com`


### <a name="control-windows-down-level-devices"></a>Controlar dispositivos de nível inferior do Windows 

Para registar dispositivos de nível inferior do Windows, tem de transferir e instalar um pacote do Windows Installer (.msi) do Centro de Transferências. Para obter mais informações, clique [aqui](hybrid-azuread-join-control.md#control-windows-down-level-devices). 



## <a name="verify-joined-devices"></a>Verificar dispositivos associados

Pode utilizar o cmdlet [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) no [módulo Azure Active Directory do PowerShell](/powershell/azure/install-msonlinev1?view=azureadps-2.0) para verificar os dispositivos da sua organização associados com êxito.

A saída deste cmdlet mostra os dispositivos que estão registados e associados ao Azure AD. Para obter todos os dispositivos, utilize o parâmetro **All** e filtre-os com a propriedade **deviceTrustType**. Os dispositivos associação ao domínio têm o valor **Associado a domínio**.



## <a name="troubleshoot-your-implementation"></a>Resolver problemas relacionados com a implementação

Se estiver a ter problemas a completar a associação ao Azure AD híbrido dos dispositivos Windows de domínio associado, veja:

- [Troubleshooting Hybrid Azure AD join for Windows current devices](troubleshoot-hybrid-join-windows-current.md) (Resolver problemas com a associação híbrida ao Azure AD para dispositivos Windows atuais)
- [Troubleshooting Hybrid Azure AD join for Windows down-level devices](troubleshoot-hybrid-join-windows-legacy.md) (Resolver problemas com a associação híbrida ao Azure AD para dispositivos Windows de nível inferior)

## <a name="next-steps"></a>Passos Seguintes

* [Introduction to device management in Azure Active Directory](overview.md) (Introdução à gestão de dispositivos no Azure Active Directory)



<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual-steps/12.png
