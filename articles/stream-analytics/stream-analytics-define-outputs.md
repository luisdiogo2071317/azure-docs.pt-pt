---
title: Tipos de saídas de tarefas do Azure Stream Analytics
description: Saiba mais sobre a filtragem de opções de saídas de dados de Stream Analytics, incluindo o Power BI para resultados de análise.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: 8d984c17ab373428b13ed59a598ca8ae4e88136a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="stream-analytics-outputs-options-for-storage-and-analysis"></a>Transmitir saídas de análise: opções de armazenamento e análise
Durante a criação de uma tarefa de Stream Analytics, considere como os dados resultantes são consumidos. Como pode ver os resultados da tarefa de Stream Analytics e onde pode armazená-lo?

Para ativar uma variedade de padrões de aplicação, o Azure Stream Analytics tem diferentes opções para armazenar a saída e ver resultados de análise. Isto torna mais fácil ver o resultado da tarefa e dá-lhe a flexibilidade de consumo e o armazenamento de resultado da tarefa de armazém de dados e outros fins. Qualquer saída configurada na tarefa tem de existir antes da tarefa é iniciada e eventos de iniciar o fluxo. Por exemplo, se utilizar o Blob storage como resultado, a tarefa não criar uma conta do storage automaticamente. Crie uma conta de armazenamento antes da tarefa de Stream Analytics é iniciada.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Suporta análise de sequência [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). O Azure Data Lake Store é um repositório de hiper escala a nível da empresa para cargas de trabalho de análise de macrodados. Arquivo data Lake permite-lhe armazenar dados de qualquer tamanho, tipo e velocidade de ingestão para análises exploratórias e operacionais. Além disso, o Stream Analytics tem de estar autorizado a aceder ao Data Lake Store.

### <a name="authorize-an-azure-data-lake-store-account"></a>Autorizar uma conta do Azure Data Lake Store

1. Quando o armazenamento do Data Lake é selecionado como uma saída no portal do Azure, lhe for pedido para autorizar uma ligação a um Data Lake Store existente.  

   ![Autorizar o arquivo Data Lake](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

2. Se já tiver acesso ao Data Lake Store, clique em "Autorizar agora" e uma página irá aparecer que indica "Redirecting para autorização". Depois de autorização é concluída com êxito, é-lhe apresentada a página que lhe permite configurar a saída do Data Lake Store.  

3. Depois de ter a conta de Data Lake Store autenticada, pode configurar as propriedades para a saída do Data Lake Store. A tabela abaixo é a lista de nomes de propriedade e a respetiva descrição para configurar a saída do Data Lake Store.

   ![Autorizar o arquivo Data Lake](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

<table>
<tbody>
<tr>
<td><B>NOME DA PROPRIEDADE</B></td>
<td><B>DESCRIÇÃO</B></td>
</tr>
<tr>
<td>Alias de saída</td>
<td>Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para este arquivo Data Lake.</td>
</tr>
<tr>
<td>Nome da Conta</td>
<td>O nome da conta de armazenamento do Data Lake onde está a enviar o resultado. É-lhe apresentada uma lista pendente de contas do Data Lake Store que estão disponíveis na sua subscrição.</td>
</tr>
<tr>
<td>Caminho prefixo padrão</td>
<td>O caminho do ficheiro utilizado para escrever ficheiros dentro do arquivo de conta especificado do Data Lake. Pode especificar um ou mais instâncias das {date} e {time} variáveis.<BR> Exemplo 1: pasta1/logs / {date} / {time}<BR>Exemplo 2: pasta1/logs / {date}<BR>Além disso, seguem-se as situações, onde é criado um novo ficheiro:<BR>1. Alteração do esquema de saída <BR>2. Reinicie o interno ou externo de uma tarefa<BR><BR>Além disso, se o padrão do caminho de ficheiro não contém à direita "/", o último padrão no caminho de ficheiro é tratado como um prefixo de nome de ficheiro.<BR></td>
</tr>
<tr>
<td>Formato de data [<I>opcional</I>]</td>
<td>Se o token de data é utilizado no caminho de prefixo, pode selecionar o formato da data em que os ficheiros estão organizados. Exemplo: DD/MM/DD</td>
</tr>
<tr>
<td>Formato de hora [<I>opcional</I>]</td>
<td>Se o token de tempo é utilizado no caminho de prefixo, especifique o formato de hora em que os ficheiros estão organizados. Atualmente, o único valor suportado é HH.</td>
</tr>
<tr>
<td>Formato de serialização de eventos</td>
<td>Formato de serialização para dados de saída. JSON, CSV e Avro são suportados.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Se utilizar formato CSV ou JSON, uma codificação tem de ser especificada. UTF-8 é o único formato de codificação suportado neste momento.</td>
</tr>
<tr>
<td>Delimitador</td>
<td>Só é aplicável a serialização de CSV. Stream Analytics suporta um número de delimitadores comuns para serializar os dados CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical.</td>
</tr>
<tr>
<td>Formato</td>
<td>Só é aplicável a serialização do JSON. Separadas por linhas Especifica que o resultado é formatado, fazendo com que cada objeto JSON separado por uma nova linha. A matriz Especifica que o resultado é formatado como uma matriz de objetos JSON. Esta matriz está fechada apenas quando o deixa de tarefa do Stream Analytics tem mover ou para a próxima janela de tempo. Em geral, é preferível a utilizar a linha de valores separados por JSON, uma vez que não requer qualquer processamento especial enquanto ainda está a ser escrito no ficheiro de saída para.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Renovar autorização do arquivo Data Lake
Terá de voltar a sua conta do Data Lake Store, se a palavra-passe tiver sido alterado desde que a tarefa foi criada ou pela última vez autenticada. Se não a reautenticação, a tarefa não será saída resultados e é registado um erro que indica a necessidade de nova autorização nos registos de operação. Atualmente, não há uma limitação em que o token de autenticação tem de ser atualizados manualmente todos os 90 dias para todas as tarefas com a saída do Data Lake Store. 

Para renovar a autorização, **parar** a tarefa > vá para a saída do Data Lake Store > clique o **renove a autorização** associar e, por um breve período de tempo a página irá aparecer que indica "Redirecting para autorização..". A página será fechada automaticamente e se tiver êxito, irá indicar "Autorização tem foi renovada com êxito". Em seguida, tem de clicar em **guardar** na parte inferior da página, pode proceder ao reiniciar a tarefa a partir do **hora da última paragem** para evitar a perda de dados.

![Autorizar o arquivo Data Lake](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Base de dados SQL do Azure](https://azure.microsoft.com/services/sql-database/) pode ser utilizado como uma saída de dados que natureza relacional ou para as aplicações que dependem de conteúdo que está a ser alojado numa base de dados relacional. Tarefas do Stream Analytics escrevem uma tabela existente numa base de dados SQL do Azure.  O esquema de tabela deve corresponder exatamente os campos e os respetivos tipos que está a ser resultado da tarefa. Um [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) também podem ser especificados como uma saída através da opção de saída da base de dados SQL, bem como. A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída de base de dados SQL.

| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para esta base de dados. |
| Base de Dados |O nome da base de dados em que está a enviar resultado |
| Nome do servidor |O nome do servidor de base de dados SQL |
| Nome de utilizador |O nome de utilizador, que tem acesso ao escrever na base de dados |
| Palavra-passe |A palavra-passe para ligar à base de dados |
| Tabela |O nome da tabela em que o resultado é escrito. O nome da tabela é sensível e o esquema desta tabela deve corresponder exatamente ao número de campos e os respetivos tipos gerados pelo seu resultado da tarefa. |

> [!NOTE]
> Atualmente, a oferta da SQL Database do Azure é suportada para uma saída da tarefa no Stream Analytics. No entanto, uma máquina de Virtual do Azure com o SQL Server com uma base de dados ligada não é suportada. Trata-se sujeita a alterações em futuros lançamentos.
> 
> 

## <a name="blob-storage"></a>Armazenamento de blobs
O blob storage oferece uma solução económica e dimensionável para armazenar grandes quantidades de dados não estruturados na nuvem.  Para uma introdução no Blob storage do Azure e a respetiva utilização, consulte a documentação em [como utilizar Blobs](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída de blob.

<table>
<tbody>
<tr>
<td>NOME DA PROPRIEDADE</td>
<td>DESCRIÇÃO</td>
</tr>
<tr>
<td>Alias de saída</td>
<td>Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para o armazenamento de Blobs.</td>
</tr>
<tr>
<td>Conta de Armazenamento</td>
<td>O nome da conta do storage onde está a enviar o resultado.</td>
</tr>
<tr>
<td>Chave da Conta de Armazenamento</td>
<td>A chave secreta associada à conta de armazenamento.</td>
</tr>
<tr>
<td>Contentor de armazenamento</td>
<td>Contentores fornecem um agrupamento lógico blobs armazenados no Microsoft serviço Blob do Azure. Quando carregar um blob para o serviço Blob, tem de especificar um contentor para esse blob.</td>
</tr>
<tr>
<td>Padrão de prefixo de caminho [opcional]</td>
<td>O padrão do caminho de ficheiro utilizado para escrever os blobs no contentor especificado. <BR> O padrão de caminho, pode optar por utilizar uma ou mais instâncias das 2 variáveis seguintes para especificar a frequência com que os blobs são escritos: <BR> {date}, {time} <BR> Exemplo 1: cluster1/logs / {date} / {time} <BR> Exemplo 2: cluster1/logs / {date} <BR> <BR> Atribuição de nome de ficheiro, segue-se a seguinte convenção: <BR> {Caminho prefixo Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR> Ficheiros de saída de exemplo: <BR> Myoutput/20170901/00/45434_gguid_1.csv <BR> Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR> Além disso, seguem-se as situações, onde é criado um novo ficheiro: <BR> 1. Ficheiro atual excede o número máximo permitido de blocos (atualmente 50 000) <BR> 2. Alteração do esquema de saída <BR> 3. Reinício de interno ou externo de uma tarefa  </td>
</tr>
<tr>
<td>Formato de data [opcional]</td>
<td>Se o token de data é utilizado no caminho de prefixo, pode selecionar o formato da data em que os ficheiros estão organizados. Exemplo: DD/MM/DD</td>
</tr>
<tr>
<td>Formato de hora [opcional]</td>
<td>Se o token de tempo é utilizado no caminho de prefixo, especifique o formato de hora em que os ficheiros estão organizados. Atualmente, o único valor suportado é HH.</td>
</tr>
<tr>
<td>Formato de serialização de eventos</td>
<td>Formato de serialização para dados de saída.  JSON, CSV e Avro são suportados.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Se utilizar formato CSV ou JSON, uma codificação tem de ser especificada. UTF-8 é o único formato de codificação suportado neste momento.</td>
</tr>
<tr>
<td>Delimitador</td>
<td>Só é aplicável a serialização de CSV. Stream Analytics suporta um número de delimitadores comuns para serializar os dados CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical.</td>
</tr>
<tr>
<td>Formato</td>
<td>Só é aplicável a serialização do JSON. Separadas por linhas Especifica que o resultado é formatado, fazendo com que cada objeto JSON separado por uma nova linha. A matriz Especifica que o resultado é formatado como uma matriz de objetos JSON. Esta matriz está fechada apenas quando o deixa de tarefa do Stream Analytics tem mover ou para a próxima janela de tempo. Em geral, é preferível a utilizar a linha de valores separados por JSON, uma vez que não requer qualquer processamento especial enquanto ainda está a ser escrito no ficheiro de saída para.</td>
</tr>
</tbody>
</table>

Ao utilizar o armazenamento de BLOBs como resultado, é criado um novo ficheiro no blob nos seguintes casos:

* Se o ficheiro excede o número máximo de blocos permitidos (tenha em atenção que o número máximo permitido de blocos pode ser contactado sem atingir o tamanho máximo de blob permitidos. Por exemplo, se a taxa de saída é elevada, pode ver bytes mais por blocos e o tamanho do ficheiro é maior. Se a taxa de saída é baixa, cada bloco tem menos de dados e o tamanho de ficheiro é menor.)  
* Se existir uma alteração de esquema no resultado, e o formato de saída requer o esquema fixo (CSV e Avro).  
* Se uma tarefa for reiniciada uma externamente ou a reinicialização interna de uma tarefa.  
* Se a consulta completamente estar particionada, é criado novo ficheiro para cada partição de saída.  
* Se um ficheiro ou de um contentor da conta de armazenamento é eliminado pelo utilizador.  
* Se o resultado é tempo particionado com o padrão de prefixo do caminho, um novo blob é utilizado quando a consulta é movido para a hora seguinte.

## <a name="event-hub"></a>Hub de Eventos
[Os Event Hubs](https://azure.microsoft.com/services/event-hubs/) é altamente dimensionável de publicação-subscrição ingestor de eventos. -Pode recolher milhões de eventos por segundo. Uma utilização de um Hub de eventos como saída é quando o resultado de uma tarefa de Stream Analytics torna-se a entrada de outra tarefa de transmissão em fluxo.

Existem alguns parâmetros que são necessários para configurar os fluxos de dados de Hub de eventos como uma saída.

| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para o Hub de eventos. |
| Espaço de Nomes do Service Bus |Um espaço de nomes do Service Bus é um contentor para um conjunto de entidades de mensagens. Quando tiver criado um novo Hub de eventos, também criou um espaço de nomes do Service Bus |
| Hub de Eventos |O nome do seu Hub de eventos de saída |
| Nome de política do Hub de eventos |A política de acesso partilhado, que pode ser criada no separador Configurar do Hub de eventos. Cada política de acesso partilhado tem um nome, as permissões que define e chaves de acesso |
| Chave de política de Hub de eventos |A chave de acesso partilhado utilizada para autenticar o acesso ao espaço de nomes de barramento de serviço |
| Coluna de chave de partição [opcional] |Esta coluna contém a chave de partição do Hub de eventos de saída. |
| Formato de serialização de eventos |Formato de serialização para dados de saída.  JSON, CSV e Avro são suportados. |
| Encoding |Para o CSV e JSON, UTF-8 é o único formato de codificação suportado neste momento |
| Delimitador |Só é aplicável a serialização de CSV. O Stream Analytics suporta um número de delimitadores comuns para serializar dados no formato CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical. |
| Formato |Só é aplicável a serialização do JSON. Separadas por linhas Especifica que o resultado é formatado, fazendo com que cada objeto JSON separado por uma nova linha. A matriz Especifica que o resultado é formatado como uma matriz de objetos JSON. Esta matriz está fechada apenas quando o deixa de tarefa do Stream Analytics tem mover ou para a próxima janela de tempo. Em geral, é preferível a utilizar a linha de valores separados por JSON, uma vez que não requer qualquer processamento especial enquanto ainda está a ser escrito no ficheiro de saída para. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) pode ser utilizado como uma saída de uma tarefa de Stream Analytics para fornecer uma experiência avançada de visualização de resultados de análise. Esta capacidade pode ser utilizada para dashboards operacionais, a geração de relatórios e métrica orientadas por relatório.

### <a name="authorize-a-power-bi-account"></a>Autorizar uma conta do Power BI
1. Quando o Power BI é selecionada como uma saída no portal do Azure, são-lhe pedido para autorizar um utilizador de BI energia existente ou criar uma nova conta do Power BI.  
   
   ![Autorizar o utilizador do Power BI](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  
2. Crie uma nova conta se não, mas tiver um, clique em autorizar agora.  É apresentado um ecrã de como o seguinte.  
   
   ![Conta do Azure Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  
3. Neste passo, forneça a conta escolar ou profissional para autorizar a saída do Power BI. Se tiver não terminado já sessão para o Power BI, escolha agora o início de sessão cópias de segurança. A conta escolar ou profissional que utiliza para o Power BI pode ser diferente da conta de subscrição do Azure, o qual tem sessão iniciada com.

### <a name="configure-the-power-bi-output-properties"></a>Configurar as propriedades de saída do Power BI
Depois de ter a conta do Power BI autenticada, pode configurar as propriedades para a saída do Power BI. A tabela abaixo é a lista de nomes de propriedade e respetiva descrição para configurar a saída do Power BI.

| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para este resultado de PowerBI. |
| Área de trabalho de grupo |Para ativar a partilha de dados com outros utilizadores do Power BI pode selecionar os grupos no interior da sua conta do Power BI ou escolha "A minha área de trabalho" se não pretender escrever a um grupo.  Atualizar um grupo existente exige renovar a autenticação do Power BI. |
| Nome do conjunto de dados |Forneça um nome de conjunto de dados que pretender para a saída do Power BI utilizar |
| Nome da Tabela |Forneça um nome de tabela em que o conjunto de dados de saída do Power BI. Atualmente, saída do Power BI de tarefas do Stream Analytics só pode ter uma tabela num conjunto de dados |

Para uma passagem de configurar um Power BI saída e o dashboard, consulte o [Azure Stream Analytics & Power BI](stream-analytics-power-bi-dashboard.md) artigo.

> [!NOTE]
> Não explicitamente crie o conjunto de dados e a tabela no dashboard do Power BI. O conjunto de dados e a tabela é preenchida automaticamente quando a tarefa é iniciada e a tarefa é iniciada a saída de bombagem no Power BI. Tenha em atenção que se a consulta da tarefa não gerar resultados, o conjunto de dados e tabela não é criada. Tenha em atenção que, se o Power BI já tinha um conjunto de dados e uma tabela com o mesmo nome que aquele fornecido nesta tarefa de Stream Analytics, os dados existentes for substituídos.
> 
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
Número de vírgula flutuante | Duplo
Matriz de registo | Cadeia de tipo, o valor constante "IRecord" ou "IArray"

### <a name="schema-update"></a>Atualização do esquema
Do Stream Analytics infere o esquema do modelo de dados com base no primeiro conjunto de eventos no resultado. Mais tarde, se necessário, o esquema do modelo de dados é atualizado para acomodar os eventos de entrada que não podem ajustar o esquema original.

O `SELECT *` consulta deve ser evitada para impedir a atualização do esquema dinâmico em linhas. Para além das implicações de desempenho potencial, também pode resultar num uncertainty de tempo decorrido para os resultados. Devem selecionar os campos exatos que precisam de ser mostrada no dashboard do Power BI. Além disso, os valores de dados devem ser compatíveis com o tipo de dados escolhidas.


Anterior/atual | Int64 | Cadeia | Datetime | Duplo
-----------------|-------|--------|----------|-------
Int64 | Int64 | Cadeia | Cadeia | Duplo
Duplo | Duplo | Cadeia | Cadeia | Duplo
Cadeia | Cadeia | Cadeia | Cadeia |  | Cadeia | 
Datetime | Cadeia | Cadeia |  Datetime | Cadeia


### <a name="renew-power-bi-authorization"></a>Renovar autorização do Power BI
Terá de voltar a sua conta do Power BI, se a palavra-passe tiver sido alterado desde que a tarefa foi criada ou pela última vez autenticada. Se a multi-factor Authentication (MFA) está configurado no seu inquilino do Azure Active Directory (AAD), que terá também de renovação da autorização do Power BI em duas semanas. Um sintoma de que este problema é nenhum resultado da tarefa e um "erro do utilizador de autenticar" nos registos de operação:

  ![Erro de token de atualização do Power BI](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Para resolver este problema, parar a tarefa em execução e avance para a saída do Power BI.  Clique na ligação "Autorização de renovação" e reinicie a tarefa da hora da última paragem para evitar a perda de dados.

  ![Power BI é renovada de autorização](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Armazenamento de Tabelas
[Table storage do Azure](../storage/common/storage-introduction.md) oferece um armazenamento de elevada disponibilidade e extremamente dimensionável, para que uma aplicação possa ser automaticamente dimensionada para satisfazer o pedido do utilizador. O Table storage é arquivo chaves/atributos NoSQL da Microsoft, que pode tirar partido para dados estruturados com restrições de menos no esquema. Table storage do Azure pode ser utilizado para armazenar dados de persistência e obtenção eficiente.

A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída de tabela.

| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para o armazenamento desta tabela. |
| Conta de Armazenamento |O nome da conta do storage onde está a enviar o resultado. |
| Chave da Conta de Armazenamento |A chave de acesso associada à conta de armazenamento. |
| Nome da Tabela |O nome da tabela. A tabela é criada, se não existir. |
| Chave de partição |O nome da coluna de saída que contém a chave de partição. A chave de partição é um identificador exclusivo para a partição dentro de determinada tabela que forma a primeira parte da chave primária de uma entidade. É um valor de cadeia que pode ser até 1 KB de tamanho. |
| Chave de linha |O nome da coluna de saída que contém a chave de linha. A chave de linha é um identificador exclusivo de uma entidade dentro de uma determinada partição. -Forma a segunda parte da chave primária de uma entidade. A chave de linha é um valor de cadeia que pode ser até 1 KB de tamanho. |
| Tamanho do lote |O número de registos para uma operação em lote. Normalmente, a predefinição é suficiente para a maioria das tarefas, consulte o [especificação de operação em lote tabela](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) para obter mais detalhes sobre a modificação esta definição. |
 
## <a name="service-bus-queues"></a>Filas de Service Bus
[Filas do Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) oferecem um primeiro In, entrega de mensagens First Out (FIFO) para um ou mais consumidores concorrentes. Normalmente, são esperadas mensagens sejam recebidas e processadas pelos recetores na ordem temporal em que foram adicionadas à fila e cada mensagem é recebida e processada por apenas um consumidor de mensagens.

A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída da fila.

| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta para esta fila do Service Bus. |
| Espaço de Nomes do Service Bus |Um espaço de nomes do Service Bus é um contentor para um conjunto de entidades de mensagens. |
| Nome da fila |O nome da fila de barramento de serviço. |
| Nome da política de fila |Quando cria uma fila, também pode criar políticas de acesso partilhado no separador de configuração. Cada política de acesso partilhado tem um nome, as permissões que define e chaves de acesso. |
| Chave de política de fila |A chave de acesso partilhado utilizada para autenticar o acesso ao espaço de nomes de barramento de serviço |
| Formato de serialização de eventos |Formato de serialização para dados de saída.  JSON, CSV e Avro são suportados. |
| Encoding |Para o CSV e JSON, UTF-8 é o único formato de codificação suportado neste momento |
| Delimitador |Só é aplicável a serialização de CSV. O Stream Analytics suporta um número de delimitadores comuns para serializar dados no formato CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical. |
| Formato |Só é aplicável para o tipo JSON. Separadas por linhas Especifica que o resultado é formatado, fazendo com que cada objeto JSON separado por uma nova linha. A matriz Especifica que o resultado é formatado como uma matriz de objetos JSON. |

## <a name="service-bus-topics"></a>Tópicos de Service Bus
Enquanto as filas do Service Bus fornece um método de comunicação de um para um do remetente para recetor, [tópicos de Service Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) fornecem uma forma de comunicação de um-para-muitos.

A tabela abaixo lista os nomes de propriedade e a respetiva descrição para a criação de uma saída de tabela.

| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída |Um nome amigável utilizado nas consultas para direcionar o resultado de consulta a este tópico de barramento de serviço. |
| Espaço de Nomes do Service Bus |Um espaço de nomes do Service Bus é um contentor para um conjunto de entidades de mensagens. Quando tiver criado um novo Hub de eventos, também criou um espaço de nomes do Service Bus |
| Nome do Tópico |Tópicos são entidades de mensagens, semelhantes a filas e hubs de eventos. Foram concebidos para recolher fluxos de eventos a partir de um número de diferentes dispositivos e serviços. Quando é criado um tópico, também são atribuído a um nome específico. As mensagens enviadas para um tópico não está disponível, a menos que é criada uma subscrição, por isso, certifique-se existem uma ou mais subscrições em tópico |
| Nome da política de tópico |Quando cria um tópico, também pode criar políticas de acesso partilhado no separador de configuração. Cada política de acesso partilhado tem o nome, as permissões que definir e chaves de acesso |
| Chave de política de tópico |A chave de acesso partilhado utilizada para autenticar o acesso ao espaço de nomes de barramento de serviço |
| Formato de serialização de eventos |Formato de serialização para dados de saída.  JSON, CSV e Avro são suportados. |
 | Encoding |Se utilizar formato CSV ou JSON, uma codificação tem de ser especificada. UTF-8 é o único formato de codificação suportado neste momento |
| Delimitador |Só é aplicável a serialização de CSV. O Stream Analytics suporta um número de delimitadores comuns para serializar dados no formato CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, separador e barra vertical. |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[BD do Azure do Cosmos](https://azure.microsoft.com/services/documentdb/) é uma base de dados globalmente distribuído, com vários modelo de serviço que oferece ilimitada horizontal elástico relativamente a globo, consulta avançada e indexação automática através de modelos de dados de esquema desconhecidas, garantidos baixa latência e líder da indústria SLAs abrangentes. Para saber mais sobre as opções de recolha do Cosmos DB de Stream Analytics, consulte o [Stream Analytics com base de dados do Cosmos como saída](stream-analytics-documentdb-output.md) artigo.

> [!Note]
> Neste momento, o Azure Stream Analytics suporta apenas a ligação para a utilização de CosmosDB **API do SQL Server**.
> APIs de BD do Cosmos outros Azure ainda não são suportadas. Se o ponto de Azure Stream Analytics para as contas de base de dados do Azure Cosmos criado com outras APIs, os dados poderão não ser corretamente armazenados. 

A tabela seguinte descreve as propriedades para criar uma saída de BD do Cosmos do Azure.
| Nome da propriedade | Descrição |
| --- | --- |
| Alias de saída | Um alias para fazer referência este resultado da consulta do Stream Analytics. |
| Sink | BD do Cosmos |
| Importar opção | Escolha o "Selecionar base de dados do Cosmos da sua subscrição", ou a "definições de forneça Cosmos DB manualmente".
| ID de conta | O nome ou o ponto final o URI da conta de base de dados do Cosmos. |
| Chave de conta | A chave de acesso partilhado para a conta de base de dados do Cosmos. |
| Base de Dados | O nome de base de dados de base de dados do Cosmos. |
| Padrão do nome de coleção | O nome da coleção ou os respetivos padrão para as coleções a ser utilizado. <br/>O formato de nome de coleção pode ser construído utilizando o token {partition} opcional, onde as partições começam do 0. Dois exemplos:  <br/>1. _MyCollection_ – uma coleção designada "MyCollection" tem de existir.  <br/>2. _MyCollection {partition}_ – com base na coluna de criação de partições. <br/>As coleções de coluna de criação de partições têm de existir – "MyCollection0", "MyCollection1", "MyCollection2" e assim sucessivamente. |
| Chave de partição | Opcional. Isto só é necessário se estiver a utilizar um token {partition} no seu padrão de nome de coleção.<br/> A chave de partição é o nome do campo em eventos de saída utilizado para especificar a chave de partições da saída nas coleções.<br/> Para o resultado única coleção, nenhuma coluna de saída arbitrários pode ser utilizada por exemplo, PartitionId. |
| ID do documento |Opcional. O nome do campo em eventos de saída utilizado para especificar a chave primária na qual insert ou update baseiam-se operações.  

## <a name="azure-functions-in-preview"></a>Funções do Azure (na pré-visualização)
As Funções do Azure são um serviço de computação sem servidor lhe que permite executar código a pedido sem ter de aprovisionar ou gerir explicitamente uma infraestrutura. Permite-lhe implementar o código que é acionado pelos eventos que ocorrem no Azure ou serviços de terceiros.  Esta capacidade das funções do Azure para responder a acionadores torna uma saída natural de um Azure Stream Analytics. Este adaptador de saída permite aos utilizadores ligar o Stream Analytics para as funções do Azure e executar um script ou um fragmento de código de resposta a uma variedade de eventos.

O Azure Stream Analytics invoca as funções do Azure através de acionadores HTTP. O novo adaptador de saída de função do Azure está disponível com as seguintes propriedades configuráveis:

| Nome da propriedade | Descrição |
| --- | --- |
| Aplicação de Funções |Nome da sua aplicação de funções do Azure |
| Função |Nome da função na sua aplicação de funções do Azure |
| Tamanho de lote máximo |Esta propriedade pode ser utilizada para definir o tamanho máximo para cada lote de saída que é enviado para a função do Azure. Por predefinição, este valor é 256 KB |
| Contagem máxima de Batch  |Como o nome indica, esta propriedade permite-lhe especificar o número máximo de eventos em cada lote que é enviado para as funções do Azure. O valor de contagem de lote máximo predefinido é 100 |
| Chave |Se pretender utilizar uma função do Azure a partir de outra subscrição, pode fazê-fornecendo a chave para aceder à sua função |

Tenha em atenção que quando o Azure Stream Analytics recebe 413 exceção de (http pedido entidade demasiado grande) da função do Azure, reduz o tamanho de lotes envia para as funções do Azure. No código da função do Azure, utilize esta exceção para se certificar de que o Azure Stream Analytics não enviar os lotes de grande dimensão. Além disso, certifique-se de que os valores de contagem e tamanho de lote máximo utilizados na função são consistentes com os valores que introduziu no portal do Stream Analytics. 

Além disso, numa situação em que nenhum evento de destino numa janela de tempo, nenhuma saída é gerada. Como resultado, a função computeResult não for chamada. Este comportamento é consistente com as funções de agregação em janela incorporadas.


## <a name="get-help"></a>Obter ajuda
Para mais assistência, tente ler o nosso [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Passos Seguintes
Agora já conhece o Stream Analytics, um serviço gerido para a transmissão da análise dos dados a partir da Internet das Coisas. Para obter mais informações sobre este serviço, consulte:

* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
