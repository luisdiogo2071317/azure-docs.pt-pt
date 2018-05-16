---
title: Início Rápido do Azure – Criar uma CLI do Key Vault | Microsoft Docs
description: Início rápido que mostra como criar um Azure Key Vault com a CLI
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: ''
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/27/2018
ms.author: barclayn
ms.openlocfilehash: 64a092de1ae6c005355063d43541a20fa9076395
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="quickstart-create-an-azure-key-vault-using-the-cli"></a>Início Rápido: Criar um Azure Key Vault com a CLI

O Azure Key Vault é um serviço cloud que funciona como um arquivo de segredos seguro. Pode armazenar chaves, palavras-passe, certificados e outros segredos em segurança. Para obter mais informações sobre o Key Vault, pode ver a [Descrição Geral](key-vault-overview.md). A CLI do Azure é utilizada para criar e gerir recursos do Azure através de comandos ou scripts. Neste início rápido, vai criar um cofre de chaves. Depois de concluir este passo, irá armazenar um segredo.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este início rápido requer a versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

Para iniciar sessão no Azure através da CLI, pode escrever:

```azurecli
az login
```

Para obter mais informações sobre as opções de início de sessão através da CLI, veja [Iniciar sessão com a CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. O exemplo seguinte cria um grupo de recursos com o nome *ContosoResourceGroup* na localização *eastus*.

```azurecli
az group create --name 'ContosoResourceGroup' --location eastus
```

## <a name="create-a-key-vault"></a>Criar um Key Vault

A seguir, vai criar um Key Vault no grupo de recursos criado no passo anterior. Terá de fornecer algumas informações:

- Para este início rápido, utilizamos **Contoso-vault2**. Tem de fornecer um nome exclusivo no teste.
- **ContosoResourceGroup** como o nome do grupo de recursos.
- **E.U.A. Leste** como a localização.

```azurecli
az keyvault create --name 'Contoso-Vault2' --resource-group 'ContosoResourceGroup' --location eastus
```

O resultado deste cmdlet mostra as propriedades do Key Vault recém-criado. Tome nota das duas propriedades listadas abaixo:

- **Nome do Cofre**: no exemplo, o nome é **Contoso-Vault2**. Irá utilizar este nome para outros comandos do Key Vault.
- **URI do Cofre**: no exemplo, isto é https://contoso-vault2.vault.azure.net/. As aplicações que utilizam o cofre através da respetiva API têm de utilizar este URI.

Nesta altura, a sua conta do Azure é a única autorizada a realizar quaisquer operações neste novo cofre.

## <a name="add-a-secret-to-key-vault"></a>Adicionar um segredo ao Key Vault

Para adicionar um segredo ao cofre, apenas tem de efetuar alguns passos adicionais. Esta palavra-passe pode ser utilizada por uma aplicação. A palavra-passe será denominada **ExamplePassword** e irá armazenar o valor **Pa$$w0rd**.

Escreva os comandos abaixo para criar um segredo no Key Vault denominado **ExamplePassword** que irá armazenar o valor **Pa$$w0rd**:

```azurecli
az keyvault secret set --vault-name 'Contoso-Vault2' --name 'ExamplePassword' --value 'Pa$$w0rd'
```

Agora, pode referenciar esta palavra-passe que adicionou ao Azure Key Vault com o respetivo URI. Utilize **https://ContosoVault.vault.azure.net/secrets/ExamplePassword** para obter a versão atual. 

Para ver o valor contido no segredo como texto simples:

```azurecli
az keyvault secret show --name 'ExamplePassword' --vault-name 'Contoso-Vault2'
```

Agora, criou um Key Vault, armazenou um segredo e obteve-o.

## <a name="clean-up-resources"></a>Limpar recursos

Outros inícios rápidos e tutoriais desta coleção têm por base este início rápido. Se quiser continuar a trabalhar com os inícios rápidos e tutoriais subsequentes, pode manter estes recursos.
Quando já não forem necessários, pode utilizar o comando [az group delete](/cli/azure/group#delete) para remover o grupo de recursos e todos os recursos relacionados. Pode eliminar os recursos da seguinte forma:

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou um Key Vault e armazenou um segredo. Para saber mais sobre o Key Vault e como utilizá-lo com as suas aplicações, avance para o tutorial referente às aplicações Web que funcionam com o Key Vault.

> [!div class="nextstepaction"]
> Para saber como ler um segredo no Key Vault através de uma aplicação Web com identidades do serviço gerido, avance para o seguinte tutorial [Configure an Azure web application to read a secret from Key vault](tutorial-web-application-keyvault.md) (Configurar uma aplicação Web do Azure para ler um segredo no Key Vault)
