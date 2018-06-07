---
title: Monitorizar a sincronização de dados SQL do Azure (pré-visualização) análise do registo | Microsoft Docs
description: Saiba como monitorizar a sincronização de dados de SQL do Azure (pré-visualização) através da utilização de análise de registos
services: sql-database
ms.date: 04/01/2018
ms.topic: conceptual
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.custom: data-sync
ms.openlocfilehash: 4c684337a904fe6984e6b5cd5cf88ef8eb976cb6
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34650252"
---
# <a name="monitor-sql-data-sync-preview-with-log-analytics"></a>Monitor SQL de sincronização de dados (pré-visualização) análise do registo 

Verifique o registo de atividade de sincronização de dados do SQL Server e detetar erros e avisos, que tinha anteriormente verificar a sincronização de dados SQL manualmente no portal do Azure ou utilizar o PowerShell ou a API REST. Siga os passos neste artigo para configurar uma solução personalizada que melhora a experiência de monitorização de sincronização de dados. Pode personalizar esta solução para que se adeque ao seu cenário.

Para obter uma descrição geral da Sincronização de Dados SQL, veja [Sincronizar dados em várias bases de dados na cloud e no local com a Sincronização de Dados SQL do Azure (Pré-visualização)](sql-database-sync-data.md).

## <a name="monitoring-dashboard-for-all-your-sync-groups"></a>Dashboard de monitorização para todos os grupos de sincronização 

Já não precisam de examine os registos de cada grupo de sincronização individualmente para procurar problemas. Pode monitorizar todos os grupos de sincronização de qualquer uma das suas subscrições no local através da utilização de uma vista personalizada de análise de registos. Esta vista apresenta as informações que mais lhe interessam para clientes de sincronização de dados do SQL Server.

![Dashboard de monitorização de sincronização de dados](media/sql-database-sync-monitor-oms/sync-monitoring-dashboard.png)

## <a name="automated-email-notifications"></a>Notificações de correio eletrónico automatizadas

Já não precisam de verificar o registo manualmente no portal do Azure ou através do PowerShell ou a API REST. Com [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), pode criar alertas diretamente para os endereços de e-mail das pessoas que necessitam para vê-los quando ocorre um erro.

![Notificações de correio eletrónico de sincronização de dados](media/sql-database-sync-monitor-oms/sync-email-notifications.png)

## <a name="how-do-you-set-up-these-monitoring-features"></a>Como pode configurar estas funcionalidades de monitorização? 

Implemente uma solução de monitorização para a sincronização de dados do SQL Server em menos de uma hora, efetuando os seguintes procedimentos de análise de registos personalizados:

Terá de configurar os três componentes:

-   Um runbook do PowerShell para o feed de dados de registo de sincronização de dados do SQL Server para análise de registos.

-   Um alerta de análise de registos para notificações por e-mail.

-   Uma vista de análise do registo para a monitorização.

### <a name="samples-to-download"></a>Exemplos para transferir

Transfira os exemplos de dois seguintes:

-   [Runbook do PowerShell de registo de sincronização de dados](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Vista de análise de registo de sincronização de dados](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

### <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que configurou as seguintes ações:

-   Uma conta de automatização do Azure

-   Área de trabalho do Log Analytics

## <a name="powershell-runbook-to-get-sql-data-sync-log"></a>Runbook do PowerShell para obter o registo de sincronização de dados do SQL Server 

Utilize um runbook de PowerShell alojado na automatização do Azure para extrair os dados de registo de sincronização de dados do SQL Server e envia-as à análise de registos. Um script de exemplo está incluído. Como pré-requisito, tem de ter uma conta de automatização do Azure. Em seguida, terá de criar um runbook e agendar para execução. 

### <a name="create-a-runbook"></a>Criar um runbook

Para obter mais informações sobre como criar um runbook, consulte [o meu primeiro runbook do PowerShell](https://docs.microsoft.com/azure/automation/automation-first-runbook-textual-powershell).

1.  Na sua conta de automatização do Azure, selecione o **Runbooks** separador em automatização de processos.

2.  Selecione **adicionar um Runbook** no canto superior esquerdo da página de Runbooks.

3.  Selecione **importar um Runbook existente**.

4.  Em **Runbook ficheiro**, utilize o determinado `DataSyncLogPowerShellRunbook` ficheiro. Definir o **tipo de Runbook** como `PowerShell`. Dê um nome de runbook.

5.  Selecione **Criar**. Tem agora um runbook.

6.  Na sua conta de automatização do Azure, selecione o **variáveis** separador em recursos partilhados.

7.  Selecione **adicionar uma variável** na página de variáveis. Crie uma variável para armazenar o último tempo de execução para o runbook. Se tiver vários runbooks, precisa de uma variável para cada runbook.

8.  Definir o nome da variável como `DataSyncLogLastUpdatedTime` e defina o respetivo tipo como valor DateTime.

9.  Selecione o runbook e clique no botão Editar no topo da página.

10. Efetue as alterações necessárias para a sua conta e a configuração de sincronização de dados do SQL Server. (Para obter informações mais detalhadas, consulte o script de exemplo).

    1.  Informações do Azure.

    2.  Informações do grupo de sincronização.

    3.  Informações do OMS. Encontrar estas informações no Portal do OMS | Definições | Origens ligadas. Para obter mais informações sobre o envio de dados para análise de registos, consulte [enviar dados para análise de registos com a API de Recoletor de dados de HTTP (pré-visualização)](../log-analytics/log-analytics-data-collector-api.md).

11. Execute o runbook no painel de teste. Verifique para se certificar de que foi efetuada com êxito.

    Se tiverem erros, certifique-se de que tem o módulo do PowerShell mais recente instalado. Pode instalar o módulo PowerShell mais recente no **Galeria de módulos** na sua conta de automatização.

12. Clique em **publicar**

### <a name="schedule-the-runbook"></a>Agendamento do runbook

Agendar o runbook:

1.  No runbook, selecione o **agendas** separador em recursos.

2.  Selecione **adicionar uma agenda** na página de agendas.

3.  Selecione **associar um agendamento ao runbook**.

4.  Selecione **criar uma nova agenda.**

5.  Definir **periodicidade** periódica e defina o intervalo pretende. Utilize o mesmo intervalo aqui, no script e no OMS.

6.  Selecione **Criar**.

### <a name="check-the-automation"></a>Verificação de automatização

Para monitorizar se o seu automatização está em execução conforme esperado em **descrição geral** para a sua conta de automatização, localizar o **estatísticas de tarefa** ver em **monitorização**. Afixe esta vista para o seu dashboard para uma visualização mais fácil. É executado com êxito de mostrar o runbook como "Concluída" e falha executa mostra como "Falhado".

## <a name="create-a-log-analytics-reader-alert-for-email-notifications"></a>Criar um alerta de leitor de análise do registo para notificações por E-Mail

Para criar um alerta que utiliza a análise de registos, efetue os seguintes procedimentos. Como pré-requisito, tem de ter ligado com uma área de trabalho de análise de registos de análise de registos.

1.  No portal do OMS, selecione **pesquisa registo**.

2.  Crie uma consulta para selecionar os erros e avisos por grupo de sincronização dentro do intervalo que selecionou. Por exemplo:

    `Type=DataSyncLog\_CL LogLevel\_s!=Success| measure count() by SyncGroupName\_s interval 60minute`

3.  Depois de executar a consulta, selecione de sino que indica que **alerta**.

4.  Em **gerar um alerta com base no**, selecione **métrica medida**.

    1.  Defina o valor de agregação **maior**.

    2.  Depois de **maior**, introduza o limiar de decorrer antes de poder receber notificações. Erros transitórios são esperados sincronização de dados. Para reduzir o ruído, defina o limiar para 5.

5.  Em **ações**, defina **notificação por E-Mail** como "Yes". Introduza os destinatários de e-mail pretendido.

6.  Clique em **Guardar**. Os destinatários especificados agora recebem notificações por e-mail quando ocorrerem erros.

## <a name="create-an-oms-view-for-monitoring"></a>Criar uma vista do OMS para monitorização

Este passo cria uma vista do OMS monitorizar visualmente todos os grupos de sincronização especificada. A vista inclui vários componentes:

-   Um mosaico de descrição geral, que mostra quantos sucessos, erros e avisos tem todos os grupos de sincronização.

-   Um mosaico para todos os grupos de sincronização, que mostra a contagem de erros e avisos por grupo de sincronização. Grupos sem problemas não são apresentados neste mosaico.

-   Um mosaico para cada grupo de sincronização, que mostra o número de erros, êxitos e avisos e mensagens de erro recentes.

Para configurar a vista do OMS, efetue os seguintes procedimentos:

1.  Na home page do OMS, selecione o sinal de adição à esquerda para abrir o **estruturador de vistas**.

2.  Selecione **importação** na barra superior do estruturador de vistas. Em seguida, selecione o ficheiro de exemplo "DataSyncLogOMSView".

3.  A vista de exemplo é para gerir dois grupos de sincronização. Edite esta vista para que se adeque ao seu cenário. Clique em **editar** e efetue as seguintes alterações:

    1.  Crie novos objetos de "Lista e de anel" na galeria do conforme necessário.

    2.  Na cada peça de mosaico, Atualize as consultas com as suas informações.

        1.  Cada mosaico, altere o intervalo de TimeStamp_t conforme pretendido.

        2.  Nos mosaicos para cada grupo de sincronização, atualize os nomes de grupo de sincronização.

    3.  Cada mosaico, atualize o título, conforme necessário.

4.  Clique em **guardar** e a vista está pronta.

## <a name="cost-of-this-solution"></a>Custo desta solução

Na maioria dos casos, esta solução é gratuita.

**A automatização do Azure:** poderá existir um custo tarifas com a conta de automatização do Azure, dependendo da sua utilização. Os primeiro 500 minutos de tempo por mês de execução da tarefa são gratuitos. Na maioria dos casos, esta solução deverá utilizar menos de 500 minutos por mês. Para evitar encargos, agende o runbook seja executado um intervalo de duas horas ou mais. Para obter mais informações, consulte [automatização preços](https://azure.microsoft.com/pricing/details/automation/).

**Análise de registos:** poderá existir um custo associado a análise de registos, dependendo da sua utilização. O escalão gratuito inclui 500 MB de dados de transmissões em por dia. Na maioria dos casos, esta solução é esperada para a ingestão de menos de 500 MB por dia. Para reduzir a utilização, utilize ao só de falha de filtragem incluída no runbook. Se estiver a utilizar mais do que 500 MB por dia, atualize para o escalão pago para evitar o risco de análise parar quando for atingido o limite. Para obter mais informações, consulte [preços de análise de registos](https://azure.microsoft.com/pricing/details/log-analytics/).

## <a name="code-samples"></a>Exemplos de código

Transfira os exemplos de código descritos neste artigo das seguintes localizações:

-   [Runbook do PowerShell de registo de sincronização de dados](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Vista de análise de registo de sincronização de dados](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a Sincronização de Dados SQL, veja:

-   [Sincronizar dados em várias bases de dados na cloud e no local com a Sincronização de Dados SQL do Azure](sql-database-sync-data.md)
-   [Configurar a Sincronização de Dados SQL do Azure](sql-database-get-started-sql-data-sync.md)
-   [Melhores práticas da Sincronização de Dados SQL do Azure](sql-database-best-practices-data-sync.md)
-   [Resolver problemas da Sincronização de Dados SQL do Azure](sql-database-troubleshoot-data-sync.md)

-   Conclua os exemplos do PowerShell que mostram como configurar a Sincronização de Dados SQL:
    -   [Utilizar o PowerShell para sincronizar entre várias bases de dados SQL do Azure](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Utilizar o PowerShell para sincronizar entre uma Base de Dados SQL do Azure e uma base de dados do SQL Server no local](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Transferir a documentação da API REST da Sincronização de Dados SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Para obter mais informações sobre a Base de Dados SQL, veja:

-   [Descrição Geral da Base de Dados SQL](sql-database-technical-overview.md)
-   [Gestão do Ciclo de Vida da Base de Dados](https://msdn.microsoft.com/library/jj907294.aspx)
