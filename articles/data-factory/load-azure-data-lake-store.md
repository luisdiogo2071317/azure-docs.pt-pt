---
title: Carregar dados para o Azure Data Lake Store utilizando o Azure Data Factory | Microsoft Docs
description: Utilizar o Azure Data Factory para copiar dados para o Azure Data Lake Store
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 4446f83563293d0834f241dcca382ccf6ea99403
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="load-data-into-azure-data-lake-store-by-using-azure-data-factory"></a>Carregar dados para o Azure Data Lake Store utilizando o Azure Data Factory

[O Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) é um repositório de hiper escala de nível da empresa para cargas de trabalho de análise de macrodados. Azure Data Lake permite-lhe capturar dados de qualquer tamanho, tipo e velocidade de ingestão. Os dados são capturados num único local para análises exploratórias e operacionais.

O Azure Data Factory é um serviço de integração de dados baseado em nuvem completamente gerido. Pode utilizar o serviço para preencher o lake com dados do sistema existente e poupar tempo ao criar as suas soluções de análise.

O Azure Data Factory oferece as seguintes vantagens para carregar dados para o Azure Data Lake Store:

* **Fácil de configurar**: um Assistente de passo 5 intuitivo com nenhuma scripting necessário.
* **Suporte de arquivo de dados avançados**: suporte incorporado para um conjunto avançado de arquivos de dados baseado na nuvem e no local. Para obter uma lista detalhada, consulte a tabela de [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
* **Segura e compatível**: os dados são transferidos através de HTTPS ou ExpressRoute. A presença de serviço global garante que os dados nunca sai do limite geográfico.
* **Elevado desempenho**: até os dados de 1-GB/s carregamento velocidade para o Azure Data Lake Store. Para obter mais informações, consulte [copiar o desempenho de atividade](copy-activity-performance.md).

Este artigo mostra-lhe como utilizar a ferramenta de dados de cópia de fábrica de dados para _carregar dados do Amazon S3 para o Azure Data Lake Store_. Pode seguir passos semelhantes para copiar dados de outros tipos de arquivos de dados.

> [!NOTE]
> Para obter mais informações, consulte [copiar dados de ou para o Azure Data Lake Store utilizando o Azure Data Factory](connector-azure-data-lake-store.md).
>
> Este artigo aplica-se à versão 2 do Azure Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte o artigo [atividade de cópia no Azure Data Factory versão 1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Pré-requisitos

* Subscrição do Azure: Se não tiver uma subscrição do Azure, crie um [conta gratuita](https://azure.microsoft.com/free/) antes de começar.
* O Azure Data Lake Store: Se não tiver uma conta de Data Lake Store, consulte as instruções no [criar uma conta de Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account).
* Amazon S3: Este artigo mostra como copiar dados do Amazon S3. Pode utilizar outros arquivos de dados ao seguir passos semelhantes.

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. No menu da esquerda, selecione **novo** > **dados + análise** > **Data Factory**:
   
   ![Criar uma nova fábrica de dados](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. No **nova fábrica de dados** página, forneça valores para os campos que são apresentados na imagem seguinte: 
      
   ![Página Nova fábrica de dados](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
    * **Nome**: introduza um nome globalmente exclusivo para a fábrica de dados do Azure. Se receber o erro "o nome de factory de dados \"LoadADLSDemo\" não está disponível," Introduza um nome diferente para a fábrica de dados. Por exemplo, pode utilizar o nome  _**yourname**_**ADFTutorialDataFactory**. Tente criar a fábrica de dados novamente. Para as regras de nomenclatura artefactos do Data Factory de dados, consulte [regras de nomenclatura do Data Factory](naming-rules.md).
    * **Subscrição**: selecione a sua subscrição do Azure na qual pretende criar a fábrica de dados. 
    * **Grupo de recursos**: selecione um grupo de recursos existente na lista pendente, ou selecione o **criar nova** opção e introduza o nome de um grupo de recursos. Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Versão**: selecione **V2 (pré-visualização)**.
    * **Localização**: selecione a localização da fábrica de dados. Apenas são apresentadas as localizações suportadas na lista pendente. Arquivos de dados que são utilizados pela fábrica de dados podem estar nas outras regiões e localizações. Estes arquivos de dados incluem o Azure Data Lake Store, o Storage do Azure, SQL Database do Azure e assim sucessivamente.

3. Selecione **Criar**.
4. Depois de concluída a criação, vá para a fábrica de dados. Pode ver o **Data Factory** home page do conforme mostrado na imagem seguinte: 
   
   ![Home page da fábrica de dados](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

   Selecione o **autor & Monitor** mosaico para iniciar a aplicação de integração de dados num separador separado.

## <a name="load-data-into-azure-data-lake-store"></a>Carregar dados para o Azure Data Lake Store

1. No **começar** página, selecione o **copiar dados** mosaico para iniciar a ferramenta de cópia de dados: 

   ![Mosaico ferramenta Copiar Dados](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. No **propriedades** página, especifique **CopyFromAmazonS3ToADLS** para o **nome da tarefa** campo e selecione **seguinte**:

    ![Página Propriedades](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. No **arquivo de dados de origem** página, selecione **Amazon S3**e selecione **seguinte**:

    ![Página de arquivo de dados de origem](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. No **especificar Amazon S3 ligação** página, efetue os seguintes passos: 
   1. Especifique o **ID da chave de acesso** valor.
   2. Especifique o **chave de acesso secreta** valor.
   3. Selecione **Seguinte**.
   
   ![Especifique a conta Amazon S3](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. No **escolher o ficheiro de entrada ou a pasta** página, navegue para a pasta e ficheiro que pretende copiar a ativação pós-falha. Selecione o pasta/ficheiro, selecione **escolha**e, em seguida, selecione **seguinte**:

    ![Escolher ficheiro ou pasta de entrada](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. No **arquivo de dados de destino** página, selecione **Azure Data Lake Store**e selecione **seguinte**:

    ![Página arquivo de dados de destino](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. Escolher o comportamento de copiar selecionando o **copiar os ficheiros em modo recursivo** e **cópia binária** (copiar ficheiros como-é) opções. Selecione **seguinte**:

    ![Especifique a pasta de saída](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. No **ligação especifique Data Lake Store** página, efetue os seguintes passos: 

   1. Selecione o Data Lake Store para o **nome da conta do Data Lake Store**.
   2. Especifique as informações de principal de serviço: **inquilino**, **ID de principal de serviço**, e **chave principal do serviço**.
   3. Selecione **Seguinte**.
   
   > [!IMPORTANT]
   > Esta explicação passo a passo, utiliza um _principal de serviço_ para autenticar o Data Lake Store. Certifique-se de conceder o principal de serviço as permissões adequadas no Azure Data Lake Store, seguindo [estas instruções](connector-azure-data-lake-store.md#using-service-principal-authentication).
   
   ![Especifique a conta do Azure Data Lake Store](./media/load-data-into-azure-data-lake-store/specify-adls.png)
9. No **escolha a pasta ou ficheiro de saída** página, introduza **copyfroms3** como o nome da pasta de saída e selecione **seguinte**: 

    ![Especifique a pasta de saída](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)

10. No **definições** página, selecione **seguinte**:

    ![Página de definições](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. No **resumo** página, reveja as definições e selecione **seguinte**:

    ![Página de resumo](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. No **página implementação**, selecione **Monitor** para monitorizar o pipeline (tarefas):

    ![Página de implementação](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Tenha em atenção que o separador **Monitorização** à esquerda é selecionado automaticamente. O **ações** coluna inclui ligações para ver os detalhes de execução da atividade e volte a executar o pipeline:

    ![Monitorizar execuções de pipeline](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Para ver as execuções de atividade que estão associadas com o pipeline de execução, selecione o **ver a atividade é executada** ligação no **ações** coluna. Não há apenas uma atividade (atividade de cópia) no pipeline, para ver apenas uma entrada. Para mudar para o pipeline é executado vista, selecione o **Pipelines** ligação na parte superior. Selecione **Atualizar** para atualizar a lista. 

    ![Monitorização de execuções de atividade](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Para monitorizar os detalhes de execução para cada atividade de cópia, selecione o **detalhes** ligação em **ações** na atividade de vista de monitorização. Pode monitorizar detalhes como o volume de dados copiado da origem para o sink, débito de dados, os passos de execução com duração correspondente e utilizado configurações:

    ![Detalhes da execução de atividade de monitorização](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Certifique-se de que os dados são copiados para o Azure Data Lake Store: 

    ![Certifique-se a saída do Data Lake Store](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Passos Seguintes

Avançar para o artigo seguinte para saber mais sobre o suporte do Azure Data Lake Store: 

> [!div class="nextstepaction"]
>[Conector do Azure Data Lake Store](connector-azure-data-lake-store.md)