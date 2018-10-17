---
title: Ativar o registo de diagnóstico para aplicações web no App Service do Azure
description: Saiba como ativar o registo de diagnósticos e adicionar instrumentação à sua aplicação, bem como aceder às informações registadas pelo Azure.
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.openlocfilehash: 5cd56abd02c55dbf72c92ed070f9988fae2b6762
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365259"
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Ativar o registo de diagnóstico para aplicações web no App Service do Azure
## <a name="overview"></a>Descrição geral
O Azure disponibiliza diagnósticos incorporados para ajudar a depurar uma [aplicação do serviço de aplicações web](http://go.microsoft.com/fwlink/?LinkId=529714). Neste artigo, saiba como ativar o registo de diagnósticos e adicionar instrumentação à sua aplicação, bem como aceder às informações registadas pelo Azure.

Este artigo utiliza a [portal do Azure](https://portal.azure.com), Azure PowerShell e a Interface de linha de comandos do Azure (CLI do Azure) para trabalhar com registos de diagnóstico. Para obter informações sobre como trabalhar com registos de diagnóstico com o Visual Studio, consulte [resolução de problemas do Azure no Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Diagnóstico do servidor Web e ao application diagnostics
Aplicações web do serviço de aplicações fornecem funcionalidade Diagnóstico de informações de registo do servidor web e a aplicação web. Estes são logicamente separados em **diagnóstico do servidor da web** e **diagnóstico de aplicação**.

### <a name="web-server-diagnostics"></a>Diagnóstico do servidor Web
Pode ativar ou desativar os seguintes tipos de registos:

* **Detalhadas de registo de erro** -erro informações detalhadas de códigos de estado HTTP que indicam uma falha (código de estado 400 ou superior). Pode conter informações que podem ajudar a determinar o motivo pelo qual o servidor devolveu o código de erro.
* **Falha do rastreio de pedido** -informações detalhadas sobre pedidos com falhas, incluindo um rastreamento dos componentes do IIS, usado para processar o pedido e o tempo decorrido em cada componente. É útil se está a tentar aumentar o desempenho do site ou isolar o que está a causar um erro HTTP específico a ser devolvido.
* **O registo do servidor da Web** -informações sobre transações de HTTP utilizando o [formato de ficheiro de registo expandido de W3C](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). É útil ao determinar geral métrica do site, tais como o número de pedidos processados ou o número de pedidos é de um endereço IP específico.

### <a name="application-diagnostics"></a>Application diagnostics
Diagnóstico de aplicação permite-lhe capturar informações produzidas por um aplicativo web. As aplicações ASP.NET podem utilizar o [Trace](http://msdn.microsoft.com/library/36hhw2t6.aspx) classe para registar informações no registo de diagnóstico de aplicação. Por exemplo:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

No tempo de execução, pode obter estes registos para ajudar a resolver problemas. Para obter mais informações, consulte [aplicações web de resolução de problemas do Azure no Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

Aplicações web do serviço de aplicações também registar informações de implementação quando publicar conteúdo para uma aplicação web. Isso acontece automaticamente e não há nenhuma definição de configuração para o registo de implementação. Registo de implementação permite-lhe determinar o motivo pelo qual uma implementação falhou. Por exemplo, se estiver a utilizar um script de implementação personalizado, poderá utilizar o registo de implementação para determinar por que o script está a falhar.

## <a name="enablediag"></a>Como ativar o diagnóstico
Para ativar o diagnóstico na [portal do Azure](https://portal.azure.com), vá para a página para a sua aplicação web e clique em **definições > os registos de diagnóstico**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Parte de registos](./media/web-sites-enable-diagnostic-log/logspart.png)

Quando ativa **diagnóstico de aplicação**, também é escolher o **nível**. Esta definição permite-lhe filtrar as informações capturadas para **informativa**, **aviso**, ou **erro** informações. Defini-la como **verboso** regista todas as informações produzidas pela aplicação.

> [!NOTE]
> Ao contrário de alterar o arquivo Web. config, ativar os diagnósticos de aplicação ou alterar os níveis de registo de diagnóstico não reciclar o domínio de aplicativo que o aplicativo é executado dentro.
>
>

Para **registo de aplicações**, pode ativar a opção de sistema de ficheiros temporariamente para fins de depuração. Esta opção se desligar automaticamente em 12 horas. Pode também ativar a opção de armazenamento de BLOBs para selecionar um contentor de BLOBs para escrever os registos.

Para **registo de servidor Web**, pode selecionar **armazenamento** ou **sistema de ficheiros**. Selecionando **armazenamento** permite-lhe selecionar uma conta de armazenamento e, em seguida, um contentor de BLOBs que os registos são escritos. 

Se armazenar os registos no sistema de ficheiros, os ficheiros podem ser acedidos por FTP ou transferidos como um arquivo Zip com o Azure PowerShell ou a Interface de linha de comandos do Azure (CLI do Azure).

Por predefinição, os registos não são automaticamente eliminados (com exceção do **registo de aplicação (sistema de ficheiros)**). Para eliminar automaticamente os registos, defina o **período de retenção (dias)** campo.

> [!NOTE]
> Se [regenerar chaves de acesso da conta de armazenamento](../storage/common/storage-create-storage-account.md), tem de repor a configuração de registo correspondente para utilizar as chaves atualizadas. Para efetuar este procedimento:
>
> 1. Na **configurar** separador, defina a funcionalidade de registo correspondentes **desativar**. Salve sua configuração.
> 2. Ative o registo para o blob da conta de armazenamento ou tabela novamente. Salve sua configuração.
>
>

Qualquer combinação de armazenamento de BLOBs, armazenamento de tabelas ou sistema de ficheiros pode ser ativada ao mesmo tempo e ter configurações de nível de registo individuais. Por exemplo, convém registar erros e avisos para armazenamento de BLOBs como uma solução de registo de longo prazo, ao ativar o registo do sistema de ficheiros com um nível de verbose.

Embora as mesmas informações básicas para eventos registrados, de fornecer todas as localizações de armazenamento de três **armazenamento de tabelas** e **armazenamento de BLOBs** registar informações adicionais, como o ID de instância, ID de thread e uma mais timestamp granular (formato de escala) que o registo para **sistema de ficheiros**.

> [!NOTE]
> Informações armazenadas na **armazenamento de tabelas** ou **armazenamento de BLOBs** só pode ser acedido através de um cliente de armazenamento ou um aplicativo que pode trabalhar diretamente com estes sistemas de armazenamento. Por exemplo, o Visual Studio 2013 contém um Explorador de armazenamento que podem ser utilizadas para explorar o armazenamento de tabela ou um blob e o HDInsight pode aceder a dados armazenados no armazenamento de Blobs. Também pode escrever uma aplicação que acede ao armazenamento do Azure ao utilizar uma da [SDKs do Azure](https://azure.microsoft.com/downloads/).
>
> [!NOTE]
> Também pode ser ativado o diagnóstico do Azure PowerShell com o **Set-AzureWebsite** cmdlet. Se não tiver instalado o Azure PowerShell, ou não tiver configurado para utilizar a sua subscrição do Azure, veja [instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.6.0).
>
>

## <a name="download"></a> Como: transferir registos
Informações de diagnóstico armazenadas para o sistema de ficheiros de aplicação web podem ser acessadas diretamente com o FTP. Também pode ser transferido como um arquivo Zip com o Azure PowerShell ou a Interface de linha de comandos do Azure.

Segue-se a estrutura de diretório que os registos são armazenados em:

* **Registos de aplicações** -/LogFiles/aplicação /. Esta pasta contém um ou mais ficheiros de texto que contém informações produzidas pelo registo de aplicações.
* **Falha de rastreios de pedido** -/ LogFiles/W3SVC # # # /. Esta pasta contém um arquivo XSL e um ou mais arquivos XML. Certifique-se de que baixe o arquivo XSL no mesmo diretório como o ficheiro (s) o XML como o arquivo XSL fornece funcionalidade de formatação e filtrar o conteúdo dos ficheiros pelo XML quando visualizado no Internet Explorer.
* **Registos de erros de detalhadas** -/LogFiles/DetailedErrors /. Esta pasta contém um ou mais arquivos. htm que fornecem informações abrangentes para quaisquer erros HTTP que ocorreram.
* **Os registos do servidor da Web** -/LogFiles/http/RawLogs. Esta pasta contém uma ou mais ficheiros de texto formatado utilizando o [formato de ficheiro de registo expandido de W3C](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Logs de implantação** -/ LogFiles/Git. Esta pasta contém registos gerados pelos processos de implantação interna utilizados por aplicações web do Azure, bem como registos para implementações do Git. Também pode encontrar registos de implementação em D:\home\site\deployments.

### <a name="ftp"></a>FTP

Para abrir uma ligação de FTP para o servidor FTP da sua aplicação, consulte [implementar a aplicação no serviço de aplicações do Azure com FTP/S](app-service-deploy-ftp.md).

Assim que estiver ligado ao servidor FTP/S da sua aplicação web, abra a **LogFiles** pasta, onde são armazenados os ficheiros de registo.

### <a name="download-with-azure-powershell"></a>Transferir com o Azure PowerShell
Para transferir os ficheiros de registo, inicie uma nova instância do Azure PowerShell e utilize o seguinte comando:

    Save-AzureWebSiteLog -Name webappname

Este comando guarda os registos da aplicação web especificado pela **-Name** parâmetro para um arquivo chamado **logs.zip** no diretório atual.

> [!NOTE]
> Se não tiver instalado o Azure PowerShell, ou não tiver configurado para utilizar a sua subscrição do Azure, veja [instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.6.0).
>
>

### <a name="download-with-azure-command-line-interface"></a>Transferir com Interface de linha de comandos do Azure
Para transferir os ficheiros de registo utilizando a Interface de linha de comandos do Azure, abra uma nova linha de comandos, o PowerShell, o Bash ou a sessão de Terminal e introduza o seguinte comando:

    az webapp log download --resource-group resourcegroupname --name webappname

Este comando guarda os registos da aplicação web com o nome webappname num ficheiro denominado **diagnostics.zip** no diretório atual.

> [!NOTE]
> Se não tiver instalado a Interface de linha de comandos do Azure (CLI) do Azure, ou não tiver configurado para utilizar a sua subscrição do Azure, veja [como utilizar o Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Como: ver registos no Application Insights
O Visual Studio Application Insights fornece ferramentas para filtragem e pesquisa de registos e para correlacionar os registos com pedidos e outros eventos.

1. Adicione o Application Insights SDK ao seu projeto no Visual Studio.
   * No Solution Explorer, clique no projeto e escolha adicionar o Application Insights. A interface orienta-o pelos passos que incluem a criação de um recurso do Application Insights. [Saiba mais](../application-insights/app-insights-asp-net.md)
2. Adicione o pacote de serviço de escuta de rastreamento para seu projeto.
   * Clique no projeto e selecione gerir pacotes NuGet. Selecione `Microsoft.ApplicationInsights.TraceListener` [Saiba mais](../application-insights/app-insights-asp-net-trace-logs.md)
3. Carregue seu projeto e execute-o para gerar dados de registo.
4. Na [portal do Azure](https://portal.azure.com/), navegue para o novo recurso do Application Insights e abra **pesquisa**. Deverá ver os dados de registo, juntamente com o pedido, a utilização e outra telemetria. Alguma telemetria poderá demorar alguns minutos a chegar: clique em Atualizar. [Saiba mais](../application-insights/app-insights-diagnostic-search.md)

[Saiba mais sobre o desempenho de controlo com o Application Insights](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a> Como: Stream registos
Ao desenvolver um aplicativo, geralmente é útil ver informações de registo em tempo quase real. Pode transmitir informações de registo para o seu ambiente de desenvolvimento com o Azure PowerShell ou a Interface de linha de comandos do Azure.

> [!NOTE]
> Alguns tipos de memória intermédia de registo de escrita para o ficheiro de registo, que pode resultar em eventos fora de ordem no fluxo. Por exemplo, uma entrada de registo de aplicação que ocorre quando um usuário visitar uma página pode ser apresentada no fluxo antes da entrada de registo HTTP correspondente para o pedido de página.
>
> [!NOTE]
> Também a transmissão de registos transmite informações gravadas em qualquer arquivo de texto armazenado no **D:\\doméstica\\LogFiles\\**  pasta.
>
>

### <a name="streaming-with-azure-powershell"></a>Transmissão em fluxo com o Azure PowerShell
Para transmitir informações de registo, iniciar uma nova instância do Azure PowerShell e utilize o seguinte comando:

    Get-AzureWebSiteLog -Name webappname -Tail

Este comando liga-se para a aplicação web especificada pelos **-nome** parâmetro e iniciar a transmissão em fluxo de informações na janela do PowerShell como ocorrem eventos de registo na aplicação web. Todas as informações de escrita em ficheiros terminados em. txt,. log ou. htm que estão armazenados no diretório /LogFiles (d: / home/logfiles) são transmitidas para a consola local.

Para filtrar eventos específicos, como erros, utilize o **-mensagem** parâmetro. Por exemplo:

    Get-AzureWebSiteLog -Name webappname -Tail -Message Error

Para filtrar os tipos de registo específicos, como HTTP, utilize o **-caminho** parâmetro. Por exemplo:

    Get-AzureWebSiteLog -Name webappname -Tail -Path http

Para ver uma lista de caminhos disponíveis, utilize o parâmetro - ListPath.

> [!NOTE]
> Se não tiver instalado o Azure PowerShell, ou não tiver configurado para utilizar a sua subscrição do Azure, veja [como utilizar o Azure PowerShell](http://azure.microsoft.com/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="streaming-with-azure-command-line-interface"></a>Transmissão em fluxo com a Interface de linha de comandos do Azure
Para transmitir informações de registo, abra uma nova linha de comandos, o PowerShell, o Bash ou a sessão de Terminal e introduza o seguinte comando:

    az webapp log tail --name webappname --resource-group myResourceGroup

Este comando liga-se para a aplicação web com o nome 'webappname' e começar a transmissão em fluxo de informações na janela à medida que ocorrem eventos de registo na aplicação web. Todas as informações de escrita em ficheiros terminados em. txt,. log ou. htm que estão armazenados no diretório /LogFiles (d: / home/logfiles) são transmitidas para a consola local.

Para filtrar eventos específicos, como erros, utilize o **– filtro** parâmetro. Por exemplo:

    az webapp log tail --name webappname --resource-group myResourceGroup --filter Error

Para filtrar os tipos de registo específicos, como HTTP, utilize o **– caminho** parâmetro. Por exemplo:

    az webapp log tail --name webappname --resource-group myResourceGroup --path http

> [!NOTE]
> Se não tiver instalado a Interface de linha de comandos do Azure, ou não tiver configurado para utilizar a sua subscrição do Azure, veja [como para utilizar o Azure de Interface de linha](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a> Como: compreender os registos de diagnóstico
### <a name="application-diagnostics-logs"></a>Registos de diagnóstico de aplicações
Diagnóstico de aplicação armazena informações num formato específico para aplicações de .NET, dependendo se armazena os registos para o sistema de ficheiros, o armazenamento de tabela ou o armazenamento de Blobs. O conjunto base de dados armazenados é o mesmo em todos os três tipos de armazenamento - a data e hora que ocorreu o evento, o ID de processo que produziu o evento, o tipo de evento (informação, aviso, erro) e a mensagem de evento.

**Sistema de Ficheiros**

Cada linha com sessão iniciada no sistema de arquivos ou recebidas usando a transmissão em fluxo é o seguinte formato:

    {Date}  PID[{process ID}] {event type/level} {message}

Por exemplo, um evento de erro seria ter um aspeto semelhante ao exemplo seguinte:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

O registo para o sistema de ficheiros fornece as informações mais básicas dos três métodos disponíveis, fornecendo apenas o tempo, ID de processo, nível do evento e mensagem.

**Armazenamento de tabelas**

Ao fazer logon no armazenamento de tabelas, as propriedades adicionais são utilizadas para simplificar a pesquisa os dados armazenados na tabela, bem como informações mais detalhadas sobre o evento. As seguintes propriedades (colunas) são utilizadas para cada entidade (linha) armazenada na tabela.

| Nome da propriedade | Formato do valor |
| --- | --- |
| PartitionKey |Data/hora do evento no formato de yyyyMMddHH |
| RowKey |Um valor GUID que identifica exclusivamente esta entidade |
| Carimbo de data/hora |A data e hora em que ocorreu o evento |
| EventTickCount |A data e hora em que ocorreu o evento, no formato de escala (maior precisão) |
| ApplicationName |O nome da aplicação web |
| Nível |Nível do evento (por exemplo, erro, aviso, informações) |
| EventId |O ID do evento deste evento<p><p>Predefinição é 0 se nenhum especificado |
| Id da Instância |Instância da aplicação web que ocorreu o mesmo no |
| PID |ID de Processo |
| NIF |O ID de thread do thread que produziu o evento |
| Mensagem |Mensagem de detalhes do evento |

**Armazenamento de blobs**

Ao iniciar sessão para o armazenamento de BLOBs, os dados são armazenados no formato de valores separados por vírgulas (CSV). Semelhante ao armazenamento de tabelas, campos adicionais são registados para fornecer informações mais detalhadas sobre o evento. As seguintes propriedades são usadas para cada linha no CSV:

| Nome da propriedade | Formato do valor |
| --- | --- |
| Date |A data e hora em que ocorreu o evento |
| Nível |Nível do evento (por exemplo, erro, aviso, informações) |
| ApplicationName |O nome da aplicação web |
| Id da Instância |Instância da aplicação web que ocorreu o evento |
| EventTickCount |A data e hora em que ocorreu o evento, no formato de escala (maior precisão) |
| EventId |O ID do evento deste evento<p><p>Predefinição é 0 se nenhum especificado |
| PID |ID de Processo |
| NIF |O ID de thread do thread que produziu o evento |
| Mensagem |Mensagem de detalhes do evento |

Os dados armazenados num blob teria uma aparência semelhantes ao seguinte exemplo:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> A primeira linha do registo contém os cabeçalhos de coluna, como ilustrado neste exemplo.
>
>

### <a name="failed-request-traces"></a>Falha de rastreios de pedido
Rastreios de pedido falhado são armazenados nos arquivos XML chamados **. XML de # # # fr**. Para que seja mais fácil ver as informações com sessão iniciada, uma folha de estilos XSL com o nome **freb.xsl** é fornecido no mesmo diretório que os arquivos XML. Se abrir um dos arquivos XML no Internet Explorer, o Internet Explorer usa a folha de estilos XSL para fornecer uma apresentação formatada das informações de rastreio, semelhantes ao seguinte exemplo:

![pedidos falhados visualizado no navegador da](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Registos de erros detalhados
Registos de erros detalhados são documentos HTML que fornecem informações mais detalhadas sobre os erros HTTP que ocorreram. Uma vez que eles são simplesmente os documentos HTML, eles podem ser exibidos num navegador da web.

### <a name="web-server-logs"></a>Registos de servidores Web
Os registos do servidor web são formatados com o [formato de ficheiro de registo expandido de W3C](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Estas informações podem ser lidos com um editor de texto ou analisado com o uso, como utilitários [Log Parser](http://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> Os registos produzidos por aplicações web do Azure não suportam o **s-computername**, **s-ip**, ou **cs-version** campos.
>
>

## <a name="nextsteps"></a> Passos seguintes
* [Como monitorizar aplicações Web](web-sites-monitor.md)
* [Resolução de problemas de aplicações web do Azure no Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analisar registos de aplicações web no HDInsight](http://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Se pretender começar a utilizar o App Service do Azure antes de se inscrever numa conta do Azure, aceda a [Experimentar o App Service](https://azure.microsoft.com/try/app-service/), onde pode criar de imediato uma aplicação Web de arranque de curta duração no App Service. Sem cartões de crédito; sem compromissos.
>
>
