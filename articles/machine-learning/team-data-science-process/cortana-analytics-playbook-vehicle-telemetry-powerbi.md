---
title: Dashboard do Power BI por Estado de funcionamento do veículo e orientar hábitos - Azure | Documentos da Microsoft
description: Utilize as capacidades do Cortana Intelligence para obter informações em tempo real e preditivo sobre o estado de funcionamento do veículo e orientar hábitos.
services: machine-learning
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: deguhath
ms.openlocfilehash: b703cb4d3ddd8b62895c9c40c7fa2fba728e884e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262286"
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Instruções de configuração do veículo telemetria Analytics solução modelo Power BI dashboard
Este links de menu para os capítulos deste manual de comunicação: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

As apresentações de solução de análise de telemetria de veículos, como os concessionários de automóveis, os fabricantes de automóveis e as companhias são capazes de utilizam as capacidades do Cortana Intelligence. O atacante pode obter informações em tempo real e preditivo sobre o estado de funcionamento do veículo e impulsionar hábitos para melhorar a experiência do cliente, pesquisa e desenvolvimento e campanhas de marketing. Estas instruções passo a passo mostram como configurar os relatórios do Power BI e o dashboard depois de implementar a solução na sua subscrição. 

## <a name="prerequisites"></a>Pré-requisitos
* Implementar o [análise Telemétrica de veículos](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) solução. 
* [Instalar o Power BI Desktop](https://www.microsoft.com/download/details.aspx?id=45331).
* Obter um [subscrição do Azure](https://azure.microsoft.com/pricing/free-trial/). Se não tiver uma subscrição do Azure, comece com a subscrição gratuita do Azure.
* Abra uma conta do Power BI.

## <a name="cortana-intelligence-suite-components"></a>Componentes do Cortana Intelligence suite
Como parte do modelo de solução de análise de telemetria de veículos, os seguintes serviços do Cortana Intelligence são implementados na sua subscrição:

* **Os Hubs de eventos do Azure** ingere milhões de eventos de telemetria de veículos para o Azure.
* **O Azure Stream Analytics** fornece informações em tempo real sobre o estado de funcionamento do veículo e mantém esses dados no armazenamento de longo prazo para análise de lotes mais sofisticada.
* **O Azure Machine Learning** Deteta anomalias em tempo real e utiliza o processamento em lotes para fornecer informações preditivas.
* **O Azure HDInsight** transforma os dados em escala.
* **O Azure Data Factory** processa a orquestração, agendamento, gestão de recursos e monitorização do pipeline de processamento em lotes.

**Power BI** fornece a esta solução um dashboard avançado para dados e visualizações de Análise Preditiva. 

A solução utiliza duas origens de dados diferentes:

* Sinais de veículos simulados e conjuntos de dados de diagnóstico
* Catálogo de veículos

Computador com o vehicle telematics simulator é incluído como parte desta solução. Emite informações de diagnóstico e sinais que correspondem para o estado do veículo e padrões motrizes num determinado ponto no tempo. 

O catálogo de veículos é um conjunto de dados de referência que mapeia VINs a modelos.

## <a name="power-bi-dashboard-preparation"></a>Preparação de dashboard do Power BI
### <a name="set-up-the-power-bi-real-time-dashboard"></a>Configurar o dashboard do Power BI em tempo real

#### <a name="start-the-real-time-dashboard-application"></a>Iniciar a aplicação de dashboard em tempo real
Depois de concluída a implementação, siga as instruções de operação manual.

1. Transferir a aplicação de dashboard em tempo real RealtimeDashboardApp.zip e Descompacte-o.

1.  Na pasta descompactada, abra o ficheiro de configuração de aplicação RealtimeDashboardApp.exe.config. Substitua appSettings para os Hubs de eventos, armazenamento de Blobs do Azure e ligações de serviço do Azure Machine Learning com os valores nas instruções de operação manual. Guarde as alterações.

1. Execute o aplicativo RealtimeDashboardApp.exe. Na janela de início de sessão, introduza as credenciais de Power BI válidas. 

   ![Power BI início de sessão janela](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
1. Selecione **Aceitar**. A aplicação começa a ser executada.

   ![Permissões de dashboard do Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

1. Inicie sessão para o site do Power BI e criar um dashboard em tempo real.

Agora, está pronto para configurar o dashboard do Power BI.  

### <a name="configure-power-bi-reports"></a>Configurar relatórios do Power BI
Os relatórios em tempo real e o dashboard demorar cerca de 30 a 45 minutos para concluir. 

1. Navegue para o [Power BI](http://powerbi.com) página da Web e iniciar sessão.

    ![Início de sessão na página do Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

1. Um novo conjunto de dados é gerado no Power BI. Selecione o **ConnectedCarsRealtime** conjunto de dados.

    ![Conjunto de dados de ConnectedCarsRealtime](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

1. Para guardar o relatório em branco, prima Ctrl + S.

    ![Relatório em branco](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

1. Introduza o nome do relatório **veículo telemetria análise em tempo real - relatórios**.

    ![Nome do relatório](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Relatórios em tempo real
Três relatórios em tempo real são nesta solução:

* Veículos na operação
* Veículos que necessitam de manutenção
* Estatísticas de estado de funcionamento do veículo

Pode configurar todos os três dos relatórios, ou pode parar após qualquer fase. Em seguida, pode avançar para a próxima seção sobre como configurar relatórios de batch. Recomendamos que crie todos os três relatórios para visualizar as informações de totais do caminho da solução em tempo real.  

### <a name="vehicles-in-operation-report"></a>Veículos no relatório de operação
1. Faça duplo clique em **página 1**e renomeá-lo **veículos numa operação**.

    ![Veículos na operação](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

1. Sobre o **campos** separador, selecione **vin**. Sobre o **visualizações** separador, selecione a **cartão** visualização.  

    O **cartão** visualização é criada, conforme mostrado na figura a seguir:

    ![Selecione vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

1. Selecione a área em branco para adicionar uma nova visualização.  

1. Sobre o **campos** separador, selecione **Cidade** e **vin**. Sobre o **visualizações** separador, selecione a **mapa** visualização. Arraste **vin** para o **valores** área. Arraste **Cidade** para o **legenda** área. 

    ![Visualização de cartão](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

1. Sobre o **visualizações** separador, selecione a **formato** secção. Selecione **Title**e altere **texto** para **veículos numa operação por cidade**.

    ![Veículos numa operação por cidade](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

    A visualização final terá um aspeto semelhante ao seguinte exemplo:

    ![Última visualização](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

1. Selecione a área em branco para adicionar uma nova visualização.  

1. Sobre o **campos** separador, selecione **Cidade** e **vin**. Sobre o **visualizações** separador, selecione a **gráfico de colunas em cluster** visualização. Arraste **Cidade** para o **eixo** área. Arraste **vin** para o **valor** área.

1. Ordene o gráfico por **contagem de vin**.

    ![Contagem de vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

1. Alterar o gráfico **Title** ao **veículos numa operação por cidade**. 

1. Selecione o **formato** secção e, em seguida, selecione **cores de dados**. Alteração **Mostrar tudo** ao **no**.

    ![Cores de dados](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

1. Altere a cor de uma cidade individual ao selecionar o símbolo de cor.

    ![Alteração de cor](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

1. Selecione a área em branco para adicionar uma nova visualização.  

1. Sobre o **visualizações** separador, selecione a **gráfico de colunas em cluster** visualização. Sobre o **campos** separador, arraste **Cidade** para o **eixo** área. Arraste **modelo** para o **legenda** área. Arraste **vin** para o **valor** área.

    ![Gráfico de colunas agrupadas](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)

    O gráfico parece semelhante à imagem seguinte:

    ![Composição](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

1. Reorganizar todas as visualizações para que a página é semelhante ao seguinte exemplo:

    ![Dashboard com visualizações](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Configurou com êxito o relatório de "Veículos na operação". É possível criar o próximo relatório em tempo real, ou pode parar por aqui e configurar o dashboard. 

### <a name="vehicles-requiring-maintenance-report"></a>Relatório de necessidade de manutenção de veículos

1. Selecione ![adicionar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) para adicionar um novo relatório. Mude o nome **manutenção da necessidade de veículos**.

    ![Veículos que necessitam de manutenção](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

1. Sobre o **campos** separador, selecione **vin**. Sobre o **visualizações** separador, selecione a **cartão** visualização.

    ![Visualização de cartão de Vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

    O conjunto de dados contém um campo chamado **MaintenanceLabel**. Este campo pode ter um valor de "0" ou "1". O valor é definido pelo modelo do machine learning que está aprovisionado como parte da solução. Ele está integrado com o caminho em tempo real. O valor "1" indica que um veículo precisa de manutenção. 

1. Para adicionar um **filtro de nível de página** para mostrar os dados para os veículos que necessitam de manutenção: 

   a. Arrastar o **MaintenanceLabel** campo **filtros de nível de página**.
  
      ![Filtros de nível de página](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)

    b. Na parte inferior **MaintenanceLabel de filtros de nível de página**, selecione **filtragem básica**.

      ![Filtragem básica](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png) 

    c. Defina o valor de filtro como **1**.

      ![Valor do filtro](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

1. Selecione a área em branco para adicionar uma nova visualização.  

1. Sobre o **visualizações** separador, selecione a **gráfico de colunas em cluster** visualização. 

    ![Vin cartão](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)

    ![Gráfico de colunas agrupadas](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

1. Sobre o **campos** separador, arraste **modelo** para o **eixo** área. Arraste **vin** para o **valor** área. Em seguida, ordenar a visualização por **contagem de vin**. Alterar o gráfico **Title** ao **veículos que necessitam de manutenção pelo modelo**. 

1. No **campos** ![campos de imagem](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) seção o **visualizações** separador, arraste **vin** para **saturação de cor**.

    ![Saturação da cor](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

1. Sobre o **formato** secção, altere **cores de dados** na visualização: 

    a. Alteração da **mínimo** colorir para **F2C812**.

    b. Alteração da **máxima** colorir para **FF6300**.

    ![Cores de dados nova](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)

    As cores de visualização nova ter um aspeto semelhante ao seguinte exemplo:

    ![Cores de visualização nova](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

1. Selecione a área em branco para adicionar uma nova visualização.  

1. Sobre o **visualizações** separador, selecione **gráfico de colunas em cluster**. Arraste **vin** para o **valor** área. Arraste **Cidade** para o **eixo** área. Ordene o gráfico por **contagem de vin**. Alterar o gráfico **Title** ao **veículos que necessitam de manutenção por cidade**.

    ![Veículos que necessitam de manutenção por cidade](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

1. Selecione a área em branco para adicionar uma nova visualização.  

1. Sobre o **visualizações** separador, selecione a **cartão de linhas múltiplas** visualização. Arraste **modelo** e **vin** para o **campos** área.

    ![Cartão de linhas múltiplas](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

1. Reorganizar todas as visualizações para que o relatório final se parece com o exemplo seguinte: 

    ![Reorganizadas relatório final](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Configurou com êxito o relatório em tempo real "Veículos necessidade de manutenção". É possível criar o próximo relatório em tempo real, ou pode parar por aqui e configurar o dashboard. 

### <a name="vehicle-health-statistics-report"></a>Relatório de estatísticas de estado de funcionamento do veículo

1. Selecione ![adicionar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) para adicionar um novo relatório. Mude o nome **estatísticas de estado de funcionamento de veículos**. 

1. Sobre o **visualizações** separador, selecione a **medidor** visualização. Arraste **velocidade** para o **valor**, **valor mínimo**, e **valor máximo** áreas.

   ![Estatísticas de estado de funcionamento de veículos](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

1. Na **valor** área, alterar a agregação predefinida de **velocidade** para **média**.

1. Na **valor mínimo** área, alterar a agregação predefinida de **velocidade** para **mínimo**.

1. Na **valor máximo** área, alterar a agregação predefinida de **velocidade** para **máximo**.

   ![Valores de velocidade](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

1. Mudar o nome da **título do medidor** para **médio de velocidade**.

   ![Medidor](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

1. Selecione a área em branco para adicionar uma nova visualização.  

    Da mesma forma, adicione uma **medidor** para **médio de petróleo motor**, **médio de combustível**, e **média de temperatura de motor**.  

1. Alterar a agregação predefinida de campos em cada medidor como fez nos passos anteriores a **médio de velocidade** do medidor.

    ![Medidores adicionais](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

1. Selecione a área em branco para adicionar uma nova visualização.

1. Sobre o **visualizações** separador, selecione a **linha e gráfico de colunas em cluster** visualização. Arraste **Cidade** ao **eixo partilhado**. Arraste **tirepressure**, **engineoil**, e **velocidade** para o **valores da coluna** área. Alterar seu tipo de agregação para **média**. 

1. Arraste **engineTemperature** para o **valores de linha** área. Altere o tipo de agregação para **média**. 

    ![Coluna e os valores de linha](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

1. Alterar o gráfico **Title** ao **médio de velocidade, tire pressão, petróleo de motor e temperatura motor**.  

    ![Título da linha e gráfico de colunas em cluster](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

1. Selecione a área em branco para adicionar uma nova visualização.

1. Sobre o **visualizações** separador, selecione a **Treemap** visualização. Arraste **modelo** para o **grupo** área. Arraste **MaintenanceProbability** para o **valores** área.

1. Alterar o gráfico **Title** ao **modelos de veículos que necessitam de manutenção**.

    ![Título do mapa de árvore](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

1. Selecione a área em branco para adicionar uma nova visualização.

1. Sobre o **visualizações** separador, selecione a **gráfico em barras empilhadas a 100%** visualização. Arraste **Cidade** para o **eixo** área. Arraste **MaintenanceProbability** e **RecallProbability** para o **valor** área.

    ![Áreas de eixo e valor](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

1. Sobre o **formato** secção, selecione **cores de dados**. Definir o **MaintenanceProbability** cor para o valor **F2C80F**.

1. Alterar o gráfico **Title** ao **manutenção de probabilidade de veículos e lembre-se por cidade**.

    ![Título do gráfico de barras empilhadas a 100%](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

1. Selecione a área em branco para adicionar uma nova visualização.

1. Sobre o **visualizações** separador, selecione a **gráfico de área** visualização. Arraste **modelo** para o **eixo** área. Arraste **engineOil**, **tirepressure**, **velocidade**, e **MaintenanceProbability** para o **valores** área. Alterar seu tipo de agregação para **média**. 

    ![Tipo de agregação](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

1. Alterar o gráfico **Title** ao **médio de petróleo mecanismo, tire pressão, velocidade e a probabilidade de manutenção pelo modelo**.

    ![Título do gráfico de área](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

1. Selecione a área em branco para adicionar uma nova visualização.

1. Sobre o **visualizações** separador, selecione a **gráfico de dispersão** visualização. Arraste **modelo** para o **detalhes** e **legenda** áreas. Arraste **combustível** para o **eixo x** área. Alterar a agregação para **média**. Arraste **engineTemperature** para o **eixo y** área. Alterar a agregação para **média**. Arraste **vin** para o **tamanho** área.

    ![Detalhes, áreas de legenda, eixo e tamanho](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

1. Alterar o gráfico **Title** ao **médio de combustível, média de engineTemperature e contagem de vin pelo modelo**.

    ![Título do gráfico de dispersão](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

    O relatório final terá um aspeto semelhante ao seguinte exemplo:

    ![Relatório final](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Afixar visualizações dos relatórios para o dashboard em tempo real
1. Criar um dashboard em branco ao selecionar o símbolo de adição junto a **Dashboards**. Introduza o nome **painel de análise de telemetria de veículos**.

    ![Símbolo de dashboard mais](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

1. Afixe as visualizações dos relatórios anteriores ao dashboard. 

    ![Símbolo de pin do dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

    Quando todos os três relatórios afixados ao dashboard, deve ter um aspeto semelhante ao seguinte exemplo. Se não criar todos os relatórios, o dashboard pode ser diferente. 

    ![Dashboard com relatórios](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Criado com êxito o dashboard em tempo real. À medida que continua a executar CarEventGenerator.exe e RealtimeDashboardApp.exe, verá as atualizações dinâmicas no dashboard. As seguintes etapas guiarão cerca de 10 a 15 minutos a concluir.

## <a name="set-up-the-power-bi-batch-processing-dashboard"></a>Configurar o dashboard de processamento de batch do Power BI
> [!NOTE]
> Demora cerca de duas horas (da conclusão com êxito da implantação) para o pipeline para concluir a execução e processar registros um ano de dados gerados de processamento em lotes ponto-a-ponto. Aguarde que o processamento ser concluídas antes de continuar com os seguintes passos:
> 
> 

### <a name="download-the-power-bi-designer-file"></a>Transfira o ficheiro de estruturador do Power BI

1. Um arquivo de designer do Power BI pré-configurada é incluído como parte das instruções de operação manual de implementação. Procure "2. Configurar o dashboard de processamento de batch do PowerBI."

1. Transferir o modelo do Power BI para o dashboard de processamento em lotes chamado aqui **ConnectedCarsPbiReport.pbix**.

1. Guarde-o localmente.

### <a name="configure-power-bi-reports"></a>Configurar relatórios do Power BI

1. Abra o arquivo de designer **ConnectedCarsPbiReport.pbix** ao utilizar o Power BI Desktop. Se ainda não tiver, instale o Power BI Desktop a partir da [instalação do Power BI Desktop](https://www.microsoft.com/download/details.aspx?id=45331) Web site.

1. Selecione **editar consultas**.

    ![Editar consultas](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

1. Faça duplo clique em **origem**.

    ![Origem](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

1. Atualize a cadeia de ligação do servidor com o servidor de SQL do Azure que foi aprovisionado como parte da implementação. Consultar as instruções de operação manual na base de dados SQL do Azure:

    * Servidor: somethingsrv.database.windows.net
    * Base de dados: connectedcar
    * Nome de utilizador: nome de utilizador
    * Palavra-passe: Pode gerir a sua palavra-passe do SQL Server no portal do Azure.

1. Deixe **base de dados** como **connectedcar**.

    ![Base de Dados](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

1. Selecione **OK**.

1. O **credenciais do Windows** separador está selecionado por predefinição. Altere-o para **credenciais de base de dados** ao selecionar o **base de dados** separador à direita.

1. Introduza o **nome de utilizador** e **palavra-passe** da base de dados SQL do Azure que foi especificada durante a sua configuração de implementação.

    ![Credenciais da base de dados](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

1. Selecione **Ligar**.

1. Repita os passos anteriores para cada um dos três consultas restantes presentes no painel da direita. Em seguida, atualize os detalhes de ligação da origem de dados.

1. Selecione **fechar e carregar**. Conjuntos de dados de ficheiro do Power BI Desktop estão ligados a tabelas de base de dados SQL.

1. Selecione **fechar** para fechar o ficheiro do Power BI Desktop.

    ![Fechar](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

1. Selecione **guardar** para guardar as alterações. 

Agora que configurou todos os relatórios que correspondem para o caminho na solução de processamento em lotes. 

## <a name="upload-to-powerbicom"></a>Carregar para o powerbi.com
1. Vá para o [portal web do Power BI](http://powerbi.com)e iniciar sessão.

1. Selecione **Obter Dados**.

1. Carregue o ficheiro do Power BI Desktop. Selecione **obter dados** > **obter ficheiros** > **ficheiro Local**.

1. Aceda a **ConnectedCarsPbiReport.pbix**.

1. Depois do ficheiro é carregado, regresse ao seu espaço de trabalho do Power BI. Um conjunto de dados, um relatório e um dashboard em branco são criados por si.  

1. Gráficos de PIN para um novo dashboard chamado **painel de análise de telemetria de veículos** no Power BI. Selecione o dashboard em branco que foi criado anteriormente e, em seguida, vá para o **relatórios** secção. Selecione o relatório carregado recentemente.  

    ![Novo dashboard do Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

    O relatório tem seis páginas:

    1 de página: Densidade de veículos  
    A página 2: Estado de funcionamento do veículo em tempo real  
    Página 3: Agressivamente controlado por veículos   
    Página 4: Resgatar os veículos  
    5 de página: Eficiente controlado por veículos de combustível  
    6 de página: Logótipo de motores de Contoso  

    ![Relatório do Power BI com seis páginas](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

1. Partir **a página 3**, afixe o seguinte conteúdo:  

    a. **Contagem de vin**  

   ![Contagem de 3 páginas de vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png)

    b. **Agressivamente controlado por veículos pelo modelo – o gráfico de cascata** 

   ![Gráfico de página 3 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

1. Partir **página 5**, afixe o seguinte conteúdo: 

    a. **Contagem de vin**

   ![Gráfico de página 5 de 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)

    b. **Veículos Fuel-Efficient pelo modelo: gráfico de colunas agrupadas**

   ![Gráfico de página 5 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

1. Partir **a página 4**, afixe o seguinte conteúdo:  

    a. **Contagem de vin** 

   ![Gráfico de página 4 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

    b. **Veículos recolhidos por cidade, do modelo: Treemap**

   ![Gráfico de página 4 de 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

1. Partir **6 página**, afixe o seguinte conteúdo:  

    * **Logótipo de motores de contoso**

    ![Logótipo de motores de contoso](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

### <a name="organize-the-dashboard"></a>Organizar o dashboard  

1. Aceda ao dashboard.

1. Coloque o cursor sobre cada gráfico. Mudar o nome de cada gráfico com base na nomenclatura surge no exemplo de dashboard concluir o procedimento seguinte:

   ![Organização de dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png) 
   
1. Mova os gráficos em torno para ter um aspeto semelhante ao seguinte exemplo de dashboard:

    ![Dashboard reorganizada](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)

1. Depois de criar todos os relatórios mencionados neste documento, o dashboard de concluir o procedimento final terá um aspeto semelhante ao seguinte exemplo: 

   ![Final dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Criou com êxito os relatórios e dashboard para obter em tempo real, previsão e informações de lote no estado de funcionamento do veículo e orientar hábitos.  
