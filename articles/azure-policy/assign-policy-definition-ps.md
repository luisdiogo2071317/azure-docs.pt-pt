---
title: "Utilizar o PowerShell para criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure | Microsoft Docs"
description: "Utilize o PowerShell para criar uma atribuição de política do Azure para identificar recursos incompatíveis."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 1/17/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 67c779b96dab088d810d22ad3053ade106aec56a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure | PowerShell

O primeiro passo para compreender a conformidade no Azure consiste em identificar o estado dos seus recursos. Este guia de introdução acompanha-o ao longo do processo de criação de uma atribuição de política para identificar máquinas virtuais que não estão a utilizar discos geridos.

No final deste processo, vai identificar com êxito as máquinas virtuais que não estão a utilizar discos geridos. Estão em *não conformidade* com a atribuição de política.

O PowerShell é utilizado para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts. Este guia detalha o uso do PowerShell para criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure.

Este guia requer a versão 4.0 ou posterior do módulo do Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

Antes de começar, certifique-se de que a versão mais recente do PowerShell está instalada. Veja [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs) para informações detalhadas.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.


## <a name="create-a-policy-assignment"></a>Criar uma atribuição de política

Neste guia de introdução vai criar uma atribuição de política e atribuir as *Máquinas Virtuais de Auditoria sem uma definição de Managed Disks*. Esta definição de política identifica recursos que não estão em conformidade com as condições definidas na definição de política.

Siga estes passos para criar uma nova atribuição de política.

1. Para garantir que a sua subscrição funciona com o fornecedor de recursos, registe o fornecedor de recursos de informações de política. Para registar um fornecedor de recursos, tem de ter permissão para efetuar a operação de ação de registo para o fornecedor de recursos. Esta operação está incluída nas funções de Contribuinte e Proprietário.

    Execute o seguinte comando para registar o fornecedor de recursos:

    ```
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.PolicyInsights
```

    Não é possível anular o registo de um fornecedor de recursos enquanto tiver tipos de recursos do fornecedor de recursos na sua subscrição.

    Para obter mais detalhes sobre como registar e visualizar os fornecedores de recursos, veja [Fornecedores e Tipos de Recursos](../azure-resource-manager/resource-manager-supported-services.md).

2. Após registar o fornecedor de recursos, execute o seguinte comando para ver todas as definições de política e descobrir a que pretende atribuir:

    ```powershell
$definition = Get-AzureRmPolicyDefinition
```

    O Azure Policy já inclui as definições de política incorporada que pode utilizar. Verá as definições de política incorporada, tais como:

    - Impor etiqueta e o respetivo valor
    - Aplicar etiqueta e o respetivo valor
    - Requer a Versão do SQL Server 12.0

3. Em seguida, atribua a definição de política para o âmbito pretendido com o cmdlet `New-AzureRmPolicyAssignment`.

Para este tutorial, utilize as seguintes informações para o comando:

- Apresente o **Nome** da atribuição de política. Neste caso, utilize Máquinas Virtuais de Auditoria sem Managed Disks.
- **Política** – A definição de política, com base na qual está a utilizar para criar a atribuição. Neste caso, é a definição de política – *Máquinas Virtuais de Auditoria sem Discos Geridos*
- O **âmbito** - Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta. Pode ir de uma subscrição aos grupos de recursos. Neste exemplo, está a atribuir a definição de política ao grupo de recursos **FabrikamOMS**.
- **$definition** – tem de apresentar o ID de recurso da definição de política – neste caso, está a utilizar o ID de definição de política - *Máquinas Virtuais de Auditoria sem Managed Disks*.

```powershell
$rg = Get-AzureRmResourceGroup -Name "FabrikamOMS"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
New-AzureRMPolicyAssignment -Name Audit Virtual Machines without Managed Disks Assignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Agora, está pronto para identificar recursos incompatíveis para compreender o estado de compatibilidade do seu ambiente.

## <a name="identify-non-compliant-resources"></a>Identificar recursos não compatíveis

1. Navegue de volta para a página de destino do Azure Policy.
2. Selecione **Conformidade** no painel esquerdo e procure a **Atribuição de Política** que criou.

   ![Conformidade com a política](media/assign-policy-definition/policy-compliance.png)

   Se existirem quaisquer recursos existentes que não sejam compatíveis com esta nova atribuição, aparecem no separador **Recursos em não conformidade**.

## <a name="clean-up-resources"></a>Limpar recursos

Outros guias desta coleção têm por base este guia de introdução. Se quiser continuar a trabalhar com os tutoriais subsequentes, não limpe os recursos criados neste guia de introdução. Se não pretender continuar, elimine a atribuição que criou ao executar este comando:

```powershell
Remove-AzureRmPolicyAssignment -Name “Audit Virtual Machines without Managed Disks Assignment” -Scope /subscriptions/ bc75htn-a0fhsi-349b-56gh-4fghti-f84852/resourceGroups/FabrikamOMS
```

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, atribuiu uma definição de política para identificar recursos incompatíveis no seu ambiente do Azure.

Para saber mais sobre a atribuição de políticas, de forma a certificar-se de que os **futuros** recursos que são criados estão em conformidade, avance para o tutorial:

> [!div class="nextstepaction"]
> [Criar e gerir políticas](./create-manage-policy.md)
