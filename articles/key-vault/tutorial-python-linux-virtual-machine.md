---
title: Tutorial – utilizar o Azure Key Vault com uma máquina virtual do Azure em Python | Documentos da Microsoft
description: Este tutorial, irá configurar uma aplicação Python ao ler um segredo a partir de um cofre de chaves
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 8c816d17807432d75b6102190fc37d25a525d7cf
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244176"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-virtual-machine-in-python"></a>Tutorial: Utilizar o Azure Key Vault com uma máquina virtual do Azure em Python

O Azure Key Vault ajuda a proteger segredos, como as chaves de API e as cadeias de ligação necessárias para acessar seus aplicativos, serviços e recursos de TI de base de dados.

Neste tutorial, siga os passos para obter uma aplicação web do Azure para ler as informações do Azure Key Vault através de identidades geridas para recursos do Azure. Saiba como:

> [!div class="checklist"]
> * Criar um cofre de chaves.
> * Armazene um segredo no cofre de chaves.
> * Crie uma máquina virtual do Azure.
> * Ativar uma [identidade gerida](../active-directory/managed-identities-azure-resources/overview.md) para a máquina virtual.
> * Conceda as permissões necessárias para a aplicação de consola para ler dados a partir do Cofre de chaves.
> * Obter um segredo do cofre de chaves.

Antes de prosseguir, leia os [conceitos básicos sobre o Key Vault](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Pré-requisitos
Para todas as plataformas, terá de:

* Git ([transferir](https://git-scm.com/downloads)).
* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
* [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versão 2.0.4 ou posterior. Está disponível para Windows, Mac e Linux.

### <a name="managed-service-identity-and-how-it-works"></a>Identidade de serviço gerida e como funciona
Este tutorial faz uso de identidade de serviço gerida (MSI).

O Azure Key Vault pode armazenar credenciais em segurança, para que eles não estão no seu código. Para recuperá-los, tem de autenticar para o Key Vault. Para autenticar para o Key Vault, precisa de uma credencial. Isso é um problema de bootstrap clássico. Através do Azure e Azure Active Directory (Azure AD), o MSI fornece uma "identidade bootstrap" que torna mais simples de dar início ao processo.

Quando ativar o MSI para um serviço do Azure como máquinas virtuais, o serviço de aplicações ou funções, o Azure cria um [principal de serviço](key-vault-whatis.md#basic-concepts) para a instância do serviço no Azure AD. Azure injeta as credenciais do principal de serviço para a instância do serviço. 

![MSI](media/MSI.png)

Em seguida, o seu código chama um serviço de metadados local que está disponível no recurso do Azure para obter um token de acesso.
O código utiliza o token de acesso que obtém a partir do ponto de final MSI local para autenticar a um serviço do Azure Key Vault. 

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Para iniciar sessão no Azure com a CLI do Azure, introduza:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az-group-create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

Selecione um nome do grupo de recursos e preencha o marcador de posição.
O exemplo seguinte cria um grupo de recursos na localização E.U.A Oeste:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

O grupo de recursos que acabou de criar é utilizado ao longo deste artigo.

## <a name="create-a-key-vault"></a>Criar um cofre de chaves

A seguir, vai criar um cofre de chaves no grupo de recursos criado no passo anterior. Forneça as seguintes informações:

* Nome do Cofre de chaves: O nome tem de ser uma cadeia de caracteres de 3 a 24 carateres e tem de conter apenas 0-9, a-z, A-Z e hífenes (-).
* Nome do grupo de recursos.
* Localização: **Oeste dos E.U.A.**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Nesta altura, a sua conta do Azure é a única autorizada a realizar quaisquer operações neste novo cofre.

## <a name="add-a-secret-to-the-key-vault"></a>Adicionar um segredo ao cofre de chaves

Estamos a adicionar um segredo para ajudar a ilustrar a forma como isto funciona. Pode armazenar uma cadeia de ligação do SQL ou outras informações que precise de manter em segurança, mas mantenha disponível para a sua aplicação.

Escreva os seguintes comandos para criar um segredo no cofre de chaves designado *AppSecret*. Este segredo irá armazenar o valor **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Criar uma VM com o [az vm criar](/cli/azure/vm#az_vm_create) comando.

O seguinte exemplo cria uma VM com o nome *myVM* e adiciona uma conta de utilizador com o nome *azureuser*. O `--generate-ssh-keys` parâmetro automaticamente gera uma chave SSH e coloca-o na localização predefinida da chave (*~/.ssh*). Para utilizar um conjunto específico de chaves em vez disso, utilize a opção `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

São necessários alguns minutos para criar a VM e os recursos de suporte. O resultado de exemplo seguinte mostra que a criação de VM foi concluída com êxito:

```
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-00-00-00-00-00",
  "powerState": "VM running",
  "privateIpAddress": "XX.XX.XX.XX",
  "publicIpAddress": "XX.XX.XXX.XXX",
  "resourceGroup": "myResourceGroup"
}
```

Tenha em atenção o seu `publicIpAddress` valor na saída da VM. Este endereço é utilizado para aceder à VM nos próximos passos.

## <a name="assign-an-identity-to-the-virtual-machine"></a>Atribuir uma identidade para a máquina virtual
Neste passo, estamos criando uma identidade atribuída de sistema para a máquina virtual. Execute o seguinte comando na CLI do Azure:

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Segue-se a saída do comando. Tenha em atenção o valor de **systemAssignedIdentity**. 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-the-virtual-machine-identity-permission-to-the-key-vault"></a>Conceder a permissão de identidade da máquina virtual para o Cofre de chaves
Agora podemos dar a permissão de identidade para o Cofre de chaves. Execute o seguinte comando:

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-virtual-machine"></a>Inicie sessão na máquina virtual

Inicie sessão na máquina virtual ao seguir [deste tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).

## <a name="create-and-run-the-sample-python-app"></a>Criar e executar a aplicação de Python de exemplo

O ficheiro de exemplo seguinte é denominado *Sample.py*. Ele usa o [pedidos](https://pypi.org/project/requests/2.7.0/) biblioteca para fazer chamadas HTTP GET.

## <a name="edit-samplepy"></a>Editar Sample.py
Depois de criar Sample.py, abra o ficheiro e copie o seguinte código. O código é um processo de dois passos: 
1. Obter um token a partir do ponto de final MSI local na VM. O ponto final, em seguida, obtém um token do Azure Active Directory.
2. Transmitir o token para o Cofre de chaves e obter o seu segredo. 

```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from an MSI-enabled Azure resource      
    # Note that the resource here is https://vault.azure.net for the public cloud, and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # Extracting data in JSON format 
    # This request gets an access token from Azure Active Directory by using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access token received from the previous HTTP GET call to the key vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Ao executar o seguinte comando, deverá ver o valor secreto: 

```
python Sample.py
```

O código anterior mostra como fazer operações com o Azure Key Vault numa máquina virtual do Windows. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [API de REST do Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/)
