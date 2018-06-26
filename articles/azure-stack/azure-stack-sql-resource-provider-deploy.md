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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e1505761a0bd1ea9dabdd0b2cbab7af902198311
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938337"
---
# <a name="deploy-the-sql-server-resource-provider-on-azure-stack"></a>Implementar o fornecedor de recursos do SQL Server na pilha do Azure

Utilize o fornecedor de recursos do servidor de SQL de pilha do Azure para expor bases de dados do SQL Server como um serviço de pilha do Azure. O fornecedor de recursos do SQL Server é executado como um serviço numa máquina virtual (VM) Server Core do Windows Server 2016.

## <a name="prerequisites"></a>Pré-requisitos

Existem vários pré-requisitos que devem ser implementados antes de poder implementar o fornecedor de recursos do SQL de pilha do Azure. Para cumprir estes requisitos, conclua os passos seguintes num computador que pode aceder ao ponto final com privilégios VM:

- Se ainda não o tiver feito deste modo, [registar Azure pilha](.\azure-stack-registration.md) com o Azure, pelo que pode transferir itens do marketplace do Azure.
- Adicionar o núcleo do Windows Server necessário VM para o mercado de pilha do Azure, transferindo-a **Datacenter do Windows Server 2016 - Server Core** imagem. Também pode utilizar um script para criar um [imagem do Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). Certifique-se de que seleciona a opção core ao executar o script.

  >[!NOTE]
  >Se precisar de instalar uma atualização, poderá colocar um único pacote MSU o caminho local da dependência. Se for encontrado mais do que um ficheiro MSU, a instalação do fornecedor de recursos de SQL Server irá falhar.

- Transferir o fornecedor de recursos SQL binário e, em seguida, execute o Self-extractor para extrair os conteúdos num diretório temporário. O fornecedor de recursos tem uma pilha do Azure correspondente mínimo, compilação. Certifique-se de que o binário correto para a versão da pilha do Azure que estiver a executar.

    |Versão de pilha do Azure|Versão do SQL Server RP|
    |-----|-----|
    |Versão 1804 (1.0.180513.1)|[SQL Server RP versão 1.1.24.0](https://aka.ms/azurestacksqlrp1804)
    |Versão 1802 (1.0.180302.1)|[SQL Server RP versão 1.1.18.0](https://aka.ms/azurestacksqlrp1802)|
    |Versão 1712 (1.0.180102.3, 1.0.180103.2 ou 1.0.180106.1 (sistemas integrados))|[SQL Server RP versão 1.1.14.0](https://aka.ms/azurestacksqlrp1712)|
    |     |     |

### <a name="certificates"></a>Certificados

Para instalações de sistemas integrada apenas. Tem de fornecer o certificado PKI de PaaS SQL descrito na secção de certificados de PaaS opcional de [requisitos do Azure pilha implementação PKI](.\azure-stack-pki-certs.md#optional-paas-certificates). Coloque o ficheiro. pfx na localização especificada pelo **DependencyFilesLocalPath** parâmetro.

## <a name="deploy-the-sql-resource-provider"></a>Implementar o fornecedor de recursos SQL

Depois de todos os pré-requisitos instalados, execute o **DeploySqlProvider.ps1** script para implementar o fornecedor de recursos do SQL Server. O script de DeploySqlProvider.ps1 é extraído como parte do binário de fornecedor de recursos SQL que transferiu para a sua versão da pilha do Azure.

> [!IMPORTANT]
> O sistema que está a executar o script no tem de ser um sistema Windows 10 ou Windows Server 2016 com a versão mais recente do tempo de execução de .NET instalada.

Para implementar o fornecedor de recursos do SQL Server, abra uma **novo** PowerShell elevada janela de consola e altere o diretório em que extraiu os ficheiros binários de fornecedor de recursos do SQL Server. Recomendamos que utilize uma nova janela do PowerShell para evitar potenciais problemas causados por módulos do PowerShell que já estão carregados.

Execute o script DeploySqlProvider.ps1, que conclui as seguintes tarefas:

- Carrega os certificados e outros artefactos para uma conta de armazenamento na pilha do Azure.
- Publica pacotes da Galeria de modo a poder implementá bases de dados do SQL Server utilizando a galeria.
- Publica um pacote de galeria para implementar servidores de alojamento.
- Implementa uma VM utilizando a imagem de núcleo do Windows Server 2016 transferidas e, em seguida, instala o fornecedor de recursos do SQL Server.
- Regista um registo DNS local, que mapeia para o fornecedor de recursos VM.
- Regista o fornecedor de recursos com o local do Azure Resource Manager para as contas de utilizador e de operador.
- Opcionalmente, instala uma única atualização do Windows Server durante a instalação do fornecedor de recursos.

> [!NOTE]
> Quando é iniciada a implementação de fornecedor de recursos do SQL Server, o **system.local.sqladapter** grupo de recursos é criado. Pode demorar até 75 minutos a concluir as implementações necessárias quatro para este grupo de recursos.

### <a name="deploysqlproviderps1-parameters"></a>Parâmetros de DeploySqlProvider.ps1

Pode especificar os parâmetros seguintes na linha de comandos. Se não, ou se falhar qualquer validação dos parâmetros, é-lhe pedido que forneça os parâmetros necessários.

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

>[!NOTE]
> SKUs podem demorar até uma hora ser visível no portal. Não é possível criar uma base de dados até que o SKU não está implementado e em execução.

## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Implementar o fornecedor de recursos do SQL Server através de um script personalizado

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
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account can be Azure Active Directory or Active Directory Federation Services.
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change to the directory If folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert

 ```

Quando termina o script de instalação do fornecedor de recursos, atualize o browser para se certificar de que pode ver as atualizações mais recentes.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Verificar a implementação através do portal de pilha do Azure

Pode utilizar os seguintes passos Certifique-se de que o fornecedor de recursos do SQL Server é implementado com êxito.

1. Inicie sessão no portal de administração como o administrador de serviço.
2. Selecione **grupos de recursos**.
3. Selecione o **system.\< localização\>.sqladapter** grupo de recursos.
4. A mensagem em **implementações**, mostrado na captura de ecrã seguinte, deve ser **com êxito 4**.

      ![Verificar a implementação do fornecedor de recursos SQL](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)

5. Pode obter informações mais detalhadas sobre a implementação do fornecedor de recursos em **definições**. Selecione **implementações** para obter informações tais como: Estado, TIMESTAMP e a duração de cada implementação.

## <a name="next-steps"></a>Passos Seguintes

[Adicionar servidores de alojamento](azure-stack-sql-resource-provider-hosting-servers.md)
