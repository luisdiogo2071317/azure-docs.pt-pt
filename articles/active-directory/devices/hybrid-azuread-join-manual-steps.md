---
title: Configurar manualmente o híbrida do Azure Active Directory associados a um dispositivos | Documentos da Microsoft
description: Saiba como configurar manualmente híbrida do Azure Active Directory associados a um dispositivos.
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
ms.topic: article
ms.date: 08/08/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: ba47223f86005809189214f26a63b75b21449e3a
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630624"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Tutorial: Configurar híbrida do Azure Active Directory associados a um dispositivos manualmente 

Gestão de dispositivos no Azure Active Directory (Azure AD), pode certificar-se de que os utilizadores estão a aceder ao seus recursos de dispositivos que cumprem as normas de segurança e conformidade. Para obter mais detalhes, consulte a [introdução à gestão de dispositivos no Azure Active Directory](overview.md).

Se tiver um ambiente do Active Directory no local e pretender associar os dispositivos associados a um domínio ao Azure AD, isso pode ser feito através da configuração de dispositivos de associados ao Azure AD híbrido. Este artigo fornece os passos relacionados. 



> [!TIP]
> Se utilizar o Azure AD Connect é uma opção para si, veja [selecione o seu cenário](hybrid-azuread-join-plan.md#select-your-scenario). Ao utilizar o Azure AD Connect, pode simplificar significativamente a configuração de associação do Azure AD híbrido.




## <a name="prerequisites"></a>Pré-requisitos

Este tutorial parte do princípio de que está familiarizado com:
    
-  [Introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md)
    
-  [Como planear a sua implementação associada híbrida do Azure Active Directory](hybrid-azuread-join-plan.md)

-  [Como controlar a associação híbrida do Azure AD dos seus dispositivos](hybrid-azuread-join-control.md)


Antes de iniciar a ativação híbridos associados ao Azure AD dispositivos na sua organização, terá de certificar-se de que:

- Estiver a executar uma versão atualizada do Azure AD connect.

- Do Azure AD connect foi sincronizado os objetos de computador dos dispositivos à sua escolha para serem híbridos associados ao Azure AD para o Azure AD. Se os objetos de computador pertencem ao específicas unidades organizacionais (UO), esses UOs têm de ser configurado para a sincronização no Azure AD connect também.

  

Do Azure AD Connect:

- Mantém a associação entre a conta de computador no seu no local do Active Directory (AD) e o objeto de dispositivo no Azure AD. 
- Permite que outros dispositivos relacionados com recursos como o Windows Hello para empresas.

Certifique-se de que os seguintes URLs são acessíveis a partir de computadores no interior da rede da organização para o registo dos computadores com o Azure AD:

- https://enterpriseregistration.windows.net

- https://login.microsoftonline.com Permitir
- https://device.login.microsoftonline.com

- STS de sua organização (domínios federados)

Se não tiver feito, o STS de sua organização (para domínios federados) devem ser incluídas nas definições de local intranet do usuário.

Se sua organização está a planear utilizar o SSO totalmente integrado, em seguida, os seguintes URLs têm de ser acessível a partir de computadores dentro de sua organização e também tem de ser adicionados à zona de local intranet do usuário:

- https://autologon.microsoftazuread-sso.com

- Além disso, a definição seguinte deve ser ativada na zona de intranet do utilizador: "Permitir atualizações da barra de estado por meio de script".

Se a organização utilizar a configuração (não federadas) gerida com o AD no local e não utiliza o ADFS para federar com o Azure AD, em seguida, a associação do Azure AD híbrido no Windows 10 depende dos objetos de computador no AD para ser sync'ed para o Azure AD. Certifique-se de que qualquer unidades organizacionais (UO) que contêm os objetos de computador que precisam para serem híbridos associados ao Azure AD estão ativadas para sincronização na configuração de sincronização do Azure AD Connect.

Para dispositivos Windows 10 versão 1703 ou anterior, se sua organização necessita de acesso à Internet através de um proxy de saída, tem de implementar descoberta automática WPAD (Web Proxy) para permitir que os computadores Windows 10 para se registrar para o Azure AD. 

## <a name="configuration-steps"></a>Passos de configuração

Pode configurar dispositivos de associados ao Azure AD híbrido para vários tipos de plataformas de dispositivos do Windows. Este tópico inclui os passos necessários para todos os cenários de configuração típica.  

Utilize a tabela seguinte para obter uma descrição geral dos passos necessários para o seu cenário:  



| Passos                                      | Sincronização de hash de atual e a palavra-passe do Windows | Windows atual e a Federação | Windows de nível inferior |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Configurar o ponto de ligação de serviço | ![Marcar][1]                            | ![Marcar][1]                    | ![Marcar][1]        |
| Instalação de emissão de afirmações           |                                        | ![Marcar][1]                    | ![Marcar][1]        |
| Permitir que os dispositivos não Windows 10      |                                        |                                | ![Marcar][1]        |
| Verifique se os dispositivos associados          | ![Marcar][1]                            | ![Marcar][1]                    | ![Marcar][1]        |



## <a name="configure-service-connection-point"></a>Configurar o ponto de ligação de serviço

O objeto de (SCP) do ponto de ligação de serviço é utilizado pelos seus dispositivos durante o registo para detetar informações de inquilino do Azure AD. No seu local Active Directory (AD), o objeto de SCP para dispositivos associados ao Azure AD híbrido tem de existir na configuração da atribuição de nomes de partição de contexto da floresta do computador. Há apenas um contexto de nomenclatura de configuração por floresta. Numa configuração várias floresta do Active Directory, o ponto de ligação de serviço tem de existir em todas as florestas que contêm computadores associados a um domínio.

Pode utilizar o [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) cmdlet para obter o contexto de nomenclatura de configuração da sua floresta.  

Para uma floresta com o nome de domínio do Active Directory *fabrikam.com*, o contexto de nomenclatura de configuração é:

`CN=Configuration,DC=fabrikam,DC=com`

Na sua floresta, o objeto de SCP para o registo automático de dispositivos associados a um domínio está localizado em:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Dependendo de como tiver implementado o Azure AD Connect, o objeto de SCP poderá ter já sido configurado.
Pode verificar a existência do objeto e recuperar os valores de deteção com o seguinte script do Windows PowerShell: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

O **$scp. Palavras-chave** resultado mostra as informações de inquilino do Azure AD, por exemplo:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Se o ponto de ligação de serviço não existir, pode criá-la ao executar o `Initialize-ADSyncDomainJoinedComputerSync` cmdlet no seu servidor do Azure AD Connect. Credencial de administrador de empresa é necessária para executar este cmdlet.  
O cmdlet:

- Cria o ponto de ligação de serviço na floresta do Active Directory do Azure AD Connect está ligado. 
- Requer que especifique o `AdConnectorAccount` parâmetro. Esta é a conta que está configurada como o Active Directory, ligar a conta de conector no Azure AD. 


O script seguinte mostra um exemplo para utilizar o cmdlet. Nesse script, `$aadAdminCred = Get-Credential` exige que escreva um nome de utilizador. Tem de fornecer o nome de utilizador no formato de nome principal (UPN) do utilizador (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

O `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:

- Utiliza o módulo do PowerShell do Active Directory e as ferramentas do AD DS, que dependem de serviços da Web do Active Directory em execução num controlador de domínio. Serviços da Web do Active Directory é suportada em controladores de domínio com o Windows Server 2008 R2 e posterior.
- Só é suportado pelos **MSOnline módulo do PowerShell versão 1.1.166.0**. Para transferir este módulo, utilize isto [link](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/).   
- Se não estiverem instaladas as ferramentas do AD DS, o `Initialize-ADSyncDomainJoinedComputerSync` irá falhar.  As ferramentas do AD DS podem ser instaladas através do Gestor de servidor em ferramentas de administração de função de recursos - ferramentas de administração de servidor remoto.

Para controladores de domínio a executar o Windows Server 2008 ou versões anteriores, utilize o script abaixo para criar o ponto de ligação de serviço.

Numa configuração de várias floresta, deve utilizar o seguinte script para criar o ponto de ligação de serviço em cada floresta onde existem computadores:
 
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

No script acima,

- `$verifiedDomain = "contoso.com"` é um marcador de posição que é necessário substituir com um dos seus nomes de domínio verificado no Azure AD. Terá de ter o nome de domínio antes de poder utilizá-lo.

Para obter mais detalhes sobre os nomes de domínio verificado, consulte [adicionar um nome de domínio personalizado ao Azure Active Directory](../active-directory-domains-add-azure-portal.md).  
Para obter uma lista dos seus domínios de empresa verificado, pode utilizar o [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0) cmdlet. 

![Get-AzureADDomain](./media/hybrid-azuread-join-manual-steps/01.png)

## <a name="setup-issuance-of-claims"></a>Instalação de emissão de afirmações

Num Azure federado configuração do AD, dispositivos dependem de serviços de Federação do Active Directory (AD FS) ou uma parte 3 no local-serviço de Federação para autenticar para o Azure AD. Os dispositivos serão autenticados para obter um token de acesso para registar com o serviço de registo de dispositivos de diretório Active Directory do Azure (Azure DRS).

Windows efetuar a autenticação de dispositivos atuais com a autenticação integrada do Windows para um ponto de extremidade do WS-Trust Active Directory (versões de 1,3 ou 2005) alojado pelo serviço de Federação no local.

> [!NOTE]
> Ao utilizar o AD FS, seja **adfs/serviços/confiança/13/windowstransport** ou **adfs/serviços/confiança/2005/windowstransport** tem de estar ativada. Se estiver a utilizar o Proxy de autenticação da Web, certifique-se também que este ponto final é publicado através do proxy. Pode ver os pontos finais estão ativados através da consola de gestão do AD FS sob **serviço > pontos finais**.
>
>Se não tiver o AD FS que o seu serviço de Federação no local, siga as instruções do seu fornecedor para se certificar de que eles oferecem suporte a WS-Trust 1.3 ou pontos finais de 2005 e estas são publicadas através do ficheiro de troca de metadados (MEX).

As seguintes declarações tem de existir no token recebido pelo Azure DRS para o registo do dispositivo para concluir. DRS do Azure irá criar um objeto de dispositivo no Azure AD com algumas destas informações que, em seguida, são utilizadas pelo Azure AD Connect para associar o objeto de dispositivo recentemente criada com o computador conta no local.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Se tiver mais de um nome de domínio verificado, terá de fornecer a declaração seguinte para computadores:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Se já está a emitir uma afirmação de ImmutableID (por exemplo, o ID de início de sessão alternativo) tem de fornecer uma afirmação correspondente para computadores:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

Nas seções a seguir, é possível encontrar informações sobre:
 
- Os valores de que cada declaração deve ter
- Como uma definição de teria o seguinte aspeto no AD FS

A definição de ajuda-o para verificar se os valores estão presentes ou se terá de criá-los.

> [!NOTE]
> Se não utilizar o AD FS para o seu servidor de Federação no local, siga as instruções do seu fornecedor para criar a configuração adequada para emitir essas declarações.

### <a name="issue-account-type-claim"></a>Declaração de tipo de conta do problema

**`http://schemas.microsoft.com/ws/2012/01/accounttype`** -Esta afirmação tem de conter um valor de **DJ**, que identifica o dispositivo como um computador associado a um domínio. No AD FS, pode adicionar uma regra de transformação de emissão que tem esta aparência:

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

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>Emitir objectGUID do computador conta local

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** -Esta afirmação tem de conter o **objectGUID** valor da conta de computador no local. No AD FS, pode adicionar uma regra de transformação de emissão que tem esta aparência:

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
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>ObjectSID do problema do computador conta no local

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** -Esta afirmação tem de conter o **objectSid** valor da conta de computador no local. No AD FS, pode adicionar uma regra de transformação de emissão que tem esta aparência:

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

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Emitir issuerID para computador quando vários verificado nomes de domínio no Azure AD

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** -Esta afirmação tem de conter o identificador URI (Uniform Resource) de qualquer um dos nomes de domínio verificado que se ligar ao serviço de Federação no local (AD FS ou 3rd party) emitir o token. No AD FS, pode adicionar regras de transformação de emissão que parecem com aqueles abaixo por essa ordem específica após os acima. Tenha em atenção que uma regra para emitir explicitamente a regra para os utilizadores é necessária. Em regras abaixo, é adicionada uma regra de primeiro identificar o utilizador vs. a autenticação de computador.

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

- `<verified-domain-name>` é um marcador de posição que é necessário substituir com um dos seus nomes de domínio verificado no Azure AD. Por exemplo, valor = "http://contoso.com/adfs/services/trust/"



Para obter mais detalhes sobre os nomes de domínio verificado, consulte [adicionar um nome de domínio personalizado ao Azure Active Directory](../active-directory-domains-add-azure-portal.md).  

Para obter uma lista dos seus domínios de empresa verificado, pode utilizar o [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet. 

![Get-MsolDomain](./media/hybrid-azuread-join-manual-steps/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Emitir ImmutableID para computador quando existe um para os utilizadores (por exemplo, início de sessão alternativo ID está definido)

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** -Esta afirmação tem de conter um valor válido para computadores. No AD FS, pode criar uma regra de transformação de emissão da seguinte forma:

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

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>Script de programa auxiliar para criar regras de transformação de emissão do AD FS

O script seguinte ajuda-o com a criação de emissão descritas acima de regras de transformação.

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

- Este script acrescenta as regras para as regras existentes. Não execute o script duas vezes porque o conjunto de regras seria adicionado duas vezes. Certifique-se de que não existem regras correspondentes existem para essas declarações (sob as condições de correspondentes) antes de executar o script novamente.

- Se tiver vários nomes de domínio verificado (como mostrado no portal do Azure AD ou através do cmdlet Get-MsolDomains), defina o valor da **$multipleVerifiedDomainNames** no script para **$true**. Além disso, certifique-se de que remova qualquer afirmação issuerid existente, que poderá ter sido criada pelo Azure AD Connect ou através de outros meios. Eis um exemplo para esta regra:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Se já ter emitido uma **ImmutableID** de afirmação para contas de utilizador, defina o valor de **$immutableIDAlreadyIssuedforUsers** no script para **$true**.

## <a name="enable-windows-down-level-devices"></a>Permitir que os dispositivos de nível inferior do Windows

Se alguns dos seus dispositivos associados a um domínio são dispositivos de nível inferior do Windows, terá de:

- Defina uma política do Azure AD para permitir que os utilizadores registar dispositivos.
 
- Configurar o seu serviço de Federação no local para emitir afirmações para suportar **autenticação integrada do Windows (IWA)** para registo de dispositivos.
 
- Adicione o ponto de final de autenticação de dispositivo do Azure AD para as zonas da Intranet locais para evitar avisos de certificado ao autenticar o dispositivo.

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Definir a política do Azure AD para permitir que os utilizadores registar dispositivos

Para registar dispositivos de nível inferior do Windows, terá de certificar-se de que a definição para permitir que os utilizadores registar dispositivos no Azure AD está definida. No portal do Azure, pode encontrar esta definição em:

`Azure Active Directory > Users and groups > Device settings`
    
A seguinte política tem de ser definida **todos os**: **os utilizadores podem registar os seus dispositivos com o Azure AD**

![Registar dispositivos](./media/hybrid-azuread-join-manual-steps/23.png)


### <a name="configure-on-premises-federation-service"></a>Configurar o serviço de Federação no local 

O serviço de Federação no local tem de suportar a emitir a **authenticationmethod** e **wiaormultiauthn** afirmações quando recebe um pedido de autenticação à parte da entidade confiadora do Azure AD que contém um resouce_params parâmetro com um valor codificado como mostrado abaixo:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

Quando é fornecido este pedido, o serviço de Federação no local tem de autenticar o usuário usando autenticação integrada do Windows e após a conclusão bem-sucedida, ela tem de emitir as afirmações de duas seguintes:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

No AD FS, tem de adicionar uma regra de transformação de emissão que o método de autenticação pass-through.  

**Para adicionar esta regra:**

1. Na consola de gestão do AD FS, aceda a `AD FS > Trust Relationships > Relying Party Trusts`.
2. O objeto de confiança de terceiros entidade confiadora da plataforma de identidade do Microsoft Office 365 com o botão direito e, em seguida, selecione **editar regras de afirmação**.
3. Sobre o **regras de transformação de emissão** separador, selecione **Adicionar regra**.
4. Na **regra de afirmação** lista de modelo, selecione **enviar afirmações utilizando uma regra personalizada**.
5. Selecione **Seguinte**.
6. Na **nome da regra de afirmação** , escreva **regra de afirmação de método de autenticação**.
7. Na **regra de afirmação** , escreva a seguinte regra:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. No servidor de Federação, escreva o comando do PowerShell abaixo depois de substituir **\<RPObjectName\>** com o nome de objeto de terceiros entidade confiadora para seu objeto de confiança de terceiros da entidade confiadora do AD do Azure. Este objeto é normalmente denominado **plataforma de identidade do Microsoft Office 365**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a>Adicionar o ponto de final de autenticação de dispositivo do Azure AD para a zona da Intranet Local

Para evitar certificado pede-lhe quando os utilizadores nos dispositivos Registre-se a autenticação para o Azure AD, pode enviar por push uma política para os seus dispositivos associados a um domínio para adicionar o seguinte URL para a zona de Local Intranet no Internet Explorer:

`https://device.login.microsoftonline.com`


## <a name="verify-joined-devices"></a>Verifique se os dispositivos associados

Pode verificar dispositivos associados com êxito na sua organização com o [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet no [módulo Azure Active Directory PowerShell](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

O resultado deste cmdlet mostra os dispositivos que são registados e associados com o Azure AD. Para obter todos os dispositivos, utilize o **-todos os** parâmetro e, em seguida, filtrá-los utilizando o **deviceTrustType** propriedade. Associado a um domínio dispositivos têm um valor de **associados a um domínio**.



## <a name="troubleshoot-your-implementation"></a>Resolver problemas relacionados com a implementação

Se estiver tendo problemas com a conclusão da híbrida associação do Azure AD para o domínio dispositivos associados ao Windows, consulte:

- [Associação ao Azure AD híbrido resolução de problemas para dispositivos atuais do Windows](troubleshoot-hybrid-join-windows-current.md)
- [Resolução de problemas de associação do Azure AD híbrido para dispositivos de nível inferior do Windows](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Passos Seguintes

* [Introdução à gestão de dispositivos no Azure Active Directory](overview.md)



<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual-steps/12.png
