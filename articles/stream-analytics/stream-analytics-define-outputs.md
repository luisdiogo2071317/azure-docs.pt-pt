---
title: Compreender as saídas do Azure Stream Analytics
description: Este artigo descreve as opções de saída de dados disponível no Azure Stream Analytics, incluindo o Power BI para resultados de análise.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/11/2018
ms.openlocfilehash: 030af72951e226d3484706e627bc8b74d5469670
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Compreender as saídas do Azure Stream Analytics
Este artigo descreve os diferentes tipos de saídas disponíveis para uma tarefa do Azure Stream Analytics. Saídas permitem-lhe armazenar e guardar os resultados da tarefa de Stream Analytics. Utilizar os dados de saída, pode fazê-lo ainda mais análise de negócio e dados do armazém de dados. 

Ao conceber a sua consulta do Stream Analytics, consulte o nome da utilização de saída a [cláusula INTO](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics). Pode utilizar um resultado por tarefa única ou várias saídas por transmissão em fluxo de trabalho se for necessário, fornecendo vários cláusulas INTO na consulta.

Para criar, editar e testar a tarefa do Stream Analytics produz, pode utilizar o [portal do Azure](stream-analytics-quick-create-portal.md#configure-output-to-the-job), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.Net API](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [REST API](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-output), e [Visual Studio](stream-analytics-tools-for-visual-studio.md).

Suporte de tipos algumas saídas [criação de partições](#partitioning), e [tamanhos de batch de saída](#output-batch-size) variar para otimizar o débito.


## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Suporta análise de sequência [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). O Azure Data Lake Store é um repositório de hiper escala a nível da empresa para cargas de trabalho de análise de macrodados. Arquivo data Lake permite-lhe armazenar dados de qualquer tamanho, tipo e velocidade de ingestão para análises exploratórias e operacionais. Do Stream Analytics tem de estar autorizado a aceder ao Data Lake Store.

### <a name="authorize-an-azure-data-lake-store-account"></a>Autorizar uma conta do Azure Data Lake Store

1. Quando o armazenamento do Data Lake é selecionado como uma saída no portal do Azure, lhe for pedido para autorizar uma ligação a um Data Lake Store existente.  

   ![Autorizar o arquivo Data Lake](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

2. Se já tiver acesso ao Data Lake Store, selecione **autorizar agora** e uma página aparece indicar **redirecionar para autorização**. Depois de autorização é concluída com êxito, é-lhe apresentada a página que lhe permite configurar a saída do Data Lake Store.

3. Depois de ter a conta de Data Lake Store autenticada, pode configurar as propriedades para a saída do Data Lake Store. A tabela abaixo é a lista de nomes de propriedade e a respetiva descrição para configurar a saída do Data Lake Store.

   ![Autorizar o arquivo Data Lake](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

| Nome da propriedade | Descrição | 
| --- | --- |
| Alias de saída | Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para este arquivo Data Lake. | 
| Nome da conta | O nome da conta de armazenamento do Data Lake onde está a enviar o resultado. É-lhe apresentada uma lista pendente de contas do Data Lake Store que estão disponíveis na sua subscrição. |
| Padrão do prefixo do caminho | O caminho do ficheiro utilizado para escrever ficheiros dentro do arquivo de conta especificado do Data Lake. Pode especificar um ou mais instâncias das {date} e {time} variáveis.</br><ul><li>Exemplo 1: pasta1/logs / {date} / {time}</li><li>Exemplo 2: pasta1/logs / {date}</li></ul>Se o padrão do caminho de ficheiro não contém à direita "/", o último padrão no caminho de ficheiro é tratado como um prefixo de nome de ficheiro. </br></br>Nestas circunstâncias, os novos ficheiros são criados:<ul><li>Alteração do esquema de saída</li><li>Reinicie o interno ou externo de uma tarefa.</li></ul> |
| Formato de data | Opcional. Se o token de data é utilizado no caminho de prefixo, pode selecionar o formato da data em que os ficheiros estão organizados. Exemplo: DD/MM/DD |
|Formato de hora | Opcional. Se o token de tempo é utilizado no caminho de prefixo, especifique o formato de hora em que os ficheiros estão organizados. Atualmente, o único valor suportado é HH. |
| Formato de serialização de eventos | Formato de serialização para dados de saída. JSON, CSV e Avro são suportados.| 
| Codificação | Se utilizar formato CSV ou JSON, uma codificação tem de ser especificada. UTF-8 é o único formato de codificação suportado neste momento.|
| Delimitador | Só é aplicável a serialização de CSV. Stream Analytics suporta um número de delimitadores comuns para serializar os dados CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical.|
| Formato | Só é aplicável a serialização do JSON. Separadas por linhas Especifica que o resultado é formatado, fazendo com que cada objeto JSON separado por uma nova linha. A matriz Especifica que o resultado é formatado como uma matriz de objetos JSON. Esta matriz está fechada apenas quando o deixa de tarefa do Stream Analytics tem mover ou para a próxima janela de tempo. Em geral, é preferível a utilizar a linha de valores separados por JSON, uma vez que não requer qualquer processamento especial enquanto ainda está a ser escrito no ficheiro de saída para.|

### <a name="renew-data-lake-store-authorization"></a>Renove a autorização de Data Lake Store
Terá de voltar a sua conta do Data Lake Store, se a palavra-passe tiver sido alterado desde que a tarefa foi criada ou pela última vez autenticada. Se não a reautenticação, a tarefa não produziu resultados de saída e mostra um erro que indica a necessidade de reautorização nos registos de operação. Atualmente, não há uma limitação em que o token de autenticação tem de ser atualizados manualmente todos os 90 dias para todas as tarefas com a saída do Data Lake Store. 

Para renovar a autorização, **parar** a tarefa > vá para a saída do Data Lake Store > clique o **renove a autorização** associar e, por um breve período de tempo a página irá aparecer que indica **redirecionar para autorização...** . A página fecha-se automaticamente e se tiver êxito, indica **autorização foi renovada com êxito**. Em seguida, tem de clicar em **guardar** na parte inferior da página, pode proceder ao reiniciar a tarefa a partir do **hora da última paragem** para evitar a perda de dados.

![Autorizar o arquivo Data Lake](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Base de dados SQL do Azure](https://azure.microsoft.com/services/sql-database/) pode ser utilizado como uma saída de dados que natureza relacional ou para as aplicações que dependem de conteúdo que está a ser alojado numa base de dados relacional. Tarefas do Stream Analytics escrevem uma tabela existente numa base de dados SQL do Azure.  O esquema de tabela deve corresponder exatamente os campos e os respetivos tipos que está a ser resultado da tarefa. Um [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) também podem ser especificados como uma saída através da opção de saída da base de dados SQL, bem como. A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída de base de dados SQL.

| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para esta base de dados. |
| Base de Dados | O nome da base de dados em que está a enviar o resultado. |
| Nome do servidor | O nome do servidor de base de dados SQL. |
| Nome de utilizador | O nome de utilizador, que tem acesso ao escrever o databas. |
| Palavra-passe | A palavra-passe para estabelecer ligação com o databas.e |
| Tabela | O nome da tabela em que o resultado é escrito. O nome da tabela é sensível e o esquema desta tabela deve corresponder exatamente ao número de campos e os respetivos tipos gerados pelo seu resultado da tarefa. |

> [!NOTE]
> Atualmente, a oferta da SQL Database do Azure é suportada para uma saída da tarefa no Stream Analytics. No entanto, uma máquina de Virtual do Azure com o SQL Server com uma base de dados ligada não é suportada. Trata-se sujeita a alterações em futuros lançamentos.
> 

## <a name="blob-storage"></a>Armazenamento de blobs
O blob storage oferece uma solução económica e dimensionável para armazenar grandes quantidades de dados não estruturados na nuvem.  Para uma introdução no Blob storage do Azure e a respetiva utilização, consulte a documentação em [como utilizar Blobs](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída de blob.

| Nome da propriedade | Descrição | 
| --- | --- |
| Alias de saída | Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para o armazenamento de Blobs. |
| Conta de Armazenamento | O nome da conta do storage onde está a enviar o resultado. |
| Chave da Conta de Armazenamento | A chave secreta associada à conta de armazenamento. |
| Contentor de armazenamento | Contentores fornecem um agrupamento lógico blobs armazenados no Microsoft serviço Blob do Azure. Quando carregar um blob para o serviço Blob, tem de especificar um contentor para esse blob. |
| Padrão do caminho | Opcional. O padrão do caminho de ficheiro utilizado para escrever os blobs no contentor especificado. </br></br> O padrão de caminho, pode optar por utilizar uma ou mais instâncias das variáveis do tempo de data para especificar a frequência com que os blobs são escritos: </br> {date}, {time} </br> </br>Também pode especificar um nome personalizado {campo} dos seus dados de eventos para particionar blobs pelo, onde o nome do campo seja alfanumérico e pode incluir espaços, hífenes e carateres de sublinhado. Restrições em campos personalizados incluem o seguinte: <ul><li>Caso insensitivity (não é possível diferente entre a coluna "ID" e coluna "id")</li><li>Campos aninhados não são permitidos (em vez disso, utilize um alias de consulta da tarefa para "aplanar" o campo)</li><li>As expressões não podem ser utilizadas como um nome de campo</li></ul>Exemplos: <ul><li>Exemplo 1: cluster1/logs / {date} / {time}</li><li>Exemplo 2: cluster1/logs / {date}</li><li>Exemplo 3: cluster1 / {client_id} / {date} / {time}</li><li>Exemplo 4: cluster1 / {myField} em que a consulta é: data.myField SELECIONE como myField de entrada;</li></ul><BR> Atribuição de nome de ficheiro, segue-se a seguinte convenção: </br> {Caminho prefixo Pattern}/schemaHashcode_Guid_Number.extension </br></br> Ficheiros de saída de exemplo: </br><ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li><li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul><br/>
| Formato de data | Opcional. Se o token de data é utilizado no caminho de prefixo, pode selecionar o formato da data em que os ficheiros estão organizados. Exemplo: DD/MM/DD |
| Formato de hora | Opcional. Se o token de tempo é utilizado no caminho de prefixo, especifique o formato de hora em que os ficheiros estão organizados. Atualmente, o único valor suportado é HH. |
| Formato de serialização de eventos | Formato de serialização para dados de saída.  JSON, CSV e Avro são suportados.
| Codificação | Se utilizar formato CSV ou JSON, uma codificação tem de ser especificada. UTF-8 é o único formato de codificação suportado neste momento. |
| Delimitador | Só é aplicável a serialização de CSV. Stream Analytics suporta um número de delimitadores comuns para serializar os dados CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical. |
| Formato | Só é aplicável a serialização do JSON. Separadas por linhas Especifica que o resultado é formatado, fazendo com que cada objeto JSON separado por uma nova linha. A matriz Especifica que o resultado é formatado como uma matriz de objetos JSON. Esta matriz está fechada apenas quando o deixa de tarefa do Stream Analytics tem mover ou para a próxima janela de tempo. Em geral, é preferível a utilizar a linha de valores separados por JSON, uma vez que não requer qualquer processamento especial enquanto ainda está a ser escrito no ficheiro de saída para. |

Ao utilizar o armazenamento de BLOBs como resultado, é criado um novo ficheiro no blob nos seguintes casos:

* Se o ficheiro excede o número máximo de blocos permitidos (atualmente 50 000). O número máximo permitido de blocos pode aceder sem atingir o tamanho máximo de blob permitidos. Por exemplo, se a taxa de saída é elevada, pode ver bytes mais por blocos e o tamanho do ficheiro é maior. Se a taxa de saída é baixa, cada bloco tem menos de dados e o tamanho do ficheiro é mais pequeno.
* Se existir uma alteração de esquema no resultado, e o formato de saída requer o esquema fixo (CSV e Avro).  
* Se uma tarefa for reiniciada, externamente por um utilizador pará-lo e ao iniciá-los, ou internamente para a recuperação de manutenção ou erro de sistema.  
* Se a consulta completamente estar particionada, é criado novo ficheiro para cada partição de saída.  
* Se um ficheiro ou de um contentor da conta de armazenamento é eliminado pelo utilizador.  
* Se o resultado é tempo particionado com o padrão de prefixo do caminho, um novo blob é utilizado quando a consulta é movido para a hora seguinte.
* Se a saída particionada um campo personalizado, um novo blob é criado por uma chave de partição se não existir.
*   Se a saída particionada um campo personalizado onde a cardinalidade de chave de partição excede 8000, um novo blob pode ser criado por uma chave de partição.

## <a name="event-hub"></a>Hub de Eventos
O [Event Hubs do Azure](https://azure.microsoft.com/services/event-hubs/) serviço é altamente dimensionável de publicação-subscrição ingestor de eventos. -Pode recolher milhões de eventos por segundo. Uma utilização de um Hub de eventos como saída é quando o resultado de uma tarefa de Stream Analytics torna-se a entrada de outra tarefa de transmissão em fluxo.

Existem alguns parâmetros que são necessários para configurar os fluxos de dados de Hub de eventos como uma saída.

| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída | Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para o Hub de eventos. |
| Espaço de nomes do Hub de Eventos |Um espaço de nomes do Hub de eventos é um contentor para um conjunto de entidades de mensagens. Quando tiver criado um novo Hub de eventos, também criou um espaço de nomes do Hub de eventos. |
| O nome do hub de eventos | O nome do seu Hub de eventos de saída. |
| Nome da política do Hub de Eventos | A política de acesso partilhado, que pode ser criada no separador Configurar do Hub de eventos. Cada política de acesso partilhado tem um nome, as permissões que define e chaves de acesso. |
| Chave da política do Hub de Eventos | A chave de acesso partilhado utilizada para autenticar o acesso ao espaço de nomes de Hub de eventos. |
| Coluna de chave de partição [opcional] | Esta coluna contém a chave de partição do Hub de eventos de saída. |
| Formato de serialização de eventos | Formato de serialização para dados de saída.  JSON, CSV e Avro são suportados. |
| Codificação | Para o CSV e JSON, UTF-8 é o único formato de codificação suportado neste momento. |
| Delimitador | Só é aplicável a serialização de CSV. O Stream Analytics suporta um número de delimitadores comuns para serializar dados no formato CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical. |
| Formato | Só é aplicável a serialização do JSON. Separadas por linhas Especifica que o resultado é formatado, fazendo com que cada objeto JSON separado por uma nova linha. A matriz Especifica que o resultado é formatado como uma matriz de objetos JSON. Esta matriz está fechada apenas quando o deixa de tarefa do Stream Analytics tem mover ou para a próxima janela de tempo. Em geral, é preferível a utilizar a linha de valores separados por JSON, uma vez que não requer qualquer processamento especial enquanto ainda está a ser escrito no ficheiro de saída para. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) pode ser utilizado como uma saída de uma tarefa de Stream Analytics para fornecer uma experiência avançada de visualização de resultados de análise. Esta capacidade pode ser utilizada para dashboards operacionais, a geração de relatórios e métrica orientadas por relatório.

### <a name="authorize-a-power-bi-account"></a>Autorizar uma conta do Power BI
1. Quando o Power BI é selecionada como uma saída no portal do Azure, são-lhe pedido para autorizar um utilizador de BI energia existente ou criar uma nova conta do Power BI.  
   
   ![Autorizar o utilizador do Power BI](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2. Crie uma nova conta se não, mas tiver um, clique em autorizar agora.  É apresentada a seguinte página:
   
   ![Conta do Azure Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3. Neste passo, forneça a conta escolar ou profissional para autorizar a saída do Power BI. Se tiver não terminado já sessão para o Power BI, escolha agora o início de sessão cópias de segurança. A conta escolar ou profissional que utiliza para o Power BI pode ser diferente da conta de subscrição do Azure, o qual tem sessão iniciada com.

### <a name="configure-the-power-bi-output-properties"></a>Configurar as propriedades de saída do Power BI
Depois de ter a conta do Power BI autenticada, pode configurar as propriedades para a saída do Power BI. A tabela abaixo é a lista de nomes de propriedade e respetiva descrição para configurar a saída do Power BI.

| Nome da propriedade | descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para este resultado de PowerBI. |
| Área de trabalho de Grupo |Para ativar a partilha de dados com outros utilizadores do Power BI pode selecionar os grupos no interior da sua conta do Power BI ou escolha "A minha área de trabalho" se não pretender escrever a um grupo.  Atualizar um grupo existente exige renovar a autenticação do Power BI. |
| Nome do conjunto de dados |Forneça um nome de conjunto de dados que pretender para a saída do Power BI utilizar |
| Nome da tabela |Forneça um nome de tabela em que o conjunto de dados de saída do Power BI. Atualmente, saída do Power BI de tarefas do Stream Analytics só pode ter uma tabela num conjunto de dados |

Para uma passagem de configurar um Power BI saída e o dashboard, consulte o [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) artigo.

> [!NOTE]
> Não explicitamente crie o conjunto de dados e a tabela no dashboard do Power BI. O conjunto de dados e a tabela é preenchida automaticamente quando a tarefa é iniciada e a tarefa é iniciada a saída de bombagem no Power BI. Tenha em atenção que se a consulta da tarefa não gerar resultados, o conjunto de dados e tabela não é criada. Tenha em atenção que, se o Power BI já tinha um conjunto de dados e uma tabela com o mesmo nome que aquele fornecido nesta tarefa de Stream Analytics, os dados existentes for substituídos.
> 

### <a name="schema-creation"></a>Criação de esquema
O Azure Stream Analytics cria um conjunto de dados do Power BI e a tabela em nome do utilizador, se ainda não existir. Noutros casos, a tabela é atualizada com novos valores. Atualmente, não há uma limitação que apenas uma tabela pode existir num conjunto de dados.

### <a name="data-type-conversion-from-stream-analytics-to-power-bi"></a>Tipo de dados de conversão do Stream Analytics para o Power BI
O Azure Stream Analytics atualiza o modelo de dados dinâmica no tempo de execução se altera o esquema de saída. Todas as alterações de nome de coluna, alterações de tipos de coluna e a adição ou remoção de colunas são registadas.

Esta tabela aborda os conversões de tipo de dados de [tipos de dados do Stream Analytics](https://msdn.microsoft.com/library/azure/dn835065.aspx) para Power BIs [tipos de modelo de dados de entidade (EDM)](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) se um conjunto de dados do POWER BI e a tabela não existe.

Partir do Stream Analytics | Para o Power BI
-----|-----|------------
bigint | Int64
nvarchar (Max) | Cadeia
datetime | Datetime
flutuante | duplo
Matriz de registo | Cadeia de tipo, o valor constante "IRecord" ou "IArray"

### <a name="schema-update"></a>Atualização do esquema
Do Stream Analytics infere o esquema do modelo de dados com base no primeiro conjunto de eventos no resultado. Mais tarde, se necessário, o esquema do modelo de dados é atualizado para acomodar os eventos de entrada que não podem ajustar o esquema original.

O `SELECT *` consulta deve ser evitada para impedir a atualização do esquema dinâmico em linhas. Para além das implicações de desempenho potencial, também pode resultar num uncertainty de tempo decorrido para os resultados. Devem selecionar os campos exatos que precisam de ser mostrada no dashboard do Power BI. Além disso, os valores de dados devem ser compatíveis com o tipo de dados escolhidas.


Anterior/atual | Int64 | Cadeia | Datetime | duplo
-----------------|-------|--------|----------|-------
Int64 | Int64 | Cadeia | Cadeia | duplo
duplo | duplo | Cadeia | Cadeia | duplo
Cadeia | Cadeia | Cadeia | Cadeia |  | Cadeia | 
Datetime | Cadeia | Cadeia |  Datetime | Cadeia


### <a name="renew-power-bi-authorization"></a>Renovar autorização do Power BI
Se a palavra-passe de conta do Power BI é alterado depois da tarefa de Stream Analytics foi criada ou pela última vez autenticada, terá de voltar a Stream Analytics. Se o multi-factor Authentication (MFA) é configurado no seu inquilino do Azure Active Directory (AAD), terá também de renovação da autorização do Power BI em duas semanas. Um sintoma de que este problema é nenhum resultado da tarefa e um "erro do utilizador de autenticar" nos registos de operação:

  ![Erro de token de atualização do Power BI](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Para resolver este problema, parar a tarefa em execução e avance para a saída do Power BI.  Selecione o **renovar autorização** ligação e reinicie a tarefa do **hora da última paragem** para evitar a perda de dados.

  ![Power BI é renovada de autorização](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Armazenamento de Tabelas
[Table storage do Azure](../storage/common/storage-introduction.md) oferece um armazenamento de elevada disponibilidade e extremamente dimensionável, para que uma aplicação possa ser automaticamente dimensionada para satisfazer o pedido do utilizador. O Table storage é arquivo chaves/atributos NoSQL da Microsoft, que pode tirar partido para dados estruturados com restrições de menos no esquema. Table storage do Azure pode ser utilizado para armazenar dados de persistência e obtenção eficiente.

A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída de tabela.

| Nome da propriedade | descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para o armazenamento desta tabela. |
| Conta de armazenamento |O nome da conta do storage onde está a enviar o resultado. |
| Chave da conta de armazenamento |A chave de acesso associada à conta de armazenamento. |
| Nome da Tabela |O nome da tabela. A tabela é criada, se não existir. |
| Chave de partição |O nome da coluna de saída que contém a chave de partição. A chave de partição é um identificador exclusivo para a partição dentro de determinada tabela que forma a primeira parte da chave primária de uma entidade. É um valor de cadeia que pode ser até 1 KB de tamanho. |
| Chave da fila |O nome da coluna de saída que contém a chave de linha. A chave de linha é um identificador exclusivo de uma entidade dentro de uma determinada partição. -Forma a segunda parte da chave primária de uma entidade. A chave de linha é um valor de cadeia que pode ser até 1 KB de tamanho. |
| Tamanho do batch |O número de registos para uma operação em lote. A predefinição (100) é suficiente para a maioria das tarefas. Consulte o [especificação de operação em lote tabela](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) para obter mais detalhes sobre a modificação esta definição. |
 
## <a name="service-bus-queues"></a>Filas de Service Bus
[Filas do Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) oferecem um primeiro In, entrega de mensagens First Out (FIFO) para um ou mais consumidores concorrentes. Normalmente, são esperadas mensagens sejam recebidas e processadas pelos recetores na ordem temporal em que foram adicionadas à fila e cada mensagem é recebida e processada por apenas um consumidor de mensagens.

A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída da fila.

| Nome da propriedade | descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para esta fila do Service Bus. |
| Espaço de nomes do Service Bus |Um espaço de nomes do Service Bus é um contentor para um conjunto de entidades de mensagens. |
| Nome da fila |O nome da fila de barramento de serviço. |
| Nome da política da fila |Quando cria uma fila, também pode criar políticas de acesso partilhado no separador de configuração. Cada política de acesso partilhado tem um nome, as permissões que define e chaves de acesso. |
| Chave da política da fila |A chave de acesso partilhado utilizada para autenticar o acesso ao espaço de nomes de barramento de serviço |
| Formato de serialização de eventos |Formato de serialização para dados de saída.  JSON, CSV e Avro são suportados. |
| Codificação |Para o CSV e JSON, UTF-8 é o único formato de codificação suportado neste momento |
| Delimitador |Só é aplicável a serialização de CSV. O Stream Analytics suporta um número de delimitadores comuns para serializar dados no formato CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical. |
| Formato |Só é aplicável para o tipo JSON. Separadas por linhas Especifica que o resultado é formatado, fazendo com que cada objeto JSON separado por uma nova linha. A matriz Especifica que o resultado é formatado como uma matriz de objetos JSON. |

O número de partições é [com base no SKU de barramento de serviço e tamanho](../service-bus-messaging/service-bus-partitioning.md). Chave de partição é um valor de número inteiro exclusivo para cada partição.

## <a name="service-bus-topics"></a>Tópicos de Service Bus
Enquanto as filas do Service Bus fornece um método de comunicação de um para um do remetente para recetor, [tópicos de Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) fornecem uma forma de comunicação de um-para-muitos.

A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída de tabela.

| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta a este tópico de barramento de serviço. |
| Espaço de nomes do Service Bus |Um espaço de nomes do Service Bus é um contentor para um conjunto de entidades de mensagens. Quando tiver criado um novo Hub de eventos, também criou um espaço de nomes do Service Bus |
| Nome do tópico |Tópicos são entidades de mensagens, semelhantes a filas e hubs de eventos. Foram concebidos para recolher fluxos de eventos a partir de um número de diferentes dispositivos e serviços. Quando é criado um tópico, também são atribuído a um nome específico. As mensagens enviadas para um tópico não está disponível, a menos que é criada uma subscrição, por isso, certifique-se existem uma ou mais subscrições em tópico |
| Nome da política do tópico |Quando cria um tópico, também pode criar políticas de acesso partilhado no separador de configuração. Cada política de acesso partilhado tem o nome, as permissões que definir e chaves de acesso |
| Chave da política do tópico |A chave de acesso partilhado utilizada para autenticar o acesso ao espaço de nomes de barramento de serviço |
| Formato de serialização de eventos |Formato de serialização para dados de saída.  JSON, CSV e Avro são suportados. |
| Codificação |Se utilizar formato CSV ou JSON, uma codificação tem de ser especificada. UTF-8 é o único formato de codificação suportado neste momento |
| Delimitador |Só é aplicável a serialização de CSV. O Stream Analytics suporta um número de delimitadores comuns para serializar dados no formato CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical. |

O número de partições é [com base no SKU de barramento de serviço e tamanho](../service-bus-messaging/service-bus-partitioning.md). Chave de partição é um valor de número inteiro exclusivo para cada partição.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[BD do Azure do Cosmos](https://azure.microsoft.com/services/documentdb/) é uma base de dados globalmente distribuído, com vários modelo de serviço que oferece ilimitada horizontal elástico relativamente a globo, consulta avançada e indexação automática através de modelos de dados de esquema desconhecidas, garantidos baixa latência e líder da indústria SLAs abrangentes. Para saber mais sobre as opções de recolha do Cosmos DB de Stream Analytics, consulte o [Stream Analytics com base de dados do Cosmos como saída](stream-analytics-documentdb-output.md) artigo.

> [!Note]
> Neste momento, o Azure Stream Analytics suporta apenas a ligação para a utilização de CosmosDB **API do SQL Server**.
> APIs de BD do Cosmos outros Azure ainda não são suportadas. Se o ponto de Azure Stream Analytics para as contas de base de dados do Azure Cosmos criado com outras APIs, os dados poderão não ser corretamente armazenados. 

A tabela seguinte descreve as propriedades para criar uma saída de BD do Cosmos do Azure.
| Nome da propriedade | descrição |
| --- | --- |
| Alias de saída | Um alias para fazer referência este resultado da consulta do Stream Analytics. |
| Sink | BD do Cosmos |
| Opção de Importar | Escolha o "Selecionar base de dados do Cosmos da sua subscrição", ou a "definições de forneça Cosmos DB manualmente".
| ID de conta | O nome ou o ponto final o URI da conta de base de dados do Cosmos. |
| Chave de conta | A chave de acesso partilhado para a conta de base de dados do Cosmos. |
| Base de Dados | O nome de base de dados de base de dados do Cosmos. |
| Padrão do nome de coleção | O nome da coleção ou os respetivos padrão para as coleções a ser utilizado. <br/>O formato de nome de coleção pode ser construído utilizando o token {partition} opcional, onde as partições começam do 0. Dois exemplos:  <br/>1. _MyCollection_ – uma coleção designada "MyCollection" tem de existir.  <br/>2. _MyCollection {partition}_ – com base na coluna de criação de partições. <br/>As coleções de coluna de criação de partições têm de existir – "MyCollection0", "MyCollection1", "MyCollection2" e assim sucessivamente. |
| Chave de partição | Opcional. Isto só é necessário se estiver a utilizar um token {partition} no seu padrão de nome de coleção.<br/> A chave de partição é o nome do campo em eventos de saída utilizado para especificar a chave de partições da saída nas coleções.<br/> Para o resultado única coleção, pode ser utilizada qualquer coluna de saída arbitrários. Por exemplo, PartitionId. |
| ID do documento |Opcional. O nome do campo em eventos de saída utilizado para especificar a chave primária na qual insert ou update baseiam-se operações.  

## <a name="azure-functions"></a>Funções do Azure
As Funções do Azure são um serviço de computação sem servidor lhe que permite executar código a pedido sem ter de aprovisionar ou gerir explicitamente uma infraestrutura. Permite-lhe implementar o código que é acionado pelos eventos que ocorrem no Azure ou serviços de terceiros.  Esta capacidade das funções do Azure para responder a acionadores torna uma saída natural de um Azure Stream Analytics. Este adaptador de saída permite aos utilizadores ligar o Stream Analytics para as funções do Azure e executar um script ou um fragmento de código de resposta a uma variedade de eventos.

O Azure Stream Analytics invoca as funções do Azure através de acionadores HTTP. O novo adaptador de saída de função do Azure está disponível com as seguintes propriedades configuráveis:

| Nome da propriedade | descrição |
| --- | --- |
| Function App |Nome da sua aplicação de funções do Azure |
| Função |Nome da função na sua aplicação de funções do Azure |
| Chave |Se pretender utilizar uma função do Azure a partir de outra subscrição, pode fazê-fornecendo a chave para aceder à sua função |
| Tamanho máximo de lote |Esta propriedade pode ser utilizada para definir o tamanho máximo para cada lote de saída que é enviado para a função do Azure. Por predefinição, este valor é 256 KB |
| Contagem máxima de lotes  |Como o nome indica, esta propriedade permite-lhe especificar o número máximo de eventos em cada lote que é enviado para as funções do Azure. O valor de contagem de lote máximo predefinido é 100 |

Quando o Azure Stream Analytics recebe 413 exceção de (http pedido entidade demasiado grande) da função do Azure, reduz o tamanho de lotes envia para as funções do Azure. No código da função do Azure, utilize esta exceção para se certificar de que o Azure Stream Analytics não enviar os lotes de grande dimensão. Além disso, certifique-se de que os valores de contagem e tamanho de lote máximo utilizados na função são consistentes com os valores que introduziu no portal do Stream Analytics. 

Além disso, numa situação em que nenhum evento de destino numa janela de tempo, nenhuma saída é gerada. Como resultado, a função computeResult não for chamada. Este comportamento é consistente com as funções de agregação em janela incorporadas.

## <a name="partitioning"></a>Criação de partições

A tabela seguinte resume o suporte de partição e o número de escritores de saída para cada tipo de saída:

| Tipo de saída | Suporte de criação de partições | Chave de partição  | Número de escritores de saída | 
| --- | --- | --- | --- |
| Azure Data Lake Store | Sim | Utilize {date} e {time} tokens. o padrão de prefixo do caminho. Escolha o formato de data, tais como aaaa/MM/DD, DD/MM/AAAA, MM-DD-AAAA. HH é utilizada para o formato de hora. | Segue-se de que a entrada partições para [consultas totalmente paralelizáveis](stream-analytics-scale-jobs.md). | 
| Base de Dados SQL do Azure | Não | Nenhuma | Não aplicável. | 
| Armazenamento de Blobs do Azure | Sim | Utilize {date} e tokens {time} ou {fieldname} única do seu campos de eventos no padrão de caminho. Escolha o formato de data, tais como aaaa/MM/DD, DD/MM/AAAA, MM-DD-AAAA. HH é utilizada para o formato de hora. | Segue-se de que a entrada partições para [consultas totalmente paralelizáveis](stream-analytics-scale-jobs.md). | 
| Hub de Eventos do Azure | Sim | Sim | Varia consoante o alinhamento de partição.</br> Quando o resultado da chave de partição igualmente está alinhada com a montante () consulta passo anterior, o número de escritores de Hub de eventos é o mesmo número de saída partições do Hub de eventos. Cada escritor utiliza do EventHub [EventHubSender classe](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) para enviar eventos para a partição específica. </br> Quando o resultado da chave de partição não está alinhada com a montante () consulta passo anterior, o número de escritores de Hub de eventos é igual ao número de partições nesse passo anterior. Cada escritor utiliza EventHubClient [SendBatchAsync classe](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) para enviar eventos para todas as partições de saída. |
| Power BI | Não | Nenhuma | Não aplicável. | 
| Armazenamento de Tabelas do Azure | Sim | Nenhuma coluna de saída.  | Segue-se de que a entrada partições para [totalmente paralelizada consultas](stream-analytics-scale-jobs.md). | 
| Tópico de barramento de serviço do Azure | Sim | Automaticamente escolhido. O número de partições é baseado no [SKU de barramento de serviço e o tamanho](../service-bus-messaging/service-bus-partitioning.md). Chave de partição é um valor de número inteiro exclusivo para cada partição.| Igual ao número de partições existentes no tópico de saída.  |
| Fila de barramento de serviço do Azure | Sim | Automaticamente escolhido. O número de partições é baseado no [SKU de barramento de serviço e o tamanho](../service-bus-messaging/service-bus-partitioning.md). Chave de partição é um valor de número inteiro exclusivo para cada partição.| Igual ao número de partições na fila de saída. |
| Azure Cosmos DB | Sim | Utilize o token {partition} no padrão de nome de coleção. valor de {partition} baseia-se a cláusula PARTITION BY na consulta. | Segue-se de que a entrada partições para [totalmente paralelizada consultas](stream-analytics-scale-jobs.md). |
| Funções do Azure | Não | Nenhuma | Não aplicável. | 

## <a name="output-batch-size"></a>Tamanho do lote de saída
O Azure Stream Analytics utiliza lotes de tamanho variável para processar os eventos e escrever saídas. Normalmente, o motor de Stream Analytics não escrever uma mensagem numa altura e utiliza lotes para uma eficiência. Quando a entrada e a taxa de eventos de saída é elevada, utiliza lotes maior. Quando a taxa de saída é baixa, utiliza os lotes mais pequena para manter a latência baixa. 

A tabela seguinte explica algumas das considerações sobre a criação de batches de saída:

| Tipo de saída | Tamanho máximo de mensagem | Otimização de tamanho de lote |
| :--- | :--- | :--- | 
| Azure Data Lake Store | Consulte [limites de armazenamento do Data Lake](../azure-subscription-service-limits.md#data-lake-store-limits) | Até 4 MB por operação de escrita |
| Base de Dados SQL do Azure | Inserir 10 000 máx. de linhas por em massa única</br>Linhas de 100 mín. por inserção em massa única </br>Consulte também [limita SQL do Azure](../sql-database/sql-database-resource-limits.md) |  Cada lote é inicialmente em massa inserida com tamanho de lote máximo e pode dividir o lote em metade (até que o tamanho do lote Min) com base na repetição erros do SQL Server. |
| Armazenamento de Blobs do Azure | Consulte [limites de armazenamento do Azure](../azure-subscription-service-limits.md#storage-limits) | O tamanho máximo do bloco de Blob é 4 MB</br>Contagem máxima de bock do Blob é 50000 |
| Hub de Eventos do Azure   | 256 KB por mensagem </br>Consulte também [limita os Event Hubs](../event-hubs/event-hubs-quotas.md) |    Quando a criação de partições de saída de entrada não alinha, cada evento é packed individualmente num EventData e enviado num batch de até ao tamanho máximo de mensagem (1 MB para Premium SKU). </br></br>  Quando a criação de partições de entrada-saída está alinhada, vários eventos são packed para um único EventData até tamanho máximo de mensagem e enviados.    |
| Power BI | Consulte [limita a API de Rest do Power BI](https://msdn.microsoft.com/library/dn950053.aspx) |
| Armazenamento de Tabelas do Azure | Consulte [limites de armazenamento do Azure](../azure-subscription-service-limits.md#storage-limits) | Predefinido é 100 entidades por transação única e pode ser configurado para um valor menor conforme necessário. |
| Fila do Service Bus do Azure   | 256 KB por mensagem</br> Consulte também [limita do Service Bus](../service-bus-messaging/service-bus-quotas.md) | Evento único por mensagem |
| Tópico do Service Bus do Azure | 256 KB por mensagem</br> Consulte também [limita do Service Bus](../service-bus-messaging/service-bus-quotas.md) | Evento único por mensagem |
| Azure Cosmos DB   | Consulte [limita a BD do Cosmos do Azure](../azure-subscription-service-limits.md#azure-cosmos-db-limits) | Batch tamanho e frequência é ajustadas dinamicamente com base CosmosDB as respostas de escrita. </br> Não existem limitações predeterminadas do Stream Analytics. |
| Funções do Azure   | | Tamanho do lote predefinido é 246 KB. </br> Contagem de eventos de predefinida por lote é 100. </br> O tamanho do lote é configurável e pode ser aumentado ou diminuído no Stream Analytics [opções de saída](#azure-functions). 

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Início rápido: Criar uma tarefa de Stream Analytics, utilizando o portal do Azure](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
