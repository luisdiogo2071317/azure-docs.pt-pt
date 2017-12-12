---
title: "Corrija um erro de ligação de SQL, erro transitório | Microsoft Docs"
description: "Saiba como resolver problemas, diagnosticar e prevenir um erro de ligação do SQL Server ou um erro transitório na SQL Database do Azure."
keywords: "ligação SQL, cadeia de ligação, problemas de conectividade, erro transitório, erro de ligação"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/29/2017
ms.author: daleche
ms.openlocfilehash: 7d393cd08ef5c20ef680e4e1ab3aded191abe932
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Resolver problemas, diagnosticar e prevenir erros de ligação e erros transitórios do SQL para a Base de Dados SQL
Este artigo descreve como impedir, resolver problemas, diagnosticar e atenuar os erros de ligação e erros transitórios que a aplicação cliente encontra quando interage com a SQL Database do Azure. Saiba como configurar a lógica de repetição, criar a cadeia de ligação e ajustar as outras definições de ligação.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Transitório erros (falhas transitórias)
Um erro transitório, também conhecido como um erro transitório, tem uma causa subjacente que em breve resolve-se automaticamente. Uma causa ocasional de erros transitórios é quando o sistema do Azure rapidamente altera recursos de hardware para um melhor balanceamento de carga várias cargas de trabalho. Na maioria destes eventos de reconfiguração concluir em menos de 60 segundos. Durante este intervalo de tempo de reconfiguração, poderá ter problemas de conectividade à base de dados do SQL Server. As aplicações que se ligar à base de dados do SQL Server devem basear-se esperar estes erros transitórios. Para lidar com os mesmos, implemente a lógica de repetição no respetivo código em vez de analisá-las aos utilizadores como erros de aplicações.

Se o seu programa cliente utiliza ADO.NET, o programa está receber a indicação sobre o erro transitório por throw de **SqlException**. Comparar o **número** propriedade face à lista de erros transitórios que se encontram perto da parte superior do artigo [códigos de erro do SQL Server para as aplicações de cliente de base de dados SQL](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-vs-command"></a>Ligação vs. o comando
Repita a ligação do SQL Server ou estabelecê-la novamente, dependendo do seguinte:

* **Ocorre um erro transitório durante uma tentativa de ligação**: depois de um atraso de vários segundos, tente novamente a ligação.
* **Ocorre um erro transitório durante um comando de consulta SQL**: imediatamente não repita o comando. Em vez disso, após um atraso raiz estabelece a ligação. Em seguida, repita o comando.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

## <a name="retry-logic-for-transient-errors"></a>Repita a lógica nos erros transitórios
Programas de cliente que ocasionalmente encontrarem um erro transitório são mais robustos quando estas contêm a lógica de repetição.

Quando o programa comunica com a base de dados SQL através de middleware de terceiros, solicitar ao fornecedor indica se o middleware contém a lógica de repetição de erros transitórios.

<a id="principles-for-retry" name="principles-for-retry"></a>

### <a name="principles-for-retry"></a>Princípios para repetição
* Se o erro transitório, tentar para abrir uma ligação.
* Não repita diretamente uma instrução SQL SELECT que falhou com um erro transitório.
  * Em vez disso, estabelecer uma ligação nova e, em seguida, repita o SELECIONAR.
* Quando uma instrução SQL UPDATE falha com um erro transitório, estabelece uma ligação nova antes de repetir a ATUALIZAÇÃO.
  * A lógica de repetição tem de garantir que a transação de base de dados completo foi concluída ou que toda a transação é revertida.

### <a name="other-considerations-for-retry"></a>Outras considerações para repetição
* Um programa de batch que for iniciado após as horas de trabalho e conclusão antes de manhã pode suportar ser muito patient com longos intervalos de tempo entre as respetivas tentativas automaticamente.
* Um programa de interface de utilizador deve ter em conta a tendência humana para desistiam após demasiado longa uma espera.
  * A solução não tem uma nova tentativa cada alguns segundos, porque essa política pode inundar o sistema com pedidos.

### <a name="interval-increase-between-retries"></a>Aumento do intervalo entre tentativas
Recomendamos que aguarde 5 segundos antes de repetir a primeira. Repetir após um atraso mais curto que riscos de 5 segundos overwhelming o serviço em nuvem. Para cada repetição subsequente, o atraso deve aumentar exponencialmente, até um máximo de 60 segundos.

Para ver um debate do período de bloqueio para clientes que utilizam ADO.NET, consulte [ligação do SQL Server agrupamento (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Pode também querer definir um número máximo de tentativas antes do programa automática termina.

### <a name="code-samples-with-retry-logic"></a>Exemplos de código com a lógica de repetição
Exemplos de código com a lógica de repetição estão disponíveis em:

- [Resiliently se ligar ao SQL Server com ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
- [Resiliently se ligar ao SQL Server com o PHP][step-4-connect-resiliently-to-sql-with-php-p42h]

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

### <a name="test-your-retry-logic"></a>Testar a lógica de repetição
Para testar a lógica de repetição, tem de simular ou originar um erro que pode ser corrigido enquanto o programa ainda está em execução.

#### <a name="test-by-disconnecting-from-the-network"></a>Testar através da rede
Uma forma de testar a sua lógica de repetição é desligar o computador de cliente da rede, enquanto o programa está em execução. O erro é:

* **SqlException.Number** = 11001
* Mensagem: "o sistema anfitrião não é conhecido"

Como parte da primeira tentativa de repetição, o programa pode corrigir o misspelling e, em seguida, tentar estabelecer ligação.

Para fazer este teste práticas, desligue o computador a partir da rede antes de iniciar o programa. Em seguida, o seu programa reconhece um parâmetro de tempo de execução que faz com que o programa para:

* Adicione temporariamente 11001 a respetiva lista de erros a considerar como transitório.
* Tente estabelecer ligação primeiro como habitualmente.
* Depois de se encontrar o erro, remova 11001 da lista.
* Apresenta uma mensagem que informa o utilizador ligue o computador da rede.
   * Colocar em pausa mais execução através de **Console.ReadLine** método ou uma caixa de diálogo com um botão OK. O utilizador premir a tecla Enter depois do computador está ligado à rede.
* Tentativa de ligar, era esperado com êxito.

#### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Testar misspelling o nome de base de dados ao ligar
O programa pode propositadamente misspell o nome de utilizador antes da primeira tentativa de ligação. O erro é:

* **SqlException.Number** = 18456
* Mensagem: "início de sessão do utilizador 'WRONG_MyUserName'."

Como parte da primeira tentativa de repetição, o programa pode corrigir o misspelling e, em seguida, tentar estabelecer ligação.

Para fazer este teste práticos, o seu programa reconhece um parâmetro de tempo de execução que faz com que o programa a:

* Adicione temporariamente 18456 a respetiva lista de erros a considerar como transitório.
* Adicione propositadamente 'WRONG_' para o nome de utilizador.
* Depois de se encontrar o erro, remova 18456 da lista.
* Remova 'WRONG_' o nome de utilizador.
* Tentativa de ligar, era esperado com êxito.


<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

## <a name="net-sqlconnection-parameters-for-connection-retry"></a>Parâmetros de .NET SqlConnection para tentativas de ligação
Se o seu programa cliente estabelecerá a ligação à base de dados do SQL Server utilizando a classe do .NET Framework **SqlConnection**, utilize o .NET 4.6.1 ou posterior (ou .NET Core) para que possa utilizar a funcionalidade de tentativas de ligação. Para obter mais informações sobre a funcionalidade, consulte [esta página Web](http://go.microsoft.com/fwlink/?linkid=393996).

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->

Quando criar o [cadeia de ligação](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) para sua **SqlConnection** objeto, coordenar os valores entre os seguintes parâmetros:

* **ConnectRetryCount**:&nbsp;&nbsp;a predefinição é 1. O intervalo é de 0 e 255.
* **ConnectRetryInterval**:&nbsp;&nbsp;predefinido é de 1 segundo. O intervalo é 1 e 60.
* **Tempo limite da ligação**:&nbsp;&nbsp;predefinição é 15 segundos. O intervalo é de 0 e 2147483647.

Especificamente, os valores que escolheu devem certificar-true de igualdade seguintes:

Tempo limite da ligação = ConnectRetryCount * ConnectionRetryInterval

Por exemplo, se a contagem de é igual a 3 e o intervalo de é igual a 10 segundos, um tempo limite de apenas 29 segundos não dar ao sistema tempo suficiente para a repetição de terceiro e final ligar: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

## <a name="connection-vs-command"></a>Ligação vs. o comando
O **ConnectRetryCount** e **ConnectRetryInterval** parâmetros permitem a **SqlConnection** objeto repita a operação de ligação sem informar ou bothering sua programa, tais como devolver controlo para o seu programa. As repetições podem ocorrer nas seguintes situações:

* chamada de método mySqlConnection.Open
* chamada de método mySqlConnection.Execute

Não há um subtlety. Se ocorrer um erro transitório ao seu *consulta* está a ser executado, o **SqlConnection** objeto não repita a operação ligar. Certamente não repita a consulta. No entanto, **SqlConnection** muito rapidamente verifica a ligação antes de enviar a consulta para execução. Se a verificação rápida Deteta um problema de ligação, **SqlConnection** repetem a operação ligar. Se a tentativa for bem sucedida, a consulta é enviada para execução.

### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>ConnectRetryCount deve ser combinada com a lógica de repetição de aplicação?
Suponha que tem da aplicação lógica de repetição de personalizado robusto. -Pode repita a operação ligar quatro vezes. Se adicionar **ConnectRetryInterval** e **ConnectRetryCount** = 3 para a cadeia de ligação, irá aumentar a contagem de repetições e 4 * 3 = 12 tentativas. Poderá já não tenciona um número elevado de tentativas.


<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-sql-database"></a>Ligações à base de dados do SQL Server
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Ligação: Cadeia de ligação
A cadeia de ligação necessárias ligar à base de dados do SQL Server é ligeiramente diferente da cadeia utilizada para ligar ao SQL Server. Pode copiar a cadeia de ligação da base de dados a partir de [portal do Azure](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Ligação: Endereço IP
Tem de configurar o servidor de base de dados SQL para aceitar comunicações do endereço IP do computador que aloja o seu programa cliente. Para configurar esta configuração, edite as definições de firewall através de [portal do Azure](https://portal.azure.com/).

Caso se esqueça configurar o endereço IP, o programa irá falhar com uma mensagem de erro à mão que indica o endereço IP necessário.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Para obter mais informações, consulte [configurar as definições da firewall da SQL Database](sql-database-configure-firewall-settings.md).
<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Ligação: portas
Normalmente, terá de garantir que só suporta a porta 1433 é aberta para comunicação de saída no computador que aloja o seu programa cliente.

Por exemplo, quando o programa de cliente é alojado num computador Windows, pode utilizar o Windows Firewall no anfitrião para abrir a porta 1433.

1. Abra o painel de controlo.

2. Selecione **controlam todos os itens do painel** > **Firewall do Windows** > **definições avançadas** > **regras de saída**   >  **Ações** > **nova regra**.

Se o seu programa cliente está alojado numa máquina virtual do Azure (VM), leia [portas para além de 1433 para ADO.NET 4.5 e a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

Para obter informações gerais sobre a configuração de portas e endereços IP, consulte [firewall da SQL Database do Azure](sql-database-firewall-configure.md).

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Ligação: ADO.NET 4.6.1
Se o programa que utiliza classes ADO.NET como **SqlConnection** para ligar à base de dados do SQL Server, recomendamos que utilize o .NET Framework versão 4.6.1 ou posterior.

ADO.NET 4.6.1:

* Para a base de dados do SQL Server, o fiabilidade é melhorada quando abrir uma ligação utilizando o **SqlConnection.Open** método. O **abra** método incorpora agora mecanismos de repetição de melhor esforço em resposta a falhas transitórias para determinados erros dentro do período de tempo limite de ligação.
* Agrupamento de ligações é suportada, que inclui uma verificação de eficiente do que o objeto de ligação fornece o seu programa está a funcionar.

Quando utiliza um objeto de ligação de um agrupamento de ligação, recomendamos que o seu programa temporariamente fechar a ligação quando não é imediatamente em utilização. Não é Caro volte a abrir uma ligação, mas é criar uma nova ligação.

Se utilizar ADO.NET 4.0 ou anterior, recomendamos que Atualize para o ADO.NET mais recente. A partir de Novembro de 2015, pode [transferir ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnóstico
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnósticos: Testar se podem ligar utilitários
Se o programa falhar a ligação à base de dados do SQL Server, é uma opção de diagnóstico tentar estabelecer ligação com um programa de utilitário. Idealmente, o utilitário liga-se utilizando a mesma biblioteca que utiliza o seu programa.

Em qualquer computador Windows, pode tentar estes utilitários:

* SQL Server Management Studio (ssms.exe), que estabelecerá a ligação utilizando ADO.NET
* SQLCMD.exe, que estabelecerá a ligação utilizando [ODBC](http://msdn.microsoft.com/library/jj730308.aspx)

Depois do programa está ligado, teste se funciona de uma consulta de SQL SELECT curta.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnósticos: Verificar as portas abertas
Se suspeitar que as tentativas de ligação falharem devido a problemas de porta, pode executar um utilitário no seu computador que comunica sobre as configurações de porta.

No Linux, os seguintes utilitários podem ser úteis:

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * Altere o valor de exemplo para ser o seu endereço IP.

No Windows, o [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) utilitário poderão ser úteis. Eis uma execução de exemplo que consultados a situação de porta num servidor de base de dados do SQL Server e que foi executada num computador portátil:

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnóstico: Os erros de registo
Um problema intermitente é por vezes, melhor diagnosticado por deteção de um padrão geral sobre dias ou semanas.

O cliente pode ajudar um diagnóstico pelo registo de todos os erros que encontrar. Poderá correlacionar as entradas de registo com dados de erro que a base de dados SQL os registos de si próprio internamente.

Enterprise biblioteca 6 (EntLib60) oferece classes .NET gerida para ajudá-lo com o registo. Para obter mais informações, consulte [5 - tão fácil como baixar desativar um registo: utilizar Logging Application Block](http://msdn.microsoft.com/library/dn440731.aspx).

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnósticos: Examine os registos do sistema erros
Seguem-se algumas instruções Transact-SQL SELECT que a consultar os registos de erros e outras informações.

| Consulta de registo | Descrição |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |O [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) vista oferece informações sobre eventos individuais, que inclui algumas que podem provocar erros transitórios ou falhas de conectividade.<br/><br/>Idealmente, pode correlacionar os **start_time** ou **end_time** valores com informações sobre quando o seu programa cliente teve problemas.<br/><br/>Tem de ligar para o *mestre* base de dados para executar esta consulta. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |O [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) vista oferece agregadas contagens de tipos de evento para obter diagnósticos adicionais.<br/><br/>Tem de ligar para o *mestre* base de dados para executar esta consulta. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnóstico: Pesquisa para eventos de problemas no registo de base de dados SQL
Pode procurar entradas sobre eventos de problemas no registo de base de dados SQL. Tente a seguinte instrução Transact-SQL SELECT no *mestre* base de dados:

```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Alguns linhas devolvidas de sys.fn_xe_telemetry_blob_target_read_file
O exemplo seguinte mostra uma linha devolvida poderá aspeto. Os valores nulos apresentados, muitas vezes, não são nulos nas outras linhas.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Biblioteca de Enterprise 6
Enterprise biblioteca 6 (EntLib60) é uma estrutura de .NET classes que ajuda a implementar clientes robustos de serviços em nuvem, é um dos quais o serviço de base de dados SQL. Para localizar tópicos dedicados a cada área na qual EntLib60 pode ajudá-lo, consulte [Enterprise biblioteca 6 - Abril de 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx).

Lógica de repetição para processamento de erros transitórios é uma área na qual EntLib60 pode ajudá-lo. Para obter mais informações, consulte [4 - Perseverance, segredo de todos os triumphs: utilizar o bloco de aplicação de processamento de falhas transitórias](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx).

> [!NOTE]
> O código de origem para EntLib60 está disponível para transferência pública a partir do [Centro de transferências](http://go.microsoft.com/fwlink/p/?LinkID=290898). A Microsoft tem não planos para tornar mais atualizações de funcionalidade ou atualizações de manutenção para EntLib.
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Classes de EntLib60 para erros transitórios e tente novamente
As seguintes classes de EntLib60 são particularmente úteis para a lógica de repetição. Todas estas classes são localizadas nos ou sob o espaço de nomes **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**.

No espaço de nomes **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

* **RetryPolicy** classe
  
  * **ExecuteAction** método
* **ExponentialBackoff** classe
* **SqlDatabaseTransientErrorDetectionStrategy** classe
* **ReliableSqlConnection** classe
  
  * **ExecuteCommand** método

No espaço de nomes **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

* **AlwaysTransientErrorDetectionStrategy** classe
* **NeverTransientErrorDetectionStrategy** classe

Seguem-se algumas hiperligações para informações sobre EntLib60:

* Transferência de livro livre: [guia para programadores do Microsoft Enterprise biblioteca, edição 2nd](http://www.microsoft.com/download/details.aspx?id=41145).
* Melhores práticas: [Repita orientações gerais](../best-practices-retry-general.md) tem um debate aprofundado excelente de lógica de repetição.
* Transferência de NuGet: [Enterprise biblioteca - transitório falhas de processamento aplicação Bloco 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: O bloco de registo
* O bloco de registo é uma solução de elevada disponibilidade flexível e configurável que pode utilizar para:
  
  * Criar e armazenar mensagens de registo numa grande variedade de localizações.
  * Categorizar e filtrar mensagens.
  * Recolha a nível contextual das informações que são os requisitos de registo útil para depuração e rastreio, bem como para auditoria e geral.
* O bloco de registo deduz a funcionalidade de registo de destino de registo para que o código de aplicação é consistente, independentemente da localização e o tipo de arquivo de registo de destino.

Para obter mais informações, consulte [5 - tão fácil como baixar desativar um registo: utilizar Logging Application Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx).

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>Código de origem do método EntLib60 IsTransient
Em seguida, a partir de **SqlDatabaseTransientErrorDetectionStrategy** classe, é o código de origem do c# para o **IsTransient** método. O código fonte esclarece que erros foram considerados transitório e worthy de repetição, a partir de Abril de 2013.

```csharp
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Passos seguintes
* Para obter mais informações sobre como resolver outros problemas de ligação de base de dados SQL comuns, consulte [resolver problemas de ligação à SQL Database do Azure](sql-database-troubleshoot-common-connection-issues.md).
* [Bibliotecas de ligação para base de dados SQL e SQL Server](sql-database-libraries.md)
* [Ligação do SQL Server agrupamento (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)
* [*Repetir* é um 2.0 Apache licenciado para fins gerais de repetir a biblioteca de escrita em Python,](https://pypi.python.org/pypi/retrying) para simplificar a tarefa de adicionar o comportamento de repetição para praticamente qualquer coisa.


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

