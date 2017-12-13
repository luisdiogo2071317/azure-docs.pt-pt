---
title: "Utilizar o PowerShell para criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure | Microsoft Docs"
description: "Utilize o PowerShell para criar uma atribuição de política do Azure para identificar recursos incompatíveis."
services: azure-policy
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 11/02/2017
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 5c00d50817e40de0a43d05eb85662b494247d8fa
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/18/2017
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-using-powershell"></a>Criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure | PowerShell

O primeiro passo para compreender a conformidade no Azure é saber onde está posicionado com os seus próprios recursos atuais. Este guia de introdução acompanha-o ao longo do processo de criação de uma atribuição de política para identificar máquinas virtuais que não estão a utilizar discos geridos.

No final deste processo, terá identificado com êxito as máquinas virtuais que não estão a utilizar discos geridos e são, por conseguinte, *incompatíveis*.


O PowerShell é utilizado para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts. Este guia detalha o uso do PowerShell para criar uma atribuição de política para identificar recursos incompatíveis no seu ambiente do Azure.

Este guia requer a versão 4.0 ou posterior do módulo do Azure PowerShell. Executar ```Get-Module -ListAvailable AzureRM``` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

Antes de começar, certifique-se de que a versão mais recente do PowerShell está instalada. Para informações detalhadas, consulte [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs).

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="opt-in-to-azure-policy"></a>Participar no Azure Policy

O Azure Policy está agora disponível em Pré-visualização Pública e tem de se registar para pedir acesso.

1. Aceda ao Azure Policy em https://aka.ms/getpolicy e selecione **Inscrever** no painel esquerdo.

   ![Pesquisar política](media/assign-policy-definition/sign-up.png)

2. Participe no Azure Policy ao selecionar as subscrições na lista de **Subscrições**com que gostaria de trabalhar. Em seguida, selecione **Registar**.

   ![Participar no Azure Policy](media/assign-policy-definition/preview-opt-in.png)

   O seu pedido é aprovado automaticamente para a Pré-visualização. Aguarde até 30 minutos para o sistema processar o registo.

## <a name="create-a-policy-assignment"></a>Criar uma atribuição de política

Neste guia de introdução iremos criar uma atribuição de política e atribuir as *Máquinas Virtuais de Auditoria sem uma definição de Discos Geridos*. Esta definição de política irá identificar recursos que não estão em conformidade com as condições definidas na definição de política.

Siga estes passos para criar uma nova atribuição de política.

Execute o seguinte comando para ver todas as definições de política e descobrir a que pretende atribuir:

```powershell
$definition = Get-AzureRmPolicyDefinition
```

O Azure Policy já inclui as definições de política incorporada que pode utilizar. Verá as definições de política incorporada, tais como:

- Impor etiqueta e o respetivo valor
- Aplicar etiqueta e o respetivo valor
- Requer a Versão do SQL Server 12.0

Em seguida, atribua a definição de política para o âmbito pretendido com o cmdlet `New-AzureRmPolicyAssignment`.

Para este tutorial, estamos a apresentar as seguintes informações para o comando:
- Apresente o **Nome** da atribuição de política. Neste caso, vamos utilizar Máquinas de Virtuais de Auditoria sem Discos Geridos.
- **Política** – Esta é a definição de política, com base na qual está a utilizar para criar a atribuição. Neste caso, é a definição de política – *Máquinas Virtuais de Auditoria sem Discos Geridos*
- O **âmbito** - Um âmbito determina que recursos ou agrupamento de recursos em que a atribuição de política é imposta. Pode ir de uma subscrição aos grupos de recursos. Neste exemplo, atribuímos a definição de política ao grupo de recursos **FabrikamOMS**.
- **$definition** – tem de apresentar o ID de recurso da definição de política – neste caso, utilizamos o ID de definição de política - *Máquinas Virtuais de Auditoria sem Discos Geridos*.

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

   Se existirem quaisquer recursos existentes que não sejam compatíveis com esta nova atribuição, estes serão apresentados no separador **Recursos não compatíveis**, conforme mostrado acima.

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
