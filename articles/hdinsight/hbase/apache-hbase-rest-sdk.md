---
title: Utilizar o SDK .NET do HBase - o Azure HDInsight | Microsoft Docs
description: Utilize o SDK .NET do HBase para criar e eliminar as tabelas e para ler e escrever dados.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: ashishth
ms.openlocfilehash: 083150fe5f8787ba791d3d692db73c5156f11e55
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-hbase-net-sdk"></a>Utilizar o SDK .NET do HBase

[HBase](apache-hbase-overview.md) fornece duas opções principais para trabalhar com os seus dados: [ramo de registo de consultas e chamadas à API RESTful do HBase](apache-hbase-tutorial-get-started-linux.md). Pode trabalhar diretamente com a REST API utilizando o `curl` comando ou um utilitário semelhante.

Para aplicações de c# e .NET, o [biblioteca de cliente do Microsoft HBase REST para .NET](https://www.nuget.org/packages/Microsoft.HBase.Client/) fornece uma biblioteca de cliente por cima da API de REST de HBase.

## <a name="install-the-sdk"></a>Instalar o SDK

O SDK .NET do HBase é fornecido como um pacote NuGet, que pode ser instalado a partir do Visual Studio **consola do Gestor de pacotes NuGet** com o seguinte comando:

    Install-Package Microsoft.HBase.Client

## <a name="instantiate-a-new-hbaseclient-object"></a>Instanciar um novo objeto de HBaseClient

Para utilizar o SDK, instanciar um novo `HBaseClient` objeto, passando na `ClusterCredentials` composto o `Uri` para o cluster e o nome de utilizador do Hadoop e a palavra-passe.

```csharp
var credentials = new ClusterCredentials(new Uri("https://CLUSTERNAME.azurehdinsight.net"), "USERNAME", "PASSWORD");
client = new HBaseClient(credentials);
```

Substitua CLUSTERNAME com o nome do cluster HBase do HDInsight e o nome de utilizador e a palavra-passe com as credenciais de Hadoop especificadas na criação do cluster. O nome de utilizador do Hadoop predefinido é **admin**.

## <a name="create-a-new-table"></a>Criar uma nova tabela

HBase armazena dados em tabelas. Uma tabela é composta por um *Rowkey*, a chave primária e um ou mais grupos de colunas chamado *famílias de coluna*. Os dados de cada tabela horizontalmente são distribuídos por um intervalo de Rowkey para *regiões*. Cada região tem uma chave de início e de fim. Uma tabela pode ter um ou mais regiões. À medida que aumenta os dados na tabela, o HBase divide grandes regiões numa regiões mais pequenas. Regiões são armazenadas no *servidores região*, onde um servidor de região pode armazenar várias regiões.

Os dados fisicamente são armazenados no *HFiles*. Um único HFile contém dados para uma tabela, uma região e uma família de colunas. Linhas HFile são armazenadas ordenado em Rowkey. Cada HFile tem um *árvore B +* índice para obtenção speedy das linhas.

Para criar uma nova tabela, especifique um `TableSchema` e colunas. O seguinte código verifica se a tabela 'RestSDKTable' já existe - caso contrário, a tabela é criado.

```csharp
if (!client.ListTablesAsync().Result.name.Contains("RestSDKTable"))
{
    // Create the table
    var newTableSchema = new TableSchema {name = "RestSDKTable" };
    newTableSchema.columns.Add(new ColumnSchema() {name = "t1"});
    newTableSchema.columns.Add(new ColumnSchema() {name = "t2"});

    await client.CreateTableAsync(newTableSchema);
}
```

Esta nova tabela tem famílias de coluna dois, t1 e t2. Uma vez que as famílias de coluna são armazenadas em separado no HFiles diferentes, faz sentido ter uma família de coluna distinta para dados consultados com frequência. A seguir [inserir dados](#insert-data) exemplo, são adicionadas colunas para a família de coluna t1.

## <a name="delete-a-table"></a>Eliminar uma tabela

Para eliminar uma tabela:

```csharp
await client.DeleteTableAsync("RestSDKTable");
```

## <a name="insert-data"></a>Inserir dados

Para inserir dados, especifique uma chave de linha exclusivo como o identificador de linha. Todos os dados são armazenados num `byte[]` matriz. O código seguinte define e adiciona o `title`, `director`, e `release_date` colunas para a família de coluna t1, como estas colunas são acedidos com maior frequência. O `description` e `tagline` são adicionadas colunas para a família de coluna no t2. Pode particionar os dados para famílias de coluna conforme necessário.

```csharp
var key = "fifth_element";
var row = new CellSet.Row { key = Encoding.UTF8.GetBytes(key) };
var value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:title"),
    data = Encoding.UTF8.GetBytes("The Fifth Element")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:director"),
    data = Encoding.UTF8.GetBytes("Luc Besson")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t1:release_date"),
    data = Encoding.UTF8.GetBytes("1997")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t2:description"),
    data = Encoding.UTF8.GetBytes("In the colorful future, a cab driver unwittingly becomes the central figure in the search for a legendary cosmic weapon to keep Evil and Mr Zorg at bay.")
};
row.values.Add(value);
value = new Cell
{
    column = Encoding.UTF8.GetBytes("t2:tagline"),
    data = Encoding.UTF8.GetBytes("The Fifth is life")
};
row.values.Add(value);

var set = new CellSet();
set.rows.Add(row);

await client.StoreCellsAsync("RestSDKTable", set);
```

HBase implementa BigTable, pelo que o formato dos dados o seguinte aspeto:

![Utilizador com função de utilizador de Cluster](./media/apache-hbase-rest-sdk/table.png)

## <a name="select-data"></a>Selecionar dados

Ler dados de uma tabela de HBase, transferir a chave de linha e nome de tabela para o `GetCellsAsync` método para devolver o `CellSet`.

```csharp
var key = "fifth_element";

var cells = await client.GetCellsAsync("RestSDKTable", key);
// Get the first value from the row.
Console.WriteLine(Encoding.UTF8.GetString(cells.rows[0].values[0].data));
// Get the value for t1:title
Console.WriteLine(Encoding.UTF8.GetString(cells.rows[0].values
    .Find(c => Encoding.UTF8.GetString(c.column) == "t1:title").data));
// With the previous insert, it should yield: "The Fifth Element"
```

Neste caso, o código devolve apenas a primeira linha correspondente, como deve apenas ser uma linha para uma chave exclusiva. O valor devolvido é alterado em `string` formatar a partir de `byte[]` matriz. Também pode converter o valor para outros tipos de, por exemplo, um número inteiro para a data de lançamento de filmes:

```csharp
var releaseDateField = cells.rows[0].values
    .Find(c => Encoding.UTF8.GetString(c.column) == "t1:release_date");
int releaseDate = 0;

if (releaseDateField != null)
{
    releaseDate = Convert.ToInt32(Encoding.UTF8.GetString(releaseDateField.data));
}
Console.WriteLine(releaseDate);
// Should return 1997
```

## <a name="scan-over-rows"></a>Analisar através de linhas

Utiliza o HBase `scan` para obter uma ou mais linhas. Este exemplo pedidos várias linhas em lotes num número 10 e obtém dados cujos valores de chave são entre 25 e 35. Depois de todas as linhas a obter, elimine scanner para limpar os recursos.

```csharp
var tableName = "mytablename";

// Assume the table has integer keys and we want data between keys 25 and 35
var scanSettings = new Scanner()
{
    batch = 10,
    startRow = BitConverter.GetBytes(25),
    endRow = BitConverter.GetBytes(35)
};
RequestOptions scanOptions = RequestOptions.GetDefaultOptions();
scanOptions.AlternativeEndpoint = "hbaserest0/";
ScannerInformation scannerInfo = null;
try
{
    scannerInfo = await client.CreateScannerAsync(tableName, scanSettings, scanOptions);
    CellSet next = null;
    while ((next = client.ScannerGetNextAsync(scannerInfo, scanOptions).Result) != null)
    {
        foreach (var row in next.rows)
        {
            // ... read the rows
        }
    }
}
finally
{
    if (scannerInfo != null)
    {
        await client.DeleteScannerAsync(tableName, scannerInfo, scanOptions);
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao exemplo Apache HBase no HDInsight](apache-hbase-tutorial-get-started-linux.md)
* Criar uma aplicação de ponto a ponto com [analisar dados de sentimento do Twitter em tempo real com o HBase](../hdinsight-hbase-analyze-twitter-sentiment.md)
