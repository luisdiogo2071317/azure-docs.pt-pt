---
title: A atualizar o fornecedor de recursos do SQL do Azure Stack | Documentos da Microsoft
description: Saiba como pode atualizar o fornecedor de recursos do SQL do Azure Stack.
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
ms.date: 09/04/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 017ac3214046b812ce003ab7a190f2f2b3f4f2e2
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43697946"
---
# <a name="update-the-sql-resource-provider"></a>Atualizar o fornecedor de recursos do SQL

*Aplica-se a: sistemas integrados do Azure Stack.*

Um novo fornecedor de recursos do SQL pode ser liberado quando o Azure Stack é atualizado para uma nova compilação. Embora o adaptador existente continua a funcionar, recomendamos que Atualize para a compilação mais recente, logo que possível.

>[!IMPORTANT]
>Tem de instalar atualizações na ordem em que são lançadas. Não é possível ignorar versões. Consulte a lista de versões no [implementar os pré-requisitos do fornecedor de recursos](.\azure-stack-sql-resource-provider-deploy.md#prerequisites).

## <a name="overview"></a>Descrição geral

Para atualizar o fornecedor de recursos, utilize o *UpdateSQLProvider.ps1* script. Este script está incluído no download do novo fornecedor de recursos do SQL. O processo de atualização é semelhante para o processo usado para [implementar o fornecedor de recursos](.\azure-stack-sql-resource-provider-deploy.md). O script de atualização utiliza os mesmos argumentos que o script de DeploySqlProvider.ps1 e terá de fornecer informações de certificado.

### <a name="update-script-processes"></a>Processos de script de atualização

O *UpdateSQLProvider.ps1* script cria uma nova máquina virtual (VM) com o código do Provedor de recursos mais recentes.

>[!NOTE]
>Recomendamos que baixe a imagem do Windows Server 2016 Core mais recente da gestão do Marketplace. Se precisar de instalar uma atualização, pode colocar um **único** pacote MSU no caminho do local de dependência. O script irá falhar se houver mais de um ficheiro MSU nesta localização.

Depois do *UpdateSQLProvider.ps1* script cria uma nova VM, o script migra as seguintes definições do fornecedor de antigo VM:

* informações da base de dados
* informações do servidor de alojamento
* registo DNS necessário

### <a name="update-script-powershell-example"></a>Atualizar o exemplo de script do PowerShell

Pode editar e execute o seguinte script de uma elevada ISE do PowerShell. Não se esqueça de alterar as informações de conta e as palavras-passe conforme necessário para o seu ambiente.

> [!NOTE]
> Este processo de atualização só se aplica aos sistemas integrados do Azure Stack.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but this might have been changed at installation.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines.
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service administrator account (this can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `

 ```

## <a name="updatesqlproviderps1-parameters"></a>Parâmetros de UpdateSQLProvider.ps1

Pode especificar os seguintes parâmetros da linha de comando ao executar o script. Se não o fizer, ou se nenhuma validação de parâmetro falhar, lhe for pedido para fornecer os parâmetros necessários.

| Nome do parâmetro | Descrição | Comentário ou o valor predefinido |
| --- | --- | --- |
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessário para acessar o ponto final com privilégios. | _Necessário_ |
| **AzCredential** | As credenciais para a conta de administrador de serviço do Azure Stack. Utilize as mesmas credenciais que utilizou para a implementação do Azure Stack. | _Necessário_ |
| **VMLocalCredential** | As credenciais para a conta de administrador local do fornecedor de recursos SQL VM. | _Necessário_ |
| **PrivilegedEndpoint** | O endereço IP ou nome DNS do ponto final com privilégios. |  _Necessário_ |
| **AzureEnvironment** | O ambiente do azure da conta de administrador de serviço que utilizou para a implementação do Azure Stack. Obrigatório apenas se não é ADFS. Os nomes de ambiente com suporte são **AzureCloud**, **AzureUSGovernment**, ou se um China Azure Active Directory, a utilizar **AzureChinaCloud**. | AzureCloud |
| **DependencyFilesLocalPath** | Também tem de colocar o ficheiro. pfx de certificado neste diretório. | _Opcional para o nó único, mas obrigatório para vários nós_ |
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx. | _Necessário_ |
| **MaxRetryCount** | O número de vezes que pretende repetir a cada operação, se ocorrer uma falha.| 2 |
| **RetryDuration** |O intervalo de tempo limite entre repetições, em segundos. | 120 |
| **Desinstalar** | Remove o fornecedor de recursos e todos os recursos associados. | Não |
| **DebugMode** | Impede que a limpeza automática em caso de falha. | Não |

## <a name="next-steps"></a>Passos Seguintes

[Manter o fornecedor de recursos do SQL](azure-stack-sql-resource-provider-maintain.md)
