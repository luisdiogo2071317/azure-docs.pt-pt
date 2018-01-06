---
title: "Vínculo de tabela externo para as funções do Azure (experimental)"
description: "Utilizar enlaces de tabela externa em funções do Azure"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 8a4358fa67e45d0b7a2df1519d649099b5ef5850
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="external-table-binding-for-azure-functions-experimental"></a>Vínculo de tabela externo para as funções do Azure (experimental)

Este artigo explica como trabalhar com dados de tabela em fornecedores de SaaS, como o Sharepoint e Dynamics, nas funções do Azure. Funções do Azure suporta entrada e saída enlaces para as tabelas externas.

> [!IMPORTANT]
> O vínculo de tabela externa é experimental e poderá nunca alcançam Estado geralmente disponível (GA). Está incluído apenas no Azure funciona 1. x e não existirem não planos para adicioná-lo para as funções do Azure 2. x. Para cenários que requerem acesso aos dados de fornecedores de SaaS, considere a utilização [as logic apps que chamam para funções](functions-twitter-email.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>Ligações de API

Enlaces de tabela tirar partido das ligações externas de API para autenticar com fornecedores de SaaS de terceiros. 

Ao atribuir um enlace pode criar uma nova ligação de API ou utilizar uma ligação de API existente no mesmo grupo de recursos.

### <a name="available-api-connections-tables"></a>Ligações de API disponíveis (tabelas)

|Conector|Acionador|Input|Saída|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 para operações](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Folhas do Google](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 para Financials](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle Database](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Serviço de dados comuns](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](http://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x

> [!NOTE]
> Ligações de tabela externas também podem ser utilizadas em [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="creating-an-api-connection-step-by-step"></a>Criar uma ligação de API: passo a passo

1. Na página de portal do Azure para a sua aplicação de função, selecione o sinal de adição (**+**) para criar uma função.

1. No **cenário** caixa, selecione **Experimental**.

1. Selecione **tabela externa**.

1. Selecione um idioma.

2. Em **ligação de tabela externa**, selecione uma ligação existente ou selecione **novo**.

1. Para uma nova ligação, configure as definições e selecione **autorizar**.

1. Selecione **criar** para criar a função.

1. Selecione **integrar > tabela externa**.

1. Configure a ligação para utilizar a tabela de destino. Estas definições irão variar entre os fornecedores de SaaS. Os exemplos são incluídos na secção seguinte.

## <a name="example"></a>Exemplo

Neste exemplo estabelece ligação a uma tabela com o nome "Contacte" com o Id, FirstName e LastName, colunas. O código de lista de entidades de contacto na tabela e regista os nomes de primeiro e últimos.

Eis o *function.json* ficheiro:

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

Eis o código de script do c#:

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

Para criar uma tabela no SQL Server para utilizar com este exemplo, eis um script. `dataSetName`é "predefinida".

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

### <a name="google-sheets-data-source"></a>Origem de dados de folhas do Google

Para criar uma tabela a utilizar com este exemplo no Google Docs, criar uma folha de cálculo com uma folha de cálculo denominada `Contact`. O conector não é possível utilizar o nome a apresentar a folha de cálculo. As necessidades de nome interno (em negrito) para ser utilizado como dataSetName, por exemplo: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  adicionar os nomes das colunas `Id`, `LastName`, `FirstName` para a primeira linha, em seguida, preencher dados em linhas subsequentes.

### <a name="salesforce"></a>Salesforce

Para utilizar este exemplo com Salesforce, `dataSetName` é "predefinida".

## <a name="configuration"></a>Configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiro.

|propriedade de Function.JSON | Descrição|
|---------|----------------------|
|**tipo** | tem de ser definido como `apiHubTable`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure.|
|**direção** | tem de ser definido como `in`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure. |
|**nome** | O nome da variável que representa o item de eventos no código da função. | 
|**ligação**| Identifica a definição de aplicação que armazena a cadeia de ligação de API. A definição de aplicação é criada automaticamente quando adicionar uma ligação de API a integrar IU.|
|**dataSetName**|O nome do conjunto de dados que contém a tabela a ler.|
|**tableName**|O nome da tabela|
|**entityId**|Deve estar vazio para enlaces de tabela.

Um conector tabela fornece conjuntos de dados e cada conjunto de dados contém tabelas. O nome do conjunto de dados predefinido é "predefinida". Os títulos de para um conjunto de dados e uma tabela de vários fornecedores de SaaS são listados abaixo:

|Conector|Conjunto de dados|Tabela|
|:-----|:---|:---| 
|**SharePoint**|Site|Lista do SharePoint
|**SQL**|Base de Dados|Tabela 
|**Folha do Google**|Folha de cálculo|Folha de Cálculo 
|**Excel**|Ficheiro do Excel|Folha 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)
