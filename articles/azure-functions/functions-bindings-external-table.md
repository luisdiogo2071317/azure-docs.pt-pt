---
title: Associação de tabelas externa para as funções do Azure (experimental)
description: Utilizar enlaces de tabela externa nas funções do Azure
services: functions
author: craigshoemaker
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: cshoe
ms.openlocfilehash: 62924488b776a1a89e1abf492db1881a44585b1a
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52997817"
---
# <a name="external-table-binding-for-azure-functions-experimental"></a>Associação de tabelas externa para as funções do Azure (experimental)

Este artigo explica como trabalhar com dados tabulares em fornecedores de SaaS, como o Sharepoint e Dynamics, nas funções do Azure. Funções do Azure suporta de entrada e saída enlaces para tabelas externas.

> [!IMPORTANT]
> O enlace da tabela externa é experimental e nunca atinja o estado de geralmente disponível (GA). Está incluído apenas no Azure funciona 1.x, e não existirem não existem planos para adicioná-lo para as funções do Azure 2.x. Para cenários que exigem acesso a dados em fornecedores de SaaS, considere a utilização [aplicações lógicas que chamam funções](functions-twitter-email.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>Ligações de API

Enlaces de tabelas tirar partido das ligações externas de API para autenticar com fornecedores de SaaS de terceiros. 

Quando atribui uma ligação pode criar uma nova ligação de API ou utilizar uma ligação de API existente no mesmo grupo de recursos.

### <a name="available-api-connections-tables"></a>Ligações de API disponíveis (tabelas)

|Conector|Acionador|Input|Saída|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 para operações](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Folhas do Google](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 for Financials](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle Database](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Common Data Service](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](https://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x

> [!NOTE]
> Ligações externas de tabela, também podem ser utilizadas no [do Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="creating-an-api-connection-step-by-step"></a>Criar uma ligação de API: passo a passo

1. Na página de portal do Azure para a sua aplicação de função, selecione o sinal de adição (**+**) para criar uma função.

1. Na **cenário** caixa, selecione **Experimental**.

1. Selecione **tabela externa**.

1. Selecione um idioma.

2. Sob **ligação de tabela externa**, selecione uma ligação existente ou selecione **novo**.

1. Para uma nova ligação, configure as definições e selecione **autorizar**.

1. Selecione **criar** para criar a função.

1. Selecione **integrar > tabela externa**.

1. Configure a ligação para utilizar a sua tabela de destino. Estas definições irão variar entre fornecedores de SaaS. Exemplos estão incluídos na secção seguinte.

## <a name="example"></a>Exemplo

Este exemplo liga a uma tabela chamada "Contact" com Id, FirstName e LastName, colunas. O código lista as entidades de contacto na tabela e regista os nomes e apelidos.

Aqui está o *Function* ficheiro:

```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```

Aqui está o código de script do c#:

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retrieve table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

### <a name="sql-server-data-source"></a>Origem de dados do SQL Server

Para criar uma tabela no SQL Server para utilizar com este exemplo, eis um script. `dataSetName` é "padrão".

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets-data-source"></a>Origem de dados do Google Sheets

Para criar uma tabela para utilizar com este exemplo no Google Docs, crie uma folha de cálculo com uma folha de cálculo denominada `Contact`. O conector não é possível utilizar o nome a apresentar a folha de cálculo. As necessidades de nome interno (em negrito) a ser utilizado como dataSetName, por exemplo: `docs.google.com/spreadsheets/d/` **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** adicionar os nomes das colunas `Id`, `LastName`, `FirstName` para a primeira linha, em seguida, preencher dados em linhas subsequentes.

### <a name="salesforce"></a>Salesforce

Para utilizar este exemplo com o Salesforce, `dataSetName` é "padrão".

## <a name="configuration"></a>Configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro.

|propriedade de Function | Descrição|
|---------|----------------------|
|**tipo** | Tem de ser definido como `apiHubTable`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure.|
|**direção** | Tem de ser definido como `in`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure. |
|**name** | O nome da variável que representa o item de evento no código de função. | 
|**ligação**| Identifica a definição de aplicação que armazena a cadeia de ligação de API. A definição de aplicação é criada automaticamente quando adicionar uma ligação de API na integrar da interface do Usuário.|
|**dataSetName**|O nome do conjunto de dados que contém a tabela de ler.|
|**tableName**|O nome da tabela|
|**entityId**|Tem de estar vazio para enlaces de tabelas.

Um conector de tabela fornece conjuntos de dados e cada conjunto de dados contém tabelas. O nome do conjunto de dados predefinido é "padrão". Os títulos de para um conjunto de dados e uma tabela em vários fornecedores de SaaS são listados abaixo:

|Conector|Conjunto de dados|Tabela|
|:-----|:---|:---| 
|**SharePoint**|Site|Lista do SharePoint
|**SQL**|Base de Dados|Tabela 
|**Folha de cálculo do Google**|Folha de cálculo|Folha de Cálculo 
|**Excel**|Ficheiro do Excel|Folha 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)
