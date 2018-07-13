---
title: Utilizar o Python para consultar a Base de Dados SQL do Azure | Microsoft Docs
description: Este tópico mostra-lhe como utilizar o Python para criar um programa que se liga a uma base de dados SQL do Azure e a consulta com instruções Transact-SQL.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,develop apps
ms.devlang: python
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: carlrab
ms.openlocfilehash: e88c069bed40bcdf1eae9d356403cc772a11ea85
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38704778"
---
# <a name="use-python-to-query-an-azure-sql-database"></a>Utilizar o Python para consultar uma base de dados SQL do Azure

 Este guia de introdução demonstra como utilizar o [Python](https://python.org) para ligar a uma base de dados SQL do Azure e utilizar as instruções Transact-SQL para consultar dados. Para obter mais detalhes de sdk, veja a nossa documentação de [referência](https://docs.microsoft.com/python/api/overview/azure/sql), um [exemplo](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) do pyodbc e o repositório do GitHub do [pyodbc](https://github.com/mkleehammer/pyodbc/wiki/) .

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este início rápido, certifique-se de que tem o seguinte:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Uma [regra de firewall ao nível do servidor](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) para o endereço IP público do computador que utilizar para este início rápido.

- Ter instalado o Python e software relacionado para o seu sistema operativo:

    - **MacOS**: instale o Homebrew e o Python, instale o controlador ODBC e o SQLCMD e, em seguida, instale o Controlador Python para SQL Server. Veja os [Passos 1.2, 1.3 e 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/mac/).
    - **Ubuntu**: instale o Python e outros pacotes necessários e, em seguida, instale o Controlador Python para SQL Server. Veja os [Passos 1.2, 1.3 e 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu/).
    - **Windows**: instale a versão mais recente do Python (a variável de ambiente está agora configurada por si), instale o controlador ODBC e o SQLCMD e, em seguida, instale o controlador Python para SQL Server. Veja os [Passos 1.2, 1.3 e 2.1](https://www.microsoft.com/sql-server/developer-get-started/python/windows/). 

## <a name="sql-server-connection-information"></a>Informações de ligação do servidor SQL

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="insert-code-to-query-sql-database"></a>Inserir código para consultar a base de dados do SQL 

1. No seu editor de texto favorito, crie um novo ficheiro, **sqltest.py**.  

2. Substitua os conteúdos pelo código seguinte e adicione os valores corretos para o seu servidor, a sua base de dados, o seu utilizador e a sua palavra-passe.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

## <a name="run-the-code"></a>Executar o código

1. Na linha de comandos, execute os comandos seguintes:

   ```Python
   python sqltest.py
   ```

2. Confirme que as 20 linhas superiores são devolvidas e, em seguida, feche a janela da aplicação.

## <a name="next-steps"></a>Passos seguintes

- [Criar a sua primeira base de dados SQL do Azure](sql-database-design-first-database.md)
- [Controladores Python da Microsoft para SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Centro para Programadores do Python](https://azure.microsoft.com/develop/python/?v=17.23h)

