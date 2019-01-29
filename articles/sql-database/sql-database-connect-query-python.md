---
title: Utilizar o Python para consultar a Base de Dados SQL do Azure | Microsoft Docs
description: Este tópico mostra-lhe como utilizar Python para criar um programa que se liga a uma base de dados SQL do Azure e a consulta com instruções Transact-SQL.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/28/2019
ms.openlocfilehash: b611eb02203c872e3497b5b7c12acddd9eab14c0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188393"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Início rápido: Utilizar o Python para consultar uma base de dados SQL do Azure

 Este guia de introdução demonstra como utilizar o [Python](https://python.org) para ligar a uma base de dados SQL do Azure e utilizar as instruções Transact-SQL para consultar dados. Para obter mais detalhes sobre o SDK, confira nossos [referência](https://docs.microsoft.com/python/api/overview/azure/sql) documentação, o [repositório do GitHub pyodbc](https://github.com/mkleehammer/pyodbc/wiki/)e um [pyodbc exemplo](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este início rápido, certifique-se de que tem o seguinte:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Python e software relacionado para o seu sistema operativo:
  
  - **MacOS**: Instale o Homebrew e o Python, instale o controlador ODBC e o SQLCMD e, em seguida, instale o controlador Python para SQL Server. Veja os passos 1.2, 1.3 e 2.1 no [aplicações de Python de criar através do SQL Server no macOS](https://www.microsoft.com/sql-server/developer-get-started/python/mac/). Para obter mais informações, consulte [instalar o Microsoft ODBC Driver no Linux e macOS](https://docs.microsoft.com/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).
    
  - **Ubuntu**: Instale o Python e outros pacotes necessários com `sudo apt-get install python python-pip gcc g++ build-essential`. Baixe e instale o controlador ODBC, SQLCMD e o controlador Python para SQL Server. Para obter instruções, consulte [configurar um ambiente de desenvolvimento para pyodbc desenvolvimento Python](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).
    
  - **Windows**: Instale o Python, o controlador ODBC e o SQLCMD e o controlador Python para SQL Server. Para obter instruções, consulte [configurar um ambiente de desenvolvimento para pyodbc desenvolvimento Python](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

## <a name="get-sql-server-connection-information"></a>Obter as informações de ligação do SQL server

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="create-code-to-query-your-sql-database"></a>Criar código para consultar a base de dados SQL 

1. Num editor de texto, crie um novo ficheiro designado *sqltest.py*.  
   
1. Adicione o seguinte código. Substitua os seus valores para \<servidor >, \<base de dados >, \<username >, e \<palavra-passe >.
   
   >[!IMPORTANT]
   >O código neste exemplo utiliza os dados de AdventureWorksLT de exemplo, o que pode escolher como origem ao criar a sua base de dados. Se a sua base de dados tiver dados diferentes, utilize a consulta SELECT tabelas da sua própria base de dados. 
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   driver= '{ODBC Driver 17 for SQL Server}'
   cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
   cursor = cnxn.cursor()
   cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
   row = cursor.fetchone()
   while row:
       print (str(row[0]) + " " + str(row[1]))
       row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Executar o código

1. No prompt de comando, execute o seguinte comando:

   ```cmd
   python sqltest.py
   ```

1. Certifique-se de que as primeiras linhas de categoria/produto 20 são devolvidas, em seguida, feche a janela de comando.

## <a name="next-steps"></a>Passos Seguintes

- [Criar a sua primeira base de dados SQL do Azure](sql-database-design-first-database.md)
- [Controladores Python da Microsoft para o SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Centro de programadores Python](https://azure.microsoft.com/develop/python/?v=17.23h)

