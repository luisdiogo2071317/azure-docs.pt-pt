---
title: Utilizar bases de dados MySQL no Azure pilha | Microsoft Docs
description: Saiba como pode implementar bases de dados MySQL como um serviço na pilha do Azure e os passos rápidos para implementar o adaptador de fornecedor de recursos do servidor MySQL.
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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e4c3eb1d7dfd4894576d5fbed52cf4de5fed9e44
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938119"
---
# <a name="deploy-the-mysql-resource-provider-on-azure-stack"></a>Implementar o fornecedor de recursos de MySQL na pilha do Azure

Utilize o fornecedor de recursos do servidor de MySQL para expor bases de dados MySQL como um serviço de pilha do Azure. O fornecedor de recursos de MySQL é executado como um serviço numa máquina virtual (VM) Server Core do Windows Server 2016.

## <a name="prerequisites"></a>Pré-requisitos

Existem vários pré-requisitos que devem ser implementados antes de poder implementar o fornecedor de recursos do Azure pilha MySQL. Para cumprir estes requisitos, conclua os passos neste artigo num computador que pode aceder ao ponto final com privilégios VM.

* Se ainda não o tiver feito deste modo, [registar Azure pilha](.\azure-stack-registration.md) com o Azure, pelo que pode transferir itens do marketplace do Azure.
* Adicionar o núcleo do Windows Server necessário VM para o mercado de pilha do Azure, transferindo-a **Datacenter do Windows Server 2016 - Server Core** imagem. Também pode utilizar um script para criar um [imagem do Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). Certifique-se de que seleciona a opção core ao executar o script.

  >[!NOTE]
  >Se precisar de instalar uma atualização, é possível colocar um único. Pacote MSU no caminho local da dependência. Se mais do que um. Ficheiro MSU for encontrado, a instalação do fornecedor de recursos de MySQL irá falhar.

* Transferir o fornecedor de recursos de MySQL binário e, em seguida, execute o Self-extractor para extrair os conteúdos num diretório temporário.

  >[!NOTE]
  >Para implementar o fornecedor de MySQL num sistema que não tem acesso à Internet, copie o [mysql conector-net 6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) ficheiro para uma partilha local. Forneça o nome de partilha quando lhe for pedida para o mesmo. Tem de instalar os módulos do Azure e o Azure PowerShell da pilha.

* O fornecedor de recursos tem uma pilha do Azure correspondente mínimo, compilação. Certifique-se de que o binário correto para a versão da pilha do Azure que estiver a executar.

    | Versão de pilha do Azure | Versão MySQL RP|
    | --- | --- |
    | Versão 1804 (1.0.180513.1)|[MySQL RP versão 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) |
    | Versão 1802 (1.0.180302.1) | [MySQL RP versão 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) |
    | Versão 1712 (1.0.180102.3 ou 1.0.180106.1 (sistemas integrados)) | [MySQL RP versão 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |

### <a name="certificates"></a>Certificados

Para ASDK, é criado um certificado autoassinado como parte deste processo de instalação. Para um sistema de pilha do Azure integrado, tem de fornecer um certificado adequado. Se tiver de fornecer o seu próprio certificado, colocar um ficheiro. pfx a **DependencyFilesLocalPath** que cumpra os seguintes critérios:

* Um certificado de caráter universal para \*.dbadapter.\< região\>.\< fqdn externo\> ou um certificado de site único com um nome comum de mysqladapter.dbadapter.\< região\>.\< fqdn externo\>.
* O certificado tem de ser fidedigno. A cadeia de confiança tem de existir sem exigir certificados intermédios.
* Existe apenas um ficheiro de certificado único no DependencyFilesLocalPath.
* O nome de ficheiro não pode ter qualquer espaços ou carateres especiais.

## <a name="deploy-the-resource-provider"></a>Implementar o fornecedor de recursos

Depois de todos os pré-requisitos instalados, execute o **DeployMySqlProvider.ps1** script para implementar o fornecedor de recursos MYSQL. O script de DeployMySqlProvider.ps1 é extraído como parte do binário de fornecedor de recursos MySQL que transferiu para a sua versão da pilha do Azure.

> [!IMPORTANT]
> O sistema que está a executar o script no tem de ser um sistema Windows 10 ou Windows Server 2016 com a versão mais recente do tempo de execução de .NET instalada.

Para implementar o fornecedor de recursos do MySQL, abra uma janela da consola do PowerShell new elevada e altere o diretório onde extraiu os MySQL recurso binários os ficheiros do fornecedor. Recomendamos que utilize uma nova janela do PowerShell para evitar potenciais problemas causados por módulos do PowerShell que já estão carregados.

Execute o **DeployMySqlProvider.ps1** script, que conclui as seguintes tarefas:

* Carrega os certificados e outros artefactos para uma conta de armazenamento na pilha do Azure.
* Publica os pacotes de galeria que pode implementar bases de dados MySQL utilizando a galeria.
* Publica um pacote de galeria para implementar servidores de alojamento.
* Implementa uma VM utilizando a imagem de núcleo do Windows Server 2016 transferidas e, em seguida, instala o fornecedor de recursos de MySQL.
* Regista um registo DNS local, que mapeia para o fornecedor de recursos VM.
* Regista o fornecedor de recursos com o local do Azure Resource Manager para as contas de utilizador e de operador.
* Opcionalmente, instala uma única atualização do Windows Server durante a instalação do fornecedor de recursos.

> [!NOTE]
> Quando é iniciada a implementação de fornecedor de recursos do MySQL, o **system.local.mysqladapter** grupo de recursos é criado. Pode demorar até 75 minutos a concluir as quatro implementações necessárias para este grupo de recursos.

### <a name="deploymysqlproviderps1-parameters"></a>Parâmetros de DeployMySqlProvider.ps1

Pode especificar estes parâmetros da linha de comandos. Se não, ou se falhar qualquer validação dos parâmetros, é-lhe pedido que forneça os parâmetros necessários.

| Nome do parâmetro | Descrição | Comentário ou o valor predefinido |
| --- | --- | --- |
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios. | _Necessário_ |
| **AzCredential** | As credenciais para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure. | _Necessário_ |
| **VMLocalCredential** | As credenciais para a conta de administrador local do fornecedor de recursos de MySQL VM. | _Necessário_ |
| **PrivilegedEndpoint** | O endereço IP ou nome DNS do ponto final com privilégios. |  _Necessário_ |
| **DependencyFilesLocalPath** | Caminho para uma partilha local que contém [mysql conector-net 6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi). Se fornecer um destes caminhos, o ficheiro de certificado tem de ser colocado bem neste diretório. | _Opcional_ de nó único, _obrigatório_ para vários nós. |
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx. | _Necessário_ |
| **MaxRetryCount** | O número de vezes que pretende repetir a cada operação, se ocorrer uma falha.| 2 |
| **RetryDuration** | O intervalo de tempo limite entre tentativas, em segundos. | 120 |
| **Desinstalar** | Remove o fornecedor de recursos e recursos de todos os associados (consulte as seguintes notas). | Não |
| **DebugMode** | Impede a limpeza automática em caso de falha. | Não |
| **AcceptLicense** | Ignora o pedido para aceitar o GPL de licenciamento.  <http://www.gnu.org/licenses/old-licenses/gpl-2.0.html> | |

> [!NOTE]
> SKUs podem demorar até uma hora ser visível no portal. Não é possível criar uma base de dados até que o SKU não está implementado e em execução.

## <a name="deploy-the-mysql-resource-provider-using-a-custom-script"></a>Implementar o fornecedor de recursos de MySQL utilizando um script personalizado

Para eliminar qualquer configuração manual quando implementar o fornecedor de recursos, pode personalizar o script seguinte. Altere as palavras-passe e informações da conta de predefinição conforme necessário para a implementação de pilha do Azure.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"  

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files.
# Find the ERCS01 IP address first, and make sure the certificate file is in the specified directory.
. $tempDir\DeployMySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert `
    -AcceptLicense

```

Quando termina o script de instalação do fornecedor de recursos, atualize o browser para se certificar de que pode ver as atualizações mais recentes.

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Verificar a implementação através do portal de pilha do Azure

1. Inicie sessão no portal de administração como o administrador de serviço.
2. Selecione **grupos de recursos**
3. Selecione o **system.\< localização\>.mysqladapter** grupo de recursos.
4. Na página de resumo para o recurso de descrição geral, a mensagem em grupo **implementações** deve ser **com êxito 3**.
5. Pode obter informações mais detalhadas sobre a implementação do fornecedor de recursos em **definições**. Selecione **implementações** para obter informações tais como: Estado, TIMESTAMP e a duração de cada implementação.

## <a name="next-steps"></a>Passos Seguintes

[Adicionar servidores de alojamento](azure-stack-mysql-resource-provider-hosting-servers.md)
