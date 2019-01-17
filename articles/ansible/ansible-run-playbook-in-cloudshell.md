---
title: Executar o Ansible com o Bash no Azure Cloud Shell
description: Saiba como efetuar várias tarefas do Ansible com o Bash no Azure Cloud Shell
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, manual de procedimentos, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: quickstart
ms.openlocfilehash: 61b23b5bc2620c82051b0ba1de4bb54b44a495e0
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359192"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Executar o Ansible com o Bash no Azure Cloud Shell

Neste tutorial, irá aprender a utilizar o Bash no Cloud Shell para configurar uma subscrição do Azure como a sua área de trabalho do Ansible. 

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure** - se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Configurar o Azure Cloud Shell** - se estiver familiarizado com o Azure Cloud Shell, o artigo [Início rápido para Bash no Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart), ilustra como iniciar e configurar o Cloud Shell. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Configuração automática de credenciais

Quando iniciar sessão no Cloud Shell, Ansible autentica com o Azure para gerir a infraestrutura sem qualquer configuração adicional. Se tiver mais de uma subscrição, pode escolher qual assinatura do Ansible deve funcionar com exportando o `AZURE_SUBSCRIPTION_ID` variável de ambiente. Para listar todas as suas subscrições do Azure, execute o seguinte comando:

```azurecli-interactive
az account list
```

Utilizar o **id** da subscrição com a qual pretende trabalhar, definir o **AZURE_SUBSCRIPTION_ID** da seguinte forma:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Verificar a configuração
Para verificar a configuração com êxito, utilize o Ansible para criar um grupo de recursos.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"] 
> [Criar uma máquina virtual básica no Azure com o Ansible](/azure/virtual-machines/linux/ansible-create-vm)