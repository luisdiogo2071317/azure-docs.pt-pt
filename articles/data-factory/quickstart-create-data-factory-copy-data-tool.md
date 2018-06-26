---
title: Copiar dados utilizando a ferramenta Copiar Dados do Azure | Microsoft Docs
description: Crie uma fábrica de dados do Azure e utilize a ferramenta Copiar Dados para copiar dados de uma localização no armazenamento de Blobs do Azure para outra localização.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: 23d4afffbe108d439dd53d95ed81f7d9fc4246ef
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284579"
---
# <a name="use-the-copy-data-tool-to-copy-data"></a>Utilizar a ferramenta Copiar Dados para copiar dados 
> [!div class="op_single_selector" title1="Select the version of Data Factory service that you are using:"]
> * [Versão 1 - GA](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versão 2 - Pré-visualização](quickstart-create-data-factory-copy-data-tool.md)

Neste início rápido, irá utilizar o portal do Azure para criar uma fábrica de dados. Utilize a ferramenta Copiar Dados para criar um pipeline que copia dados de uma pasta num armazenamento de Blobs do Azure para outra pasta. 

> [!NOTE]
> Se não estiver familiarizado com o Azure Data Factory, veja [Introdução ao Azure Data Factory](data-factory-introduction.md) antes de começar o início rápido. 
>
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço, que está em disponibilidade geral (GA), veja [Introdução à versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Selecione **Novo** no menu da esquerda, selecione **Dados + Análise** e, em seguida, selecione **Data Factory**. 
   
   ![Seleção do Data Factory no painel "Novo"](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory-menu.png)
2. Na página **Nova fábrica de dados**, introduza **ADFTutorialDataFactory** em **Nome**. 
      
   ![Página "Nova fábrica de dados"](./media/quickstart-create-data-factory-copy-data-tool/new-azure-data-factory.png)
 
   O nome do Azure Data Factory deve ser *globalmente exclusivo*. Se receber o seguinte erro, altere o nome da fábrica de dados (por exemplo, **&lt;oseunome&gt;ADFTutorialDataFactory**) e tente criá-la novamente. Para regras de nomenclatura de artefactos do Data Factory, veja o artigo [Data Factory – Regras de Nomenclatura](naming-rules.md).
  
   ![Erro quando um nome não está disponível](./media/quickstart-create-data-factory-portal/name-not-available-error.png)
3. Em **Subscrição**, selecione a sua subscrição do Azure na qual pretende criar a fábrica de dados. 
4. Em **Grupo de Recursos**, siga um destes passos:
     
   - Selecione **Utilizar existente** e selecione um grupo de recursos já existente na lista. 
   - Selecione **Criar novo** e introduza o nome de um grupo de recursos.   
         
   Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
4. Em **Versão**, selecione **V2 (Pré-visualização)**.
5. Em **Localização**, selecione a localização para a fábrica de dados. 

   A lista mostra apenas localizações suportadas. Os arquivos de dados (como o Armazenamento do Microsoft Azure e a Base de Dados SQL do Azure) e as computações (como o Azure HDInsight) utilizados pelo Data Factory podem estar noutras localizações/regiões.

6. Selecione **Afixar ao dashboard**.     
7. Selecione **Criar**.
8. No dashboard, é apresentado o mosaico seguinte, com o estado **A Implementar o Data Factory**: 

    ![Mosaico "A Implementar o Data Factory"](media/quickstart-create-data-factory-copy-data-tool/deploying-data-factory.png)
9. Após concluir a criação, verá a página **Data Factory**. Selecione o mosaico **Criar e Monitorizar** para iniciar a aplicação de interface de utilizador (IU) do Azure Data Factory num separador à parte.
   
   ![Home page da fábrica de dados, com o mosaico "Criar e Monitorizar"](./media/quickstart-create-data-factory-copy-data-tool/data-factory-home-page.png)

## <a name="start-the-copy-data-tool"></a>Iniciar a ferramenta Copiar Dados

1. Na página **Vamos começar**, selecione o mosaico **Copiar Dados** para iniciar a ferramenta Copiar Dados. 

   ![Mosaico "Copiar Dados"](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-tile.png)

2. Na página **Propriedades** da ferramenta Copiar Dados, pode especificar um nome para o pipeline e a respetiva descrição e, em seguida, selecionar **Seguinte**. 

   ![Página "Propriedades"](./media/quickstart-create-data-factory-copy-data-tool/copy-data-tool-properties-page.png)
3. Na página **Arquivo de dados de origem**, conclua os seguintes passos:

    a. Clique em **+ Criar nova ligação** para adicionar uma ligação.

    ![Página "Arquivo de dados de origem"](./media/quickstart-create-data-factory-copy-data-tool/new-source-linked-service.png)

    b. Selecione **Armazenamento de Blobs do Azure** na galeria e, em seguida, selecione **Seguinte**.

    ![Selecione o armazenamento de blobs na galeria](./media/quickstart-create-data-factory-copy-data-tool/select-blob-source.png)

    c. Na página **Especificar a conta de armazenamento de Blobs do Azure**, selecione a sua conta de armazenamento da lista **Nome da conta de armazenamento** e, em seguida, selecione **Seguinte**. 

   ![Configurar a conta de armazenamento de Blobs do Azure](./media/quickstart-create-data-factory-copy-data-tool/configure-blob-storage.png)

   d. Selecione o serviço ligado criado recentemente como origem e, em seguida, clique em **Seguinte**.

   ![Selecionar serviço ligado de origem](./media/quickstart-create-data-factory-copy-data-tool/select-source-linked-service.png)


4. Na página **Escolher o ficheiro ou pasta de entrada**, complete os seguintes passos:

   a. Clique em **Navegar** para navegar para a pasta **adftutorial/input**, selecione o ficheiro **emp.txt** e clique em **Escolher**. 

   ![Página "Escolher o ficheiro ou pasta de entrada"](./media/quickstart-create-data-factory-copy-data-tool/configure-source-path.png)

   d. Verifique a opção **Cópia binária** para copiar o ficheiro conforme está e, em seguida, selecione **Seguinte**. 

   ![Página "Escolher o ficheiro ou pasta de entrada"](./media/quickstart-create-data-factory-copy-data-tool/select-binary-copy.png)


5. Na página **Arquivo de dados de destino**, selecione o serviço ligado **Armazenamento de Blobs do Azure** que acabou de criar e, em seguida, selecione **Seguinte**. 

   ![Página "Arquivo de dados de destino"](./media/quickstart-create-data-factory-copy-data-tool/select-sink-linked-service.png)

6. Na página **Escolher ficheiro ou pasta de saída**, introduza **adftutorial/output**, no caminho da pasta e, em seguida, clique em **Seguinte**. 

   ![Página "Escolher ficheiro ou pasta de saída"](./media/quickstart-create-data-factory-copy-data-tool/configure-sink-path.png) 

7. Na página **Definições**, selecione **Seguinte** para utilizar as configurações predefinidas. 

8. Reveja todas as definições na página **Resumo** e selecione **Seguinte**. 

    ![Página "Resumo"](./media/quickstart-create-data-factory-copy-data-tool/summary-page.png)

9. Na página **Implementação concluída**, selecione **Monitorização** para monitorizar o pipeline que criou. 

    ![Página "Implementação concluída"](./media/quickstart-create-data-factory-copy-data-tool/deployment-page.png)

10. A aplicação muda para o separador **Monitorização**. Verá o estado do pipeline neste separador. Selecione **Atualizar** para atualizar a lista. 
    
    ![Monitorizar execução do pipeline](./media/quickstart-create-data-factory-copy-data-tool/pipeline-monitoring.png)

11. Selecione a ligação **Ver Execuções de Atividades** na coluna **Ações**. O pipeline apenas tem uma atividade do tipo **Cópia**. 

    ![Monitorizar execução de atividade](./media/quickstart-create-data-factory-copy-data-tool/activity-monitoring.png)
    
12. Para ver os detalhes da operação de cópia, selecione a ligação **Detalhes** (imagem de óculos), na coluna **Ações**. Para obter os detalhes das propriedades, veja [Copy Activity overview](copy-activity-overview.md) (Descrição geral da Atividade Copy).

    ![Detalhes da operação de cópia](./media/quickstart-create-data-factory-copy-data-tool/activity-execution-details.png)

13. Verifique se é criado um ficheiro **emp.txt** na pasta **saída** do contentor **adftutorial**. Se a pasta de saída não existir, o serviço Data Factory cria-a automaticamente. 

14. Mude para o separador **Criar** acima do **Monitor** no painel esquerdo, para que possa editar os serviços ligados, conjuntos de dados e pipelines. Para saber mais sobre a edição dos mesmos na IU do Data Factory, consulte [Criar uma fábrica de dados através do portal do Azure](quickstart-create-data-factory-portal.md).

## <a name="next-steps"></a>Passos seguintes
O pipeline neste exemplo copia dados de uma localização para outra localização num armazenamento de Blobs do Azure. Para saber como utilizar o Data Factory em mais cenários, aceda aos [tutoriais](tutorial-copy-data-portal.md). 