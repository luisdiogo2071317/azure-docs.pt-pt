---
title: Atualizar o fornecedor de recursos do SQL de pilha do Azure | Microsoft Docs
description: Saiba como pode atualizar o fornecedor de recursos do SQL de pilha do Azure.
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
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: ac5073d1abc32b7598a869750f9c5a801559e9e6
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264082"
---
# <a name="update-the-sql-resource-provider"></a>Atualize o fornecedor de recursos SQL

*Aplica-se a: Azure pilha integrado sistemas.*

Um novo fornecedor de recursos do SQL Server pode ser lançado quando pilha do Azure é atualizada para uma nova compilação. Embora o adaptador existente continua a funcionar, recomendamos que a atualização para a compilação mais recente logo que possível.

>[!IMPORTANT]
>Tem de instalar as atualizações pela ordem que está a ser lançados. Não é possível ignorar versões. Consulte a lista de versões no [implementar os pré-requisitos do fornecedor de recursos](.\azure-stack-sql-resource-provider-deploy.md#prerequisites).

## <a name="overview"></a>Descrição geral

Para atualizar o fornecedor de recursos, utilize o *UpdateSQLProvider.ps1* script. Este script está incluído com a transferência do novo fornecedor de recursos do SQL Server. O processo de atualização é semelhante ao processo utilizado para [implementar o fornecedor de recursos](.\azure-stack-sql-resource-provider-deploy.md). O script de atualização utiliza os mesmos argumentos como o script de DeploySqlProvider.ps1 e terá de fornecer as informações do certificado.

### <a name="update-script-processes"></a>Processos de script de atualização

O *UpdateSQLProvider.ps1* script cria uma nova máquina virtual (VM) com o código mais recente do fornecedor de recursos.

>[!NOTE]
>Recomendamos que transfira a imagem do Windows Server 2016 Core mais recente da gestão do Marketplace. Se precisar de instalar uma atualização, pode colocar um **único** pacote MSU no caminho local da dependência. O script irá falhar se não houver mais de um ficheiro MSU nesta localização.

Depois do *UpdateSQLProvider.ps1* script cria uma nova VM, o script migra as seguintes definições do fornecedor de antigo VM:

* Informações de base de dados
* informações do servidor de alojamento
* registo DNS necessário

### <a name="update-script-powershell-example"></a>Atualizar o exemplo de script do PowerShell

Pode editar e execute o seguinte script de um ISE do PowerShell elevada. Lembre-se alterar as informações de conta e as palavras-passe conforme necessário para o seu ambiente.

> [!NOTE]
> Este processo de atualização aplica-se apenas a sistemas de pilha do Azure integrado.

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

Pode especificar os parâmetros seguintes na linha de comandos quando executar o script. Se não, ou se falhar qualquer validação dos parâmetros, é-lhe pedido que forneça os parâmetros necessários.

| Nome do parâmetro | Descrição | Comentário ou o valor predefinido |
| --- | --- | --- |
| **CloudAdminCredential** | A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios. | _Necessário_ |
| **AzCredential** | As credenciais para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure. | _Necessário_ |
| **VMLocalCredential** | As credenciais da conta de administrador local do fornecedor de recursos VM do SQL Server. | _Necessário_ |
| **PrivilegedEndpoint** | O endereço IP ou nome DNS do ponto final com privilégios. |  _Necessário_ |
| **DependencyFilesLocalPath** | Também deve colocar o ficheiro de. pfx do certificado neste diretório. | _Opcional para um único nó, mas é obrigatória para vários nós_ |
| **DefaultSSLCertificatePassword** | A palavra-passe para o certificado. pfx. | _Necessário_ |
| **MaxRetryCount** | O número de vezes que pretende repetir a cada operação, se ocorrer uma falha.| 2 |
| **RetryDuration** |O intervalo de tempo limite entre tentativas, em segundos. | 120 |
| **Desinstalar** | Remove o fornecedor de recursos e recursos todos os associados. | Não |
| **DebugMode** | Impede a limpeza automática em caso de falha. | Não |

## <a name="next-steps"></a>Passos Seguintes

[Manter o fornecedor de recursos SQL](azure-stack-sql-resource-provider-maintain.md)
