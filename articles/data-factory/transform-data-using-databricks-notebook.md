---
title: Executar um Databricks Notebook com a atividade do Databricks Notebook no Azure Data Factory
description: Saiba como pode utilizar a Atividade do Databricks Notebook num Azure Data Factory para executar um Databricks Notebook no cluster de tarefas do Databricks.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/12/2018
ms.author: abnarain
ms.reviewer: douglasl
ms.openlocfilehash: 78954e2dd00e425d2dfdd81d2c3e386f199f4f8f
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
ms.locfileid: "32311051"
---
# <a name="run-a-databricks-notebook-with-the-databricks-notebook-activity-in-azure-data-factory"></a>Executar um Databricks Notebook com a Atividade do Databricks Notebook no Azure Data Factory

Neste tutorial, utiliza o portal do Azure para criar um pipeline do Azure Data Factory que executa um Databricks Notebook no cluster de tarefas do Databricks. Também transmite os parâmetros do Azure Data Factory ao Databricks Notebook durante a execução.

Vai executar os seguintes passos neste tutorial:

  - Criar uma fábrica de dados.

  - Crie um pipeline que utilize a atividade do Databricks Notebook.

  - Acionar uma execução de pipeline.

  - Monitorizar a execução do pipeline.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

Para uma introdução e demonstração de onze minutos desta funcionalidade, veja o seguinte vídeo:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/ingest-prepare-and-transform-using-azure-databricks-and-data-factory/player]

## <a name="prerequisites"></a>Pré-requisitos

  - **Área de trabalho do Azure Databricks**. [Crie uma área de trabalho do Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) ou utilize uma já existente. Crie um Python Notebook na sua área de trabalho do Azure Databricks. Em seguida, execute o Notebook e transmita os parâmetros ao mesmo através do Azure Data Factory.

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1.  Abra o browser **Microsoft Edge** ou **Google Chrome**. Atualmente, a IU do Data Factory é suportada apenas nos browsers Microsoft Edge e Google Chrome.

2.  Selecione **Novo** no menu da esquerda, selecione **Dados + Análise** e, em seguida, selecione **Data Factory**.

    ![Criar uma nova fábrica de dados](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image1.png)

3.  No painel **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** em **Nome**.

    O nome do Azure Data Factory deve ser *globalmente exclusivo*. Se vir o seguinte erro, altere o nome da fábrica de dados. (Por exemplo, utilize **\<oseunome\>ADFTutorialDataFactory**). Para regras de nomenclatura de artefactos do Data Factory, veja o artigo [Data Factory – Regras de Nomenclatura](https://docs.microsoft.com/azure/data-factory/naming-rules).

    ![Indicar um nome para a nova fábrica de dados](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image2.png)

4.  Em **Subscrição**, selecione a sua subscrição do Azure na qual pretende criar a fábrica de dados.

5.  Em **Grupo de Recursos**, efetue um destes passos:
    
    - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente.
    
    - Selecione **Criar novo** e introduza o nome de um grupo de recursos.

    Alguns dos passos deste guia de início rápido pressupõem que utiliza o nome **ADFTutorialResourceGroup** para o grupo de recursos. Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1.  Em **Versão**, selecione **V2 (Pré-visualização)**.

2.  Em **Localização**, selecione a localização para a fábrica de dados.

    Atualmente, o Data Factory V2 só permite criar fábricas de dados nas regiões E.U.A. Leste, E.U.A. Leste 2 e Europa Ocidental. Os arquivos de dados (como o Armazenamento do Azure e a Base de Dados SQL do Azure) e as computações (como o HDInsight) utilizados pelo Data Factory podem estar noutras regiões.

3.  Selecione **Afixar ao dashboard**.

4.  Selecione **Criar**.

5.  No dashboard, é apresentado o mosaico seguinte, com o estado **A Implementar o Data Factory**:

    ![](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image3.png)

6.  Após concluir a criação, verá a página **Fábrica de dados**. Selecione o mosaico **Criar e Monitorizar** para iniciar a aplicação de IU do Azure Data Factory num separador à parte.

    ![Iniciar a aplicação da IU da fábrica de dados](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image4.png)

## <a name="create-linked-services"></a>Criar serviços ligados

Nesta secção, vai criar um serviço ligado do Databricks. Este serviço ligado contém as informações de ligação ao cluster do Databricks:

### <a name="create-an-azure-databricks-linked-service"></a>Criar um serviço ligado do Azure Databricks

1.  Na página **Introdução**, mude para o separador **Editar**, no painel esquerdo.

    ![Editar o novo serviço ligado](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image5.png)

2.  Selecione **Ligações**, na parte inferior da janela, e selecione **+ Novo**.
    
    ![Criar uma nova ligação](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image6.png)

3.  Na janela **Novo Serviço Ligado**, selecione **Arquivo de Dados** \> **Azure Databricks** e, em seguida, **Continuar**.
    
    ![Especificar um serviço ligado do Databricks](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image7.png)

4.  Na janela **Novo Serviço Ligado**, conclua os passos abaixo:
    
    1.  Para **Nome**, introduza ***AzureDatabricks\_LinkedService***
    
    2.  Para **Cluster**, selecione um **Novo Cluster**
    
    3.  Para **Domínio/Região**, selecione a região onde está localizada a sua área de trabalho do Azure Databricks.
    
    4.  Para **Tipo de nó de cluster**, selecione **Standard\_D3\_v2** para este tutorial.
    
    5.  Para **Token de Acesso**, gere-o a partir da área de trabalho do Azure Databricks. Pode encontrar os passos [aqui](https://docs.databricks.com/api/latest/authentication.html#generate-token).
    
    6.  Para **Versão do cluster**, selecione **4.0 Beta** (versão mais recente)
    
    7.  Para **Número de nós de trabalho**, introduza **2**.
    
    8.  Selecione **Concluir**

        ![Concluir a criação do serviço ligado](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image8.png)

## <a name="create-a-pipeline"></a>Criar um pipeline

1.  Selecione o botão **+** (mais) e, em seguida, selecione **Pipeline** no menu.

    ![Botões para criar um novo pipeline](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image9.png)

2.  Crie um **parâmetro** para ser utilizado no **Pipeline**. Mais tarde, transmita este parâmetro à Atividade do Databricks Notebook. No pipeline vazio, clique no separador **Parâmetros** e, em seguida, em **Novo** e dê-lhe um nome no formato "**nome**".

    ![Criar um novo parâmetro](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image10.png)

    ![Criar o parâmetro de nome](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image11.png)

3.  Na caixa de ferramentas **Atividades**, expanda **Databricks**. Arraste a atividade do **Notebook** da caixa de ferramentas **Atividades** para a superfície de desenho do pipeline.

    ![Arrastar o Notebook para a superfície de desenho](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image12.png)

4.  Nas propriedades da janela de atividade do **Databricks** **Notebook** na parte inferior, conclua os seguintes passos:

    a. Mudar para o separador **Definições**.

    b. Selecione **myAzureDatabricks\_LinkedService** (criado no procedimento anterior).

    c. Selecione um **caminho do Databricks Notebook**. Vamos criar um Notebook e especificar o caminho aqui. O Caminho do Notebook é obtido ao seguir os próximos passos.

       1. Iniciar a Área de trabalho do Azure Databricks

       2. Crie uma **Nova Pasta** na Área de Trabalho e denomine-a **adftutorial**.

          ![Criar uma nova pasta](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image13.png)

       3. [Crie um novo Notebook](https://docs.databricks.com/user-guide/notebooks/index.html#creating-a-notebook) (Python) e denomine-o **mynotebook** na **Pasta** adftutorial**e**clique em **Criar**.

          ![Criar um novo Notebook](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image14.png)

          ![Definir as propriedades do novo Notebook](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image15.png)

       4. No Notebook recém-criado denominado "mynotebook", adicione o seguinte código:

           ```
           # Creating widgets for leveraging parameters, and printing the parameters

           dbutils.widgets.text("input", "","")
           dbutils.widgets.get("input")
           y = getArgument("input")
           print "Param -\'input':"
           print y
           ```

           ![Criar widgets para parâmetros](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image16.png)

       5. Neste caso, o **Caminho do Notebook** é **/adftutorial/mynotebook**

5.  Volte à **ferramenta de criação da IU do Data Factory**. Navegue para o separador **Definições** em **Atividade do Notebook1**. 
    
    a.  **Adicione o parâmetro** à atividade do Notebook. Utiliza o mesmo parâmetro adicionado anteriormente ao **Pipeline**.

       ![Adicionar um parâmetro](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image17.png)

    b.  Dê o nome **input** ao parâmetro e forneça o valor como expressão **@pipeline().parameters.name**.

6.  Para validar o pipeline, selecione o botão **Validar** na barra de ferramentas. Para fechar a janela de validação, selecione o botão **\>\>** (seta para a direita).

    ![Validar o pipeline](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image18.png)

7.  Selecione **Publicar Tudo**. A IU do Data Factory publica as entidades (serviços ligados e pipeline) no serviço Azure Data Factory.

    ![Publicar as novas entidades da fábrica de dados](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image19.png)

## <a name="trigger-a-pipeline-run"></a>Acionar uma execução de pipeline

Selecione **Acionar** na barra de ferramentas e, em seguida, selecione **Acionar Agora**.

![Selecionar o comando Acionar Agora](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image20.png)

A caixa de diálogo **Execução do Pipeline** pede-lhe o parâmetro **name**. Utilize **/path/filename** como parâmetro aqui. Clique em **Concluir**.

![Fornecer um valor para os parâmetros de nome](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image21.png)

## <a name="monitor-the-pipeline-run"></a>Monitorizar a execução do pipeline.

1.  Mude para o separador **Monitorizar**. Confirme que vê uma execução de pipeline. Demora aproximadamente 5 a 8 minutos a criar um cluster de tarefas do Databricks, onde o Notebook é executado.

    ![Monitorizar o pipeline](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image22.png)

2.  Selecione **Atualizar** periodicamente para verificar o estado da execução do pipeline.

3.  Para ver as execuções de atividades associadas à execução do pipeline, selecione **Ver Execuções de Atividades**, na coluna **Ações**.

    ![Ver as execuções de atividades](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image23.png)

Pode selecionar a ligação **Pipelines**, na parte superior, para regressar à vista de execuções do pipeline.

## <a name="verify-the-output"></a>Verificar a saída

Pode iniciar sessão na **Área de trabalho do Azure Databricks**, aceder a **Clusters** e ver o estado da **Tarefa** como *execução pendente, em execução ou terminada*.

![Ver o cluster de tarefas e a tarefa](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image24.png)

Pode clicar no **Nome da tarefa** e navegar para ver mais detalhes. Numa execução bem-sucedida, pode validar os parâmetros transmitidos e o resultado do Python Notebook.

![Ver os detalhes de execução e o resultado](media/transform-data-using-databricks-notebook/databricks-notebook-activity-image25.png)

## <a name="next-steps"></a>Passos seguintes

O pipeline neste exemplo aciona uma atividade do Databricks Notebook e transmite um parâmetro à mesma. Aprendeu a:

  - Criar uma fábrica de dados.

  - Crie um pipeline que utilize uma atividade do Databricks Notebook.

  - Acionar uma execução de pipeline.

  - Monitorizar a execução do pipeline.
