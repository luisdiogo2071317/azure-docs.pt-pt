---
title: Criar uma máquina virtual Linux com a API de REST do Azure | Documentos da Microsoft
description: Saiba como criar uma máquina virtual Linux no Azure que utiliza a autenticação de Managed Disks e o SSH com a API REST do Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2018
ms.author: cynthn
ms.openlocfilehash: 11d9f5efb452d46e5ca30169861582f6f2bbbd1b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969398"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>Criar uma máquina virtual do Linux que utiliza autenticação SSH com a API REST

Uma máquina virtual (VM) no Azure é definida por vários parâmetros, como a localização e tamanho de hardware, imagem do sistema operativo e as credenciais de início de sessão. Este artigo mostra-lhe como utilizar a API REST para criar uma máquina virtual do Linux que utiliza autenticação SSH.

Para criar ou atualizar uma máquina virtual, utilize o seguinte procedimento *colocar* operação:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

## <a name="create-a-request"></a>Criar um pedido

Para criar o *colocar* pedido, o `{subscription-id}` é necessário o parâmetro. Se tiver várias subscrições, veja [trabalhar com várias subscrições](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions). Define um `{resourceGroupName}` e `{vmName}` para os seus recursos, juntamente com o `api-version` parâmetro. Este artigo utiliza `api-version=2017-12-01`.

Os seguintes cabeçalhos são necessários:

| Cabeçalho do pedido   | Descrição |
|------------------|-----------------|
| *Tipo de conteúdo:*  | Necessário. Definido como `application/json`. |
| *Autorização:* | Necessário. Definido como válido `Bearer` [token de acesso](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

Para obter mais informações sobre como criar o pedido, consulte [componentes de uma REST API de solicitação/resposta](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>Criar o corpo do pedido

As seguintes definições comuns são utilizadas para criar um corpo de pedido:

| Nome                       | Necessário | Tipo                                                                                | Descrição  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| localização                   | Verdadeiro     | cadeia                                                                              | Localização do recurso. |
| nome                       |          | cadeia                                                                              | Nome da máquina virtual. |
| properties.hardwareProfile |          | [HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Especifica as definições de hardware para a máquina virtual. |
| properties.storageProfile  |          | [StorageProfile](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Especifica as definições de armazenamento para os discos da máquina virtual. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Especifica as definições de sistema operativo da máquina virtual. |
| properties.networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Especifica as interfaces de rede da máquina virtual. |

Para obter uma lista completa das definições de disponíveis no corpo do pedido, consulte [máquinas virtuais criar ou atualizar as definições de corpo de pedido](/rest/api/compute/virtualmachines/createorupdate#definitions).

### <a name="example-request-body"></a>Corpo do pedido de exemplo

O corpo do pedido de exemplo seguinte define uma imagem do Ubuntu 18.04 LTS que utiliza discos geridos Premium. Autenticação de chave pública SSH é utilizada e a VM utiliza um rede virtual interface cartão existente (NIC) que tenha [criado anteriormente](../../virtual-network/virtual-network-network-interface.md). Forneça a chave pública de SSH a *osProfile.linuxConfiguration.ssh.publicKeys.keyData* campo. Se for necessário, pode [gerar um par de chaves SSH](mac-create-ssh-keys.md).

```json
{
  "location": "eastus",
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_DS1_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "18.04-LTS",
        "publisher": "Canonical",
        "version": "latest",
        "offer": "UbuntuServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      }
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "myVM",
      "linuxConfiguration": {
        "ssh": {
          "publicKeys": [
            {
              "path": "/home/{your-username}/.ssh/authorized_keys",
              "keyData": "ssh-rsa AAAAB3NzaC1{snip}mf69/J1"
            }
          ]
        },
        "disablePasswordAuthentication": true
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  },
  "name": "myVM"
}
```

## <a name="responses"></a>Respostas

Existem duas respostas com êxito para a operação criar ou atualizar uma máquina virtual:

| Nome        | Tipo                                                                              | Descrição |
|-------------|-----------------------------------------------------------------------------------|-------------|
| 200 OK      | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | OK          |
| 201 criado | [VirtualMachine](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Criado     |

Para obter mais informações sobre as respostas de REST API, consulte [processar a mensagem de resposta](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Resposta de exemplo

Um condensada *201 criado* mostra a resposta do corpo de pedido de exemplo anterior que cria uma VM uma *vmId* foi atribuído e o *provisioningState* é *Criando*:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as APIs de REST do Azure ou outras ferramentas de gestão, como a CLI do Azure ou do Azure PowerShell, consulte o seguinte:

- [API de REST do fornecedor de computação do Azure](/rest/api/compute/)
- [Introdução à API REST do Azure](/rest/api/azure/)
- [CLI do Azure](/cli/azure/)
- [Módulo Azure PowerShell](/powershell/azure/overview)
