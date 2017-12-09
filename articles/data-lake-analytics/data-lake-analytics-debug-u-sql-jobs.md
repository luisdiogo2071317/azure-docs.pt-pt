---
title: "Definido pelo utilizador código c# para tarefas com falha do Azure Data Lake U-SQL de depuração | Microsoft Docs"
description: "Saiba como depurar uma vértice falhada de U-SQL com o Azure Data Lake Tools para Visual Studio."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/31/2017
ms.author: yanacai
ms.openlocfilehash: 739d46753729b70a24dbd3d6e2d78f8513e143e6
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Depurar definido pelo utilizador código c# para as tarefas que falharam U-SQL

U-SQL fornece um modelo de extensibilidade com c#. Em scripts U-SQL, é fácil chamar as funções de c# e executar as funções analíticas que não suporta o idioma declarativo como o SQL Server. Para saber mais extensibilidade do U-SQL, consulte [guia de programação para U-SQL](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). 

Na prática, qualquer código poderá ter de depuração, mas é difícil de uma tarefa distribuída com o código personalizado na nuvem de depuração com ficheiros de registo limitado. [Ferramentas do Azure Data Lake para Visual Studio](http://aka.ms/adltoolsvs) fornece uma funcionalidade denominada **falha vértice depurar**, que ajuda a mais facilmente depurar falhas que ocorrem no seu código personalizado. Quando a tarefa de U-SQL falha, o serviço mantém o estado de falha e a ferramenta ajuda-o a transferir o ambiente de falha de nuvem para o computador local para depuração. A transferência local captura o ambiente de nuvem completo, incluindo quaisquer dados de entrada e o código de utilizador.

O vídeo seguinte demonstra falha vértice de depuração no Azure Data Lake Tools para Visual Studio.

> [!VIDEO https://www.youtube.com/embed/3enkNvprfm4]
>

> [!IMPORTANT]
> Visual Studio requer as seguintes duas atualizações para utilizar esta funcionalidade: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) e [Universal C Runtime para o Windows](https://www.microsoft.com/download/details.aspx?id=50410).
>

## <a name="download-failed-vertex-to-local-machine"></a>Falha ao transferir o vértice no computador local

Quando abre uma tarefa no Azure Data Lake Tools para Visual Studio, verá uma barra de alerta amarela com mensagens de erro detalhadas no separador erro.

1. Clique em **transferir** para transferir todos os recursos necessários e fluxos de entrada. Se não concluir a transferência, clique em **repita**.

2. Clique em **abra** depois de concluída a transferência para gerar um ambiente de depuração local. Uma nova instância do Visual Studio com uma solução de depuração é automaticamente criada e aberta.

![Azure vértice de transferência do visual studio de depuração do U-SQL do Data Lake Analytics](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

## <a name="configure-the-debugging-environment"></a>Configure o ambiente de depuração

> [!NOTE]
> Antes de depuração, não se esqueça de verificar **exceções de tempo de execução de idioma comum** na janela definições de exceção (**Ctrl + Alt + I**).

![Definição do visual studio de depuração de U-SQL do Data Lake Analytics do Azure](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

Nova instância iniciada do Visual Studio, pode ou não pode localizar o definido pelo utilizador c# código de origem:

1. [Pode encontrar o meu código de origem na solução](#source-code-is-included-in-debugging-solution)

2. [Não é possível encontrar o meu código de origem na solução](#source-code-is-not-included-in-debugging-solution)

### <a name="source-code-is-included-in-debugging-solution"></a>Código de origem está incluído na solução de depuração

Existem dois cenários que é capturado o código de origem do c#:

1. O código de utilizador está definido no ficheiro code-behind (normalmente designado `Script.usql.cs` num projeto U-SQL).

2. O código de utilizador está definido na classe biblioteca projeto c# para aplicação de U-SQL e registado como uma assemblagem com **informações de depuração**.

Se o código de origem é importado para a solução, pode utilizar as ferramentas de depuração do Visual Studio (veja, variáveis, etc.) para resolver o problema:

1. Prima **F5** para iniciar a depuração. O código seja executado até, mas está parado por uma exceção.

2. Abra o ficheiro de código fonte e configurar pontos de interrupção, em seguida, prima **F5** para depurar o código de passo a passo.

    ![Exceção de depuração de U-SQL do Data Lake Analytics do Azure](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

### <a name="source-code-is-not-included-in-debugging-solution"></a>Código de origem não está incluído na solução de depuração

Se o código de utilizador não está incluído no ficheiro code-behind ou não foi possível registar a assemblagem com o **informações de depuração**, em seguida, o código de origem não é automaticamente incluído na solução de depuração. Neste caso, terá de passos adicionais para adicionar o seu código de origem:

1. Clique com botão direito **solução 'VertexDebug' > Adicionar > projeto existente...**  para localizar a assemblagem de código de origem e adicionar o projeto para a solução de depuração.

    ![Projeto de adicionar de depuração de U-SQL do Data Lake Analytics do Azure](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Obter o caminho de pasta do projeto para **FailedVertexDebugHost** projeto. 

3. Clique com botão direito **o projeto de código de origem de assemblagem foi adicionada > propriedades**, selecione o **criar** separador no lado esquerdo e cole o caminho copiado terminando \bin\debug como **saída >docaminhodesaída**. O caminho de saída final é como "<DataLakeTemp path>\fd91dd21-776e-4729-a78b-81ad85a4fba6\loiu0t1y.mfo\FailedVertexDebug\FailedVertexDebugHost\bin\Debug\".

    ![Caminho do pdb de conjunto de depuração do Azure Data Lake Analytics U-SQL](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

Depois destas definições, iniciar depuração com **F5** e pontos de interrupção. Também pode utilizar o Visual Studio (veja, variáveis, etc.) para resolver o problema de ferramentas de depuração.

> [!NOTE]
> Reconstrua o projeto de código de origem de assemblagem sempre depois de modificar o código para gerar ficheiros .pdb atualizado.

## <a name="resubmit-the-job"></a>Volte a submeter a tarefa

Depois de depuração, se o projeto for concluída com êxito a janela de resultados mostra a mensagem seguinte:

    The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).

![Êxito de depuração de U-SQL do Data Lake Analytics do Azure](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

Para submeter a tarefa falhada:

1. Para as tarefas com soluções de code-behind, copie o código c# para o ficheiro de origem do code-behind (normalmente `Script.usql.cs`).

2. Para as tarefas com assemblagens, faça duplo clique o projeto de código de origem de assemblagem na solução de depuração e registe as assemblagens. dll atualizado para o catálogo de Azure Data Lake.

3. Volte a submeter a tarefa de U-SQL.

## <a name="next-steps"></a>Passos seguintes

- [Guia de programação para U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
- [Desenvolver operadores definido pelo utilizador U-SQL para tarefas do Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Testar e depurar tarefas U-SQL utilizando a execução local e o SDK U-SQL do Azure Data Lake](data-lake-analytics-data-lake-tools-local-run.md)
- [Como resolver problemas de uma tarefa periódica anormal](data-lake-analytics-data-lake-tools-debug-recurring-job.md)