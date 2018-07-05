---
title: Origens de dados suportadas no Azure Analysis Services | Documentos da Microsoft
description: Descreve as origens de dados suportadas para modelos de dados no Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f6ad95eb45cc208fe2289cb2095214f98a0b250b
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442394"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Origens de dados suportadas no Azure Analysis Services

Origens de dados e conectores mostradas em obter dados ou o Assistente de importação no Visual Studio são apresentadas para o Azure Analysis Services e o SQL Server Analysis Services. No entanto, nem todas as origens de dados e conectores mostrados são suportadas no Azure Analysis Services. Os tipos de origens de dados, que pode ligar a dependem muitos fatores, tais como modelam o nível de compatibilidade, conectores de dados disponíveis, tipo de autenticação, fornecedores e suporte de gateway de dados no local. 

## <a name="azure-data-sources"></a>Origens de dados do Azure

|Origem de dados  |Dentro da memória  |DirectQuery  |
|---------|---------|---------|
|Base de Dados SQL do Azure     |   Sim      |    Sim      |
|Azure SQL Data Warehouse     |   Sim      |   Sim       |
|Armazenamento de Blobs do Azure *     |   Sim       |    Não      |
|Armazenamento de tabelas do Azure *    |   Sim       |    Não      |
|Azure Cosmos DB *     |  Sim        |  Não        |
|Azure Data Lake Store *     |   Sim       |    Não      |
|O Azure HDInsight HDFS *     |     Sim     |   Não       |
|O Azure HDInsight Spark *     |   Sim       |   Não       |
||||

\* Modelos 1400 em tabela só.

**Fornecedor**   
Dentro da memória e os modelos do DirectQuery ligar a origens de dados do Azure utilizam o fornecedor de dados do .NET Framework para o SQL Server.

## <a name="on-premises-data-sources"></a>Origens de dados no local

A ligar no local origens de dados de e o servidor do Azure requerem um gateway no local. Quando utilizar um gateway, os fornecedores de 64 bits são necessários.

### <a name="in-memory-and-directquery"></a>Dentro da memória e DirectQuery

|Origem de dados | Fornecedor de dentro da memória | Fornecedor de DirectQuery |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0, fornecedor Microsoft OLE DB para SQL Server, o fornecedor de dados do .NET Framework para o SQL Server | Fornecedor de dados do .NET framework para o SQL Server |
| Armazém de dados do SQL Server |SQL Server Native Client 11.0, fornecedor Microsoft OLE DB para SQL Server, o fornecedor de dados do .NET Framework para o SQL Server | Fornecedor de dados do .NET framework para o SQL Server |
| Oracle |Fornecedor Microsoft OLE DB para Oracle, o provedor de dados Oracle para .NET |Fornecedor de dados do Oracle para .NET | |
| Teradata |Fornecedor OLE DB para Teradata, fornecedor de dados do Teradata para .NET |Fornecedor de dados do Teradata para .NET | |
| | | |

### <a name="in-memory-only"></a>Na memória apenas

|Origem de dados  |  
|---------|---------|
|Base de dados de acesso     |  
|Active Directory *     |  
|Analysis Services     |  
|Analytics Platform System     |  
|Dynamics CRM *     |  
|Livro do Excel     |  
|Exchange *     |  
|Pasta *     |
|IBM Informix * (Beta) |
|Documento JSON *     |  
|Linhas de binário *     | 
|Base de Dados MySQL     | 
|Feed de OData *     |  
|Consulta ODBC     | 
|OLE DB     |   
|Base de dados Postgre SQL *    | 
|Objetos do Salesforce * |  
|Relatórios do Salesforce * |
|SAP HANA*    |  
|SAP Business Warehouse *    |  
|SharePoint*     |   
|Base de dados Sybase     |  
|Tabela XML *    |  
|||
 
\* Modelos 1400 em tabela só.

## <a name="specifying-a-different-provider"></a>Especificar um fornecedor diferente

Modelos de dados no Azure Analysis Services podem exigir provedores de dados diferentes, ao ligar a determinadas origens de dados. Em alguns casos, os modelos em tabela, ligar a origens de dados utilizar fornecedores nativos, como o SQL Server Native Client (SQLNCLI11) podem devolver um erro. Se utilizar fornecedores nativos que não seja SQLOLEDB, poderá ver a mensagem de erro: **o fornecedor 'SQLNCLI11.1' não está registada**. Ou, se tem um modelo DirectQuery, ligação a origens de dados no local e utilizar fornecedores nativos, poderá ver a mensagem de erro: **erro ao criar o conjunto de linhas OLE DB. Sintaxe incorreta perto de "LIMIT"**.

Ao migrar um modelo de tabela do SQL Server Analysis Services no local para o Azure Analysis Services, poderá ser necessário alterar o fornecedor.

**Para especificar um fornecedor**

1. No SSDT > **Explorador de modelos tabulares** > **origens de dados**, uma ligação de origem de dados com o botão direito e, em seguida, clique em **Editar origem de dados**.
2. Na **ligação editar**, clique em **avançadas** para abrir a janela de propriedades de avanço.
3. Na **definir propriedades avançadas** > **fornecedores**, em seguida, selecione o provedor apropriado.

## <a name="impersonation"></a>Representação
Em alguns casos, poderá ser necessário especificar uma conta de representação diferentes. Conta de representação pode ser especificada no Visual Studio (SSDT) ou o SSMS.

Para origens de dados no local:

* Se utilizar a autenticação de SQL, representação deve ser a conta de serviço.
* Se utilizar a autenticação do Windows, defina Windows utilizador/palavra-passe. Para o SQL Server, autenticação do Windows com uma conta de representação específico só é suportada para modelos de dados na memória.

Para origens de dados de cloud:

* Se utilizar a autenticação de SQL, representação deve ser a conta de serviço.

## <a name="next-steps"></a>Passos Seguintes
[Gateway no local](analysis-services-gateway.md)   
[Gerir o seu servidor](analysis-services-manage.md)   

