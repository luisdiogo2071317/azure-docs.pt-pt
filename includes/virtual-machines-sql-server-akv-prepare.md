---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows
author: rothja
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 19be449528481b4e35cad4418f82f2250917966b
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
ms.locfileid: "32787421"
---
## <a name="prepare-for-akv-integration"></a>Preparar a integração AKV
Para utilizar a integração do Cofre de chaves do Azure para configurar a VM do SQL Server, existem vários pré-requisitos: 

1. [Instalar o Azure Powershell](#install)
2. [Criar um Azure Active Directory](#register)
3. [Criar um cofre de chaves](#createkeyvault)

As secções seguintes descrevem os pré-requisitos e as informações que necessárias para recolher mais tarde executar os cmdlets do PowerShell.

### <a id="install"></a> Instalar o Azure PowerShell
Certifique-se de que instalou o Azure PowerShell SDK mais recente. Para obter mais informações, veja [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azureps-cmdlets-docs).

### <a id="register"></a> Registar uma aplicação no Azure Active Directory

Em primeiro lugar, tem de ter um [do Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) na sua subscrição. Entre muitas vantagens, isto permite-lhe conceder permissão ao seu Cofre de chaves para determinados utilizadores e aplicações.

Em seguida, registe uma aplicação com o AAD. Isto irá dar-lhe uma conta do Principal de serviço que tenha acesso ao seu Cofre de chaves, terá da VM. No artigo do Cofre de chaves do Azure, pode encontrar estes passos no [registar uma aplicação no Azure Active Directory](../articles/key-vault/key-vault-get-started.md#register) secção, ou pode ver os passos com capturas de ecrã do **obter uma identidade para a secção da aplicação**  de [esta mensagem de blogue](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Antes de concluir estes passos, terá de recolher as informações seguintes durante este registo que é necessária mais tarde, quando ativar a integração do Cofre de chaves do Azure na sua VM do SQL Server.

* Depois da aplicação é adicionada, localizar o **ID da aplicação** no **aplicação registada** painel.
    O ID de aplicação é atribuído a mais tarde a **$spName** parâmetro (nome Principal de serviço) no script de PowerShell para ativar a integração do Cofre de chaves do Azure.

   ![ID da aplicação](./media/virtual-machines-sql-server-akv-prepare/aad-application-id.png)

* Durante estes passos ao criar a chave, copie o segredo para a sua chave conforme é mostrado na captura de ecrã seguinte. Este segredo chave está atribuído a mais tarde a **$spSecret** parâmetro (segredo Principal de serviço) no PowerShell script.

   ![Segredo do AAD](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)

* O ID da aplicação e o segredo também serão utilizados para criar uma credencial no SQL Server.

* Tem de autorizar este novo ID de cliente para ter as seguintes permissões de acesso: **encriptar**, **desencriptar**, **wrapKey**, **unwrapKey**, **sessão**, e **verificar**. Isto é feito com o [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625.aspx) cmdlet. Para obter mais informações, consulte [autorizar a aplicação a utilizar a chave ou segredo](../articles/key-vault/key-vault-get-started.md#authorize).

### <a id="createkeyvault"></a> Criar um cofre de chaves
Para utilizar o Cofre de chaves do Azure para armazenar as chaves que irá utilizar para encriptação na sua VM, precisa de acesso para um cofre de chaves. Se já não configurar o seu Cofre de chaves, crie um seguindo os passos a [introdução ao Cofre de chaves do Azure](../articles/key-vault/key-vault-get-started.md) artigo. Antes de concluir estes passos, há algumas informações necessárias para recolher durante este conjunto de cópias de segurança que é necessária mais tarde, quando ativar a integração do Cofre de chaves do Azure na sua VM do SQL Server.

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Quando chegar a criar um passo do Cofre de chaves, tenha em atenção o devolvido **vaultUri** propriedade, o que é o URL do Cofre de chaves. O exemplo fornecido nesse passo, abaixo, o nome do Cofre de chaves é ContosoKeyVault, pelo que o Cofre de chaves URL seria https://contosokeyvault.vault.azure.net/.

O URL do Cofre de chaves é atribuído a mais tarde a **$akvURL** parâmetro do script do PowerShell para ativar a integração do Cofre de chaves do Azure.

Depois do Cofre de chaves é criado, precisamos de adicionar uma chave para o Cofre de chaves, esta chave vai ser referida quando criamos uma chave assimétrica criar no SQL Server mais tarde.
