---
title: Encriptar discos para conjuntos de dimensionamento do Azure com a CLI do Azure | Microsoft Docs
description: Saiba como utilizar o 2.0 CLI do Azure para encriptar as instâncias VM e os discos ligados num conjunto de dimensionamento de máquina virtual do Linux
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: iainfou
ms.openlocfilehash: 22d3c763317def137b4e0beb155f28585d7c6ae1
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
ms.locfileid: "32776419"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-the-azure-cli-20-preview"></a>Encriptar o SO e discos de dados anexados num dimensionamento da máquina virtual com o 2.0 do CLI do Azure (pré-visualização)

Para proteger e proteger dados em descanso ao utilizar tecnologias de encriptação padrão da indústria, os conjuntos de dimensionamento de máquina virtual suportam encriptação de disco do Azure (ADE). Encriptação pode ser ativada para a máquina virtual com Linux e Windows conjuntos de dimensionamento. Para obter mais informações, consulte [Azure Disk Encryption para Linux e Windows](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Encriptação de disco do Azure para conjuntos de dimensionamento de máquina virtual está atualmente em pré-visualização pública, disponível em todas as regiões públicas do Azure.

A encriptação de disco do Azure é suportada:
- para o dimensionamento conjuntos criado com discos geridos e não suportadas para conjuntos de dimensionamento de disco nativo (ou não geridos).
- para volumes de dados e SO em conjuntos de dimensionamento do Windows. Desativar a encriptação é suportada para volumes de dados e SO para conjuntos de dimensionamento do Windows.
- volumes de dados em conjuntos de dimensionamento do Linux. Encriptação de disco de SO não é suportada na pré-visualização atual para conjuntos de dimensionamento do Linux.

Conjunto VM recriação de imagem e atualizar as operações de dimensionamento não são suportadas na pré-visualização atual. A encriptação de disco do Azure para a pré-visualização de conjuntos de dimensionamento de máquina virtual é recomendada apenas em ambientes de teste. Na pré-visualização, não ative a encriptação de disco em ambientes de produção, onde poderá ter de atualizar a imagem do SO num conjunto de dimensionamento encriptados.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial, necessita que está a executar a CLI do Azure versão 2.0.31 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="register-for-disk-encryption-preview"></a>Registar-se na pré-visualização de encriptação de disco

A encriptação de disco do Azure para a pré-visualização de conjuntos de dimensionamento de máquina virtual requer a Self-registar a sua subscrição com [o registo de funcionalidade az](/cli/azure/feature#az_feature_register). Só precisa de efetuar os passos seguintes na primeira vez que utilize a funcionalidade de pré-visualização de encriptação de disco:

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

Pode demorar até 10 minutos para que o pedido de registo se propagarem. Pode verificar o estado de registo com [mostrar de funcionalidade az](/cli/azure/feature#az_feature_show). Quando o `State` relatórios *registada*, volte a registar o *Mirosoft.Compute* fornecedor com [o registo de fornecedor az](/cli/azure/provider#az_provider_register):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

## <a name="create-a-scale-set"></a>Criar um conjunto de dimensionamento

Para poder criar um conjunto de dimensionamento, crie primeiro um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Agora, crie um conjunto de dimensionamento de máquinas virtuais com [az vmss create](/cli/azure/vmss#az_vmss_create). O exemplo seguinte cria um conjunto de dimensionamento com o nome *myScaleSet*, que está definido para atualizar automaticamente à medida que as alterações são aplicadas, e gera chaves SSH, caso não existam em *~/.ssh/id_rsa*. Um disco de dados de 32Gb está ligado a cada instância VM e do Azure [extensão de Script personalizado](../virtual-machines/linux/extensions-customscript.md) é utilizado para preparar os discos de dados com [az vmss extensão conjunto](/cli/azure/vmss/extension#az_vmss_extension_set):

```azurecli-interactive
# Create a scale set with attached data disk
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 32

# Prepare the data disk for use with the Custom Script Extension
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"],"commandToExecute":"./prepare_vm_disks.sh"}'
```

A criação e configuração de todas as VMs e recursos do conjunto de dimensionamento demora alguns minutos.

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Criar um cofre de chaves do Azure ativado para encriptação de disco

O Cofre de chaves do Azure pode armazenar as chaves, os segredos ou palavras-passe que permitem-lhe implementá-los em segurança nos seus serviços e aplicações. As chaves criptográficas são armazenadas no Cofre de chaves do Azure utilizando a proteção de software, ou pode importar ou gerar as suas chaves nos módulos de segurança de Hardware (HSMs) com certificação FIPS 140-2 normas de nível 2. Estas chaves criptográficas são utilizados para encriptar e desencriptar discos virtuais anexados à VM. Manter o controlo destas chaves criptográficas e pode auditar a sua utilização.

Definir os seus próprios exclusivo *keyvault_name*. Em seguida, crie um KeyVault com [az keyvault criar](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-create) na mesma subscrição e região que a escala definir e o *– ativada-para--encriptação de disco* política de acesso.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault create --resource-group myResourceGroup --name $keyvault_name --enabled-for-disk-encryption
```

### <a name="use-an-existing-key-vault"></a>Utilize um cofre de chaves existente

Este passo só é necessário se tiver um cofre de chaves existente que pretende utilizar com a encriptação de disco. Ignore este passo se criou um cofre de chaves na secção anterior.

Definir os seus próprios exclusivo *keyvault_name*. Em seguida, atualizar o KeyVault com [atualização de keyvault az](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-update) e defina o *– ativada-para--encriptação de disco* política de acesso.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault update --name $keyvault_name --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Ativar encriptação

Para encriptar as instâncias de VM num conjunto de dimensionamento, obtenha primeiro algumas informações sobre o ID de recurso do Cofre de chaves com [mostrar de keyvault az](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-show). Estas variáveis são utilizadas para, em seguida, iniciar o processo de encriptação com [ativar a encriptação de vmss az](/cli/azure/vmss/encryption#az-vmss-encryption-enable):

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

Pode demorar um minuto ou dois para iniciar o processo de encriptação.

Como o conjunto de dimensionamento é a política de atualização na escala definir criada no passo anterior está definido como *automática*, as instâncias de VM iniciar automaticamente o processo de encriptação. Em conjuntos de dimensionamento em que a política de atualização é manual, iniciar a política de encriptação nas instâncias de VM com [az vmss update-instâncias](/cli/azure/vmss#az-vmss-update-instances).

## <a name="check-encryption-progress"></a>Verificar progresso de encriptação

Para verificar o estado de encriptação de disco, utilize [mostrar de encriptação de vmss az](/cli/azure/vmss/encryption#az-vmss-encryption-show):

```azurecli-interactive
az vmss encryption show --resource-group myResourceGroup --name myScaleSet
```

Quando são encriptadas instâncias de VM, o código de estado relatórios *EncryptionState/encriptados*, conforme mostrado no seguinte exemplo de saída:

```bash
[
  {
    "disks": [
      {
        "encryptionSettings": null,
        "name": "myScaleSet_myScaleSet_0_disk2_3f39c2019b174218b98b3dfae3424e69",
        "statuses": [
          {
            "additionalProperties": {},
            "code": "EncryptionState/encrypted",
            "displayStatus": "Encryption is enabled on disk",
            "level": "Info",
            "message": null,
            "time": null
          }
        ]
      }
    ],
    "id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualMachines/0",
    "resourceGroup": "MYRESOURCEGROUP"
  }
]
```

## <a name="disable-encryption"></a>Desativar a encriptação

Se já não pretender utilizar discos de instâncias VM encriptados, pode desativar a encriptação com [desativar a encriptação de vmss az](/cli/azure/vmss/encryption?view=azure-cli-latest#az-vmss-encryption-disable) da seguinte forma:

```azurecli-interactive
az vmss encryption disable --resource-group myResourceGroup --name myScaleSet
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, utilizou o 2.0 CLI do Azure para encriptar um conjunto de dimensionamento de máquina virtual. Também pode utilizar [Azure PowerShell](virtual-machine-scale-sets-encrypt-disks-ps.md) ou modelos para [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) ou [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

É possível encontrar um exemplo de ficheiro batch ponto-a-ponto para encriptação de disco de dados de conjunto de dimensionamento de Linux [aqui](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat). Este exemplo cria um grupo de recursos, o conjunto de dimensionamento do Linux, monta um disco de dados de 5 GB e encripta o conjunto de dimensionamento de máquina virtual.