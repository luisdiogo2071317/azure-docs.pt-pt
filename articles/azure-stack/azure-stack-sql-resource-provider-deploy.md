---
title: Bases de dados SQL a utilizar na pilha do Azure | Microsoft Docs
description: Saiba como pode implementar bases de dados do SQL Server como um serviço na pilha do Azure e os passos rápidos para implementar o adaptador de fornecedor de recursos do SQL Server.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 70b07cae9a1dc8b45e27f95e19fbc84f06a0b6d3
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados do SQL Server na pilha do Microsoft Azure
Utilize o fornecedor de recursos do servidor de SQL de pilha do Azure para expor bases de dados do SQL Server como um serviço da pilha do Azure. O serviço de fornecedor de recursos do SQL Server é executado no fornecedor de recursos SQL VM, que é uma máquina de virtual do Windows Server core.

## <a name="prerequisites"></a>Pré-requisitos
Existem vários pré-requisitos que devem ser implementados antes de poder implementar o fornecedor de recursos do SQL de pilha do Azure. Execute os passos seguintes num computador que pode aceder ao ponto final com privilégios VM:

- Se que ainda não tiver feito, [registar Azure pilha](.\azure-stack-registration.md) com o Azure para que possam transferir itens do marketplace do Azure.
- Adicionar o núcleo do Windows Server necessário VM para o mercado de pilha do Azure, transferindo-a **núcleo de servidor do Windows Server 2016** imagem. Se precisar de instalar uma atualização, é possível colocar um único. Pacote MSU no caminho local da dependência. Se mais do que um. Ficheiro MSU for encontrado, a instalação do fornecedor de recursos de SQL Server irá falhar.
- Transfira o fornecedor de recursos do SQL Server binário e, em seguida, execute o Self-extractor para extrair os conteúdos num diretório temporário. O fornecedor de recursos tem uma pilha do Azure correspondente mínimo, compilação. Não se esqueça de transferir o binário correto para a versão da pilha do Azure que está a executar:
    - Pilha do Azure versão 1802 (1.0.180302.1): [RP SQL versão 1.1.18.0](https://aka.ms/azurestacksqlrp1802).
    - Pilha do Azure versão 1712 (1.0.180102.3, 1.0.180103.2 ou 1.0.180106.1 (sistemas integrados)): [RP SQL versão 1.1.14.0](https://aka.ms/azurestacksqlrp1712).
- Para instalações de sistemas integrada apenas, tem de fornecer o certificado PKI de PaaS do SQL Server, tal como descrito na secção de certificados de PaaS opcional de [requisitos do Azure pilha implementação PKI](.\azure-stack-pki-certs.md#optional-paas-certificates), colocando o ficheiro. pfx na localização especificada pelo **DependencyFilesLocalPath** parâmetro.
- Certifique-se de que tem o [versão mais recente do Azure PowerShell de pilha](.\azure-stack-powershell-install.md) (v1.2.11) instalado. 

## <a name="deploy-the-sql-resource-provider"></a>Implementar o fornecedor de recursos SQL
Depois de ter preparado com êxito para instalar o fornecedor de recursos do SQL Server ao cumprir todos os pré-requisitos, agora, pode executar o **DeploySqlProvider.ps1** script para implementar o fornecedor de recursos do SQL Server. O script de DeploySqlProvider.ps1 é extraído como parte do fornecedor de recursos SQL binário do que transferiu correspondente a sua versão de pilha do Azure. 

> [!IMPORTANT]
> O sistema onde está a ser executado o script tem de ser um sistema Windows 10 ou Windows Server 2016 com a versão mais recente do tempo de execução de .NET instalada.


Para implementar o fornecedor de recursos do SQL Server, abra uma consola de PowerShell (administrativa) novos elevada e altere o diretório em que extraiu os ficheiros binários de fornecedor de recursos do SQL Server.

> [!NOTE]
> Utilize uma nova janela da consola do PowerShell para evitar problemas que possam surgir de incorretos módulos do PowerShell que já estão carregados no sistema.

Execute o script de DeploySqlProvider.ps1 que efetua os seguintes passos:
- Carrega os certificados e outros artefactos para uma conta de armazenamento na pilha do Azure.
- Publica os pacotes de galeria que pode implementar as bases de dados do SQL Server através da galeria.
- Publica um pacote de galeria para implementar servidores de alojamento.
- Implementa uma VM utilizando a imagem do Windows Server 2016 que foi criada no passo 1 e, em seguida, instala o fornecedor de recursos.
- Regista um registo DNS local, que mapeia para o fornecedor de recursos VM.
- Regista o fornecedor de recursos com o local do Azure Resource Manager (utilizador e administrador).
- Opcionalmente, instala uma única atualização do Windows durante a instalação de RP.

Implementação de fornecedor de recursos do SQL Server começa e cria o grupo de recursos system.local.sqladapter. Pode demorar até 75 minutos a concluir as implementações necessárias quatro para este grupo de recursos.

### <a name="deploysqlproviderps1-parameters"></a>Parâmetros de DeploySqlProvider.ps1
Pode especificar estes parâmetros na linha de comandos. Se não o fizer, ou se falhar qualquer validação dos parâmetros, lhe for pedido para fornecer os parâmetros necessários.

| Nome do parâmetro | Descrição | Comentário ou o valor predefinido |
| --- | --- | --- |
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios. | _Necessário_ |
| **AzCredential** | As credenciais para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure. | _Necessário_ |
| **VMLocalCredential** | As credenciais da conta de administrador local do fornecedor de recursos VM do SQL Server. | _Necessário_ |
| **PrivilegedEndpoint** | O endereço IP ou nome DNS do ponto final com privilégios. |  _Necessário_ |
| **DependencyFilesLocalPath** | O ficheiro de. pfx do certificado deve ser colocado neste diretório bem. | _Opcional_ (_obrigatório_ para sistemas integrados) |
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx. | _Necessário_ |
| **MaxRetryCount** | O número de vezes que pretende repetir a cada operação, se ocorrer uma falha.| 2 |
| **RetryDuration** | O intervalo de tempo limite entre tentativas, em segundos. | 120 |
| **Desinstalar** | Remove o fornecedor de recursos e recursos de todos os associados (consulte as seguintes notas). | Não |
| **DebugMode** | Impede a limpeza automática em caso de falha. | Não |



## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Implementar o fornecedor de recursos do SQL Server através de um script personalizado
Para evitar introduzir manualmente as informações necessárias quando o script de DeploySqlProvider.ps1 é executado, pode personalizar o seguinte exemplo de script alterando as informações de conta de predefinido e as palavras-passe conforme necessário:

```powershell
# Install the AzureRM.Bootstrapper module, set the profile, and install the AzureRM and AzureStack modules.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert
 ```

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Verificar a implementação através do portal de pilha do Azure
Os passos nesta secção podem ser utilizados para se certificar de que o fornecedor de recursos do SQL Server foi implementado com êxito.

> [!NOTE]
>  Após o script de instalação em execução, terá de atualizar o portal para ver os itens de painel e Galeria de admin.

1. Inicie sessão no portal de administração como o administrador de serviço.

2. Certifique-se de que a implementação concluída com êxito. Aceda a **grupos de recursos**. Em seguida, selecione o **system.\< localização\>.sqladapter** grupo de recursos. Certifique-se de que todas as implementações de quatro teve êxito.

      ![Verificar a implementação do fornecedor de recursos SQL](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)


## <a name="next-steps"></a>Passos Seguintes

[Adicionar servidores de alojamento](azure-stack-sql-resource-provider-hosting-servers.md)
