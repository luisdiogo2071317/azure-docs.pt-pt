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
ms.openlocfilehash: 3f73cd65b0ceb3148ce8ceb83d7b4e1be1280077
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-data-lake-store-using-azure-data-factory"></a>Carregar dados para o Azure Data Lake Store utilizando o Azure Data Factory

[O Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) é um repositório de hiper escala de nível da empresa para cargas de trabalho de análise de macrodados. O Azure Data Lake permite-lhe capturar dados de qualquer tamanho, tipo e velocidade de ingestão num único local para análises exploratórias e operacionais.

O Azure Data Factory é um serviço de integração de dados baseado em nuvem completamente gerido, que pode ser utilizado para preencher o lake com os dados do sistema existente e guardar a importantes tempo ao criar as suas soluções de análise. Seguem-se as principais vantagens do carregamento de dados para o Azure Data Lake Store utilizando o Azure Data Factory:

* **Fácil de configurar**: Assistente intuitivo do passo 5 com nenhuma scripting necessário.
* **Suporte de arquivo de dados avançados**: suporte incorporado para um conjunto avançado de no local e arquivos de dados baseado na nuvem, consulte uma lista detalhada no [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.
* **Segura e compatível**: os dados são transferidos através de HTTPS ou ExpressRoute e presença global serviço assegura que os dados nunca sai do limite geográfico
* **Elevado desempenho**: até os dados de 1 Gbps a carregar de velocidade para o Azure Data Lake Store. Saiba os detalhes da [copiar o desempenho de atividade](copy-activity-performance.md).

Este artigo mostra como utilizar a ferramenta de dados de cópia de fábrica de dados para **carregar dados do Amazon S3 para o Azure Data Lake Store**. Pode seguir os passos semelhantes para copiar dados de outros tipos de arquivos de dados.

> [!NOTE]
>  Para obter informações gerais sobre as capacidades de fábrica de dados ao copiar dados da Azure Data Lake Store, consulte [copiar dados de ou para o Azure Data Lake Store utilizando o Azure Data Factory](connector-azure-data-lake-store.md) artigo.
>
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [atividade de cópia no V1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Pré-requisitos

* **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.
* **O Azure Data Lake Store**. Se não tiver uma conta de Data Lake Store, consulte o [criar uma conta de Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account) artigo para obter os passos criar um.
* **Amazon S3**. Este artigo mostra como copiar dados do Amazon S3. Pode utilizar outros arquivos de dados ao seguir passos semelhantes.

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados

1. Clique em **NOVO**, no menu da esquerda, clique em **Dados + Análise** e, em seguida, em **Data Factory**. 
   
   ![Novo -> DataFactory](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. No **nova fábrica de dados** , indique os valores, conforme mostrado na captura de ecrã seguinte: 
      
     ![Nova página da fábrica de dados](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
   * **Name.** Introduza um nome exclusivo global para a fábrica de dados. Se receber o seguinte erro, altere o nome da fábrica de dados (por exemplo, oseunomeADFTutorialDataFactory) e tente criá-la novamente. Consulte [fábrica de dados - as regras de nomenclatura](naming-rules.md) artigo para regras de nomenclatura artefactos do Data Factory de dados.
  
            `Data factory name "LoadADLSDemo" is not available`

    * **Subscrição.** Selecione a sua subscrição do **Azure** na qual pretende criar a fábrica de dados. 
    * **Grupo de recursos.** Selecione um grupo de recursos existente na lista pendente, ou selecione **criar nova** opção e introduza o nome de um grupo de recursos. Para saber mais sobre os grupos de recursos, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Versão.** Selecione **V2 (pré-visualização)**.
    * **Localização.** Selecione a localização da fábrica de dados. Apenas as localizações suportadas são apresentadas na lista pendente. Arquivos de dados (Azure Data Lake Store, armazenamento do Azure, SQL Database do Azure, etc.) utilizados pela fábrica de dados podem ser noutras localizações/regiões.

3. Clique em **Criar**.
4. Após a criação for concluída, aceda à fábrica de dados e ver o **Data Factory** página conforme mostrado na imagem. Clique em **autor & Monitor** mosaico para iniciar a aplicação de integração de dados num separador separado. 
   
   ![Home page da fábrica de dados](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

## <a name="load-data-into-azure-data-lake-store"></a>Carregar dados para o Azure Data Lake Store

1. Na página de introdução de get, clique em **copiar dados** mosaico para iniciar a ferramenta de dados de cópia. 

   ![Mosaico de ferramenta de dados de cópia](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. No **propriedades** página da ferramenta de copiar os dados, especifique **CopyFromAmazonS3ToADLS** para o **nome da tarefa**e clique em **seguinte**. 

    ![Copie a página de propriedades da ferramenta de dados](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. No **arquivo de dados de origem** página, selecione **Amazon S3**e clique em **seguinte**.

    ![Página de arquivo de dados de origem](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. No **especificar Amazon S3 ligação** página, efetue os seguintes passos: 
    1. Especifique o **aceder ao ID de chave**.
    2. Especifique o **chave de acesso secreta**.
    3. Clique em **Seguinte**. 

        ![Especifique a conta Amazon S3](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. No **escolher o ficheiro de entrada ou a pasta** página, navegue para o ficheiro/pasta que pretende copiar através de, selecione e clique em **escolha**, em seguida, clique em **seguinte**. 

    ![Escolha a pasta ou ficheiro de entrada](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. No **arquivo de dados de destino** página, selecione **Azure Data Lake Store**e clique em **seguinte**. 

    ![Página de arquivo de dados de destino](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. Nesta página, escolha o comportamento de cópia verificando **copiar os ficheiros em modo recursivo** e **cópia binária** (copiar ficheiros como-é) opções. Clique em **Seguinte**.

    ![Especifique a pasta de saída](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. No **ligação especifique Data Lake Store** página, efetue os seguintes passos: 

    1. Selecione o Data Lake Store para o **nome da conta do Data Lake Store**.
    2. Especifique as informações de principais de serviço, incluindo **inquilino**, **ID de principal de serviço**, e **chave principal do serviço**.
    3. Clique em **Seguinte**. 

    > [!IMPORTANT]
    > Esta explicação passo a passo, utiliza um **principal de serviço** para autenticar o arquivo data lake. Siga as instruções [aqui](connector-azure-data-lake-store.md#using-service-principal-authentication) e certifique-se de conceder as serviço principal permissões adequadas no Azure Data Lake Store.

    ![Especifique a conta do Azure Data Lake Store](./media/load-data-into-azure-data-lake-store/specify-adls.png)

9. No **escolha a pasta ou ficheiro de saída** página, especifique **copyfroms3**, em seguida, clique em **seguinte**. 

    ![Especifique a pasta de saída](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)


10. No **definições** página, clique em **seguinte**. 

    ![Página de definições](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. No **resumo** página, reveja as definições e clique em **seguinte**.

    ![Página de resumo](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. No **página implementação**, clique em **Monitor** para monitorizar o pipeline (tarefas).

    ![Página de implementação](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Tenha em atenção que o **Monitor** separador à esquerda é selecionado automaticamente. Pode ver as ligações para ver detalhes de execução da atividade e volte a executar o pipeline de **ações** coluna. 

    ![Monitorizar o pipeline é executada](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Para ver as execuções de atividade associadas com o pipeline executar, clique em **ver a atividade é executada** ligação no **ações** coluna. Não há apenas uma atividade (atividade de cópia) no pipeline, para ver apenas uma entrada. Para mudar para o pipeline é executado vista, clique em **Pipelines** ligação na parte superior. Clique em **atualizar** para atualizar a lista. 

    ![Execuções de atividade do monitor](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Ainda pode monitorizar detalhes da execução de cada atividade de cópia, clicando a **detalhes** ligação em **ações** na vista de monitorização de atividade. Mostra informações, incluindo o volume dos dados copiados a partir da origem para sink, débito, os passos que realiza com duração correspondente e utilizado configurações.

    ![Detalhes da execução de atividade de monitorização](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Certifique-se de que os dados são copiados para o Azure Data Lake Store. 

    ![Certifique-se a saída do Data Lake Store](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Passos Seguintes

Avançar para o artigo seguinte para saber mais sobre o suporte do Azure Data Lake Store: 

> [!div class="nextstepaction"]
>[Conector do Azure Data Lake Store](connector-azure-data-lake-store.md)