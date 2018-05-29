---
title: Introdução ao Azure Data Lake Analytics com o Visual Studio | Microsoft Docs
description: Saiba como instalar as Ferramentas do Data Lake para Visual Studio e como desenvolver e testar scripts U-SQL.
services: data-lake-analytics
documentationcenter: ''
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/02/2018
ms.author: saveenr, yanacai
ms.openlocfilehash: d0974e3258e0def09fe12d348180dcedf216401c
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33885256"
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>Desenvolver scripts U-SQL com as Ferramentas do Data Lake para Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Saiba como utilizar o Visual Studio para criar contas do Azure Data Lake Analytics, definir tarefas em [U-SQL](data-lake-analytics-u-sql-get-started.md) e submeter tarefas para o serviço Data Lake Analytics. Para mais informações sobre a Data Lake Analytics, consulte [Descrição geral da Análise do Azure Data Lake](data-lake-analytics-overview.md).

>[!IMPORTANT]
>
>Em preparação para o novo Regulamento Geral Sobre a Proteção de Dados (GDPR), que entrará em vigor no dia 25 de maio de 2018, recomenda-se que os utilizadores das Ferramentas do Azure Data Lake para o Visual Studio façam a atualização para a versão 2.3.3000.4 ou posterior. Esta versão inclui alterações com base nos requisitos de proteção de dados mais recentes. Tenha em atenção que as versões anteriores não estão disponíveis para transferência e foram preteridas. 
>
>**O que tenho de fazer?**
>
>1. Verifique se está a utilizar uma versão anterior à versão 2.3.3000.4 das Ferramentas do Azure Data Lake para o Visual Studio. 
>   
>   ![Verificar a versão da ferramenta](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-about-data-lake.png)
> 
>2. Se a sua versão for anterior à versão 2.3.3000.4, atualize as Ferramentas do Azure Data Lake para o Visual Studio, visitando o centro de transferências: 
>    - [Para Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools)
>    - [Para Visual Studio 2013 e 2015](https://www.microsoft.com/en-us/download/details.aspx?id=49504)


## <a name="prerequisites"></a>Pré-requisitos

* **Visual Studio**: são suportadas todas as edições exceto a Express.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* Versão 2.7.1 ou superior do **Microsoft Azure SDK para .NET**.  Instale-a através do [instalador de plataforma Web](http://www.microsoft.com/web/downloads/platform.aspx).
* A conta do **Data Lake Analytics**. Para criar uma conta, consulte [Introdução ao Azure Data Lake Analytics com o portal do Azure](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Instalar as Ferramentas do Azure Data Lake para Visual Studio

### <a name="install-azure-data-lake-tools-for-visual-studio-2017"></a>Instalar as Ferramentas do Azure Data Lake para Visual Studio 2017

As ferramentas do Azure Data Lake para Visual Studio são suportadas no Visual Studio 2017 15.3 ou superior. Esta ferramenta faz parte das cargas de trabalho de **Armazenamento e processamento de dados** e de **Desenvolvimento do Azure** no Instalador do Visual Studio. Ative uma destas duas cargas de trabalho como parte da sua instalação do Visual Studio.  

Ative a carga de trabalho de **Armazenamento e processamento de dados** da seguinte forma: ![Ativar a carga de trabalho de armazenamento e processamento de dados](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-01.png)

Ative a carga de trabalho de **Desenvolvimento do Azure** da seguinte forma: ![Ativar a carga de trabalho de desenvolvimento do Azure](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-02.png)

### <a name="install-azure-data-lake-tools-for-visual-studio-2013-and-2015"></a>Instalar as Ferramentas do Azure Data Lake para Visual Studio 2013 e 2015

Transfira e instale as Ferramentas do Azure Data Lake para Visual Studio [a partir do centro de transferências](http://aka.ms/adltoolsvs). Após a instalação, tenha em atenção que:
* O nó do **Server Explorer** > **Azure** contém um nó do **Data Lake Analytics**. 
* O menu **Ferramentas** inclui um item do **Data Lake**.

## <a name="connect-to-an-azure-data-lake-analytics-account"></a>Ligar-se a uma conta do Azure Data Lake Analytics

1. Abra o Visual Studio.
2. Abra o Server Explorer ao selecionar **Ver** > **Server Explorer**.
3. Clique com botão direito do rato em **Azure**. Em seguida, selecione **Ligar-se à subscrição do Microsoft Azure** e siga as instruções.
4. No Server Explorer, selecione **Azure** > **Data Lake Analytics**. Verá uma lista das suas contas do Data Lake Analytics.


## <a name="write-your-first-u-sql-script"></a>Escrever o seu primeiro script U-SQL

O texto apresentado em seguida é um script U-SQL simples. Este define um pequeno conjunto de dados e grava o conjunto de dados no Data Lake Store como um ficheiro com o nome `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

### <a name="submit-a-data-lake-analytics-job"></a>Submeter uma tarefa do Data Lake Analytics

1. Selecione **Ficheiro** > **Novo** > **Projeto**.

2. Selecione o tipo **Projeto U-SQL** e clique em **OK**. O Visual Studio cria uma solução com um ficheiro **Script.usql**.

3. Cole o script anterior na janela **Script.usql**.

4. No canto superior esquerdo da janela **Script.usql**, especifique a conta do Data Lake Analytics.

    ![Submeter projeto do Visual Studio em U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. No canto superior esquerdo da janela **Script.usql**, selecione **Submeter**.
6. Verifique a **Conta di Analytics** e, em seguida, selecione **Submeter**. Os resultados da submissão ficam disponíveis nas Ferramentas do Data Lake para Visual Studio quando a submissão estiver concluída.

    ![Submeter projeto do Visual Studio em U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. Para ver o estado da tarefa mais recente e atualizar o ecrã, clique em **Atualizar**. Quando a tarefa for concluída com êxito, será apresentado o **Gráfico da Tarefa**, as **Operações de Metadados**, o **Histórico do Estado** e o **Diagnóstico**:

    ![Gráfico de desempenho da tarefa do Data Lake Analytics no Visual Studio em U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * **Resumo da Tarefa** apresenta o resumo da tarefa.   
   * **Detalhes da Tarefa** apresenta informações mais específicas sobre a tarefa, incluindo o script, recursos e vértices.
   * **Gráfico da Tarefa** apresenta o progresso da tarefa.
   * **Operações de Metadados** apresenta todas as ações que foram executadas no catálogo de U-SQL.
   * **Dados** apresenta todas as entradas e saídas.
   * **Diagnóstico** oferece uma análise avançada para a otimização do desempenho e a execução da tarefa.

### <a name="to-check-job-state"></a>Para verificar o estado da tarefa

1. No Server Explorer, selecione **Azure** > **Data Lake Analytics**. 
2. Expanda o nome da conta do Data Lake Analytics.
3. Faça duplo clique em **Tarefas**.
4. Selecione a tarefa submetida anteriormente.

### <a name="to-see-the-output-of-a-job"></a>Para ver a saída de uma tarefa

1. No Server Explorer, navegue até à tarefa submetida.
2. Clique no separador **Dados**.
3. No separador **Saídas de Tarefas**, selecione o ficheiro `"/data.csv"`.

## <a name="next-steps"></a>Passos seguintes

* [Run U-SQL scripts on your own workstation for testing and debugging](data-lake-analytics-data-lake-tools-local-run.md) (Executar scripts U-SQL na sua própria área de trabalho para testes e depuração)
* [Depurar o código C# em tarefas U-SQL com Ferramentas do Azure Data Lake para o Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
* [Utilizar as Ferramentas do Azure Data Lake para o Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
