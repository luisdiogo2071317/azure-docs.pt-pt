---
title: Executar a primeira consulta do Resource Graph com o Azure PowerShell
description: Este artigo explica os passos para ativar o módulo do Resource Graph para o Azure PowerShell e executar a primeira consulta.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 001805aaf87ed6c3481a8ad8378cdc22ef74d274
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646393"
---
# <a name="run-your-first-resource-graph-query-using-azure-powershell"></a>Executar a primeira consulta do Resource Graph com o Azure PowerShell

O primeiro passo para utilizar o Azure Resource Graph é garantir que o módulo do Azure PowerShell está instalado. Este início rápido explica-lhe o processo para adicionar o módulo à instalação do Azure PowerShell.

No final deste processo, terá adicionado o módulo à instalação do Azure PowerShell à escolha e executado a sua primeira consulta do Resource Graph.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="add-the-resource-graph-module"></a>Adicionar o módulo do Resource Graph

Para ativar o Azure PowerShell para consultar o Azure Resource Graph, tem de adicionar o módulo. Este módulo pode ser utilizado com o Windows PowerShell instalado localmente e o PowerShell Core ou com a [imagem do Docker do Azure PowerShell](https://hub.docker.com/r/azuresdk/azure-powershell/).

### <a name="base-requirements"></a>Requisitos de base

O módulo do Azure Resource Graph necessita do seguinte software:

- Azure PowerShell 6.3.0 ou superior. Se não estiver ainda instalado, siga [estas instruções](/powershell/azure/install-azurerm-ps).

  - Para o PowerShell Core, utilize a versão **Az** do módulo Azure PowerShell.

  - Para o Windows PowerShell, utilize a versão **AzureRm** do módulo Azure PowerShell.

  > [!NOTE]
  > Atualmente, não se recomenda instalar o módulo no Cloud Shell.

- PowerShellGet 2.0.1 ou superior. Se não estiver ainda instalado ou atualizado, siga [estas instruções](/powershell/gallery/installing-psget).

### <a name="cloud-shell"></a>Cloud Shell,

Para adicionar o módulo do Azure Resource Graph no Cloud Shell, siga as instruções abaixo para o PowerShell Core.

### <a name="powershell-core"></a>PowerShell Core

O módulo do Resource Graph para o PowerShell Core é **Az.ResourceGraph**.

1. Na linha de comandos **administrativa** do PowerShell Core, execute o seguinte comando:

   ```azurepowershell-interactive
   # Install the Resource Graph module from PowerShell Gallery
   Install-Module -Name Az.ResourceGraph
   ```

1. Confirme que o módulo foi importado e se é a versão correta (0.3.0):

   ```azurepowershell-interactive
   # Get a list of commands for the imported Az.ResourceGraph module
   Get-Command -Module 'Az.ResourceGraph' -CommandType 'Cmdlet'
   ```

1. Ative os aliases retrocompatíveis de **Az** até **AzureRm** com o seguinte comando:

   ```azurepowershell-interactive
   # Enable backwards alias compatibility
   Enable-AzureRmAlias
   ```

### <a name="windows-powershell"></a>Windows PowerShell

O módulo do Resource Graph para o Windows PowerShell Core é **AzureRm.ResourceGraph**.

1. Na linha de comandos **administrativa** do Windows PowerShell, execute o seguinte comando:

   ```powershell
   # Install the Resource Graph (prerelease) module from PowerShell Gallery
   Install-Module -Name AzureRm.ResourceGraph -AllowPrerelease
   ```

1. Confirme que o módulo foi importado e se é a versão correta (0.1.1-preview):

   ```powershell
   # Get a list of commands for the imported AzureRm.ResourceGraph module
   Get-Command -Module 'AzureRm.ResourceGraph' -CommandType 'Cmdlet'
   ```

## <a name="run-your-first-resource-graph-query"></a>Executar a primeira consulta do Resource Graph

Com o módulo Azure PowerShell adicionado ao seu ambiente escolhido, é altura de experimentar uma consulta simples do Resource Graph. A consulta devolverá os cinco primeiros recursos do Azure com o **Nome** e o **Tipo de Recurso** de cada recurso.

1. Execute a primeira consulta do Azure Resource Graph com o cmdlet `Search-AzureRmGraph`:

   ```azurepowershell-interactive
   # Login first with Connect-AzureRmAccount if not using Cloud Shell

   # Run Azure Resource Graph query
   Search-AzureRmGraph -Query 'project name, type | limit 5'
   ```

   > [!NOTE]
   > Como este exemplo de consulta não fornece um modificador de ordenação como `order by`, é provável que executar esta consulta várias vezes produza um conjunto diferente de recursos em cada pedido.

1. Atualize a consulta para `order by` propriedade **Nome**:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with 'order by'
   Search-AzureRmGraph -Query 'project name, type | limit 5 | order by name asc'
   ```

  > [!NOTE]
  > Assim como na primeira consulta, é provável que executar esta consulta várias vezes produza um conjunto diferente de recursos em cada pedido. A ordem dos comandos da consulta é importante. Neste exemplo, `order by` vem depois de `limit`, ou seja, primeiro limita os resultados da consulta e, em seguida, ordena-os.

1. Atualize a consulta para primeiro `order by` propriedade **Nome** e, em seguida, `limit` os cinco resultados principais:

   ```azurepowershell-interactive
   # Run Azure Resource Graph query with `order by` first, then with `limit`
   Search-AzureRmGraph -Query 'project name, type | order by name asc | limit 5'
   ```

Quando a consulta final é executada várias vezes, partindo do princípio de que nada no seu ambiente está a mudar, os resultados devolvidos serão consistentes e conforme o esperado – ordenados pela propriedade **Nome**, mas continuam a ser limitados aos cinco resultados principais.

## <a name="cleanup"></a>Limpeza

Se quiser remover o módulo Resource Graph do seu ambiente do Azure PowerShell, poderá fazê-lo com o seguinte comando:

```powershell
# Remove the Resource Graph module from the Azure PowerShell environment
Remove-Module -Name 'AzureRm.ResourceGraph'
```

> [!NOTE]
> Este comando não elimina o ficheiro do módulo que transferiu anteriormente. Apenas o remove da sessão do PowerShell.

## <a name="next-steps"></a>Passos seguintes

- Obter mais informações sobre a [linguagem de consulta](./concepts/query-language.md)
- Aprender a [explorar recursos](./concepts/explore-resources.md)
- Executar a primeira consulta com a [CLI do Azure](first-query-azurecli.md)
- Ver exemplos de [Consultas de introdução](./samples/starter.md)
- Ver exemplos de [Consultas avançadas](./samples/advanced.md)
- Enviar comentários sobre o [UserVoice](https://feedback.azure.com/forums/915958-azure-governance)