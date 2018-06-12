---
title: Utilizar uma ferramenta de validação do modelo para procurar modelos a pilha do Azure | Microsoft Docs
description: Verificação de modelos de implementação para a pilha do Azure
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 61c893848176a89b4b6ed8d7a46f27bdeff5cec1
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294498"
---
# <a name="check-your-templates-for-azure-stack-with-the-template-validation-tool"></a>Verifique os modelos de pilha do Azure com a ferramenta de validação do modelo

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode utilizar a ferramenta de validação do modelo para verificar se o Azure Resource Manager [modelos](azure-stack-arm-templates.md) , estará pronto para implementar a pilha do Azure. A ferramenta de validação do modelo está disponível como parte das ferramentas de pilha do Azure. Transferir as ferramentas de pilha do Azure utilizando os passos descritos no [descarregar as ferramentas a partir do GitHub](azure-stack-powershell-download.md) artigo.

## <a name="overview"></a>Descrição geral

Para validar um modelo, terá de criar uma nuvem capacidades primeiro o ficheiro e, em seguida, execute a ferramenta de validação. Utilize os seguintes módulos do PowerShell de ferramentas de pilha do Azure:

- No **CloudCapabilities** pasta:<br>         AzureRM.CloudCapabilities.psm1 cria um ficheiro JSON de capacidades de nuvem que representa os serviços e as versões de uma nuvem de pilha do Azure.
- No **TemplateValidator** pasta:<br>
AzureRM.TemplateValidator.psm1 utiliza um ficheiro JSON de capacidades de nuvem para testar os modelos de implementação na pilha do Azure.

## <a name="build-the-cloud-capabilities-file"></a>Criar o ficheiro de capacidades de nuvem

Antes de utilizar a validação do modelo, execute o módulo do PowerShell de AzureRM.CloudCapabilities para criar um ficheiro JSON.

>[!NOTE]
>Se atualizar o sistema integrado ou adicionar quaisquer novos serviços ou extensões virtual, deve executar novamente este módulo.

1. Certifique-se de que tem ligação à pilha do Azure. Estes passos podem ser executados a partir do anfitrião de kit de desenvolvimento de pilha do Azure ou pode utilizar um [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) para ligar a partir da sua estação de trabalho.
2. Importe o módulo PowerShell do AzureRM.CloudCapabilities:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. Utilize o cmdlet Get-CloudCapabilities para obter versões de service e crie um ficheiro JSON de capacidades de nuvem. Se não especificar **- OutputPath**, o ficheiro AzureCloudCapabilities.Json é criado no diretório atual. Utilize a localização real:

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>Validar modelos

Utilize estes passos para validar os modelos utilizando o módulo do AzureRM.TemplateValidator PowerShell. Pode utilizar os seus próprios modelos ou validar o [modelos de início rápido do Azure pilha](https://github.com/Azure/AzureStack-QuickStart-Templates).

1. Importe o módulo PowerShell do AzureRM.TemplateValidator.psm1:

    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2. Execute a validação do modelo:

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Erros ou avisos de validação do modelo tem sessão iniciados a consola do PowerShell e um ficheiro HTML no diretório de origem. A seguinte captura de ecrã mostra um exemplo de um relatório de validação:

![Relatório de validação do modelo](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parâmetros

A validação do modelo suporta os seguintes parâmetros.

| Parâmetro | Descrição | Necessário |
| ----- | -----| ----- |
| TemplatePath | Especifica o caminho para recursivamente localizar modelos Azure Resource Manager | Sim | 
| TemplatePattern | Especifica o nome dos ficheiros de modelo para fazer corresponder. | Não |
| CapabilitiesPath | Especifica o caminho para o ficheiro JSON de capacidades de nuvem | Sim | 
| IncludeComputeCapabilities | Inclui a avaliação dos recursos IaaS como extensões de VM e tamanhos de VM | Não |
| IncludeStorageCapabilities | Inclui a avaliação dos recursos de armazenamento como tipos SKU | Não |
| Relatório | Especifica o nome do relatório HTML gerado | Não |
| Verboso | Os registos de erros e avisos para a consola do | Não|

### <a name="examples"></a>Exemplos

Neste exemplo valida todas as [modelos de início rápido do Azure pilha](https://github.com/Azure/AzureStack-QuickStart-Templates) transferido para o armazenamento local. O exemplo também valida tamanhos de máquinas virtuais e extensões de capacidades do Kit de desenvolvimento de pilha do Azure.

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json `
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```

## <a name="next-steps"></a>Passos seguintes

- [Implementar modelos pilha do Azure](azure-stack-arm-templates.md)
- [Desenvolver modelos para a pilha do Azure](azure-stack-develop-templates.md)
