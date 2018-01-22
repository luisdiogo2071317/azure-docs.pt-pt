---
title: Copiar dados utilizando a ferramenta Copiar Dados do Azure | Microsoft Docs
description: "Crie uma fábrica de dados do Azure e utilize a ferramenta Copiar Dados para copiar dados de uma pasta num armazenamento de blobs do Azure para outra pasta."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/16/2018
ms.author: jingwang
ms.openlocfilehash: c17e738e3db18503f7cdda24a5f01ceb78e4e6a3
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="use-copy-data-tool-to-copy-data"></a>Utilizar a ferramenta Copiar Dados para copiar dados 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versão 2 - Pré-visualização](quickstart-create-data-factory-copy-data-tool.md)

Neste início rápido, irá utilizar o portal do Azure para criar uma fábrica de dados. Utilize a ferramenta Copiar Dados para criar um pipeline que copia dados de uma pasta num armazenamento de blobs do Azure para outra pasta. 

> [!NOTE]
> Se não estiver familiarizado com o Azure Data Factory, veja [Introdução ao Azure Data Factory](data-factory-introduction.md) antes de começar o início rápido. 
>
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Clique em **Novo** no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** no **nome**. 
      
     ![Página Nova fábrica de dados](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser **globalmente exclusivo**. Se vir o erro seguinte no campo de nome, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory) e tente a criação novamente. Veja o artigo [Data Factory – Naming Rules](naming-rules.md) (Data Factory – Regras de Nomenclatura) para obter as regras de nomenclatura dos artefactos do Data Factory.
  
     ![Nome não disponível - erro](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
4. No **Grupo de Recursos**, siga um destes passos:
     
      - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista pendente. 
      - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
      Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Selecione **V2 (Pré-visualização)** para a **versão**.
5. Selecione a **localização** da fábrica de dados. Apenas são apresentadas as localizações suportadas na lista pendente. Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras localizações/regiões.
6. Selecione **Afixar ao dashboard**.     
7. Clique em **Criar**.
8. No painel, vai ver o mosaico seguinte com o estado **A implementar fábrica de dados**. 

    ![Mosaico “implementar a fábrica de dados”](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Depois de concluída a criação, vai ver a página **Data Factory**, conforme mostrado na imagem.
   
   ![Home page da fábrica de dados](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)
10. Clique no mosaico **Criar e Monitorizar** para iniciar a interface de utilizador (IU) do Azure Data Factory num separador à parte. 

## <a name="launch-copy-data-tool"></a>Iniciar a ferramenta Copiar Dados

1. Na página de introdução, clique no mosaico **Copiar Dados** para iniciar a ferramenta Copiar Dados. 

   ![Mosaico ferramenta Copiar Dados](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)
2. Na página **Propriedades** da ferramenta Copiar os Dados, clique em **Seguinte**. Pode especificar um nome para o pipeline e a respetiva descrição nesta página. 

    ![Ferramenta Copiar Dados – Página propriedades](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. Na página **Arquivo de dados de origem**, selecione **Armazenamento de Blobs do Azure** e clique em **Seguinte**.

    ![Página de arquivo de dados de origem](./media/quickstart-create-data-factory-copy-data-tool/source-data-store-page.png)
4. Na página **Especificar a conta de armazenamento de Blobs do Azure**, selecione o **nome da conta de armazenamento** da lista pendente lista e clique em Seguinte. 

    ![Especifique a conta de armazenamento de blobs](./media/quickstart-create-data-factory-copy-data-tool/specify-blob-storage-account.png)
5. Na página **Escolher o ficheiro ou pasta de entrada**, execute os seguintes passos:

    1. Navegue até à pasta **adftutorial/input**. 
    2. Selecione o ficheiro **emp.txt**.
    3. Clique em **Escolher**. Pode fazer duplo clique em **emp.txt** para ignorar este passo. 
    4. Clique em **Seguinte**. 

    ![Escolher ficheiro ou pasta de entrada](./media/quickstart-create-data-factory-copy-data-tool/choose-input-file-folder.png)
6. Na página **Definições do formato do ficheiro**, tenha em atenção que a ferramenta deteta automaticamente os delimitadores de linha e coluna e clique em **Seguinte**. Também pode pré-visualizar os dados e ver o esquema dos dados de entrada nesta página. 

    ![Página de definições do formato do ficheiro](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-page.png)
7. Na página **Arquivo de dados de destino**, selecione **Armazenamento de Blobs do Azure** e clique em **Seguinte**. 

    ![Página arquivo de dados de destino](./media/quickstart-create-data-factory-copy-data-tool/destination-data-store-page.png)    
8. Na página **Especificar a conta de armazenamento de Blobs do Azure**, selecione a conta de armazenamento do Blob do Azure e clique em **Seguinte**. 

    ![Especifique a página de arquivo de dados dependente](./media/quickstart-create-data-factory-copy-data-tool/specify-sink-blob-storage-account.png)
9. Na página **Escolher o ficheiro ou pasta de saída**, execute os seguintes passos: 

    1. Introduza **adftutorial/output** no **caminho da pasta**.
    2. Introduza **emp.txt** para o **nome de ficheiro**. 
    3. Clique em **Seguinte**. 

    ![Escolher ficheiro ou pasta de saída](./media/quickstart-create-data-factory-copy-data-tool/choose-output-file-folder.png) 
10. Na página **Definições do formato do ficheiro**, clique em **Seguinte**. 

    ![Página de definições do formato do ficheiro](./media/quickstart-create-data-factory-copy-data-tool/file-format-settings-output-page.png)
11. Clique em **Seguinte** na página **Definições**. 

    ![Página definições avançadas](./media/quickstart-create-data-factory-copy-data-tool/advanced-settings-page.png)
12. Reveja todas as definições na página **Resumo** e clique em Seguinte. 

    ![Página de resumo](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)
13. Na página **Implementação concluída**, clique em **Monitorização** para monitorizar o pipeline que criou. 

    ![Página de implementação](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)
14. A aplicação muda para o separador **Monitorização**. Verá o estado do pipeline neta página. Clique em **Atualizar** para atualizar a lista. 
    
    ![Página monitorizar execuções de pipeline](./media/quickstart-create-data-factory-copy-data-tool/monitor-pipeline-runs-page.png)
15. Clique na ligação **Ver Execuções de Atividades**, na coluna Ações. O pipeline apenas tem uma atividade do tipo **Cópia**. 

    ![Página Execuções de Atividade](./media/quickstart-create-data-factory-copy-data-tool/activity-runs.png)
16. Para ver os detalhes da operação de cópia, clique na ligação **Detalhes** (imagem de óculos), na coluna **Ações**. Para obter os detalhes das propriedades, veja [Copy Activity overview](copy-activity-overview.md) (Descrição geral da Atividade Copy). 

    ![Detalhes da operação de cópia](./media/quickstart-create-data-factory-copy-data-tool/copy-operation-details.png)
17. Verifique se é criado um ficheiro **emp.txt** na pasta **saída** do contentor **adftutorial**. Se a pasta de saída não existir, o serviço Data Factory cria-a automaticamente. 
18. Mudar para o separador **Editar** para conseguir editar serviços ligados, conjuntos de dados e pipelines. Para saber mais sobre a edição dos mesmos na IU do Data Factory, veja [Criar uma fábrica de dados através do portal do Azure](quickstart-create-data-factory-portal.md).

    ![Separador Editar](./media/quickstart-create-data-factory-copy-data-tool/edit-tab.png)

## <a name="next-steps"></a>Passos seguintes
O pipeline neste exemplo copia dados de uma localização para outra localização num armazenamento de blobs do Azure. Leia os [tutoriais](tutorial-copy-data-portal.md) para saber como utilizar o Data Factory em mais cenários. 