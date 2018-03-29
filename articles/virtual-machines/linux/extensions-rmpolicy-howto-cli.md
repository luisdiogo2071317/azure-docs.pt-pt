---
title: Utilizar a política do Azure para restringir a instalação da extensão de VM | Microsoft Docs
description: Utilize a política do Azure para restringir as implementações de extensão VM.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/23/2018
ms.author: danis;cynthn
ms.openlocfilehash: 8e65b82730884947633688db9ed50080b96e0b8e
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="use-azure-policy-to-restrict-extensions-installation-on-linux-vms"></a>Utilizar a política do Azure para restringir a instalação de extensões em VMs do Linux

Se pretender impedir a utilização ou a instalação de determinadas extensões em VMs do Linux, pode criar uma política do Azure utilizando a CLI para restringir as extensões para VMs dentro de um grupo de recursos. 

Este tutorial utiliza o CLI na Shell de nuvem do Azure, que é constantemente atualizado para a versão mais recente. Se pretender executar localmente a CLI do Azure, tem de instalar a versão 2.0.26 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-rules-file"></a>Criar um ficheiro de regras

Para restringir a que extensões podem ser instalados, tem de ter um [regra](/azure/azure-policy/policy-definition#policy-rule) para fornecer a lógica para identificar a extensão.

Este exemplo mostra como negar instalar extensões publicadas pelo 'Microsoft.OSTCExtensions' através da criação de um ficheiro de regras na Shell de nuvem do Azure, mas se estiver a trabalhar na CLI localmente, também pode criar um ficheiro local e substitua o caminho para o caminho (~/clouddrive) o ficheiro local no seu computador.

Num [bash nuvem Shell](https://shell.azure.com/bash), tipo:

```azurecli-interactive 
vim ~/clouddrive/azurepolicy.rules.json
```

Copie e cole o seguinte. de JSON para o ficheiro.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.OSTCExtensions/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.OSTCExtensions/virtualMachines/extensions/publisher",
                "equals": "Microsoft.OSTCExtensions"
            },
            {
                "field": "Microsoft.OSTCExtensions/virtualMachines/extensions/type",
                "in": "[parameters('notAllowedExtensions')]"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

Quando tiver terminado, prima a **Esc** da chave e, em seguida, escreva **: wq** para guardar e fechar o ficheiro.


## <a name="create-a-parameters-file"></a>Criar um ficheiro de parâmetros

Também precisa de um [parâmetros](/azure/azure-policy/policy-definition#parameters) ficheiro que cria uma estrutura para que possa utilizar para transmitir numa lista de extensões para bloquear. 

Este exemplo mostra como criar um ficheiro de parâmetros para VMs do Linux na Shell de nuvem, mas se estiver a trabalhar na CLI localmente, também pode criar um ficheiro local e substitua o caminho (~/clouddrive) com o caminho para o ficheiro no seu computador local.

No [bash nuvem Shell](https://shell.azure.com/bash), tipo:

```azurecli-interactive
vim ~/clouddrive/azurepolicy.parameters.json
```

Copie e cole o seguinte. de JSON para o ficheiro.

```json
{
    "notAllowedExtensions": {
        "type": "Array",
        "metadata": {
            "description": "The list of extensions that will be denied. Example: CustomScriptForLinux, VMAccessForLinux etc.",
            "strongType": "type",
            "displayName": "Denied extension"
        }
    }
}
```

Quando tiver terminado, prima a **Esc** da chave e, em seguida, escreva **: wq** para guardar e fechar o ficheiro.

## <a name="create-the-policy"></a>Criar uma política

Uma definição de política é um objeto utilizado para armazenar a configuração que pretende utilizar. A definição de política utiliza os ficheiros de parâmetros e as regras para definir a política. Criar a definição de política utilizando [Criar definição de política de az](/cli/azure/role/assignment?view=azure-cli-latest#az_role_assignment_create).

Neste exemplo, os parâmetros e as regras são os ficheiros criados e armazenados como ficheiros. JSON na sua shell de nuvem.

```azurecli-interactive
az policy definition create \
   --name 'not-allowed-vmextension-linux' \
   --display-name 'Block VM Extensions' \
   --description 'This policy governs which VM extensions that are blocked.' \
   --rules '~/clouddrive/azurepolicy.rules.json' \
   --params '~/clouddrive/azurepolicy.parameters.json' \
   --mode All
```


## <a name="assign-the-policy"></a>Atribuir a política

Neste exemplo atribui a política a um grupo de recursos utilizando [de criação da atribuição de política de az](/cli/azure/policy/assignment#az_policy_assignment_create). Qualquer VM criada no **myResourceGroup** grupo de recursos não será possível instalar o acesso de VM Linux ou as extensões de Script personalizado para o Linux. O grupo de recursos tem de existir antes de pode atribuir a política.

Utilizar [lista de contas az](/cli/azure/account?view=azure-cli-latest#az_account_list) para obter o ID de subscrição para utilizar em vez do exemplo.


```azurecli-interactive
az policy assignment create \
   --name 'not-allowed-vmextension-linux' \
   --scope /subscriptions/<subscription Id>/resourceGroups/myResourceGroup \
   --policy "not-allowed-vmextension-linux" \
   --params '{
        "notAllowedExtensions": {
            "value": [
                "VMAccessForLinux",
                "CustomScriptForLinux"
            ]
        }
    }'
```

## <a name="test-the-policy"></a>Testar a política

Teste a política ao criar uma nova VM e a tentar adicionar um novo utilizador.


```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --generate-ssh-keys
```

Tente criar um novo utilizador com o nome **myNewUser** através da extensão de acesso de VM.

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --password 'mynewuserpwd123!'
```



## <a name="remove-the-assignment"></a>Remover a atribuição

```azurecli-interactive
az policy assignment delete --name 'not-allowed-vmextension-linux' --resource-group myResourceGroup
```
## <a name="remove-the-policy"></a>Remover a política

```azurecli-interactive
az policy definition delete --name 'not-allowed-vmextension-linux'
```


## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte [política Azure](../../azure-policy/azure-policy-introduction.md).
