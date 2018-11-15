---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 831a72fff0931d116a669060b160f51fde6e1d3e
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572595"
---
## <a name="verify-the-output"></a>Verificar a saída
O pipeline cria automaticamente a pasta de saída no contentor de blobs adftutorial. Em seguida, copia o ficheiro emp.txt da pasta de entrada para a pasta de saída. 

1. No portal do Azure, na página do contentor **adftutorial**, clique em **Atualizar** para ver a pasta de saída. 
    
    ![Atualizar](media/data-factory-quickstart-verify-output-cleanup/output-refresh.png)
2. Clique em **saída** na lista de pastas. 
2. Confirme se o ficheiro **emp.txt** foi copiado para a pasta de saída. 

    ![Atualizar](media/data-factory-quickstart-verify-output-cleanup/output-file.png)

## <a name="clean-up-resources"></a>Limpar recursos
Pode limpar os recursos que criou no Guia de Introdução de duas formas. Pode eliminar o [grupo de recursos do Azure](../articles/azure-resource-manager/resource-group-overview.md), que inclui todos os recursos no grupo de recursos. Se quiser manter os outros recursos intactos, elimine apenas a fábrica de dados que criou neste tutorial.

A eliminação de um grupo de recursos dita a eliminação de todos os recursos, incluindo as fábricas de dados nele incluídas. Execute o seguinte comando para eliminar todo o grupo de recursos: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Nota: remover um grupo de recursos pode demorar algum tempo. Seja paciente durante o processo

Se quiser eliminar apenas a fábrica de dados, e não o grupo de recursos completo, execute o seguinte comando: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```