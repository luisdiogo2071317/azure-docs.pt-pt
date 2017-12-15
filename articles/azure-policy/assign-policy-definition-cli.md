---
title: "Utilizar a CLI do Azure para criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure | Microsoft Docs"
description: "Utilize o PowerShell para criar uma atribuição de política do Azure para identificar recursos incompatíveis."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 12/06/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 88ceb47d46b66e716c6c263098d5b9458e4aff22
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure com a CLI do Azure

O primeiro passo para compreender a conformidade no Azure é saber onde está posicionado com os seus próprios recursos atuais. Este guia de introdução acompanha-o ao longo do processo de criação de uma atribuição de política para identificar máquinas virtuais que não estão a utilizar discos geridos.

No final deste processo, terá identificado com êxito as máquinas virtuais que não estão a utilizar discos geridos e são, por conseguinte, *incompatíveis*.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-policy-assignment"></a>Criar uma atribuição de política

Neste guia de introdução iremos criar uma atribuição de política e atribuir as Máquinas Virtuais de Auditoria sem uma definição de Discos Geridos. Esta definição de política identifica recursos que não estão em conformidade com as condições definidas na definição de política.

Siga estes passos para criar uma nova atribuição de política.

Veja todas as definições de política e encontre a definição de política "Máquinas Virtuais de Auditoria sem Discos Geridos":

```azurecli
az policy definition list
```

O Azure Policy já inclui as definições de política incorporada que pode utilizar. Verá as definições de política incorporada, tais como:

- Impor etiqueta e o respetivo valor
- Aplicar etiqueta e o respetivo valor
- Requer a Versão do SQL Server 12.0

Em seguida, apresente as seguintes informações e execute o seguinte comando para atribuir a definição de política:

- Apresente o **Nome** da atribuição de política. Neste caso, vamos utilizar *Máquinas de Virtuais de Auditoria sem Discos Geridos*.
- **Política** – Esta é a definição de política, com base na qual está a utilizar para criar a atribuição. Neste caso, é a definição de política – *Máquinas Virtuais de Auditoria sem Discos Geridos*
- O **âmbito** - Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta. Pode ir de uma subscrição aos grupos de recursos.

  Utilize a subscrição (ou grupo de recursos) que registou anteriormente. Neste exemplo, estamos a utilizar este ID de subscrição - **bc75htn-a0fhsi-349b-56gh-4fghti-f84852** e o nome do grupo de recursos - **FabrikamOMS**. Lembre-se de alterá-los para o ID da subscrição e o nome do grupo de recursos em que está a trabalhar.

O comando deve estar da seguinte forma:

```azurecli
az policy assignment create --name Audit Virtual Machines without Managed Disks Assignment --policy Audit Virtual Machines without Managed Disks --scope /subscriptions/
bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

Uma atribuição de política é uma política que foi atribuída para ter lugar num âmbito específico. Este âmbito também pode ir desde um grupo de gestão a um grupo de recursos.

## <a name="identify-non-compliant-resources"></a>Identificar recursos não compatíveis

Para ver os recursos que não são compatíveis com esta nova atribuição:

1. Navegue de volta para a página do Azure Policy.
2. Selecione **Conformidade** no painel esquerdo e procure a **Atribuição de Política** que criou.

   ![Conformidade com a política](media/assign-policy-definition/policy-compliance.png)

   Se existirem quaisquer recursos existentes que não sejam compatíveis com esta nova atribuição, estes são apresentados no separador **Recursos não compatíveis**, conforme mostrado acima.

## <a name="clean-up-resources"></a>Limpar recursos

Outros guias desta coleção têm por base este guia de introdução. Se quiser continuar a trabalhar com os tutoriais subsequentes, não limpe os recursos criados neste guia de introdução. Se não pretender continuar, elimine a atribuição que criou ao executar este comando:

```azurecli
az policy assignment delete –name  Assignment --scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852 resourceGroups/ FabrikamOMS
```

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, atribuiu uma definição de política para identificar recursos incompatíveis no seu ambiente do Azure.

Para saber mais sobre a atribuição de políticas, de forma a certificar-se de que os recursos que cria no **futuro** estão em conformidade, avance para o tutorial:

> [!div class="nextstepaction"]
> [Criar e gerir políticas](./create-manage-policy.md)
