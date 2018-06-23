---
title: Gerir o Cofre de chaves do Azure com a CLI | Microsoft Docs
description: Utilize este artigo para automatizar tarefas comuns no Cofre de chaves utilizando o CLI 2.0
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: ''
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: barclayn
ms.openlocfilehash: 8c2501b5e89e81709de074c0b0c93b317ecebd7b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316599"
---
# <a name="manage-key-vault-using-cli-20"></a>Gerir o Cofre de chaves a utilizar o CLI 2.0

Este artigo abrange como começar a trabalhar com o Cofre de chaves do Azure utilizando o 2.0 CLI do Azure. Pode ver informações sobre:
- Como criar um contentor protegido (cofre) no Azure
- Como armazenar e gerir chaves criptográficas e segredos no Azure. 
- Utilizar a CLI do Azure para criar um cofre.
- Criar uma chave ou a palavra-passe que, em seguida, pode utilizar com uma aplicação do Azure. 
- Como uma aplicação pode utilizar a chave de criação ou a palavra-passe.

O Cofre de Chaves do Azure chave está disponível na maior parte das regiões. Para obter mais informações, consulte a [página de preços do Cofre de Chaves](https://azure.microsoft.com/pricing/details/key-vault/).


> [!NOTE]
> Este artigo não inclui instruções sobre como escrever a aplicação Azure que um dos passos inclui, que mostra como autorizar uma aplicação para utilizar uma chave ou segredo no Cofre de chaves.
>

Para obter uma descrição geral do Cofre de chaves do Azure, consulte [que é o Cofre de chaves do Azure?](key-vault-whatis.md)

## <a name="prerequisites"></a>Pré-requisitos
Para utilizar os comandos da CLI do Azure neste artigo, tem de ter os seguintes itens:

* Uma subscrição ao Microsoft Azure. Se não tiver uma, pode inscrever-se numa [avaliação gratuita](https://azure.microsoft.com/pricing/free-trial).
* Interface de linha de comandos versão 2.0 ou posterior. Para instalar a versão mais recente, consulte [instalar e configurar o 2.0 de Interface de linha de comandos de plataforma do Azure](/cli/azure/install-azure-cli).
* Uma aplicação que será configurada para utilizar a chave ou a palavra-passe que criar neste artigo. Uma aplicação de exemplo está disponível a partir do [Centro de Transferências da Microsoft](http://www.microsoft.com/download/details.aspx?id=45343). Para obter instruções, consulte o ficheiro Leia-me incluído.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Obter ajuda com Interface de linha de comandos de plataforma do Azure
Este artigo pressupõe que está familiarizado com a interface de linha de comandos (Bash, Terminal, linha de comandos).

O – ajuda ou -h parâmetro pode ser utilizado para ver a ajuda para comandos específicos. Em alternativa, do Azure ajuda [comando] também pode ser utilizado demasiado o formato [opções]. Quando em dúvida sobre os parâmetros necessários para um comando, consulte a ajuda. Por exemplo, todos os seguintes comandos devolvem as mesmas informações:

```azurecli-interactive
az account set --help
az account set -h
```

Também pode ler os artigos seguintes para se familiarizar com o Azure Resource Manager na Interface de linha de comandos de plataforma do Azure:

* [Instalar a CLI do Azure](/cli/azure/install-azure-cli)
* [Introdução ao Azure CLI 2.0](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a>Estabelecer a ligação às suas subscrições

Para iniciar sessão interativamente, utilize o seguinte comando:

```azurecli
az login
```
Inicie sessão com uma conta institucional, pode passar no seu nome de utilizador e palavra-passe.

```azurecli
az login -u username@domain.com -p password
```

Se tiver mais do que uma subscrição e tem de especificar que pretende utilizar, escreva o seguinte para ver as subscrições da sua conta:

```azurecli
az account list
```

Especifique uma subscrição com o parâmetro de subscrição.

```azurecli
az account set --subscription <subscription name or ID>
```

Para obter mais informações sobre a configuração de Interface de linha de comandos de plataforma do Azure, consulte [instalar CLI do Azure](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Criar um novo grupo de recursos
Ao utilizar o Azure Resource Manager, todos os recursos relacionados são criados dentro de um grupo de recursos. Pode criar um cofre de chaves num grupo de recursos existente. Se pretender utilizar um novo grupo de recursos, pode criar um novo.

```azurecli
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

O primeiro parâmetro é o nome do grupo de recursos e o segundo parâmetro é a localização. Para obter uma lista de todas as localizações possíveis tipo:

```azurecli
az account list-locations
``` 

## <a name="register-the-key-vault-resource-provider"></a>Registar o fornecedor de recursos do Cofre de chaves
 Poderá ver o erro "a subscrição não está registada para utilizar o espaço de nomes 'Keyvault'" quando tenta criar um novo cofre de chaves. Se essa mensagem for apresentada, certifique-se de que fornecedor de recursos do Cofre de chaves está registado na sua subscrição. Esta é uma operação única para cada subscrição.

```azurecli
az provider register -n Microsoft.KeyVault
```


## <a name="create-a-key-vault"></a>Criar um cofre de chaves

Utilize o `az keyvault create` comando para criar um cofre de chaves. Este script tem três parâmetros obrigatórios: um nome de grupo de recursos, um nome do Cofre de chaves e a localização geográfica.

Para criar um novo cofre com o nome **ContosoKeyVault**, no grupo de recursos **ContosoResourceGroup**, que reside no **Oriental** localização, escreva: 

```azurecli
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

O resultado deste comando mostra as propriedades do Cofre de chaves que criou. As duas propriedades mais importantes são:

* **nome**: no exemplo, o nome é ContosoKeyVault. Irá utilizar este nome para outros comandos do Cofre de chaves.
* **vaultUri**: no exemplo, o URI é https://contosokeyvault.vault.azure.net. As aplicações que utilizam o cofre através da respetiva API têm de utilizar este URI.

A sua conta do Azure pode agora realizar quaisquer operações neste cofre de chaves. Chegou, foi autorizado mais ninguém está autorizado.

## <a name="add-a-key-secret-or-certificate-to-the-key-vault"></a>Adicionar uma chave, o segredo ou o certificado para o Cofre de chaves

Se pretender que o Cofre de chaves do Azure para criar uma chave protegida por software para si, utilize o `az key create` comando.

```azurecli
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```

Se tiver uma chave existente num ficheiro. pem, pode carregá-lo ao Cofre de chaves do Azure. Pode optar por proteger a chave com o software ou de HSM. Utilize o seguinte para importar a chave do ficheiro. pem e protegê-lo com o software:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'Pa$$w0rd' --protection software
```

Agora, pode referenciar a chave criou ou carregou no Cofre de chaves do Azure, utilizando o seu respetivo URI. Utilize **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** obter sempre a versão atual. Utilize https://[keyvault-name].vault.azure.net/keys/[keyname]/[key-unique-id] para obter esta versão específica. Por exemplo, **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**. 

Adicione um segredo no cofre, que é uma palavra-passe designada SQLPassword e que tem o valor de Pa$ $w0rd para cofres de chaves do Azure. 

```azurecli
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```

Referencia esta palavra-passe utilizando o seu respetivo URI. Utilize **https://ContosoVault.vault.azure.net/secrets/SQLPassword** obter sempre a versão atual e https://[keyvault-name].vault.azure.net/secret/[secret-name]/[secret-unique-id] para obter esta versão específica. Por exemplo, **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Importe um certificado para o Cofre de utilizar um. pem ou. pfx.

```azurecli
az keyvault certificate import --vault-name 'ContosoKeyVault' --file 'c:\cert\cert.pfx' --name 'ContosoCert' --password 'Pa$$w0rd'
```

Vejamos a chave, o segredo ou o certificado que criou:

* Para ver as suas chaves, escreva: 

```azurecli
az keyvault key list --vault-name 'ContosoKeyVault'
```

* Para ver os segredos, escreva: 

```azurecli
az keyvault secret list --vault-name 'ContosoKeyVault'
```

* Para ver os certificados, escreva: 

```azurecli
az keyvault certificate list --vault-name 'ContosoKeyVault'
```

## <a name="register-an-application-with-azure-active-directory"></a>Registar uma aplicação com o Azure Active Directory
Geralmente, este passo deve ser realizado por um programador, num computador à parte. -Não é específico ao Cofre de chaves do Azure, embora esteja incluído aqui, para deteção. Para concluir o registo de aplicação, a conta, o Cofre e a aplicação tem de estar no mesmo diretório do Azure.

As aplicações que utilizam um cofre de chaves devem ser autenticadas através de um token do Azure Active Directory.  O proprietário da aplicação tem registá-lo no Azure Active Directory primeiro. No final do registo, o proprietário da aplicação obtém os seguintes valores:

- Um **ID da aplicação** (também conhecido como o ID de cliente do AAD ou appID)
- **Chave de autenticação** (também conhecida como segredo partilhado). 

Para obter um token, a aplicação deve apresentar ambos os valores ao Azure Active Directory. Como uma aplicação está configurada para obter um token irá depender da aplicação. Na [aplicação de exemplo do Key Vault](https://www.microsoft.com/download/details.aspx?id=45343), o proprietário da aplicação define estes valores no ficheiro app.config.

Para obter passos detalhados sobre como registar uma aplicação no Azure Active Directory deve rever os artigos intitulados [integrar aplicações com o Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md), [portal de utilização para criar um Active Directory do Azure Aplicação de diretório e um principal de serviço que pode aceder aos recursos](../azure-resource-manager/resource-group-create-service-principal-portal.md), e [criar um Azure principal de serviço com o Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

Para registar uma aplicação no Azure Active Directory:

```azurecli
az ad sp create-for-rbac -n "MyApp" --password 'Pa$$w0rd' --skip-assignment
# If you don't specify a password, one will be created for you.
```

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorizar a aplicação a utilizar a chave ou o segredo

Para autorizar a aplicação a aceder à chave ou segredo no cofre, utilize o `az keyvault set-policy` comando.

Por exemplo, se o nome do seu Cofre for ContosoKeyVault, a aplicação tem um appID do 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed e pretende autorizar a aplicação a desencriptar e assinar com chaves no seu Cofre, utilize o seguinte comando:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Para autorizar a mesma aplicação a ler os segredos no seu Cofre, escreva o seguinte comando:

```azurecli
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```

## <a name="bkmk_KVperCLI"></a> Cofre de chaves conjunto avançado de políticas de acesso 
Utilize [atualização de keyvault az](/cli/azure/keyvault#az-keyvault-update) para ativar políticas avançadas para o Cofre de chaves. 

 Ativar o Cofre de chaves para a implementação: permite que as máquinas virtuais obter certificados armazenados como segredos do cofre.
 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-deployment 'true'
 ``` 

Ativar o Cofre de chaves de encriptação de disco: é necessário quando utilizar o cofre para a encriptação de disco do Azure.

 ```azurecli
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-disk-encryption 'true'
 ```  

Ativar o Cofre de chaves para implementação do modelo: permite que o Resource Manager para obter segredos do cofre.
 ```azurecli 
 az keyvault update --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --enabled-for-template-deployment 'true'
 ```

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Se pretender utilizar um módulo de segurança de hardware (HSM)

Para maior segurança, pode importar ou gerar chaves de módulos de segurança de hardware (HSMs) que nunca deixam o limite do HSM. Os HSMs têm a certificação FIPS 140-2 de nível 2 validada. Se este requisito não se aplica a si, ignore esta secção e aceda a [Eliminar o cofre de chaves e as chaves e segredos associados](#delete-the-key-vault-and-associated-keys-and-secrets).

Para criar estas chaves protegidas de HSM, tem de ter uma subscrição do cofre que suporta chaves protegidas de HSM.

Quando cria o keyvault, adicione o parâmetro de "sku":

```azurecli
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

Neste cofre, pode adicionar chaves protegidas por software (conforme mostrado anteriormente) e chaves protegidas por HSM. Para criar uma chave protegida por HSM, defina o parâmetro de destino para 'HSM':

```azurecli
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

Pode utilizar o seguinte comando para importar uma chave a partir de um ficheiro. pem no seu computador. Este comando importa a chave os HSMs no serviço Cofre de Chaves:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```

O comando seguinte importa um pacote "traga a sua própria chave" (BYOK). Isto permite-lhe gerar a chave no seu HSM local e transferi-la para HSMs no serviço Cofre de Chaves, sem que a chave saia do limite HSM:

```azurecli
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```

Para obter instruções mais detalhadas sobre como gerar este pacote BYOK, consulte [como utilizar chaves de HSM-Protected com o Cofre de chaves do Azure](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Eliminar o cofre de chaves e as chaves e segredos associados

Se já não precisar do Cofre de chaves e respetivas chaves ou segredos, pode eliminar o Cofre de chaves utilizando o `az keyvault delete` comando:

```azurecli
az keyvault delete --name 'ContosoKeyVault'
```

Em alternativa, pode eliminar um grupo de recursos completo do Azure, que inclui o Cofre de Chaves e quaisquer outros recursos que incluiu nesse grupo:

```azurecli
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Outros comandos de Interface de linha de comandos de plataforma do Azure

Outros comandos que poderão ser úteis para gerir o Cofre de chaves do Azure.

Este comando lista uma tabela de apresentação de todas as chaves e propriedades selecionadas:

```azurecli
az keyvault key list --vault-name 'ContosoKeyVault'
```

Este comando apresenta uma lista completa das propriedades para a chave especificada:

```azurecli
az keyvault key show --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Este comando lista uma tabela de apresentação de todos os nomes secretos e propriedades selecionadas:

```azurecli
az keyvault secret list --vault-name 'ContosoKeyVault'
```

Eis um exemplo de como remover uma chave específica:

```azurecli
az keyvault key delete --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey'
```

Eis um exemplo de como remover um segredo específico:

```azurecli
az keyvault secret delete --vault-name 'ContosoKeyVault' --name 'SQLPassword'
```

## <a name="next-steps"></a>Passos Seguintes

- Para referência CLI do Azure completa para comandos do Cofre de chaves, consulte [referência da CLI do Cofre de chave](/cli/azure/keyvault).

- Para as referências de programação, consulte o [Guia para programadores do Cofre de Chaves do Azure](key-vault-developers-guide.md).

- Para obter informações sobre o Cofre de chaves do Azure e HSMs, consulte [como utilizar chaves de HSM-Protected com o Cofre de chaves do Azure](key-vault-hsm-protected-keys.md).
