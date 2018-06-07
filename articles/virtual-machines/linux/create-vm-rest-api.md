---
title: Criar uma máquina virtual Linux com a API de REST do Azure | Microsoft Docs
description: Saiba como criar uma máquina virtual Linux no Azure que utiliza a autenticação discos geridos e o SSH com a API REST do Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: e3f41bea26e9a5ff45b31ae9d9a2e5955317ad7a
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825326"
---
# <a name="create-a-linux-virtual-machine-that-uses-ssh-authentication-with-the-rest-api"></a>Criar uma máquina virtual de Linux que utiliza autenticação SSH com a API REST

Uma máquina virtual (VM) no Azure é definida por vários parâmetros, tais como localização, tamanho de hardware, imagem do sistema operativo e as credenciais de início de sessão. Este artigo mostra como utilizar a API REST para criar uma máquina virtual de Linux que utiliza a autenticação SSH.

Para criar ou atualizar uma máquina virtual, utilize o seguinte *colocar* operação:

``` http
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2017-12-01
```

## <a name="create-a-request"></a>Criar um pedido

Para criar o *colocar* pedido, o `{subscription-id}` é necessário o parâmetro. Se tiver várias subscrições, consulte [trabalhar com várias subscrições](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions). É possível definir um `{resourceGroupName}` e `{vmName}` para os seus recursos, juntamente com o `api-version` parâmetro. Este artigo utiliza `api-version=2017-12-01`.

Os cabeçalhos seguintes são necessários:

| Cabeçalho de pedido   | Descrição |
|------------------|-----------------|
| *Tipo de conteúdo:*  | Necessário. Definido como `application/json`. |
| *Autorização:* | Necessário. Definido como um `Bearer` [token de acesso](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

Para obter mais informações sobre como criar o pedido, consulte [componentes de uma REST API pedido/resposta](/rest/api/azure/#components-of-a-rest-api-requestresponse).

## <a name="create-the-request-body"></a>Criar o corpo do pedido

As seguintes definições comuns são utilizadas para criar um corpo do pedido:

| Nome                       | Necessário | Tipo                                                                                | Descrição  |
|----------------------------|----------|-------------------------------------------------------------------------------------|--------------|
| localização                   | Verdadeiro     | cadeia                                                                              | Localização do recurso. |
| name                       |          | cadeia                                                                              | Nome da máquina virtual. |
| properties.hardwareProfile |          | [HardwareProfile](/rest/api/compute/virtualmachines/createorupdate#hardwareprofile) | Especifica as definições de hardware da máquina virtual. |
| properties.storageProfile  |          | [StorageProfile](/rest/api/compute/virtualmachines/createorupdate#storageprofile)   | Especifica as definições de armazenamento para os discos da máquina virtual. |
| properties.osProfile       |          | [OSProfile](/rest/api/compute/virtualmachines/createorupdate#osprofile)             | Especifica as definições de sistema operativo da máquina virtual. |
| properties.networkProfile  |          | [NetworkProfile](/rest/api/compute/virtualmachines/createorupdate#networkprofile)   | Especifica as interfaces de rede da máquina virtual. |

Para obter uma lista completa das definições disponíveis no corpo do pedido, consulte [máquinas virtuais criar ou atualizar defintions de corpo do pedido](/rest/api/compute/virtualmachines/createorupdate#definitions).

### <a name="example-request-body"></a>Corpo do pedido de exemplo

O corpo do pedido de exemplo seguinte define uma imagem de 18.04 LTS Ubuntu que utiliza discos Premium gerido. Autenticação de chave pública SSH é utilizada e a VM utiliza um existente rede virtual cartão de interface (NIC) que tiver [criado anteriormente](../../virtual-network/virtual-network-network-interface.md). Forneça a chave pública SSH no *osProfile.linuxConfiguration.ssh.publicKeys.keyData* campo. Se necessário, pode [gerar um par de chaves SSH](mac-create-ssh-keys.md).

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
| 200 OK      | [Máquina virtual](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | OK          |
| 201 criado | [Máquina virtual](/rest/api/compute/virtualmachines/createorupdate#virtualmachine) | Criado     |

Para obter mais informações sobre as respostas de REST API, consulte [processar a mensagem de resposta](/rest/api/azure/#process-the-response-message).

### <a name="example-response"></a>Resposta de exemplo

Um condensed *criado 201* mostra a resposta do anterior exemplo corpo do pedido que cria uma VM um *vmId* foi atribuída e o *provisioningState* é *Criar*:

```json
{
    "vmId": "e0de9b84-a506-4b95-9623-00a425d05c90",
    "provisioningState": "Creating"
}
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as APIs REST do Azure ou outras ferramentas de gestão, tais como 2.0 de CLI do Azure ou do Azure PowerShell, consulte o seguinte:

- [Fornecedor de computação do Azure REST API](/rest/api/compute/)
- [Introdução à API REST do Azure](/rest/api/azure/)
- [CLI 2.0 do Azure](/cli/azure/)
- [Módulo do PowerShell do Azure](/powershell/azure/overview)
