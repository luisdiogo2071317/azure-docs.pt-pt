---
title: "Bash no guia de introdução de Shell de nuvem do Azure | Microsoft Docs"
description: "Início rápido para Bash na Shell de nuvem"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: juluk
ms.openlocfilehash: e48c54216c5c4ae8e53d4802aafce8883ee97c11
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Início rápido para Bash na Shell de nuvem do Azure

Este documento fornece detalhes sobre como utilizar Bash na Shell de nuvem do Azure no [portal do Azure](https://ms.portal.azure.com/).

> [!NOTE]
> A [PowerShell na Shell de nuvem do Azure](quickstart-powershell.md) início rápido também está disponível.

## <a name="start-cloud-shell"></a>Iniciar a Shell de nuvem
1. Iniciar **nuvem Shell** da parte superior da navegação do portal do Azure. <br>
![](media/quickstart/shell-icon.png)

2. Selecione uma subscrição para criar uma conta de armazenamento e partilha de ficheiros do Microsoft Azure.
3. Selecione "Armazenamento criar"

> [!TIP]
> São automaticamente autenticado para Azure CLI 2.0 em cada sessão.

### <a name="select-the-bash-environment"></a>Selecione o ambiente de Bash
Verifique se o ambiente de lista pendente do lado esquerdo da janela de shell indica `Bash`. <br>
![](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Definir a sua subscrição
1. Subscrições da lista que tem acesso.
```azurecli-interactive
az account list
```

2. Defina a sua subscrição preferencial: <br>
```azurecli-interactive
az account set --subscription my-subscription-name`
```

> [!TIP]
> A subscrição irá ser memorizada para futuras sessões utilizando `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos
Crie um novo grupo de recursos no WestUS com o nome "MyRG".
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Criar uma VM do Linux
Crie uma VM com Ubuntu no seu novo grupo de recursos. O 2.0 CLI do Azure irá criar chaves SSH e configurar a VM com os mesmos. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Utilizar `--generate-ssh-keys` dá instruções ao 2.0 do CLI do Azure para criar e configurar as chaves públicas e privadas na VM e `$Home` diretório. Por predefinição, as chaves são colocadas na Shell de nuvem no `/home/<user>/.ssh/id_rsa` e `/home/<user>/.ssh/id_rsa.pub`. O `.ssh` pasta é mantida na imagem de 5 GB a partilha de ficheiros anexados utilizada para manter `$Home`.

O nome de utilizador nesta VM será o seu nome de utilizador utilizada na Shell de nuvem ($User@Azure:).

### <a name="ssh-into-your-linux-vm"></a>SSH para a VM com Linux
1. Procure o nome da VM na barra de pesquisa de portal do Azure.
2. Clique em "Ligar" para obter o nome da VM e o endereço IP público. <br>
![](media/quickstart/sshcmd-copy.png)

3. SSH para a VM com o `ssh` cmd.
```
ssh username@ipaddress
```

Depois de estabelecer a ligação SSH, deverá ver a linha de comandos de boas-vindas da Ubuntu. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Limpeza 
1. Saída do ssh sessão.
```azurecli-interactive
exit
```

2. Elimine o grupo de recursos e quaisquer recursos dentro da mesma.
```azurecli-interactive
Run `az group delete -n MyRG`
```

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre ficheiros persistentes para Bash na Shell de nuvem](persisting-shell-storage.md) <br>
[Saiba mais sobre a CLI do Azure 2.0](https://docs.microsoft.com/cli/azure/) <br>
[Saiba mais sobre armazenamento de ficheiros do Azure](../storage/files/storage-files-introduction.md) <br>
