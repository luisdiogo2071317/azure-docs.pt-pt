---
title: Depurar código do Azure Data Lake Analytics localmente | Documentos da Microsoft
description: Saiba como utilizar o Azure Data Lake Tools para Visual Studio para depurar tarefas U-SQL na estação de trabalho local.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: 181512c12c1e72e6aa8205aabd5ea22816d4c5df
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889676"
---
# <a name="debug-azure-data-lake-analytics-code-locally"></a>Depurar código do Azure Data Lake Analytics localmente

Pode utilizar o Azure Data Lake Tools para Visual Studio para executar e depurar seu código do Azure Data Lake Analytics na sua estação de trabalho, tal como no serviço do Azure Data Lake.

Saiba mais [como executar o script de U-SQL no seu computador local](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="debug-scripts-and-c-assemblies-locally"></a>Depurar scripts e assemblagens C# localmente

Pode depurar assemblagens c# sem as submeter e registar no serviço do Azure Data Lake Analytics. Pode definir pontos de interrupção no ficheiro code-behind e num projeto C# referenciado.

### <a name="to-debug-local-code-in-code-behind-file"></a>Para depurar código local no ficheiro code-behind

1. Configurar pontos de interrupção no ficheiro code-behind.
2. Pressione F5 para depurar o script localmente.

> [!NOTE]
   > O procedimento seguinte só funciona no Visual Studio 2015. No Visual Studio anterior poderá ter de adicionar manualmente os ficheiros pdb.  
   >
   >

### <a name="to-debug-local-code-in-a-referenced-c-project"></a>Para depurar código local num projeto C# referenciado

1. Crie um projeto de Assemblagem C# e crie-o para gerar a dll de saída.
2. Registe a dll com uma instrução U-SQL:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Defina pontos de interrupção no código C#.
4. Pressione F5 para depurar o script com referência à dll c# localmente.


## <a name="next-steps"></a>Passos Seguintes

- Para ver uma consulta mais complexa, consulte [analisar registos de sites com o Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Para ver os detalhes da tarefa, consulte [utilizar o Browser de trabalho e a vista de tarefas para tarefas de Azure Data Lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md).
- Para utilizar a vista de execução de vértice, consulte [utilizar a vista de execução de vértice no Data Lake Tools para Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
