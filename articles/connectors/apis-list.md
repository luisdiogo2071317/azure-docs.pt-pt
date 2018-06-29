---
title: Conectores para o Azure Logic Apps | Microsoft Docs
description: Automatizar fluxos de trabalho com incorporada, geridos API, no local, a conta de integração e conectores de enterprise para o Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 06/29/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 2bb3e2ce29037372395aa0b30e9f76f3e712667d
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096616"
---
# <a name="connectors-for-azure-logic-apps"></a>Conectores para aplicações lógicas do Azure

Conectores reproduzir uma parte integral quando criar fluxos de trabalho automatizados com Azure Logic Apps. Ao utilizar conectores nas suas logic apps, expandir as capacidades no local e nuvem aplicações para efetuar tarefas com os dados que criar e já tiver. Enquanto as Logic Apps ofertas ~ 200 + conectores, este artigo descreve populares e mais frequentemente utilizados conectores que são utilizados com êxito pelo milhares de aplicações e milhões de execuções de processamento de dados e as informações.
Os conectores estão disponíveis como built-ins ou conetores geridos. 

* [**Built-ins**](#built-ins): estas ações incorporadas e ajuda acionadores criar as logic apps, que são executadas em agendamentos personalizados, comunicar com outros pontos finais, receber e responder a pedidos e chamar Azure funções, Azure API Apps (aplicações Web), os seus próprios APIs geridas e aplicações que podem receber pedidos publicadas com a API Management do Azure e lógica aninhada. Também pode utilizar incorporado ações que o ajudam a organizar e controlar o fluxo de trabalho da sua aplicação lógica e também funcionam com dados.

* **Conetores geridos**: Estes conectores fornecem acionadores e ações para aceder a outros sistemas e serviços. Alguns conectores requerem que se crie primeiro ligações que são geridas pelo Azure Logic Apps. Os conetores geridos são organizados nestes grupos:

  |   |   |
  |---|---|
  | [**Conetores geridos de API**](#managed-api-connectors) | Crie aplicações lógicas que utilizam serviços como o Blob Storage do Azure, Office 365, Dynamics, Power BI, OneDrive, Salesforce, SharePoint Online e muitos mais. | 
  | [**Conectores no local**](#on-premises-connectors) | Depois de instalar e configurar o [gateway de dados no local][gateway-doc], ajuda estes conectores o acesso de aplicações lógicas sistemas no local, como o SQL Server, SharePoint Server, Oracle DB, partilhas de ficheiros e outros utilizadores. | 
  | [**Conectores de conta de integração**](#integration-account-connectors) | Disponível quando criar e pagar para uma conta de integração, estes transformação de conectores e validar XML, codificar e descodificar ficheiros simples e processar empresa-empresa (B2B) mensagens com AS2, EDIFACT e X12 protocolos. | 
  | [**Conetores do Enterprise**](#enterprise-connectors) | Fornece acesso a sistemas empresariais, como o SAP e IBM MQ um custo adicional. |
  ||| 

  Por exemplo, se estiver a utilizar o Microsoft BizTalk Server, as logic apps podem ligar a e comunicar com o BizTalk Server, utilizando o [conector BizTalk Server](#on-premises-connectors). 
  Em seguida, pode expandir ou efetuar operações como o BizTalk nas suas logic apps utilizando o [conectores de integração do conta](#integration-account-connectors). 

Para obter informações técnicas sobre cada conector acionadores e ações, que são definidas por uma descrição do Swagger, além de limites, consulte [detalhes do conector](/connectors/). Para informações de custo, consulte [Logic Apps detalhes de preços](https://azure.microsoft.com/pricing/details/logic-apps/) e [Logic Apps do modelo de preços](../logic-apps/logic-apps-pricing.md). 

<a name="built-ins"></a>

## <a name="built-ins"></a>Incorporações

As Logic Apps fornece incorporadas acionadores e ações para que possa criar fluxos de trabalho baseadas na agenda, ajudam as logic apps comunicar com outras aplicações e serviços, controlo de fluxo de trabalho através das logic apps e gerir ou manipular dados. 

|   |   |   |   | 
|---|---|---|---| 
| [![Ícone de API][schedule-icon]<br/>**agenda**][recurrence-doc] | -Executar a sua aplicação lógica numa agenda especificada, entre básico e recurrences complexas, com o **periodicidade** acionador. <p>-Interromper a sua aplicação lógica para uma duração especificada com o **atraso** ação. <p>-Interromper a sua aplicação lógica até que a data especificada e a hora com a **atraso até** ação. | [![Ícone de API][http-icon]<br/>**HTTP**][http-doc] | Comunicar com qualquer ponto final através de HTTP com acionadores e ações para HTTP, HTTP + Swagger e HTTP + Webhook. | 
| [![Ícone de API][http-request-icon]<br/>**pedido**][http-request-doc] | -Se a aplicação lógica possível chamar EndRead de outras aplicações ou serviços, acionador em eventos de recursos de grelha de evento ou acionador em respostas aos alertas do Centro de segurança do Azure com o **pedido** acionador. <p>-Enviar as respostas a uma aplicação ou serviço com o **resposta** ação. | [![Ícone de API][batch-icon]<br/>**Batch**][batch-doc] | -Processar mensagens em lotes com o **Batch mensagens** acionador. <p>-Chamada as logic apps que tenham existente batch acionadores com o **enviar mensagens para batch** ação. | 
| [![Ícone de API][azure-functions-icon]<br/>**das funções do Azure**][azure-functions-doc] | Chame as funções do Azure que executam os fragmentos de código personalizado (c# ou Node.js) das logic apps. | [![Ícone de API][azure-api-management-icon]</br>**API Management do Azure**][azure-api-management-doc] | Invocar acionadores e ações definidas pela sua própria que gerir e publicar APIs com API Management do Azure. | 
| [![Ícone de API][azure-app-services-icon]<br/>**serviços de aplicações do Azure**][azure-app-services-doc] | Chamada de Azure API Apps, ou as aplicações Web, alojada no App Service do Azure. Os acionadores e ações definidas por essas aplicações aparecem como quaisquer outros primeira classe acionadores e ações quando Swagger está incluído. | [![Ícone de API][azure-logic-apps-icon]<br/>**Azure<br/>Logic Apps**][nested-logic-app-doc] | Chame outras as logic apps que começam com um acionador pedido. | 
||||| 

### <a name="control-workflow"></a>Fluxo de trabalho de controlo

Seguem-se as ações incorporadas para structuring e controlar as ações no fluxo de trabalho da sua aplicação lógica:

|   |   |   |   | 
|---|---|---|---| 
| [![Ícone incorporada][condition-icon]<br/>**condição**][condition-doc] | Avaliar uma condição e executadas ações diferentes com base em se a condição for true ou false. | [![Ícone incorporada][for-each-icon]</br>**para cada**][for-each-doc] | Efetue as mesmas ações em todos os itens numa matriz. | 
| [![Ícone incorporada][scope-icon]<br/>**âmbito**][scope-doc] | Grupo ações em *âmbitos*, qual obter os seus próprios estado depois das ações no âmbito termine a execução. | [![Ícone incorporada][switch-icon]</br>**comutador**][switch-doc] | Grupo ações em *casos*, que são atribuídos valores exclusivos, exceto o cenário de predefinição. Execute apenas nesse caso cujo valor atribuído corresponde o resultado de uma expressão, objetos ou token. Se existirem não corresponde a executar o caso de predefinição. | 
| [![Ícone incorporada][terminate-icon]<br/>**terminar**][terminate-doc] | Pare um fluxo de trabalho de aplicação de lógica ativamente em execução. | [![Ícone incorporada][until-icon]<br/>**até**][until-doc] | Repita ações até que a condição especificada é verdadeira ou algum Estado é alterado. | 
||||| 

### <a name="manage-or-manipulate-data"></a>Gerir ou manipular dados

Seguem-se as ações incorporadas para trabalhar com dados saídas e os respetivos formatos:  

|   |   | 
|---|---| 
| ![Ícone incorporada][data-operations-icon]<br/>**Operações de dados** | Efetue operações com dados: <p>- **Componha**: criar um resultado único a partir de várias entradas com vários tipos. <br>- **Criar a tabela CSV**: criar uma tabela de valores separados por vírgulas (CSV) a partir de uma matriz com objetos JSON. <br>- **Criar a tabela HTML**: criar uma tabela HTML a partir de uma matriz com objetos JSON. <br>- **Matriz de filtro**: criar uma matriz de itens na matriz outro que satisfazem os critérios. <br>- **Associar**: criar uma cadeia de todos os itens numa matriz e separe os itens com o delimitador especificado. <br>- **Analisar JSON**: criar tokens amigável de utilizador de propriedades e os respetivos valores no JSON conteúdo, para que possa utilizar essas propriedades no fluxo de trabalho. <br>- **Selecione**: criar uma matriz com objetos JSON transformar itens ou valores na matriz da outra e esses itens de mapeamento para as propriedades especificadas. | 
| ![Ícone incorporada][date-time-icon]<br/>**Data hora** | Efetue operações com carimbos: <p>- **Adicionar a hora**: adicionar o número especificado de unidades para um carimbo. <br>- **Converter o fuso horário**: converter um carimbo do fuso horário de origem para o fuso horário de destino. <br>- **Hora atual**: devolver o timestamp atual como uma cadeia. <br>- **Obter intervalos de tempo futuros**: devolver o timestamp atual e as unidades de tempo especificado. <br>- **Obter passado tempo**: devolver o timestamp atual menos as unidades de tempo especificado. <br>- **Subtrair tempo**: subtrair um número de unidades de tempo de um carimbo. |
| [![Ícone incorporada][variables-icon]<br/>**variáveis**][variables-doc] | Efetue operações com variáveis: <p>- **Acrescentar a variável de matriz**: Inserir um valor como último item de uma matriz armazenada por uma variável. <br>- **Acrescentar a variável de cadeia**: Inserir um valor como o último caráter numa cadeia armazenada por uma variável. <br>- **Variável de diminuir**: diminuir uma variável por um valor constante. <br>- **Variável de incremento**: aumentar uma variável por um valor constante. <br>- **Inicializar a variável**: criar uma variável e declarar o respetivo tipo de dados e o valor inicial. <br>- **Definir variável**: atribuir um valor diferente para uma variável existente. |
|  |  | 

<a name="managed-api-connectors"></a>

## <a name="managed-api-connectors"></a>Conetores geridos de API

Seguem-se os conectores mais populares para automatizar tarefas, processos e fluxos de trabalho com estes sistemas ou de serviços:

|   |   |   |   | 
|---|---|---|---| 
| [![Ícone de API][azure-service-bus-icon]<br/>**Service Bus do Azure**][azure-service-bus-doc] | Gerir mensagens assíncronas, sessões e subscrições de tópico com o conector frequentemente utilizada no Logic Apps. | [![Ícone de API][sql-server-icon]<br/>**do SQL Server**][sql-server-doc] | Liga ao SQL Server no local ou uma base de dados do SQL do Azure na nuvem pelo que pode gerir os registos, execução de procedimentos armazenados ou efetuar consultas. | 
| [![Ícone de API][office-365-outlook-icon]<br/>**do Office 365<br/>Outlook**][office-365-outlook-doc] | Liga à sua conta de e-mail do Office 365 para que possa criar e gerir os e-mails, tarefas, eventos de calendário e reuniões, contactos, pedidos e muito mais. | [![Ícone de API][azure-blob-storage-icon]<br/>**Blob do Azure<br/>armazenamento**][azure-blob-storage-doc] | Liga à sua conta de armazenamento para que possa criar e gerir conteúdo de blob. | 
| [![Ícone de API][sftp-icon]<br/>**SFTP**][sftp-doc] | Ligar a servidores SFTP que podem aceder a partir da internet para que possa trabalhar com os seus ficheiros e pastas. | [![Ícone de API][sharepoint-online-icon]<br/>**SharePoint<br/>Online**][sharepoint-online-doc] | Ligar-se ao SharePoint Online, para que consiga gerir ficheiros, os anexos, pastas e muito mais. | 
| [![Ícone de API][dynamics-365-icon]<br/>**Dynamics 365<br/>CRM Online**][dynamics-365-doc] | Liga à sua conta de Dynamics 365 para que possa criar e gerir registos, itens e muito mais. | [![Ícone de API][ftp-icon]<br/>**FTP**][ftp-doc] | Ligar a servidores FTP que podem aceder a partir da internet para que possa trabalhar com os seus ficheiros e pastas. | 
| [![Ícone de API][salesforce-icon]<br/>**Salesforce**][salesforce-doc] | Liga à sua conta do Salesforce, para que possa criar e gerir itens, tais como registos, tarefas, objetos e muito mais. | [![Ícone de API][twitter-icon]<br/>**Twitter**][twitter-doc] | Liga à sua conta do Twitter para que consiga gerir tweets, followers, a linha cronológica e muito mais. Guarde os tweets SQL, o Excel ou o SharePoint. | 
| [![Ícone de API][azure-event-hubs-icon]<br/>**Event Hubs do Azure**][azure-event-hubs-doc] | Consumir e publicar eventos através de um Hub de eventos. Por exemplo, obter o resultado da sua aplicação lógica com os Event Hubs e, em seguida, envie que de saída para um fornecedor de análise em tempo real. | [![Ícone de API][azure-event-grid-icon]<br/>**Azure eventos**</br>**grelha**][azure-event-grid-doc] | Monitorizar eventos publicados por um grelha de eventos, por exemplo, quando a alteração de recursos do Azure ou recursos de terceiros. | 
||||| 

<a name="on-premises-connectors"></a>

## <a name="on-premises-connectors"></a>Conectores no local 

Seguem-se algumas conectores frequentemente utilizados, que fornecem acesso a dados e recursos nos sistemas no local. Antes de poder criar uma ligação a um sistema local, tem primeiro [transferir, instalar e configurar um gateway de dados no local][gateway-doc]. Este gateway disponibiliza um canal de comunicação segura sem ter de configurar a infraestrutura de rede necessário. 

|   |   |   |   |   | 
|---|---|---|---|---| 
| ![Ícone de API][biztalk-server-icon]<br/>**BizTalk**</br> **Servidor** | [![Ícone de API][file-system-icon]<br/>**ficheiro</br> sistema**][file-system-doc] | [![Ícone de API][ibm-db2-icon]<br/>**IBM DB2**][ibm-db2-doc] | [![Ícone de API][ibm-informix-icon]<br/>**IBM** </br> **Informix**][ibm-informix-doc] | ![Ícone de API][mysql-icon]<br/>**MySQL** | 
| [![Ícone de API][oracle-db-icon]<br/>**Oracle DB**][oracle-db-doc] | ![Ícone de API][postgre-sql-icon]<br/>**PostgreSQL** | [![Ícone de API][sharepoint-server-icon]<br/>**SharePoint</br> servidor**][sharepoint-server-doc] | [![Ícone de API][sql-server-icon]<br/>**SQL</br> servidor**][sql-server-doc] | ![Ícone de API][teradata-icon]<br/>**Teradata** | 
||||| 

<a name="integration-account-connectors"></a>

## <a name="integration-account-connectors"></a>Conectores de conta de integração 

Seguem-se os conectores para criar soluções de empresa-empresa (B2B) com as logic apps quando criar e pague por um [conta integração](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), que está disponível através do Enterprise Integration Pack (EIP) no Azure. Com esta conta, pode criar e armazenar artefactos B2B como comerciais parceiros, contratos, mapas, esquemas, certificados e assim sucessivamente. Para utilizar estes artefactos, associe as logic apps com a sua conta de integração. Se utilizar atualmente o BizTalk Server, estes conectores podem parecer familiares já.

|   |   |   |   | 
|---|---|---|---| 
| [![Ícone de API][as2-icon]<br/>**AS2</br> descodificação**][as2-decode-doc] | [![Ícone de API][as2-icon]<br/>**AS2</br> codificação**][as2-encode-doc] | [![Ícone de API][edifact-icon]<br/>**EDIFACT</br> descodificação**][edifact-decode-doc] | [![Ícone de API][edifact-icon]<br/>**EDIFACT</br> codificação**][edifact-encode-doc] | 
| [![Ícone de API][flat-file-decode-icon]<br/>**ficheiro simples</br> descodificação**][flat-file-decode-doc] | [![Ícone de API][flat-file-encode-icon]<br/>**ficheiro simples</br> codificação**][flat-file-encode-doc] | [![Ícone de API][integration-account-icon]<br/>**integração<br/>conta**][integration-account-doc] | [![Ícone de API][liquid-icon]<br/>**Liquid**</br>**transforma**][json-liquid-transform-doc] | 
| [![Ícone de API][x12-icon]<br/>**X12</br> descodificação**][x12-decode-doc] | [![Ícone de API][x12-icon]<br/>**X12</br> codificação**][x12-encode-doc] | [![Ícone de API][xml-transform-icon]<br/>**XML**</br>**transforma**][xml-transform-doc] | [![Ícone de API][xml-validate-icon]<br/>**XML <br/>validação**][xml-validate-doc] |  
||||| 

<a name="enterprise-connectors"></a>

## <a name="enterprise-connectors"></a>Conectores empresariais

As logic apps podem aceder a sistemas empresariais, como o SAP e IBM MQ:

|   |   | 
|---|---| 
| [![Ícone de API][ibm-mq-icon]<br/>**IBM MQ**][ibm-mq-doc] | [![Ícone de API][sap-icon]<br/>**SAP**][sap-connector-doc] |
||| 

## <a name="more-about-triggers-and-actions"></a>Mais sobre acionadores e ações

Alguns conectores fornecem *acionadores* que notificar a sua aplicação lógica quando ocorrem eventos específicos. Por isso, quando estes eventos ocorrem, o acionador cria e executa uma instância da sua aplicação lógica. Por exemplo, o conector FTP fornece um acionador de "quando um ficheiro é adicionado ou modificado" que começa a sua aplicação lógica, quando um ficheiro for atualizado. 

As Logic Apps fornece estes tipos de acionadores:  

* *Acionadores de consulta*: estes acionadores consultam o serviço uma frequência especificada e verificações para novos dados. 

  Quando estão disponíveis novos dados, uma nova instância da sua aplicação lógica é criada e executa com os dados que seja transmitidos como entrada. 

* *Push acionadores*: estes acionadores estão à escuta para novos dados de um ponto final ou para um evento ocorrer, que cria e executa a nova instância da sua aplicação lógica.

* *Acionador de recorrência*: este acionador cria e executa uma instância da sua aplicação lógica com base numa agenda especificada.

Os conectores também fornecem *ações* que realizam tarefas no fluxo de trabalho da sua aplicação lógica. Por exemplo, a aplicação lógica pode ler os dados e utilizar estes dados frente da sua aplicação lógica. Mais especificamente, a aplicação lógica pode localizar dados de cliente a partir de uma base de dados do SQL Server e processar dados mais à frente no fluxo de trabalho da sua aplicação lógica. 

Para mais informações sobre os acionadores e ações, consulte o [descrição geral de conectores](connectors-overview.md). 

## <a name="custom-apis-and-connectors"></a>APIs personalizadas e conectores 

Para chamar as APIs que executar código personalizado ou não estão disponíveis como conectores, pode expandir a plataforma de Logic Apps por [criar aplicações API personalizadas](../logic-apps/logic-apps-create-api-app.md). Também pode [criar conectores personalizados](../logic-apps/custom-connector-overview.md) para *qualquer* REST ou APIs baseado em SOAP, que disponibiliza as APIs para qualquer aplicação lógica na sua subscrição do Azure.
Para tornar personalizado API Apps ou conectores público para a utilizar no Azure, pode [submeter conectores para certificação Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Para submeter ou votar em ideias para Azure Logic Apps e conectores, visite o [site de comentários do utilizador Logic Apps](http://aka.ms/logicapps-wish).

* São docs falta artigos ou detalhes que pensa que são importantes? Se Sim, pode ajudar ao adicionar os artigos existentes ou ao escrever os seus próprios. A documentação é open source e alojada no GitHub. Introdução à documentação do Azure [repositório do GitHub](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Passos Seguintes

* [Criar a sua primeira aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Criar conectores personalizados para as logic apps](https://docs.microsoft.com/connectors/custom-connectors/)
* [Criar APIs personalizadas para aplicações lógicas](../logic-apps/logic-apps-create-api-app.md)

<!--Misc doc links-->
[gateway-doc]: ../logic-apps/logic-apps-gateway-connection.md "Ligar a origens de dados no local a partir de aplicações lógicas com gateway de dados no local"

<!--Built-ins doc links-->
[azure-functions-doc]: ../logic-apps/logic-apps-azure-functions.md "Integrar aplicações lógicas com Funções do Azure"
[azure-api-management-doc]: ../api-management/get-started-create-service-instance.md "Criar uma instância de serviço de API Management do Azure para gerir e publicar as suas APIs"
[azure-app-services-doc]: ../logic-apps/logic-apps-custom-hosted-api.md "Integrar aplicações lógicas com Aplicações API do Serviço de Aplicações"
[azure-service-bus-doc]: ./connectors-create-api-servicebus.md "Envie mensagens das Filas e dos Tópicos do Service Bus e receba mensagens das Filas e das Subscrições do Service Bus"
[batch-doc]: ../logic-apps/logic-apps-batch-process-send-receive-messages.md "Processar mensagens no grupos, ou lotes"
[condition-doc]: ../logic-apps/logic-apps-control-flow-conditional-statement.md "Avaliar uma condição e executadas ações diferentes com base em se a condição for true ou false"
[delay-doc]: ./connectors-native-delay.md "Efetue ações atrasadas"
[for-each-doc]: ../logic-apps/logic-apps-control-flow-loops.md#foreach-loop "Efetuar as mesmas ações em todos os itens numa matriz"
[http-doc]: ./connectors-native-http.md "Faça chamadas HTTP com o conector HTTP"
[http-request-doc]: ./connectors-native-reqres.md "Adicione ações para pedidos HTTP e respostas para as suas aplicações lógicas"
[http-response-doc]: ./connectors-native-reqres.md "Adicione ações para pedidos HTTP e respostas para as suas aplicações lógicas"
[http-swagger-doc]: ./connectors-native-http-swagger.md "Faça chamadas HTTP com o conector HTTP + Swagger."
[http-webook-doc]: ./connectors-native-webhook.md "Adicione as ações de webhook HTTP e acionadores para as logic apps"
[nested-logic-app-doc]: ../logic-apps/logic-apps-http-endpoint.md "Integre aplicações lógicas com fluxos de trabalho aninhados"
[query-doc]: ./connectors-native-query.md "Selecione e filtre matrizes com a ação Consultar"
[recurrence-doc]:  ./connectors-native-recurrence.md "Acione ações periódicas para aplicações lógicas"
[scope-doc]: ../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md "Organizar ações em grupos, o qual obtenham os seus próprios estado depois das ações no grupo termine a execução" 
[switch-doc]: ../logic-apps/logic-apps-control-flow-switch-statement.md "Organize ações em casos, o que são atribuídos valores exclusivos. Executar apenas as maiúsculas e minúsculas cujo valor corresponde o resultado de uma expressão, objetos ou token. Se não existem correspondências existem, execute o caso de predefinido"
[terminate-doc]: ../logic-apps/logic-apps-workflow-actions-triggers.md#terminate-action "Parar ou cancelar a um fluxo de trabalho ativamente em execução para a sua aplicação lógica"
[until-doc]: ../logic-apps/logic-apps-control-flow-loops.md#until-loop "Repetir ações até que a condição especificada é verdadeira ou algum Estado é alterado"
[variables-doc]: ../logic-apps/logic-apps-create-variables-store-values.md "Efetuar operações com variáveis, tais como inicialização, conjunto, incremento, diminuir e acrescentar a variável de cadeia ou matriz"

<!--Managed API doc links-->
[azure-blob-storage-doc]: ./connectors-create-api-azureblobstorage.md "Gerir ficheiros no seu contentor de blobs com o conector de armazenamento de blobs do Azure"
[azure-event-grid-doc]: ../event-grid/monitor-virtual-machine-changes-event-grid-logic-app.md " Monitorizar eventos publicados por um grelha de eventos, por exemplo, quando a alteração de recursos do Azure ou recursos de terceiros"
[azure-event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Ligue aos Hubs de Eventos do Azure. Receba e envie eventos entre aplicações lógicas e os Hubs de Eventos"
[box-doc]: ./connectors-create-api-box.md "Ligue à Caixa. Carregue, obtenha, elimine, liste os seus ficheiros e muito mais"
[dropbox-doc]: ./connectors-create-api-dropbox.md "Ligue à Dropbox. Carregue, obtenha, elimine, liste os seus ficheiros e muito mais"
[dynamics-365-doc]: ./connectors-create-api-crmonline.md "Ligue ao Dynamics CRM Online para que possa trabalhar com os seus dados do CRM Online"
[facebook-doc]: ./connectors-create-api-facebook.md "Ligue ao Facebook. Publique numa linha cronológica, obtenha um feed de página, entre outras coisas"
[file-system-doc]: ../logic-apps/logic-apps-using-file-connector.md "Ligue a um sistema de ficheiros no local"
[ftp-doc]: ./connectors-create-api-ftp.md "Ligue a um servidor FTP / FTPS e para tarefas de FTP, como carregar, obter, eliminar ficheiros, entre outras"
[github-doc]: ./connectors-create-api-github.md "Ligue o GitHub e controle problemas"
[google-calendar-doc]: ./connectors-create-api-googlecalendar.md "Liga ao Calendário Google e pode gerir o calendário."
[google-drive-doc]: ./connectors-create-api-googledrive.md "Ligue ao GoogleDrive para que possa trabalhar com os seus dados"
[google-sheets-doc]: ./connectors-create-api-googlesheet.md "Liga ao folhas do Google, pelo que pode modificar as suas folhas de"
[google-tasks-doc]: ./connectors-create-api-googletasks.md "Ligue o Google Tarefas para que possa gerir as suas tarefas"
[ibm-db2-doc]: ./connectors-create-api-db2.md "Ligue ao IBM DB2 na cloud ou no local. Atualize uma linha, obtenha uma tabela e muito mais"
[ibm-informix-doc]: ./connectors-create-api-informix.md "Ligue o Informix na cloud ou no local. Leia uma linha, liste as tabelas e muito mais"
[ibm-mq-doc]: ./connectors-create-api-mq.md "Ligar para o IBM MQ no local ou no Azure para enviar e receber mensagens"
[instagram-doc]: ./connectors-create-api-instagram.md "Ligue ao Instagram. Acione ou interaja em eventos"
[mailchimp-doc]: ./connectors-create-api-mailchimp.md "Ligue à sua conta do MailChimp. Gira e automatize e-mails"
[mandrill-doc]: ./connectors-create-api-mandrill.md "Ligue ao Mandrill para comunicação"
[microsoft-translator-doc]: ./connectors-create-api-microsofttranslator.md "Ligue-se ao Microsoft Translator. Traduza texto, detete idiomas e muito mais" 
[office-365-outlook-doc]: ./connectors-create-api-office365-outlook.md "Ligue à sua conta do Office 365. Envie e receba e-mails, gira o seu calendário e contactos, e muito mais"
[office-365-users-doc]: ./connectors-create-api-office365-users.md 
[office-365-video-doc]: ./connectors-create-api-office365-video.md "Obtenha informações de vídeo, listas de vídeos e canais, e URLs de reprodução para vídeos do Office 365"
[onedrive-doc]: ./connectors-create-api-onedrive.md "Ligue ao seu Microsoft OneDrive pessoal. Carregue, elimine, liste os ficheiros e muito mais"
[onedrive-for-business-doc]: ./connectors-create-api-onedriveforbusiness.md "Ligue ao seu Microsoft OneDrive empresarial. Carregue, elimine, liste os seus ficheiros e muito mais"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Ligue a uma base de dados Oracle para adicionar, inserir, eliminar linhas e mais"
[outlook.com-doc]: ./connectors-create-api-outlook.md "Ligue à sua caixa de correio do Outlook. Gira o seu e-mail, calendários, contactos e muito mais"
[project-online-doc]: ./connectors-create-api-projectonline.md "Ligue-se ao Microsoft Project Online. Gira os seus projetos, tarefas, recursos e muito mais"
[rss-doc]: ./connectors-create-api-rss.md "Publique e obtenha itens de feeds, acione operações quando um novo item é publicado num feed RSS."
[salesforce-doc]: ./connectors-create-api-salesforce.md "Ligue à sua conta do Salesforce. Gira contas, responsáveis, oportunidades e muito mais"
[sap-connector-doc]: ../logic-apps/logic-apps-using-sap-connector.md "Ligue a um sistema SAP no local"
[sendgrid-doc]: ./connectors-create-api-sendgrid.md "Ligue ao SendGrid. Enviar correio eletrónico e gerir apresenta uma lista de destinatário"
[sftp-doc]: ./connectors-create-api-sftp.md "Ligue à sua conta do SFTP. Carregue, obtenha, elimine ficheiros e muito mais"
[sharepoint-server-doc]: ./connectors-create-api-sharepointserver.md "Ligue ao servidor do SharePoint no local. Gira documentos, liste itens e muito mais"
[sharepoint-online-doc]: ./connectors-create-api-sharepointonline.md "Ligue ao SharePoint Online. Gira documentos, liste itens e muito mais"
[slack-doc]: ./connectors-create-api-slack.md "Ligue ao Slack e publica mensagens nos canais do Slack"
[smtp-doc]: ./connectors-create-api-smtp.md "Ligar a um servidor de SMTP e enviar correio eletrónico com anexos"
[sparkpost-doc]: ./connectors-create-api-sparkpost.md "Ligue ao SparkPost para comunicação"
[sql-server-doc]: ./connectors-create-api-sqlazure.md "Ligar à base de dados SQL do Azure ou SQL Server. Crie, atualize, obtenha e elimine entradas numa tabela da Base de Dados SQL."
[trello-doc]: ./connectors-create-api-trello.md "Ligue ao Trello. Gira os seus projetos e organize tudo com qualquer pessoa"
[twilio-doc]: ./connectors-create-api-twilio.md "Ligue ao Twilio. Envie e receba mensagens, obtenha números disponíveis, gira números de telefone de chamadas recebidas, entre outras ações"
[twitter-doc]: ./connectors-create-api-twitter.md "Ligue ao Twitter. Obtenha linhas cronológicas, publique tweets e muito mais"
[wunderlist-doc]: ./connectors-create-api-wunderlist.md "Ligue ao Wunderlist. Gira tarefas e listas de itens pendentes, mantenha a sua vida em sincronização e muito mais"
[yammer-doc]: ./connectors-create-api-yammer.md "Ligue ao Yammer. Publique mensagens, obtenha novas mensagens e muito mais"
[youtube-doc]: ./connectors-create-api-youtube.md "Ligue ao YouTube. Gira os seus vídeos e canais"

<!--Enterprise Intregation Pack doc links-->
[as2-doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Saiba mais sobre a integração empresarial AS2."
[as2-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Saiba mais sobre a integração empresarial de descodificação AS2."
[as2-encode-doc]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Saiba mais sobre a integração empresarial de codificação AS2."
[edifact-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Saiba mais sobre a integração empresarial de descodificação EDIFACT."
[edifact-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Saiba mais sobre a integração empresarial de codificação EDIFACT."
[flat-file-decode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Saiba mais sobre a integração empresarial de ficheiros simples."
[flat-file-encode-doc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Saiba mais sobre a integração empresarial de ficheiros simples."
[integration-account-doc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Procure esquemas, mapas, parceiros e mais na sua conta de integração"
[json-liquid-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-liquid-transform.md "Saiba mais sobre transformações de JSON com Liquid"
[x12-doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Saiba mais sobre a integração empresarial X12."
[x12-decode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Saiba mais sobre a integração empresarial de descodificação X12."
[x12-encode-doc]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Saiba mais sobre a integração empresarial de codificação X12."
[xml-transform-doc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Saiba mais sobre a integração empresarial de transformações."
[xml-validate-doc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Saiba mais sobre a integração empresarial de validação XML."

<!-- Built-ins icons -->
[azure-api-management-icon]: ./media/apis-list/azure-api-management.png
[azure-app-services-icon]: ./media/apis-list/azure-app-services.png
[azure-functions-icon]: ./media/apis-list/azure-functions.png
[azure-logic-apps-icon]: ./media/apis-list/azure-logic-apps.png
[batch-icon]: ./media/apis-list/batch.png
[condition-icon]: ./media/apis-list/condition.png
[data-operations-icon]: ./media/apis-list/data-operations.png
[date-time-icon]: ./media/apis-list/date-time.png
[for-each-icon]: ./media/apis-list/for-each-loop.png
[http-icon]: ./media/apis-list/http.png
[http-request-icon]: ./media/apis-list/request.png
[http-response-icon]: ./media/apis-list/response.png
[http-swagger-icon]: ./media/apis-list/http-swagger.png
[http-webhook-icon]: ./media/apis-list/http-webhook.png
[schedule-icon]: ./media/apis-list/recurrence.png
[scope-icon]: ./media/apis-list/scope.png
[switch-icon]: ./media/apis-list/switch.png
[terminate-icon]: ./media/apis-list/terminate.png
[until-icon]: ./media/apis-list/until.png
[variables-icon]: ./media/apis-list/variables.png

<!--Managed API icons-->
[appfigures-icon]: ./media/apis-list/appfigures.png
[asana-icon]: ./media/apis-list/asana.png
[azure-automation-icon]: ./media/apis-list/azure-automation.png
[azure-blob-storage-icon]: ./media/apis-list/azure-blob-storage.png
[azure-cognitive-services-text-analytics-icon]: ./media/apis-list/azure-cognitive-services-text-analytics.png
[azure-data-lake-icon]: ./media/apis-list/azure-data-lake.png
[azure-document-db-icon]: ./media/apis-list/azure-document-db.png
[azure-event-grid-icon]: ./media/apis-list/azure-event-grid.png
[azure-event-grid-publish-icon]: ./media/apis-list/azure-event-grid-publish.png
[azure-event-hubs-icon]: ./media/apis-list/azure-event-hubs.png
[azure-ml-icon]: ./media/apis-list/azure-ml.png
[azure-queues-icon]: ./media/apis-list/azure-queues.png
[azure-resource-manager-icon]: ./media/apis-list/azure-resource-manager.png
[azure-service-bus-icon]: ./media/apis-list/azure-service-bus.png
[basecamp-3-icon]: ./media/apis-list/basecamp.png
[bitbucket-icon]: ./media/apis-list/bitbucket.png
[bitly-icon]: ./media/apis-list/bitly.png
[biztalk-server-icon]: ./media/apis-list/biztalk.png
[blogger-icon]: ./media/apis-list/blogger.png
[box-icon]: ./media/apis-list/box.png
[campfire-icon]: ./media/apis-list/campfire.png
[common-data-service-icon]: ./media/apis-list/common-data-service.png
[dropbox-icon]: ./media/apis-list/dropbox.png
[dynamics-365-icon]: ./media/apis-list/dynamics-crm-online.png
[dynamics-365-financials-icon]: ./media/apis-list/dynamics-365-financials.png
[dynamics-365-operations-icon]: ./media/apis-list/dynamics-365-operations.png
[easy-redmine-icon]: ./media/apis-list/easyredmine.png
[facebook-icon]: ./media/apis-list/facebook.png
[file-system-icon]: ./media/apis-list/file-system.png
[ftp-icon]: ./media/apis-list/ftp.png
[github-icon]: ./media/apis-list/github.png
[google-calendar-icon]: ./media/apis-list/google-calendar.png
[google-drive-icon]: ./media/apis-list/google-drive.png
[google-sheets-icon]: ./media/apis-list/google-sheet.png
[google-tasks-icon]: ./media/apis-list/google-tasks.png
[hipchat-icon]: ./media/apis-list/hipchat.png
[ibm-db2-icon]: ./media/apis-list/ibm-db2.png
[ibm-informix-icon]: ./media/apis-list/ibm-informix.png
[ibm-mq-icon]: ./media/apis-list/ibm-mq.png
[insightly-icon]: ./media/apis-list/insightly.png
[instagram-icon]: ./media/apis-list/instagram.png
[instapaper-icon]: ./media/apis-list/instapaper.png
[jira-icon]: ./media/apis-list/jira.png
[mailchimp-icon]: ./media/apis-list/mailchimp.png
[mandrill-icon]: ./media/apis-list/mandrill.png
[microsoft-translator-icon]: ./media/apis-list/microsoft-translator.png
[mysql-icon]: ./media/apis-list/mysql.png
[office-365-outlook-icon]: ./media/apis-list/office-365.png
[office-365-users-icon]: ./media/apis-list/office-365-users.png
[office-365-video-icon]: ./media/apis-list/office-365-video.png
[onedrive-icon]: ./media/apis-list/onedrive.png
[onedrive-for-business-icon]: ./media/apis-list/onedrive-business.png
[oracle-db-icon]: ./media/apis-list/oracle-db.png
[outlook.com-icon]: ./media/apis-list/outlook.png
[pagerduty-icon]: ./media/apis-list/pagerduty.png
[pinterest-icon]: ./media/apis-list/pinterest.png
[postgre-sql-icon]: ./media/apis-list/postgre-sql.png
[project-online-icon]: ./media/apis-list/projecton-line.png
[redmine-icon]: ./media/apis-list/redmine.png
[rss-icon]: ./media/apis-list/rss.png
[salesforce-icon]: ./media/apis-list/salesforce.png
[sap-icon]: ./media/apis-list/sap.png
[send-grid-icon]: ./media/apis-list/sendgrid.png
[sftp-icon]: ./media/apis-list/sftp.png
[sharepoint-online-icon]: ./media/apis-list/sharepoint-online.png
[sharepoint-server-icon]: ./media/apis-list/sharepoint-server.png
[slack-icon]: ./media/apis-list/slack.png
[smartsheet-icon]: ./media/apis-list/smartsheet.png
[smtp-icon]: ./media/apis-list/smtp.png
[sparkpost-icon]: ./media/apis-list/sparkpost.png
[sql-server-icon]: ./media/apis-list/sql.png
[teradata-icon]: ./media/apis-list/teradata.png
[todoist-icon]: ./media/apis-list/todoist.png
[trello-icon]: ./media/apis-list/trello.png
[twilio-icon]: ./media/apis-list/twilio.png
[twitter-icon]: ./media/apis-list/twitter.png
[vimeo-icon]: ./media/apis-list/vimeo.png
[visual-studio-team-services-icon]: ./media/apis-list/visual-studio-team-services.png
[wordpress-icon]: ./media/apis-list/wordpress.png
[wunderlist-icon]: ./media/apis-list/wunderlist.png
[yammer-icon]: ./media/apis-list/yammer.png
[youtube-icon]: ./media/apis-list/youtube.png

<!-- Enterprise Integration Pack icons -->
[as2-icon]: ./media/apis-list/as2.png
[edifact-icon]: ./media/apis-list/edifact.png
[flat-file-encode-icon]: ./media/apis-list/flat-file-encoding.png
[flat-file-decode-icon]: ./media/apis-list/flat-file-decoding.png
[integration-account-icon]: ./media/apis-list/integration-account.png
[liquid-icon]: ./media/apis-list/liquid-transform.png
[x12-icon]: ./media/apis-list/x12.png
[xml-validate-icon]: ./media/apis-list/xml-validation.png
[xml-transform-icon]: ./media/apis-list/xsl-transform.png