---
title: Tutorial de BI de energia para o conector Azure Cosmos DB | Microsoft Docs
description: Utilize este tutorial do Power BI para importar JSON, criar relatórios insightful e visualizar dados utilizando o conector de BD do Cosmos do Azure e o Power BI.
keywords: Power bi tutorial, visualize os dados, o conector do power bi
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: cd1b7f70-ef99-40b7-ab1c-f5f3e97641f7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: sngun
ms.openlocfilehash: 8a0f50ad6df1135e05cd69be78e6b7f7820f90c6
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="power-bi-tutorial-for-azure-cosmos-db-visualize-data-using-the-power-bi-connector"></a>Tutorial de BI de energia de base de dados do Azure Cosmos: visualizar dados utilizando o conector do Power BI
[PowerBI.com](https://powerbi.microsoft.com/) é um serviço online, onde pode criar e partilhar os dashboards e relatórios com os dados que são importantes para si e a sua organização para.  Ambiente de trabalho do Power BI é um ferramenta que permite-lhe obter dados de várias origens de dados, intercalar e transformar os dados, criar relatórios poderosos e visualizações e publicar os relatórios no Power BI de criação de relatórios dedicado.  Com a versão mais recente do Power BI Desktop, pode agora ligar à sua conta de base de dados do Azure Cosmos através do conector Azure Cosmos DB para o Power BI.   

Neste tutorial do Power BI, vamos percorrer os passos para ligar a uma conta de base de dados do Azure Cosmos no Power BI Desktop, navegue para uma coleção em que pretende extrair os dados utilizando o navegador, transformar dados JSON em formato tabular, utilizando o Editor de consultas de ambiente de trabalho do Power BI e criar e publicar um relatório no PowerBI.com.

Depois de concluir este tutorial do Power BI, poderá responder às seguintes questões:  

* Como pode incorporar relatórios com os dados da base de dados do Azure Cosmos utilizando o ambiente de trabalho do Power BI?
* Como ligar a uma conta de base de dados do Azure Cosmos no Power BI Desktop
* Como posso obter dados de uma coleção no Power BI Desktop?
* Como pode transformar os dados JSON aninhados no Power BI Desktop?
* Como publicar e partilhar os meus relatórios em PowerBI.com?

> [!NOTE]
> O conector do Power BI para a base de dados do Azure Cosmos estabelece ligação ao ambiente de trabalho do Power BI para extração e a transformação de dados. Relatórios criados no Power BI Desktop, em seguida, podem ser publicados para o site PowerBI.com. Extração direta e a transformação de dados de base de dados do Azure Cosmos não podem ser efetuadas no PowerBI.com. 

> [!NOTE]
> Para ligar a base de dados do Azure Cosmos ao Power BI através da API do MongoDB, tem de utilizar o [o controlador ODBC do Simba MongoDB](http://www.simba.com/drivers/mongodb-odbc-jdbc/).

## <a name="prerequisites"></a>Pré-requisitos
Antes de seguir as instruções neste tutorial do Power BI, certifique-se de que tem acesso para os seguintes recursos:

* [A versão mais recente do Power BI Desktop](https://powerbi.microsoft.com/desktop).
* Acesso a nossa conta de demonstração ou dados na sua conta de base de dados do Azure Cosmos.
  * A conta de demonstração é preenchida com os dados de volcano mostrados neste tutorial. Esta conta de demonstração não está vinculada por qualquer SLA e destina-se apenas a fins de demonstração.  Estamos a reservar o direito de Efetue alterações para esta conta de demonstração, incluindo mas não se limitando a terminar a conta, alterar a chave, restringir o acesso, a alteração e eliminar os dados em qualquer altura, sem aviso prévio ou motivo.
    * URL: https://analytics.documents.azure.com
    * Read-only key: MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==
  * Ou, para criar a sua própria conta, consulte [criar uma conta de base de dados de base de dados do Azure Cosmos no portal do Azure](https://azure.microsoft.com/documentation/articles/create-account/). Em seguida, obter volcano exemplo dados que é semelhantes à que são utilizados neste tutorial (mas não contém os blocos de GeoJSON), consulte o [NOAA site](https://www.ngdc.noaa.gov/nndc/struts/form?t=102557&s=5&d=5) e, em seguida, importar os dados através de [ferramenta de migração de dados de base de dados do Azure Cosmos](import-data.md).

Para partilhar os seus relatórios em PowerBI.com, tem de ter uma conta no site PowerBI.com.  Para mais informações sobre o Power BI para gratuito e Power BI Pro, visite [ https://powerbi.microsoft.com/pricing ](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Vamos começar
Neste tutorial, vamos imagine que é um geologist studying princípios volcanoes em todo o mundo.  Os dados de volcano são armazenados numa conta de base de dados do Azure Cosmos e os documentos JSON parecerem ser o documento de exemplo seguinte.

    {
        "Volcano Name": "Rainier",
           "Country": "United States",
          "Region": "US-Washington",
          "Location": {
            "type": "Point",
            "coordinates": [
              -121.758,
              46.87
            ]
          },
          "Elevation": 4392,
          "Type": "Stratovolcano",
          "Status": "Dendrochronology",
          "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }

Pretende obter os dados de volcano da conta de base de dados do Azure Cosmos e visualizar dados de um relatório do Power BI interativo como o relatório seguinte.

![Por concluir este tutorial do Power BI com o conector do Power BI, poderá visualizar dados com o relatório do Power BI Desktop volcano](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)

Está pronto para experimentá-la? Vamos começar.

1. Execute o ambiente de trabalho do Power BI na sua estação de trabalho.
2. Assim que o Power BI Desktop é iniciada, um *boas-vindas* é apresentado o ecrã.
   
    ![Ecrã de boas-vindas do BI ambiente de trabalho de energia - conector do Power BI](./media/powerbi-visualize/power_bi_connector_welcome.png)
3. Pode **obter dados**, consulte **origens recentes**, ou **abrir relatórios de outros** diretamente a partir de *boas-vindas* ecrã.  Clique no X no canto superior direito para fechar o ecrã. O **relatório** é apresentada a vista de ambiente de trabalho do Power BI.
   
    ![Power BI Desktop relatório vista - conector do Power BI](./media/powerbi-visualize/power_bi_connector_pbireportview.png)
4. Selecione o **home page** do Friso, em seguida, clique em **obter dados**.  O **obter dados** deve aparecer a janela.
5. Clique em **Azure**, selecione **BD do Cosmos Azure (Beta)**e, em seguida, clique em **Connect**. 

    ![Ambiente de trabalho do Power BI obter dados - conector do Power BI](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   
6. No **pré-visualização do conector** página, clique em **continuar**. O **Azure Cosmos DB** surge a janela.
7. Especifique o URL de ponto final de conta do Azure Cosmos DB teria de como obter os dados conforme mostrado abaixo e, em seguida, clique em **OK**. Para utilizar a sua própria conta, pode obter o URL na caixa URI no **[chaves](manage-account.md#keys)** painel do portal do Azure. Para utilizar a conta de demonstração, introduza `https://analytics.documents.azure.com` para o URL. 
   
    Deixe o nome de base de dados, o nome da coleção e a instrução SQL em branco como estes campos são opcionais.  Em vez disso, utilizamos o navegador para selecionar a base de dados e a coleção para identificar de onde vêm os dados.
   
    ![Tutorial do Power BI para o conector do Power BI do Azure Cosmos DB - janela de estabelecer a ligação de ambiente de trabalho](./media/powerbi-visualize/power_bi_connector_pbiconnectwindow.png)
8. Se estiver a ligar a este ponto final pela primeira vez, é-lhe pedida a chave de conta. Para a sua própria conta, obter a chave a partir de **chave primária** caixa o **[chaves só de leitura](manage-account.md#keys)** painel do portal do Azure. Para a conta de demonstração, a chave é `MSr6kt7Gn0YRQbjd6RbTnTt7VHc5ohaAFu7osF0HdyQmfR+YhwCH2D2jcczVIR1LNK3nMPNBD31losN7lQ/fkw==`. Introduza a chave adequada e, em seguida, clique em **Connect**.
   
    Recomendamos que utilize a chave apenas de leitura durante a criação de relatórios.  Isto impede a exposição desnecessária da chave mestra para potenciais riscos de segurança. A chave apenas de leitura está disponível a partir de [chaves](manage-account.md#keys) painel do portal do Azure ou pode utilizar as informações de conta de demonstração fornecidas acima.
   
    ![Tutorial de BI de energia para o conector do Power BI do Azure Cosmos DB - chave da conta](./media/powerbi-visualize/power_bi_connector_pbidocumentdbkey.png)
    
    > [!NOTE] 
    > Se obtiver um erro que indica "a base de dados especificada não encontrada." Veja a solução passos deste [problema do Power BI](https://community.powerbi.com/t5/Issues/Document-DB-Power-BI/idi-p/208200).
    
9. Quando a conta está ligada com êxito, o **navegador** painel aparece.  O **navegador** mostra uma lista de bases de dados sob a conta.
10. Clique e expanda na base de dados onde os dados para o relatório vêm, se estiver a utilizar a conta de demonstração, selecionar **volcanodb**.   
11. Agora, selecione uma coleção que contém os dados para obter. Se estiver a utilizar a conta de demonstração, selecione **volcano1**.
    
    O painel de pré-visualização mostra uma lista de **registo** itens.  Um documento é representado como uma **registo** tipo no Power BI. Da mesma forma, um bloco JSON aninhado dentro de um documento é também uma **registo**.
    
    ![Tutorial do Power BI para o conector do Power BI do Azure Cosmos DB - janela do navegador](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Clique em **editar** para iniciar o Editor de consultas numa nova janela para transformar os dados.

## <a name="flattening-and-transforming-json-documents"></a>Flattening e transformar documentos JSON
1. Mudar para a janela do Editor de consultas do Power BI, onde o **documento** coluna no painel central.
   ![Editor de consultas de ambiente de trabalho do Power BI](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Clique em expander no lado direito do **documento** cabeçalho da coluna.  Será apresentado o menu de contexto com uma lista de campos.  Selecione os campos que necessita para o relatório, por exemplo, nome Volcano, país, região, localização, elevação, tipo, o estado e última Eruption saber. Desmarque a **utilize nome de coluna original como prefixo** caixa e, em seguida, clique em **OK**.
   
    ![Tutorial de BI para o conector Azure Cosmos DB Power BI de energia - expanda documentos](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. O painel central apresenta uma versão de pré-visualização do resultado com os campos selecionados.
   
    ![Tutorial de BI para o conector Azure Cosmos DB Power BI de energia - aplanar resultados](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. No nosso exemplo, a propriedade de localização é um bloco de GeoJSON num documento.  Como pode ver, a localização é representada como uma **registo** tipo no Power BI Desktop.  
5. Clique em expander no lado direito no cabeçalho da coluna Document.Location.  O menu de contexto com os campos do tipo e as coordenadas são apresentados.  Selecione o campo de coordenadas, certifique-se de vamos **utilize nome de coluna original como prefixo** não está selecionada e clique em **OK**.
   
    ![Tutorial de BI de energia para o conector do Power BI do Azure Cosmos DB - registo de localização](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. O painel central mostra agora uma coluna de coordenadas do **lista** tipo.  Como é mostrado no início do tutorial, os dados de GeoJSON neste tutorial são do tipo de ponto com valores de Latitude e Longitude registados na matriz de coordenadas.
   
    O elemento de coordenadas [0] representa Longitude enquanto coordenadas [1] representa Latitude.
    ![Tutorial de BI de energia para o conector do Power BI do Azure Cosmos DB - lista de coordenadas](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. Para aplanar a matriz de coordenadas, crie um **coluna personalizado** chamado LatLong.  Selecione o **Add Column** do Friso e clique em **coluna personalizado**.  O **coluna personalizado** surge a janela.
8. Forneça um nome para a nova coluna, por exemplo, LatLong.
9. Em seguida, especifique a fórmula personalizada para a nova coluna.  Para o nosso exemplo, iremos irá concatenar os valores de Latitude e Longitude separados por vírgulas, como mostrado abaixo utilizando a fórmula seguinte: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. Clique em **OK**.
   
    Para obter mais informações em expressões de análise de dados (DAX), incluindo funções DAX, visite [DAX básico no Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).
   
    ![Tutorial de BI de energia para o conector do Power BI do Azure Cosmos DB - Add Column personalizada](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)

10. Agora, o painel central mostra as novas colunas LatLong preenchidas com os valores.
    
    ![Tutorial de BI de energia para o conector Azure Cosmos DB Power BI - coluna LatLong personalizada](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Se receber um erro na coluna nova, certifique-se de que os passos aplicados em definições de consulta correspondem a figura seguinte:
    
    ![Passos aplicados devem ser a origem de navegação, documento expandido, Document.Location expandido, adicionado personalizada](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    Se os passos são diferentes, elimine os passos adicionais e tente novamente a adicionar a coluna personalizada. 

11. Clique em **fechar e aplicar** para guardar o modelo de dados.
    
    ![Tutorial do Power BI para o conector do Power BI do Azure Cosmos DB - feche & Aplicar](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-the-reports"></a>Os relatórios de compilação
Vista do Power BI Desktop relatório é onde pode iniciar a criação de relatórios para visualizar dados.  Pode criar relatórios ao arrastar e largar os campos para o **relatório** tela.

![Power BI Desktop relatório vista - conector do Power BI](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

Na vista de relatório, encontrará:

1. O **campos** painel, este é onde pode ver uma lista de modelos de dados com campos que pode utilizar para os seus relatórios.
2. O **visualizações** painel. Um relatório pode conter um único ou vários visualizações.  Escolha os tipos de visual às suas necessidades de ajuste o **visualizações** painel.
3. O **relatório** tela, este é onde compilar visuais para o relatório.
4. O **relatório** página. Pode adicionar várias páginas de relatório no Power BI Desktop.

O seguinte mostra os passos básicos de criação de um relatório de vista do mapa interativo simples.

1. Para o nosso exemplo, iremos criar uma vista de mapa que mostra a localização de cada volcano.  No **visualizações** painel, clique no tipo de visual de mapa conforme realçado na captura de ecrã acima.  Deverá ver o tipo de visual de mapa desenhado no **relatório** tela.  O **visualização** painel também deverá apresentar um conjunto de propriedades relacionadas com o tipo de visual do mapa.
2. Agora, arrastar e largar o campo de LatLong do **campos** painel para o **localização** propriedade no **visualizações** painel.
3. Em seguida, arrastar e largar o campo de nome de Volcano para o **legenda** propriedade.  
4. Em seguida, arrastar e largar o campo de elevação para o **tamanho** propriedade.  
5. Deverá ver o mapa que mostra um conjunto de bolhas que indica a localização de cada volcano com o tamanho de bolha correlacionando para a elevação do volcano visual.
6. Agora criou um relatório básico.  Pode personalizar ainda mais o relatório adicionando mais visualizações.  No nosso caso, vamos adicionar uma segmentação de dados do tipo Volcano para tornar o relatório interativo.  
   
    ![Captura de ecrã do relatório do Power BI Desktop final após a conclusão do tutorial do Power BI para a base de dados do Azure Cosmos](./media/powerbi-visualize/power_bi_connector_pbireportfinal.png)
7. No menu ficheiro, clique em **guardar** e guarde o ficheiro como PowerBITutorial.pbix.

## <a name="publish-and-share-your-report"></a>Publicar e partilhar o relatório
Para partilhar o relatório, tem de ter uma conta no site PowerBI.com.

1. No Power BI Desktop, clique em de **home page** Friso.
2. Clique em **Publicar**.  São solicitado para introduzir o nome de utilizador e palavra-passe para a sua conta de site PowerBI.com.
3. Depois de ter sido autenticada a credencial, o relatório é publicado para o destino selecionado.
4. Clique em **aberto 'PowerBITutorial.pbix' no Power BI** para ver e partilhar o relatório no PowerBI.com.
   
    ![Publicar o Power BI êxito! Tutorial abrir no Power BI](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>Criar um dashboard no PowerBI.com
Agora que tem um relatório, permite partilhá-la no site PowerBI.com

Ao publicar o relatório do Power BI Desktop powerbi.com, será gerado um **relatório** e um **Dataset** no inquilino da sua PowerBI.com. Por exemplo, após publicou um relatório chamado **PowerBITutorial** powerbi.com, verá PowerBITutorial em ambos os **relatórios** e **conjuntos de dados** secções em PowerBI.com.

   ![Captura de ecrã do novo relatório e conjunto de dados no site PowerBI.com](./media/powerbi-visualize/powerbi-reports-datasets.png)

Para criar um dashboard partilháveis, clique o **Pin em direto página** botão no relatório PowerBI.com.

   ![Captura de ecrã do novo relatório e conjunto de dados no site PowerBI.com](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Em seguida, siga as instruções em [afixar um mosaico de um relatório](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) para criar um novo dashboard. 

Também pode fazer alterações ad hoc ao relatório antes de criar um dashboard. No entanto, é recomendado que utilize o ambiente de trabalho do Power BI para efetuar as modificações e voltar a publicar o relatório em PowerBI.com.

<!-- ## Refresh data in PowerBI.com
There are two ways to refresh data, ad hoc and scheduled.

For an ad hoc refresh, simply click on the eclipses (…) by the **Dataset**, e.g. PowerBITutorial. You should see a list of actions including **Refresh Now**. Click **Refresh Now** to refresh the data.

![Screenshot of Refresh Now in PowerBI.com](./media/powerbi-visualize/power-bi-refresh-now.png)

For a scheduled refresh, do the following.

1. Click **Schedule Refresh** in the action list. 

    ![Screenshot of the Schedule Refresh in PowerBI.com](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. In the **Settings** page, expand **Data source credentials**. 
3. Click on **Edit credentials**. 
   
    The Configure popup appears. 
4. Enter the key to connect to the Azure Cosmos DB account for that data set, then click **Sign in**. 
5. Expand **Schedule Refresh** and set up the schedule you want to refresh the dataset. 
6. Click **Apply** and you are done setting up the scheduled refresh.
-->
## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre o Power BI, consulte [introdução ao Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* Para saber mais sobre a BD do Cosmos do Azure, consulte o [página de destino de documentação do Azure Cosmos DB](https://azure.microsoft.com/documentation/services/cosmos-db/).

