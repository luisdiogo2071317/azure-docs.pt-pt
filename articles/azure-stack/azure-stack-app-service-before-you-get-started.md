---
title: "Antes de implementar o serviço de aplicações na pilha do Azure | Microsoft Docs"
description: "Passos para concluir antes de implementar o serviço de aplicações na pilha do Azure"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: anwestg
ms.openlocfilehash: d4398d1c292548b08d91d70a8ba35b31234c5d5f
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Antes de começar com o serviço de aplicações na pilha do Azure

Antes de implementar o serviço de aplicações do Azure na pilha do Azure, tem de concluir os pré-requisitos neste artigo.

## <a name="download-the-installer-and-helper-scripts"></a>Transferir os instalador e do programa auxiliar de scripts

1. Transferir o [do serviço de aplicações em scripts de programa auxiliar de implementação do Azure pilha](https://aka.ms/appsvconmashelpers).
2. Transferir o [do serviço de aplicações no instalador do Azure pilha](https://aka.ms/appsvconmasinstaller).
3. Extraia os ficheiros a partir do ficheiro. zip de scripts de programa auxiliar. Os seguintes ficheiros e a estrutura da pasta são apresentados:
   - Common.ps1
   - AADIdentityApp.ps1 criar
   - ADFSIdentityApp.ps1 criar
   - AppServiceCerts.ps1 criar
   - Get-AzureStackRootCert.ps1
   - Remover AppService.ps1
   - Módulos
     - GraphAPI.psm1
    
## <a name="prepare-for-high-availability"></a>Preparar para elevada disponibilidade

Atualmente não é possível ao App Service do Azure na pilha do Azure oferecem elevada disponibilidade porque a pilha do Azure implementa cargas de trabalho num único domínio de falhas.

Para preparar o serviço de aplicações do Azure na pilha do Azure para elevada disponibilidade, implemente o servidor de ficheiros necessários e a instância do SQL Server numa configuração altamente disponível. Quando a pilha do Azure suporta vários domínios de falhas, iremos fornecem orientações sobre como ativar o serviço de aplicações do Azure na pilha do Azure numa configuração altamente disponível.


## <a name="get-certificates"></a>Obter certificados

### <a name="certificates-required-for-the-azure-stack-development-kit"></a>Certificados necessários para o Kit de desenvolvimento de pilha do Azure

O script primeiro funciona com a autoridade de certificação de pilha do Azure para criar quatro certificados que necessita do serviço de aplicações:

| Nome de ficheiro | Utilizar |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | Certificado SSL do serviço de aplicações predefinido |
| API.appservice.local.azurestack.external.pfx | Certificado de SSL de API do serviço de aplicações |
| FTP.appservice.local.azurestack.external.pfx | Certificado SSL de publicador do serviço de aplicações |
| SSO.appservice.local.azurestack.external.pfx | Certificado de aplicação de identidade de serviço de aplicações |

Executar o script no anfitrião do Kit de desenvolvimento de pilha do Azure e certifique-se de que está a executar o PowerShell como azurestack\CloudAdmin:

1. Numa sessão do PowerShell em execução como azurestack\AzureStackAdmin, execute o script de criar AppServiceCerts.ps1 da pasta onde extraiu os scripts de programa auxiliar. O script cria quatro certificados na mesma pasta que o script que necessita de serviço de aplicações para a criação de certificados.
2. Introduza uma palavra-passe para proteger os ficheiros. pfx e tome nota do mesmo. Tem de a introduzir no serviço de aplicações no instalador da pilha do Azure.

#### <a name="create-appservicecertsps1-parameters"></a>Parâmetros de AppServiceCerts.ps1 criar

| Parâmetro | Necessária ou opcional | Valor predefinido | Descrição |
| --- | --- | --- | --- |
| PfxPassword | Necessário | Valor nulo | Palavra-passe que o ajuda a proteger a chave privada do certificado |
| domainName | Necessário | local.azurestack.external | Sufixo do Azure de região e o domínio de pilha |

### <a name="certificates-required-for-a-production-deployment-of-azure-app-service-on-azure-stack"></a>Certificados necessários para uma implementação de produção do serviço de aplicações do Azure na pilha do Azure

Operar o fornecedor de recursos na produção, tem de fornecer os seguintes quatro certificados.

#### <a name="default-domain-certificate"></a>Certificado de domínio predefinido

O certificado de domínio predefinido é colocado na função de Front-End. As aplicações de utilizador para pedidos de domínio de caráter universal ou predefinição App Service do Azure utilizam este certificado. O certificado também é utilizado para operações de controlo de origem (Kudu).

O certificado tem de estar no formato. pfx e deve ser um certificado de caráter universal de dois requerente. Isto permite que um certificado para o domínio predefinido e o ponto final SCM para operações de controlo de origem.

| Formato | Exemplo |
| --- | --- |
| \*.appservice. \<região\>.\< DomainName\>.\< extensão\> | \*. appservice.redmond.azurestack.external |
| \*. scm.appservice. <region>. <DomainName>.<extension> | \*. appservice.scm.redmond.azurestack.external |

#### <a name="api-certificate"></a>Certificado da API

O certificado da API é colocado na função de gestão. O fornecedor de recursos utiliza-o para o ajudar a proteger chamadas de API. O certificado para a publicação tem de conter um assunto que corresponde a entrada DNS de API.

| Formato | Exemplo |
| --- | --- |
| API.appservice. \<região\>.\< DomainName\>.\< extensão\> | API.appservice.Redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Certificado de publicação

O certificado para a função de editor protege o tráfego FTPS para os proprietários da aplicação quando estes carregam conteúdo. O certificado para a publicação tem de conter um assunto que corresponde a entrada FTPS DNS.

| Formato | Exemplo |
| --- | --- |
| FTP.appservice. \<região\>.\< DomainName\>.\< extensão\> | API.appservice.Redmond.azurestack.external |

#### <a name="identity-certificate"></a>Certificado de identidade

Permite que o certificado para a aplicação de identidade:
- Integração entre o Azure Active Directory (Azure AD) ou serviços de Federação do Active Directory (AD FS) diretório, pilha do Azure e do serviço de aplicações para suportar a integração com o fornecedor de recursos de computação.
- Único início de sessão cenários para ferramentas de programador avançadas no serviço de aplicações do Azure na pilha do Azure.

O certificado para a identidade tem de conter um assunto que corresponde ao formato seguinte:

| Formato | Exemplo |
| --- | --- |
| SSO.appservice. \<região\>.\< DomainName\>.\< extensão\> | SSO.appservice.Redmond.azurestack.external |

### <a name="azure-resource-manager-root-certificate-for-azure-stack"></a>Certificado de raiz do Gestor de recursos do Azure para a pilha do Azure

Numa sessão do PowerShell em execução como azurestack\CloudAdmin, execute o script Get-AzureStackRootCert.ps1 da pasta onde extraiu os scripts de programa auxiliar. O script cria quatro certificados na mesma pasta que o script que necessita de serviço de aplicações para a criação de certificados.

| Parâmetro de GET-AzureStackRootCert.ps1 | Necessária ou opcional | Valor predefinido | Descrição |
| --- | --- | --- | --- |
| PrivelegedEndpoint | Necessário | AzS ERCS01 | Ponto final com privilégios |
| CloudAdminCredential | Necessário | AzureStack\CloudAdmin | Credencial da conta de domínio para os administradores de nuvem de pilha do Azure |


## <a name="prepare-the-file-server"></a>Preparar o servidor de ficheiros

App Service do Azure requer a utilização de um servidor de ficheiros. Para implementações de produção, o servidor de ficheiros tem de ser configurado para ser de elevada disponibilidade e capacidade de processamento de falhas.

Para apenas para implementações Kit de desenvolvimento de pilha do Azure, pode utilizar o [modelo de implementação Azure Resource Manager exemplo](https://aka.ms/appsvconmasdkfstemplate) para implementar um servidor de ficheiros de nó único configurado. O servidor de ficheiros de nó único será num grupo de trabalho.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Aprovisionar grupos e contas no Active Directory


1. Crie os seguintes grupos de segurança global do Active Directory:
   - FileShareOwners
   - FileShareUsers
2. Crie as seguintes contas do Active Directory como contas de serviço:
   - FileShareOwner
   - FileShareUser

   Como procedimento de segurança melhor prática, os utilizadores para estas contas (e para todas as funções da web) deve ser diferentes entre si e ter forte nomes de utilizador e palavras-passe. Defina as palavras-passe com as seguintes condições:
   - Ativar **palavra-passe nunca expira**.
   - Ativar **utilizador não é possível alterar a palavra-passe**.
   - Desativar **o utilizador deve alterar a palavra-passe no próximo início de sessão**.
3. Adicione as contas para as associações da seguinte forma:
   - Adicionar **FileShareOwner** para o **FileShareOwners** grupo.
   - Adicionar **FileShareUser** para o **FileShareUsers** grupo.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Aprovisionar grupos e contas num grupo de trabalho

>[!NOTE]
> Quando estiver a configurar um servidor de ficheiros, execute os seguintes comandos numa janela de linha de comandos administrativa. *Não utilize o PowerShell.*

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

Para qualquer uma das funções do SQL Server, pode utilizar uma instância predefinida ou numa instância nomeada. Se utilizar uma instância nomeada, não se esqueça de iniciar o serviço do SQL Server Browser e abrir a porta 1434 manualmente.

## <a name="create-an-azure-active-directory-application"></a>Criar uma aplicação do Azure Active Directory

Configure um principal de serviço do Azure AD para suportar o seguinte:
- Integração do conjunto de dimensionamento de máquina virtual em camadas de trabalho
- SSO para as ferramentas de Programador de portal e avançadas das funções do Azure

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
13. Selecione **as permissões necessárias** > **conceder permissões** > **Sim**.

| Parâmetro AADIdentityApp.ps1 criar | Necessária ou opcional | Valor predefinido | Descrição |
| --- | --- | --- | --- |
| DirectoryTenantName | Necessário | Valor nulo | ID de inquilino do Azure AD Forneça o GUID ou uma cadeia. Um exemplo é myazureaaddirectory.onmicrosoft.com. |
| AdminArmEndpoint | Necessário | Valor nulo | Ponto final de administração do Azure Resource Manager. Um exemplo é adminmanagement.local.azurestack.external. |
| TenantARMEndpoint | Necessário | Valor nulo | Ponto de final do inquilino do Azure Resource Manager. Um exemplo é management.local.azurestack.external. |
| AzureStackAdminCredential | Necessário | Valor nulo | Credencial de administrador de serviço de Azure AD. |
| CertificateFilePath | Necessário | Valor nulo | Caminho para o ficheiro de certificado de aplicação de identidade gerado anteriormente. |
| CertificatePassword | Necessário | Valor nulo | Palavra-passe que o ajuda a proteger a chave privada do certificado. |

## <a name="create-an-active-directory-federation-services-application"></a>Criar uma aplicação de serviços de Federação do Active Directory

Para ambientes de pilha do Azure protegidos pelo AD FS, tem de configurar um principal de serviço do AD FS para suportar o seguinte:
- Integração do conjunto de dimensionamento de máquina virtual em camadas de trabalho
- SSO para as ferramentas de Programador de portal e avançadas das funções do Azure

Os administradores tem de configurar o SSO para:
- Configure um principal de serviço para a integração de conjunto de dimensionamento de máquina virtual em camadas de trabalho.
- Ative as ferramentas de programador avançadas no serviço de aplicações (Kudu).
- Ative a utilização da experiência de portal das funções do Azure.

Siga estes passos.

1. Abra uma instância do PowerShell como azurestack\AzureStackAdmin.
2. Vá para a localização dos scripts que transferiu e extraiu no [passo pré-requisitos](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Instale o PowerShell para a pilha do Azure](azure-stack-powershell-install.md).
4.  Execute o **criar ADFSIdentityApp.ps1** script.
5.  No **credencial** janela, introduza a sua conta de administrador de nuvem do AD FS e a palavra-passe. Selecione **OK**.
6.  Forneça o caminho do ficheiro de certificado e a palavra-passe de certificado para o [certificado que criou anteriormente](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). O certificado que criou para este passo por predefinição é **sso.appservice.local.azurestack.external.pfx**.

| Parâmetro ADFSIdentityApp.ps1 criar | Necessária ou opcional | Valor predefinido | Descrição |
| --- | --- | --- | --- |
| AdminArmEndpoint | Necessário | Valor nulo | Ponto final de administração do Azure Resource Manager. Um exemplo é adminmanagement.local.azurestack.external. |
| PrivilegedEndpoint | Necessário | Valor nulo | Ponto final com privilégios. Um exemplo é AzS ERCS01. |
| CloudAdminCredential | Necessário | Valor nulo | Credenciais de conta de domínio para os administradores de nuvem de pilha do Azure. Um exemplo é Azurestack\CloudAdmin. |
| CertificateFilePath | Necessário | Valor nulo | Caminho do ficheiro PFX de certificado a aplicação de identidade. |
| CertificatePassword | Necessário | Valor nulo | Palavra-passe que o ajuda a proteger a chave privada do certificado. |


## <a name="next-steps"></a>Passos seguintes

[Instalar o fornecedor de recursos do serviço de aplicações](azure-stack-app-service-deploy.md)
