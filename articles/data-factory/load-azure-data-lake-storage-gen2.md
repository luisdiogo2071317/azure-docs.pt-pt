---
title: Carregar dados para o Azure Data Lake armazenamento Gen2 (pré-visualização) com o Azure Data Factory
description: Utilizar o Azure Data Factory para copiar dados para Gen2 de armazenamento do Azure Data Lake (pré-visualização)
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: 961c8dea4dbb6b6600d10b75e84a9a84c34c329b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036663"
---
# <a name="load-data-into-azure-data-lake-storage-gen2-preview-with-azure-data-factory"></a>Carregar dados para o Azure Data Lake armazenamento Gen2 pré-visualização do Azure Data Factory

[Azure Data Lake armazenamento Gen2 Preview](../storage/data-lake-storage/introduction.md) adiciona um protocolo com funcionalidades de segurança e de espaço de nomes do sistema de ficheiros hierárquico para tornar mais fácil ligar as estruturas de análise a uma camada de armazenamento durável de Blob Storage do Azure. No Data Lake armazenamento Gen2 (pré-visualização), todas as qualities do armazenamento de objeto de permanecerem ao adicionar as vantagens de uma interface de sistema de ficheiros.

O Azure Data Factory é um serviço de integração de dados baseado em nuvem completamente gerido. Pode utilizar o serviço para preencher o lake com dados a partir de um conjunto avançado de no local e baseado na nuvem dados armazenam e poupam tempo ao criar as suas soluções de análise. Para uma lista detalhada dos conectores suportados, consulte a tabela de [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).

Utilitário de transferência de ofertas de fábrica de dados do Azure Escalamento horizontal, solução de movimento de dados geridos por oposição AzCopy, dados de linha de comandos. Devido à arquitetura de escalamento horizontal do ADF,-lo pode ingerir dados num débito elevado. Para obter mais informações, consulte [copiar o desempenho de atividade](copy-activity-performance.md).

Este artigo mostra como utilizar a ferramenta de dados de cópia do Data Factory para carregar dados a partir de _serviço Amazon Web Services S3_ para _Gen2 de armazenamento do Azure Data Lake_. Pode seguir passos semelhantes para copiar dados de outros tipos de arquivos de dados.

## <a name="prerequisites"></a>Pré-requisitos

* Subscrição do Azure: Se não tiver uma subscrição do Azure, crie um [conta gratuita](https://azure.microsoft.com/free/) antes de começar.
* Conta de armazenamento do Azure com o Data Lake armazenamento Gen2 ativada: Se não tiver uma conta de armazenamento, clique em [aqui](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) para criar um.
* Conta AWS com um registo de S3 que contém os dados: Este artigo mostra como copiar dados do Amazon S3. Pode utilizar outros arquivos de dados ao seguir passos semelhantes.

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. No menu da esquerda, selecione **novo** > **dados + análise** > **Data Factory**:
   
   ![Criar uma nova fábrica de dados](./media/load-azure-data-lake-storage-gen2/new-azure-data-factory-menu.png)
2. No **nova fábrica de dados** página, forneça valores para os campos que são apresentados na imagem seguinte: 
      
   ![Página Nova fábrica de dados](./media/load-azure-data-lake-storage-gen2//new-azure-data-factory.png)
 
    * **Nome**: introduza um nome globalmente exclusivo para a fábrica de dados do Azure. Se receber o erro "o nome de factory de dados \"LoadADLSDemo\" não está disponível," Introduza um nome diferente para a fábrica de dados. Por exemplo, pode utilizar o nome  _**yourname**_**ADFTutorialDataFactory**. Tente criar a fábrica de dados novamente. Para ter acesso às regras de nomenclatura para artefactos do Data Factory, veja [Regras de nomenclatura do Data Factory](naming-rules.md).
    * **Subscrição**: selecione a sua subscrição do Azure na qual pretende criar a fábrica de dados. 
    * **Grupo de recursos**: selecione um grupo de recursos existente na lista pendente, ou selecione o **criar nova** opção e introduza o nome de um grupo de recursos. Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Versão**: selecione **V2(Preview)**.
    * **Localização**: selecione a localização da fábrica de dados. Apenas são apresentadas as localizações suportadas na lista pendente. Arquivos de dados que são utilizados pela fábrica de dados podem estar nas outras regiões e localizações. 

3. Selecione **Criar**.
4. Depois de concluída a criação, vá para a fábrica de dados. Pode ver o **Data Factory** home page do conforme mostrado na imagem seguinte: 
   
   ![Home page da fábrica de dados](./media/load-azure-data-lake-storage-gen2/data-factory-home-page.png)

   Selecione o **autor & Monitor** mosaico para iniciar a aplicação de integração de dados num separador separado.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Carregar dados para Gen2 de armazenamento do Azure Data Lake

1. No **começar** página, selecione o **copiar dados** mosaico para iniciar a ferramenta de cópia de dados: 

   ![Mosaico ferramenta Copiar Dados](./media/load-azure-data-lake-storage-gen2/copy-data-tool-tile.png)
2. No **propriedades** página, especifique **CopyFromAmazonS3ToADLS** para o **nome da tarefa** campo e selecione **seguinte**:

    ![Página Propriedades](./media/load-azure-data-lake-storage-gen2/copy-data-tool-properties-page.png)
3. No **arquivo de dados de origem** página, clique em **+ criar nova ligação**:

    ![Página de arquivo de dados de origem](./media/load-azure-data-lake-storage-gen2/source-data-store-page.png)
    
    Selecione **Amazon S3** na galeria do conector e seleccione **continuar**
    
    ![Página de s3 de arquivo de dados de origem](./media/load-azure-data-lake-storage-gen2/source-data-store-page-s3.png)
    
4. No **especificar Amazon S3 ligação** página, efetue os seguintes passos:
   1. Especifique o **ID da chave de acesso** valor.
   2. Especifique o **chave de acesso secreta** valor.
   3. Clique em **Testar ligação** para validar as definições, em seguida, selecione **concluir**.
   
   ![Especifique a conta Amazon S3](./media/load-azure-data-lake-storage-gen2/specify-amazon-s3-account.png)
   
   4. Verá que é criada uma nova ligação. Selecione **Seguinte**.
   
5. No **escolher o ficheiro de entrada ou a pasta** página, navegue para a pasta e ficheiro que pretende copiar a ativação pós-falha. Selecione o pasta/ficheiro, selecione **escolha**:

    ![Escolher ficheiro ou pasta de entrada](./media/load-azure-data-lake-storage-gen2/choose-input-folder.png)

6. Especificar o comportamento de cópia ao verificar o **copiar os ficheiros em modo recursivo** e **cópia binária** opções. Selecione **seguinte**:

    ![Especifique a pasta de saída](./media/load-azure-data-lake-storage-gen2/specify-binary-copy.png)
    
7. No **arquivo de dados de destino** página, clique em **+ criar nova ligação**e, em seguida, selecione **Gen2 de armazenamento do Azure Data Lake (pré-visualização)** e selecione **continuar** :

    ![Página arquivo de dados de destino](./media/load-azure-data-lake-storage-gen2/destination-data-storage-page.png)

8. No **ligação especificar Azure armazenamento do Data Lake** página, efetue os seguintes passos:

   1. Selecione o seu Gen2 armazenamento do Data Lake com capacidade de conta do "nome de conta de armazenamento" na lista pendente.
   2. Selecione **Seguinte**.
   
   ![Especifique a conta do Azure Data Lake Store](./media/load-azure-data-lake-storage-gen2/specify-adls.png)

9. No **escolha a pasta ou ficheiro de saída** página, introduza **copyfroms3** como o nome da pasta de saída e selecione **seguinte**: 

    ![Especifique a pasta de saída](./media/load-azure-data-lake-storage-gen2/specify-adls-path.png)

10. No **definições** página, selecione **seguinte** para utilizar as predefinições:

    ![Página de definições](./media/load-azure-data-lake-storage-gen2/copy-settings.png)
11. No **resumo** página, reveja as definições e selecione **seguinte**:

    ![Página de resumo](./media/load-azure-data-lake-storage-gen2/copy-summary.png)
12. No **página implementação**, selecione **Monitor** para monitorizar o pipeline:

    ![Página de implementação](./media/load-azure-data-lake-storage-gen2/deployment-page.png)
13. Tenha em atenção que o separador **Monitorização** à esquerda é selecionado automaticamente. O **ações** coluna inclui ligações para ver os detalhes de execução da atividade e volte a executar o pipeline:

    ![Monitorizar execuções de pipeline](./media/load-azure-data-lake-storage-gen2/monitor-pipeline-runs.png)

14. Para ver as execuções de atividade que estão associadas com o pipeline de execução, selecione o **ver a atividade é executada** ligação no **ações** coluna. Há apenas uma atividade (atividade copiar) no pipeline, pelo que só vai ver uma entrada. Para mudar para o pipeline é executado vista, selecione o **Pipelines** ligação na parte superior. Selecione **Atualizar** para atualizar a lista. 

    ![Monitorização de execuções de atividade](./media/load-azure-data-lake-storage-gen2/monitor-activity-runs.png)

15. Para monitorizar os detalhes de execução para cada atividade de cópia, selecione o **detalhes** ligação (imagem eyeglasses) em **ações** na atividade de vista de monitorização. Pode monitorizar detalhes como o volume de dados copiado da origem para o sink, débito de dados, os passos de execução com duração correspondente e utilizado configurações:

    ![Detalhes da execução de atividade de monitorização](./media/load-azure-data-lake-storage-gen2/monitor-activity-run-details.png)

16. Certifique-se de que os dados são copiados para a sua conta do Data Lake armazenamento Gen2.

## <a name="best-practice"></a>Melhores práticas

Quando copiar grande volume de dados do arquivo de dados baseada em ficheiros, são sugeridos para:

- Os ficheiros de partição para 10TB para 20TB fileset.
- Não accionam demasiadas execuções de cópia em simultâneo para evitar a limitação de arquivos de dados de origem ou sink. Pode começar com uma cópia executar e monitorizar o débito e gradualmente adicionar mais conforme necessário.

## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral da atividade de cópia](copy-activity-overview.md)
* [Conector de Gen2 de armazenamento do Data Lake do Azure](connector-azure-data-lake-storage.md)