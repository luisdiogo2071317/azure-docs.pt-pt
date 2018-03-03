---
title: Origens de dados suportadas no Azure Analysis Services | Microsoft Docs
description: Descreve as origens de dados suportadas para modelos de dados no Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/27/2018
ms.author: owend
ms.openlocfilehash: 9f2a4acdd0a2b29bc1485f62c0049f0065cbf711
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Origens de dados suportadas no Azure Analysis Services

São apresentados as origens de dados e conectores apresentados no Assistente de importação no Visual Studio ou obter dados para o Azure Analysis Services e o SQL Server Analysis Services. No entanto, nem todas as origens de dados e conectores mostrados são suportados no Azure Analysis Services. Os tipos de origens de dados que pode ligar ao dependem em vários fatores, tais como modelo de nível de compatibilidade, os conectores de dados disponíveis, tipo de autenticação, fornecedores e suporte de gateway de dados no local. 

## <a name="azure-data-sources"></a>Origens de dados do Azure

|Origem de dados  |Dentro da memória  |DirectQuery  |
|---------|---------|---------|
|Base de Dados SQL do Azure     |   Sim      |    Sim      |
|Azure SQL Data Warehouse     |   Sim      |   Sim       |
|Armazenamento de Blobs do Azure *     |   Sim       |    Não      |
|Armazenamento de tabela do Azure *    |   Sim       |    Não      |
|Azure Cosmos BD (Beta) *     |  Sim        |  Não        |
|Azure Data Lake Store *     |   Sim       |    Não      |
|Azure HDInsight HDFS *     |     Sim     |   Não       |
|Azure HDInsight Spark (Beta) *     |   Sim       |   Não       |
|Base de dados do Azure para MySQL (pré-visualização) *     |   Sim       |   Não      |
|Base de dados do Azure para PostgreSQL (pré-visualização) *     | Sim         |  Não       |
||||

\* Modelos em tabela 1400 apenas.

**Fornecedor**   
Na memória e modelos DirectQuery a ligar a origens de dados do Azure utilizam o .NET Framework Data Provider para o SQL Server.

## <a name="on-premises-data-sources"></a>Origens de dados no local

A ligar no local origens de dados de e do Azure que o servidor requerem um gateway no local. Quando utilizar um gateway, fornecedores de 64 bits, são necessários.

### <a name="in-memory-and-directquery"></a>Na memória e DirectQuery

|Origem de dados | Fornecedor de memória | Fornecedor de DirectQuery |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0, fornecedor Microsoft OLE DB para SQL Server, o .NET Framework Data Provider para o SQL Server | Fornecedor de dados .NET framework para o SQL Server |
| SQL Server Data Warehouse |SQL Server Native Client 11.0, fornecedor Microsoft OLE DB para SQL Server, o .NET Framework Data Provider para o SQL Server | Fornecedor de dados .NET framework para o SQL Server |
| Oracle |Fornecedor Microsoft OLE DB para Oracle, Oracle Data Provider para .NET |Fornecedor de dados Oracle para .NET | |
| Teradata |Fornecedor OLE DB para Teradata, o fornecedor de dados Teradata para .NET |Fornecedor de dados Teradata para .NET | |
| | | |

### <a name="in-memory-only"></a>Dentro da memória apenas

|Origem de dados  |  
|---------|---------|
|Base de dados de acesso     |  
|Active Directory *     |  
|Analysis Services     |  
|Analytics Platform System     |  
|Dynamics CRM*     |  
|Livro do Excel     |  
|Exchange*     |  
|Pasta *     | 
|Documento JSON *     |  
|Linhas de binário *     | 
|Base de Dados MySQL     | 
|Feed de OData *     |  
|Consulta ODBC     | 
|OLE DB     |   
|Base de dados Postgre SQL *    | 
|SAP HANA*    |  
|SAP Business Warehouse *    |  
|SharePoint*     |   
|Base de dados Sybase     |  
|Tabela XML *    |  
|||
 
\* Modelos em tabela 1400 apenas.

## <a name="specifying-a-different-provider"></a>Especificar um fornecedor diferente

Modelos de dados no Azure Analysis Services podem necessitar de fornecedores de dados diferentes ao ligar a determinadas origens de dados. Em alguns casos, os modelos em tabela a ligar a origens de dados a utilizar fornecedores nativos, tais como o SQL Server Native Client (SQLNCLI11) podem devolver um erro. Se utilizar fornecedores nativos que não sejam SQLOLEDB, poderá ver a mensagem de erro: **o fornecedor 'SQLNCLI11.1' não está registado**. Ou, se tiver um modelo do DirectQuery a ligar a origens de dados no local e utilizar fornecedores nativos, poderá ver a mensagem de erro: **erro ao criar o conjunto de linhas OLE DB. Sintaxe incorreta perto de 'Limite'**.

Quando migrar um modelo de tabela do SQL Server Analysis Services no local ao Azure Analysis Services, poderá ser necessário alterar o fornecedor.

**Para especificar um fornecedor**

1. No SSDT > **Explorador de modelo em tabela** > **origens de dados**, faça duplo clique uma ligação à origem de dados e, em seguida, clique em **editar a origem de dados**.
2. No **editar ligação**, clique em **avançadas** para abrir a janela de propriedades avançadas.
3. No **definir propriedades avançadas** > **fornecedores**, em seguida, selecione o fornecedor apropriado.

## <a name="impersonation"></a>Representação
Em alguns casos, poderá ser necessário especificar uma conta de representação diferentes. Conta de representação pode ser especificada no Visual Studio (SSDT) ou o SSMS.

Para origens de dados no local:

* Se utilizar a autenticação de SQL, representação deve ser a conta de serviço.
* Se utilizar a autenticação do Windows, defina Windows utilizador/palavra-passe. Para o SQL Server, a autenticação do Windows com uma conta de representação específico só é suportada para modelos de dados em memória.

Para origens de dados de nuvem:

* Se utilizar a autenticação de SQL, representação deve ser a conta de serviço.

## <a name="next-steps"></a>Passos Seguintes
[Gateway no local](analysis-services-gateway.md)   
[Gerir o seu servidor](analysis-services-manage.md)   

