---
title: Antes de implementar o serviço de aplicações na pilha do Azure | Microsoft Docs
description: Passos para concluir antes de implementar o serviço de aplicações na pilha do Azure
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2018
ms.author: anwestg
ms.openlocfilehash: 37d6ee2f047768f08ea7a113b7d97911d58a46e2
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110580"
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Antes de começar com o serviço de aplicações na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Antes de implementar o serviço de aplicações do Azure na pilha do Azure, tem de concluir os passos de pré-requisitos neste artigo.

> [!IMPORTANT]
> Aplicar a atualização 1804 ao seu sistema de pilha do Azure integrado ou implementar o Kit de desenvolvimento de pilha de Azure mais recente (ASDK) antes de implementar 1.2 de serviço de aplicações do Azure.

## <a name="download-the-installer-and-helper-scripts"></a>Transferir os instalador e do programa auxiliar de scripts

1. Transferir o [do serviço de aplicações em scripts de programa auxiliar de implementação do Azure pilha](https://aka.ms/appsvconmashelpers).
2. Transferir o [do serviço de aplicações no instalador do Azure pilha](https://aka.ms/appsvconmasinstaller).
3. Extraia os ficheiros a partir do ficheiro. zip de scripts de programa auxiliar. Os seguintes ficheiros e pastas são extraídas:

   - Common.ps1
   - Create-AADIdentityApp.ps1
   - Create-ADFSIdentityApp.ps1
   - Create-AppServiceCerts.ps1
   - Get-AzureStackRootCert.ps1
   - Remove-AppService.ps1
   - Pasta de módulos
     - GraphAPI.psm1

## <a name="high-availability"></a>Elevada disponibilidade

A atualização do Azure pilha 1802 foi adicionado suporte para domínios de falhas. Novas implementações do serviço de aplicações do Azure na pilha do Azure irão ser distribuídas em vários domínios de falhas e fornecem tolerância a falhas.

Para as implementações existentes do serviço de aplicações do Azure na pilha do Azure, que foram implementadas antes da atualização da 1802, consulte o [rebalancear um fornecedor de recursos do serviço de aplicações em vários domínios de falhas](azure-stack-app-service-fault-domain-update.md) artigo.

Além disso, implemente o servidor de ficheiros necessários e instâncias do SQL Server numa configuração altamente disponível.

## <a name="get-certificates"></a>Obter certificados

### <a name="azure-resource-manager-root-certificate-for-azure-stack"></a>Certificado de raiz do Gestor de recursos do Azure para a pilha do Azure

Abra uma sessão do PowerShell elevada num computador que pode alcançar o ponto final com privilégios no anfitrião de Kit de desenvolvimento de pilha do Azure ou sistema integrada de pilha do Azure.

Execute o *Get-AzureStackRootCert.ps1* script da pasta onde extraiu os scripts de programa auxiliar. O script cria um certificado de raiz na mesma pasta que o script que necessita de serviço de aplicações para a criação de certificados.

Quando executar o seguinte comando do PowerShell, terá de fornecer o ponto final com privilégios e as credenciais para o AzureStack\CloudAdmin.

```PowerShell
    Get-AzureStackRootCert.ps1
```

#### <a name="get-azurestackrootcertps1-script-parameters"></a>Parâmetros do script Get-AzureStackRootCert.ps1

| Parâmetro | Obrigatório ou opcional | Valor predefinido | Descrição |
| --- | --- | --- | --- |
| PrivilegedEndpoint | Necessário | AzS-ERCS01 | Ponto final com privilégios |
| CloudAdminCredential | Necessário | AzureStack\CloudAdmin | Credencial da conta de domínio para os administradores de nuvem de pilha do Azure |

### <a name="certificates-required-for-asdk-deployment-of-azure-app-service"></a>Certificados necessários para a implementação de ASDK do App Service do Azure

O *criar AppServiceCerts.ps1* script funciona com a autoridade de certificação de pilha do Azure para criar os certificados de quatro que necessita do serviço de aplicações.

| Nome de ficheiro | Utilizar |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | Certificado SSL predefinido do Serviço de Aplicações |
| api.appservice.local.azurestack.external.pfx | Certificado de SSL de API do serviço de aplicações |
| ftp.appservice.local.azurestack.external.pfx | Certificado SSL de publicador do serviço de aplicações |
| sso.appservice.local.azurestack.external.pfx | Certificado de aplicação de identidade de serviço de aplicações |

Para criar os certificados, siga estes passos:

1. A iniciar sessão para o anfitrião de Kit de desenvolvimento de pilha do Azure utilizando a conta de AzureStack\AzureStackAdmin.
2. Abra uma sessão elevada do PowerShell.
3. Execute o *criar AppServiceCerts.ps1* script da pasta onde extraiu os scripts de programa auxiliar. Este script cria quatro certificados na mesma pasta que o script que necessita de serviço de aplicações para a criação de certificados.
4. Introduza uma palavra-passe para proteger os ficheiros. pfx e tome nota do mesmo. Terá de introduzi-lo no serviço de aplicações no instalador da pilha do Azure.

#### <a name="create-appservicecertsps1-script-parameters"></a>Parâmetros do script AppServiceCerts.ps1 criar

| Parâmetro | Obrigatório ou opcional | Valor predefinido | Descrição |
| --- | --- | --- | --- |
| pfxPassword | Necessário | Null | Palavra-passe que o ajuda a proteger a chave privada do certificado |
| DomainName | Necessário | local.azurestack.external | Sufixo do Azure de região e o domínio de pilha |

### <a name="certificates-required-for-azure-stack-production-deployment-of-azure-app-service"></a>Certificados necessários para a implementação de produção do Azure pilha do App Service do Azure

Para executar o fornecedor de recursos na produção, tem de fornecer os seguintes certificados:

- Certificado de domínio predefinido
- Certificado da API
- Certificado de publicação
- Certificado de identidade

#### <a name="default-domain-certificate"></a>Certificado de domínio predefinido

O certificado de domínio predefinido é colocado na função de Front-End. As aplicações de utilizador para o pedido de domínio de caráter universal ou predefinição App Service do Azure utilizam este certificado. O certificado também é utilizado para operações de controlo de origem (Kudu).

O certificado tem de estar no formato. pfx e deve ser um certificado de caráter universal de três requerente. Este requisito permite que um certificado para o domínio predefinido e o ponto final SCM para operações de controlo de origem.

| Formato | Exemplo |
| --- | --- |
| \*.appservice.\<region\>.\<DomainName\>.\<extension\> | \*.appservice.redmond.azurestack.external |
| \*.scm.appservice.<region>.<DomainName>.<extension> | \*.scm.appservice.redmond.azurestack.external |
| \*.sso.appservice.<region>.<DomainName>.<extension> | \*.sso.appservice.redmond.azurestack.external |

#### <a name="api-certificate"></a>Certificado da API

O certificado da API é colocado na função de gestão. O fornecedor de recursos utiliza-o para o ajudar a proteger chamadas de API. O certificado para a publicação tem de conter um assunto que corresponde a entrada DNS de API.

| Formato | Exemplo |
| --- | --- |
| api.appservice.\<region\>.\<DomainName\>.\<extension\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Certificado de publicação

O certificado para a função de editor protege o tráfego FTPS para os proprietários da aplicação quando estes carregam conteúdo. O certificado para a publicação tem de conter um assunto que corresponde a entrada FTPS DNS.

| Formato | Exemplo |
| --- | --- |
| ftp.appservice.\<region\>.\<DomainName\>.\<extension\> | ftp.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Certificado de identidade

Permite que o certificado para a aplicação de identidade:

- Integração entre o Azure Active Directory (Azure AD) ou serviços de Federação do Active Directory (AD FS) diretório, pilha do Azure e do serviço de aplicações para suportar a integração com o fornecedor de recursos de computação.
- Único início de sessão cenários para ferramentas de programador avançadas no serviço de aplicações do Azure na pilha do Azure.

O certificado para a identidade tem de conter um assunto que corresponde ao formato seguinte.

| Formato | Exemplo |
| --- | --- |
| sso.appservice.\<region\>.\<DomainName\>.\<extension\> | sso.appservice.redmond.azurestack.external |

## <a name="virtual-network"></a>Rede virtual

App Service do Azure na pilha do Azure permite-lhe implementar o fornecedor de recursos para uma rede virtual existente ou permite-lhe criar uma rede virtual como parte da implementação. Utilizar uma rede virtual existente permite a utilização de IPs interno para estabelecer ligação com o servidor de ficheiros e SQL server necessários para o serviço de aplicações do Azure na pilha do Azure. A rede virtual deve ser configurada com o intervalo de endereços e sub-redes seguintes antes de instalar o App Service do Azure na pilha do Azure:

Rede virtual - /16

Sub-redes

- ControllersSubnet /24
- ManagementServersSubnet /24
- FrontEndsSubnet /24
- PublishersSubnet /24
- WorkersSubnet /21

## <a name="prepare-the-file-server"></a>Preparar o servidor de ficheiros

App Service do Azure requer a utilização de um servidor de ficheiros. Para implementações de produção, o servidor de ficheiros tem de ser configurado para ser de elevada disponibilidade e capacidade de processamento de falhas.

Para apenas para implementações Kit de desenvolvimento de pilha do Azure, pode utilizar o [modelo de implementação Azure Resource Manager exemplo](https://aka.ms/appsvconmasdkfstemplate) para implementar um servidor de ficheiros de nó único configurado. O servidor de ficheiros de nó único será num grupo de trabalho.

>[!IMPORTANT]
> Se optar por implementar o serviço de aplicações numa rede Virtual existente, o servidor de ficheiros devem ser implementado para uma sub-rede separada do serviço de aplicações.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Aprovisionar grupos e contas no Active Directory

1. Crie os seguintes grupos de segurança global do Active Directory:

   - FileShareOwners
   - FileShareUsers

2. Crie as seguintes contas do Active Directory como contas de serviço:

   - FileShareOwner
   - FileShareUser

   Como procedimento de segurança melhor prática, os utilizadores para estas contas (e para todas as funções da web) deve ser exclusivo e ter forte nomes de utilizador e palavras-passe. Defina as palavras-passe com as seguintes condições:

   - Ativar **palavra-passe nunca expira**.
   - Ativar **utilizador não é possível alterar a palavra-passe**.
   - Desativar **o utilizador deve alterar a palavra-passe no próximo início de sessão**.

3. Adicione as contas para as associações da seguinte forma:

   - Adicionar **FileShareOwner** para o **FileShareOwners** grupo.
   - Adicionar **FileShareUser** para o **FileShareUsers** grupo.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Aprovisionar grupos e contas num grupo de trabalho

>[!NOTE]
> Quando estiver a configurar um servidor de ficheiros, execute todos os seguintes comandos de um **linha de comandos de administrador**. <br>***Não utilize o PowerShell.***

Quando utiliza o modelo Azure Resource Manager, os utilizadores já se encontram criados.

1. Execute os seguintes comandos para criar as contas FileShareOwner e FileShareUser. Substitua `<password>` com os seus próprios valores.

    ``` DOS
    net user FileShareOwner <password> /add /expires:never /passwordchg:no
    net user FileShareUser <password> /add /expires:never /passwordchg:no
    ```
2. Defina as palavras-passe para as contas para nunca expirar, executando os comandos WMIC seguintes:

    ``` DOS
    WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
    WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
    ```
3. Criar os grupos locais FileShareUsers e FileShareOwners e adicionar as contas no primeiro passo aos mesmos:

    ``` DOS
    net localgroup FileShareUsers /add
    net localgroup FileShareUsers FileShareUser /add
    net localgroup FileShareOwners /add
    net localgroup FileShareOwners FileShareOwner /add
    ```

### <a name="provision-the-content-share"></a>Aprovisionar a partilha de conteúdo

A partilha de conteúdo contém conteúdo do Web site de inquilino. O procedimento para aprovisionar a partilha de conteúdo num servidor de ficheiros única é o mesmo para ambientes do Active Directory e o grupo de trabalho. Mas é diferente para um cluster de ativação pós-falha no Active Directory.

#### <a name="provision-the-content-share-on-a-single-file-server-active-directory-or-workgroup"></a>Aprovisionar a partilha de conteúdo num servidor único ficheiro (Active Directory ou grupo de trabalho)

No servidor de ficheiros único, execute os seguintes comandos numa linha de comandos elevada. Substitua o valor para `C:\WebSites` com os caminhos correspondentes no seu ambiente.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="add-the-fileshareowners-group-to-the-local-administrators-group"></a>Adicione o grupo de FileShareOwners ao grupo de administradores local

Gestão remota do Windows funcionar corretamente, tem de adicionar o grupo de FileShareOwners ao grupo de administradores local.

#### <a name="active-directory"></a>Active Directory

Execute os seguintes comandos numa linha de comandos elevada no servidor de ficheiros ou em cada servidor de ficheiros que age como um nó de cluster de ativação pós-falha. Substitua o valor para `<DOMAIN>` com o nome de domínio que pretende utilizar.

```DOS
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Grupo de trabalho

Execute o seguinte comando numa linha de comandos elevada no servidor de ficheiros:

```DOS
net localgroup Administrators FileShareOwners /add
```

### <a name="configure-access-control-to-the-shares"></a>Configurar o controlo de acesso para as partilhas

Execute os seguintes comandos numa linha de comandos elevada no servidor de ficheiros ou o nó de cluster de ativação pós-falha, o que é o proprietário atual do recurso de cluster. Substitua os valores na italics com valores que são específicos para o seu ambiente.

#### <a name="active-directory"></a>Active Directory

```DOS
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Grupo de trabalho

```DOS
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server-instance"></a>Preparar a instância do SQL Server

Para o App Service do Azure em bases de dados de medição e alojamento de pilha do Azure, tem de preparar uma instância do SQL Server para armazenar as bases de dados do serviço de aplicações.

Para implementações do Kit de desenvolvimento de pilha do Azure, pode utilizar o SQL Server Express 2014 SP2 ou posterior.

Para produção e fins de elevada disponibilidade, deve utilizar uma versão completa do SQL Server 2014 SP2 ou posterior, ativar a autenticação de modo misto e implementar uma [configuração altamente disponíveis](https://docs.microsoft.com/sql/sql-server/failover-clusters/high-availability-solutions-sql-server).

Instância do SQL Server para o Azure App Service na pilha do Azure tem de ser acessível a partir de todas as funções do serviço de aplicações. Pode implementar o SQL Server dentro da subscrição do fornecedor predefinida na pilha do Azure. Pode ainda utilizar a infraestrutura existente na sua organização (desde que está estabelecida conetividade à pilha do Azure). Se estiver a utilizar uma imagem do Azure Marketplace, lembre-se configurar a firewall em conformidade.

>[!NOTE]
> Um número de imagens da máquina virtual IaaS do SQL Server está disponível através da funcionalidade de gestão do Marketplace. Certifique-se de que sempre transferir a versão mais recente da extensão de IaaS SQL antes de implementar uma VM com um item do Marketplace. As imagens do SQL Server são as mesmas que as VMs de SQL que estão disponíveis no Azure. Para as VMs de SQL criadas a partir destas imagens, a extensão de IaaS e as correspondentes melhoramentos portais fornecer funcionalidades como a aplicação de patches automática e as capacidades de cópia de segurança.
>
Para qualquer uma das funções do SQL Server, pode utilizar uma instância predefinida ou numa instância nomeada. Se utilizar uma instância nomeada, não se esqueça de iniciar o serviço do SQL Server Browser e abrir a porta 1434 manualmente.

>[!IMPORTANT]
> Se optar por implementar o serviço de aplicações numa rede Virtual existente do SQL Server devem ser implementado para uma sub-rede separada do serviço de aplicações e o servidor de ficheiros.
>

## <a name="create-an-azure-active-directory-application"></a>Criar uma aplicação do Azure Active Directory

Configure um principal de serviço do Azure AD para suportar as seguintes operações:

- Conjunto de dimensionamento de máquina virtual integração em camadas de trabalho.
- SSO para as ferramentas de Programador de portal e avançadas das funções do Azure.

Estes passos aplicam-se apenas a ambientes de pilha de Azure protegida por AD do Azure.

Os administradores tem de configurar o SSO para:

- Ative as ferramentas de programador avançadas no serviço de aplicações (Kudu).
- Ative a utilização da experiência de portal das funções do Azure.

Siga estes passos.

1. Abra uma instância do PowerShell como azurestack\AzureStackAdmin.
2. Vá para a localização dos scripts que transferiu e extraiu no [passo pré-requisitos](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Instale o PowerShell para a pilha do Azure](azure-stack-powershell-install.md).
4. Execute o **criar AADIdentityApp.ps1** script. Quando lhe for pedido, introduza o ID de inquilino do Azure AD que estiver a utilizar para a implementação de pilha do Azure. Por exemplo, introduza **myazurestack.onmicrosoft.com**.
5. No **credencial** janela, introduza a sua conta de administrador de serviço do Azure AD e a palavra-passe. Selecione **OK**.
6. Introduza o caminho do ficheiro de certificado e a palavra-passe de certificado para o [certificado que criou anteriormente](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). O certificado que criou para este passo por predefinição é **sso.appservice.local.azurestack.external.pfx**.
7. O script cria uma nova aplicação na instância do inquilino do Azure AD. Anote o ID da aplicação que é devolvido na saída do PowerShell. Terá destas informações durante a instalação.
8. Abra uma nova janela do browser e inicie sessão para o [portal do Azure](https://portal.azure.com) como o administrador de serviço do Azure Active Directory.
9. Abra o fornecedor de recursos do Azure AD.
10. Selecione **registos de aplicação**.
11. Procure o ID da aplicação devolvido como parte do passo 7. Uma aplicação de serviço de aplicações está listada.
12. Selecione **aplicação** na lista.
13. Selecione **definições**.
14. Selecione **as permissões necessárias** > **conceder permissões** > **Sim**.

```PowerShell
    Create-AADIdentityApp.ps1
```

| Parâmetro | Obrigatório ou opcional | Valor predefinido | Descrição |
| --- | --- | --- | --- |
| DirectoryTenantName | Necessário | Null | ID de inquilino do Azure AD Forneça o GUID ou uma cadeia. Um exemplo é myazureaaddirectory.onmicrosoft.com. |
| AdminArmEndpoint | Necessário | Null | Ponto final de administração do Azure Resource Manager. Um exemplo é adminmanagement.local.azurestack.external. |
| TenantARMEndpoint | Necessário | Null | Ponto de final do inquilino do Azure Resource Manager. Um exemplo é management.local.azurestack.external. |
| AzureStackAdminCredential | Necessário | Null | Credencial de administrador de serviço de Azure AD. |
| CertificateFilePath | Necessário | Null | **Caminho completo do** para o ficheiro de certificado de aplicação de identidade gerado anteriormente. |
| CertificatePassword | Necessário | Null | Palavra-passe que o ajuda a proteger a chave privada do certificado. |

## <a name="create-an-active-directory-federation-services-application"></a>Criar uma aplicação de serviços de Federação do Active Directory

Para ambientes de pilha do Azure protegidos pelo AD FS, tem de configurar um principal de serviço do AD FS para suportar as seguintes operações:

- Conjunto de dimensionamento de máquina virtual integração em camadas de trabalho.
- SSO para as ferramentas de Programador de portal e avançadas das funções do Azure.

Os administradores tem de configurar o SSO para:

- Configure um principal de serviço para a integração de conjunto de dimensionamento de máquina virtual em camadas de trabalho.
- Ative as ferramentas de programador avançadas no serviço de aplicações (Kudu).
- Ative a utilização da experiência de portal das funções do Azure.

Siga estes passos.

1. Abra uma instância do PowerShell como azurestack\AzureStackAdmin.
2. Vá para a localização dos scripts que transferiu e extraiu no [passo pré-requisitos](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Instale o PowerShell para a pilha do Azure](azure-stack-powershell-install.md).
4. Execute o **criar ADFSIdentityApp.ps1** script.
5. No **credencial** janela, introduza a sua conta de administrador de nuvem do AD FS e a palavra-passe. Selecione **OK**.
6. Forneça o caminho do ficheiro de certificado e a palavra-passe de certificado para o [certificado que criou anteriormente](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). O certificado que criou para este passo por predefinição é **sso.appservice.local.azurestack.external.pfx**.

```PowerShell
    Create-ADFSIdentityApp.ps1
```

| Parâmetro | Obrigatório ou opcional | Valor predefinido | Descrição |
| --- | --- | --- | --- |
| AdminArmEndpoint | Necessário | Null | Ponto final de administração do Azure Resource Manager. Um exemplo é adminmanagement.local.azurestack.external. |
| PrivilegedEndpoint | Necessário | Null | Ponto final com privilégios. Um exemplo é AzS ERCS01. |
| CloudAdminCredential | Necessário | Null | Credenciais de conta de domínio para os administradores de nuvem de pilha do Azure. Um exemplo é Azurestack\CloudAdmin. |
| CertificateFilePath | Necessário | Null | **Caminho completo do** ao ficheiro PFX de certificado a aplicação de identidade. |
| CertificatePassword | Necessário | Null | Palavra-passe que o ajuda a proteger a chave privada do certificado. |

## <a name="next-steps"></a>Passos Seguintes

[Instalar o fornecedor de recursos do serviço de aplicações](azure-stack-app-service-deploy.md)
