---
title: Restringir o acesso de rede para recursos de PaaS - CLI do Azure | Microsoft Docs
description: Neste artigo, irá aprender a limitar e restringir o acesso de rede para recursos do Azure, tais como o Storage do Azure e SQL Database do Azure, com pontos finais do serviço de rede virtual, utilizando a CLI do Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: f357861a7a44b249e06f091a8693b7f2d8dd5178
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
ms.locfileid: "30841984"
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-cli"></a>Restringir o acesso de rede para recursos de PaaS com pontos finais do serviço de rede virtual, utilizando a CLI do Azure

Pontos finais do serviço de rede virtual permitem-lhe limitar o acesso de rede a alguns recursos do serviço do Azure para uma sub-rede de rede virtual. Também pode remover o acesso à internet para os recursos. Pontos finais de serviço fornecem ligação direta a partir da sua rede virtual para os serviços do Azure suportadas, permitindo-lhe utilizar o espaço de endereços privados da sua rede virtual para aceder aos serviços do Azure. O tráfego destinado aos recursos do Azure através de pontos finais de serviço sempre permanece da rede principal do Microsoft Azure. Neste artigo, saiba como:

* Criar uma rede virtual com uma sub-rede
* Adicionar uma sub-rede e ativar um ponto final de serviço
* Criar um recurso do Azure e permitir o acesso de rede ao mesmo de apenas uma sub-rede
* Implementar uma máquina virtual (VM) em cada sub-rede
* Confirmar o acesso a um recurso de sub-rede
* Confirmar o acesso é negado a um recurso de uma sub-rede e a internet

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este guia de introdução requer que está a executar a CLI do Azure versão 2.0.28 ou posterior. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Antes de criar uma rede virtual, tem de criar um grupo de recursos para a rede virtual e todos os outros recursos criados neste artigo. Crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Criar uma rede virtual com uma sub-rede com [az rede vnet criar](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

## <a name="enable-a-service-endpoint"></a>Ativar um ponto final de serviço 

Pode ativar a pontos finais do serviço apenas para serviços que suportam pontos finais de serviço. Ver os serviços de ativado o ponto final de serviço disponíveis numa localização do Azure com [vnet ponto final de lista-Serviços de rede az](/cli/azure/network/vnet#az_network_vnet_list_endpoint_services). O exemplo seguinte devolve uma lista de serviços ativados de ponto final de serviço disponíveis no *eastus* região. A lista de serviços devolvidos irá aumentar ao longo do tempo, como serviços mais Azure tornam-se o ponto final de serviço ativado.

```azurecli-interactive
az network vnet list-endpoint-services \
  --location eastus \
  --out table
``` 

Criar uma sub-rede adicional na rede virtual com [az rede vnet sub-rede](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Neste exemplo, um ponto final de serviço para *Microsoft* é criado para a sub-rede: 

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24 \
  --service-endpoints Microsoft.Storage
```

## <a name="restrict-network-access-for-a-subnet"></a>Restringir o acesso de rede para uma sub-rede

Criar um grupo de segurança de rede com [az rede nsg criar](/cli/azure/network/nsg#az_network_nsg_create). O exemplo seguinte cria um grupo de segurança de rede com o nome *myNsgPrivate*.

```azurecli-interactive
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsgPrivate
```

Associar o grupo de segurança de rede para o *privada* sub-rede com [atualização de sub-rede da vnet de rede de az](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update). O exemplo seguinte associa a *myNsgPrivate* grupo de segurança de rede para o *privada* sub-rede:

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Private \
  --resource-group myResourceGroup \
  --network-security-group myNsgPrivate
```

Criar regras de segurança com [criar regras de nsg de rede az](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). A regra que se segue permite acesso de saída para os endereços IP públicos atribuída para este serviço de armazenamento do Azure: 

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-Storage-All \
  --access Allow \
  --protocol "*" \
  --direction Outbound \
  --priority 100 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Storage" \
  --destination-port-range "*"

Each network security group contains several [default security rules](security-overview.md#default-security-rules). The rule that follows overrides a default security rule that allows outbound access to all public IP addresses. The `destination-address-prefix "Internet"` option denies outbound access to all public IP addresses. The previous rule overrides this rule, due to its higher priority, which allows access to the public IP addresses of Azure Storage.

az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Deny-Internet-All \
  --access Deny \
  --protocol "*" \
  --direction Outbound \
  --priority 110 \
  --source-address-prefix "VirtualNetwork" \
  --source-port-range "*" \
  --destination-address-prefix "Internet" \
  --destination-port-range "*"

The following rule allows SSH traffic inbound to the subnet from anywhere. The rule overrides a default security rule that denies all inbound traffic from the internet. SSH is allowed to the subnet so that connectivity can be tested in a later step.

az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsgPrivate \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 120 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "VirtualNetwork" \
  --destination-port-range "22"
```

## <a name="restrict-network-access-to-a-resource"></a>Restringir o acesso de rede a um recurso

Os passos necessários para restringir o acesso de rede para recursos criados através de serviços do Azure ativados para pontos finais de serviço varia em serviços. Consulte a documentação para serviços individuais para obter passos específicos para cada serviço. O resto deste artigo inclui os passos para restringir o acesso de rede para uma conta de armazenamento do Azure, como um exemplo.

### <a name="create-a-storage-account"></a>Create a storage account

Criar uma conta de armazenamento do Azure com [criar conta de armazenamento az](/cli/azure/storage/account#az_storage_account_create). Substitua `<replace-with-your-unique-storage-account-name>` com um nome que é exclusivo em todas as localizações do Azure, entre 3 e 24 carateres de comprimento, a utilizar apenas números e letras minúsculas.

```azurecli-interactive
storageAcctName="<replace-with-your-unique-storage-account-name>"

az storage account create \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --sku Standard_LRS \
  --kind StorageV2
```

Depois de criar a conta de armazenamento, obter a cadeia de ligação para a conta de armazenamento para uma variável com [cadeia de ligação de mostrar de conta de armazenamento az](/cli/azure/storage/account#az_storage_account_show_connection_string). A cadeia de ligação é utilizada para criar uma partilha de ficheiros num passo posterior.

```azurecli-interactive
saConnectionString=$(az storage account show-connection-string \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --query 'connectionString' \
  --out tsv)
```

<a name="account-key"></a>Ver o conteúdo da variável e anote o valor para **AccountKey** devolvido na saída, porque é utilizado num passo posterior.

```azurecli-interactive
echo $saConnectionString
```

### <a name="create-a-file-share-in-the-storage-account"></a>Criar uma partilha de ficheiros na conta de armazenamento

Criar uma partilha de ficheiros na conta de armazenamento com [criar partilha de armazenamento az](/cli/azure/storage/share#az_storage_share_create). Num passo posterior, esta partilha de ficheiros está montada para confirmar o acesso de rede ao mesmo.

```azurecli-interactive
az storage share create \
  --name my-file-share \
  --quota 2048 \
  --connection-string $saConnectionString > /dev/null
```

### <a name="deny-all-network-access-to-a-storage-account"></a>Negar o acesso de rede de todos os a uma conta de armazenamento

Por predefinição, as contas do storage aceitam ligações de rede de clientes em qualquer rede. Para limitar o acesso a redes selecionadas, altere a ação predefinida para *negar* com [atualização az da conta de armazenamento](/cli/azure/storage/account#az_storage_account_update). Quando é negado o acesso à rede, a conta de armazenamento não está acessível a partir de qualquer rede.

```azurecli-interactive
az storage account update \
  --name $storageAcctName \
  --resource-group myResourceGroup \
  --default-action Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Ativar o acesso de rede de sub-rede

Permitir o acesso de rede para a conta de armazenamento do *privada* sub-rede com [adicionar conta de armazenamento az rede-regra](/cli/azure/storage/account/network-rule#az_storage_account_network_rule_add).

```azurecli-interactive
az storage account network-rule add \
  --resource-group myResourceGroup \
  --account-name $storageAcctName \
  --vnet-name myVirtualNetwork \
  --subnet Private
```
## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Para testar o acesso de rede a uma conta de armazenamento, implemente uma VM para cada sub-rede.

### <a name="create-the-first-virtual-machine"></a>Criar primeira máquina virtual

Criar uma VM com o *pública* sub-rede com [az vm criar](/cli/azure/vm#az_vm_create). Se as chaves SSH ainda não existir numa localização chaves predefinido, o comando cria-los. Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --generate-ssh-keys
```

A VM demora alguns minutos a criar. Depois de criada a VM, a CLI do Azure mostra as informações semelhante ao seguinte exemplo: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmPublic",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Tome nota do **publicIpAddress** na saída devolvida. Este endereço é utilizado para aceder a VM a partir da internet num passo posterior.

### <a name="create-the-second-virtual-machine"></a>Criar a máquina virtual segundo

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --generate-ssh-keys
```

A VM demora alguns minutos a criar. Após a criação, tome nota do **publicIpAddress** no resultado devolvido. Este endereço é utilizado para aceder a VM a partir da internet num passo posterior.

## <a name="confirm-access-to-storage-account"></a>Confirmar o acesso à conta de armazenamento

SSH para a *myVmPrivate* VM. Substitua *<publicIpAddress>* com o endereço IP público do seu *myVmPrivate* VM.

```bash 
ssh <publicIpAddress>
```

Crie uma pasta para um ponto de montagem:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Monte a partilha de ficheiros do Azure para o diretório que criou. Antes de executar o seguinte comando, substitua `<storage-account-name>` com o nome da conta e `<storage-account-key>` com a chave que obteve no [criar uma conta de armazenamento](#create-a-storage-account).

```bash
sudo mount --types cifs //<storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Recebe o `user@myVmPrivate:~$` linha. A partilha de ficheiros do Azure com êxito montado para *mnt/MyAzureFileShare*.

Confirme que a VM possui sem conectividade de saída para outros endereços IP públicos:

```bash
ping bing.com -c 4
```

Receberá não existem respostas, porque o grupo de segurança de rede associados para o *privada* sub-rede não permite o acesso de saída para endereços IP públicos que não sejam endereços atribuídos para este serviço de armazenamento do Azure.

Sair da sessão SSH para a *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Confirmar o acesso foi negado à conta de armazenamento

Utilize o seguinte comando para criar uma sessão SSH com o *myVmPublic* VM. Substitua `<publicIpAddress>` com o endereço IP público do seu *myVmPublic* VM: 

```bash 
ssh <publicIpAddress>
```

Crie um diretório para um ponto de montagem:

```bash
sudo mkdir /mnt/MyAzureFileShare
```

Tentativa de montar a partilha de ficheiros do Azure para o diretório que criou. Este artigo pressupõe que implementou a versão mais recente do Ubuntu. Se estiver a utilizar versões anteriores do Ubuntu, consulte o artigo [montar no Linux](../storage/files/storage-how-to-use-files-linux.md?toc=%2fazure%2fvirtual-network%2ftoc.json) para obter instruções adicionais sobre como montar partilhas de ficheiros. Antes de executar o seguinte comando, substitua `<storage-account-name>` com o nome da conta e `<storage-account-key>` com a chave que obteve no [criar uma conta de armazenamento](#create-a-storage-account):

```bash
sudo mount --types cifs //storage-account-name>.file.core.windows.net/my-file-share /mnt/MyAzureFileShare --options vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
```

Acesso negado e recebe um `mount error(13): Permission denied` erro, porque o *myVmPublic* VM é implementada dentro do *pública* sub-rede. O *pública* sub-rede não tem um ponto final de serviço ativado para o Storage do Azure e a conta de armazenamento só permite o acesso de rede do *privada* sub-rede, não o *público*sub-rede.

Sair da sessão SSH para a *myVmPublic* VM.

Tentativa do seu computador ver as partilhas na sua conta de armazenamento com [lista de partilha do armazenamento az](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list). Substitua `<account-name>` e `<account-key>` com o nome da conta de armazenamento e a chave de [criar uma conta de armazenamento](#create-a-storage-account):

```azurecli-interactive
az storage share list \
  --account-name <account-name> \
  --account-key <account-key>
```

Acesso negado e recebe um *este pedido não está autorizado a efetuar esta operação* erro, porque o computador não está a ser o *privada* sub-rede do *MyVirtualNetwork* rede virtual.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [eliminação do grupo de az](/cli/azure#az_group_delete) para remover o grupo de recursos e todos os recursos nele contidos.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, ativar um ponto final de serviço para uma sub-rede de rede virtual. Aprendeu a que os pontos finais de serviço podem ser ativados para recursos implementados com vários serviços do Azure. Criou uma conta de armazenamento do Azure e o acesso limitado à rede para a conta de armazenamento para apenas os recursos dentro de uma sub-rede de rede virtual. Para obter mais informações sobre pontos finais de serviço, consulte o artigo [descrição geral de pontos finais de serviço](virtual-network-service-endpoints-overview.md) e [gerir sub-redes](virtual-network-manage-subnet.md).

Se tiver várias redes virtuais na sua conta, poderá pretender ligar duas redes virtuais em conjunto para que recursos dentro de cada rede virtual podem comunicar entre si. Para saber como, consulte [ligar redes virtuais](tutorial-connect-virtual-networks-cli.md).