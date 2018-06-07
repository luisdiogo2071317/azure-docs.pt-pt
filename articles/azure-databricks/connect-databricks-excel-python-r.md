---
title: Ligar ao Azure Databricks do Excel, Python ou R | Microsoft Docs
description: Saiba como utilizar o controlador de Simba ligar Databricks do Azure para o Excel, Python ou R.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: bbf75a03fb771aa415a26e151614cecfaa14c485
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598883"
---
# <a name="connect-to-azure-databricks-from-excel-python-or-r"></a>Ligar ao Azure Databricks do Excel, Python ou R

Neste artigo, irá aprender a utilizar o controlador Databricks ODBC para ligar Databricks do Azure com o Microsoft Excel, Python ou R idioma. Depois de estabelecer a ligação, pode aceder aos dados do Azure Databricks dos clientes Excel, Python ou R. Também pode utilizar os clientes para analisar ainda mais os dados. 

## <a name="prerequisites"></a>Pré-requisitos

* Tem de ter uma área de trabalho do Azure Databricks, um cluster do Spark e dados de exemplo associados ao seu cluster. Se ainda não tiver estes pré-requisitos, concluir o guia de introdução no [execute uma tarefa de Spark Databricks do Azure no portal do Azure](quickstart-create-databricks-workspace-portal.md).

* Transferir o controlador Databricks ODBC do [página de transferência do controlador Databricks](https://databricks.com/spark/odbc-driver-download). Instale a versão de 32 bits ou 64 bits, dependendo da aplicação a partir de onde pretende ligar ao Azure Databricks. Por exemplo, para ligar a partir do Excel, instale a versão de 32 bits do controlador. Para ligar a partir de R e Python, instale a versão de 64 bits do controlador.

* Configure um token de acesso pessoal Databricks. Para obter instruções, consulte [Token gestão](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="set-up-a-dsn"></a>Configurar um DSN

Um nome de origem de dados (DSN) contém as informações sobre uma origem de dados específica. Um controlador ODBC tem este DSN para ligar a uma origem de dados. Nesta secção, iremos configurar um DSN que pode ser utilizada com o controlador Databricks ODBC para ligar ao Azure Databricks de clientes, como o Microsoft Excel, Python ou R.

1. A área de trabalho do Azure Databricks, navegue para o cluster Databricks.

    ![Abra Databricks cluster](./media/connect-databricks-excel-python-r/open-databricks-cluster.png "Databricks abra cluster")

2. Sob o **configuração** separador, clique em de **JDBC/ODBC** separador e copie os valores para **nome de anfitrião do servidor** e **caminho HTTP**. É necessário estes valores para concluir os passos neste artigo.

    ![Obter a configuração de Databricks](./media/connect-databricks-excel-python-r/get-databricks-jdbc-configuration.png "Databricks obter configuração")

3. No seu computador, inicie **origens de dados de ODBC** aplicação (32 bits ou 64 bits), dependendo da aplicação. Para ligar a partir do Excel, utilize a versão de 32 bits. Para ligar a partir de R e Python, utilize a versão de 64 bits.

    ![Iniciar ODBC](./media/connect-databricks-excel-python-r/launch-odbc-app.png "iniciar a aplicação ODBC")

4. Sob o **utilizador DSN** separador, clique em **adicionar**. No **criar uma nova origem de dados** caixa de diálogo, selecione o **o controlador ODBC do Simba Spark**e, em seguida, clique em **concluir**.

    ![Iniciar ODBC](./media/connect-databricks-excel-python-r/add-new-user-dsn.png "iniciar a aplicação ODBC")

5. No **o controlador ODBC do Simba Spark** diálogo caixa, forneça os valores seguintes:

    ![Configurar o DSN](./media/connect-databricks-excel-python-r/odbc-dsn-setup.png "configurar DSN")

    A tabela seguinte fornece informações sobre os valores a fornecer na caixa de diálogo.
    
    |Campo  | Valor  |
    |---------|---------|
    |**Nome da origem de dados**     | Forneça um nome para a origem de dados.        |
    |**Ou mais anfitriões**     | Forneça o valor que copiou do espaço de trabalho Databricks para *nome de anfitrião do servidor*.        |
    |**Porta**     | Introduza *443*.        |
    |**Autenticação** > **mecanismo**     | Selecione *nome de utilizador e palavra-passe*.        |
    |**Nome de utilizador**     | Introduza *token*.        |
    |**Palavra-passe**     | Introduza o valor de token que copiou do espaço de trabalho Databricks. |
    
    Execute os seguintes passos adicionais na caixa de diálogo de configuração de DSN.
    
    * Clique em **opções HTTP**. Na caixa de diálogo que se abre, cole o valor para *caminho HTTP* que copiou do Databricks área de trabalho. Clique em **OK**.
    * Clique em **SSL opções**. Na caixa de diálogo que se abre, selecione o **ativar SSL** caixa de verificação. Clique em **OK**.
    * Clique em **testar** para testar a ligação ao Azure Databricks. Clique em **OK** para guardar a configuração.
    * No **administrador de origem de dados de ODBC** caixa de diálogo, clique em **OK**.

Tem agora o DSN configurar. As secções seguintes, utiliza este DSN para ligar ao Azure Databricks do Excel, Python ou R.

## <a name="connect-from-microsoft-excel"></a>Ligar a partir do Microsoft Excel

Nesta secção, a extrair dados do Azure Databricks para Microsoft Excel utilizando o DSN que criou anteriormente. Antes de começar, certifique-se de que tem o Microsoft Excel instalado no seu computador. Pode utilizar uma versão de avaliação do Excel do [ligação de avaliação do Microsoft Excel](https://products.office.com/excel).

1. Abra um livro em branco no Microsoft Excel. Do **dados** do Friso, clique em **obter dados**. Clique em **de outras origens** e, em seguida, clique em **de ODBC**.

    ![Iniciar ODBC a partir do Excel](./media/connect-databricks-excel-python-r/launch-odbc-from-excel.png "iniciar ODBC a partir do Excel")

2. No **de ODBC** caixa de diálogo, selecione o DSN que criou anteriormente e, em seguida, clique em **OK**.

    ![Selecione o DSN](./media/connect-databricks-excel-python-r/excel-select-dsn.png "selecione DSN")

3. Se lhe forem pedidas as credenciais, para nome de utilizador, introduza **token**. Palavra-passe, forneça o valor de token obtidos a partir da área de trabalho Databricks.

    ![Forneça credenciais para Databricks](./media/connect-databricks-excel-python-r/excel-databricks-token.png "selecione DSN")

4. Na janela do navegador, selecione a tabela em Databricks que pretende carregar para o Excel e, em seguida, clique em **carregar**. 

    ![Carregar o dta para Excel](./media/connect-databricks-excel-python-r/excel-load-data.png "dta de carga para o Excel")

Depois de ter os dados no seu livro do Excel, pode efetuar operações analíticas no mesmo.

## <a name="connect-from-r"></a>Ligar a partir de R

Nesta secção, é utilizar uma linguagem de R IDE para dados de referência disponível no Azure Databricks. Antes de começar, tem de ter o seguinte instalado no computador.

* Um IDE para o idioma de R. Este artigo utiliza RStudio para ambiente de trabalho. Pode instalar a [transferências de R Studio](https://www.rstudio.com/products/rstudio/download/).
* Se utilizar RStudio para ambiente de trabalho como o IDE, também instalar o Microsoft R Client [ http://aka.ms/rclient/ ](http://aka.ms/rclient/). 

Abrir RStudio e efetue os seguintes passos:

- Referência a `RODBC` pacote. Isto permite-lhe ligar ao Databricks do Azure utilizando o DSN que criou anteriormente.
- Estabelece uma ligação o DSN a utilizar.
- Execute uma consulta de SQL Server nos dados existentes Databricks do Azure. No seguinte fragmento, *radio_sample_data* é uma tabela que já exista na Databricks do Azure.
- Execute algumas operações de consulta para verificar o resultado. 

O fragmento de código seguinte executa estas tarefas:

    # reference the 'RODBC' package
    require(RODBC)
    
    # establish a connection using the DSN you created earlier
    conn <- odbcConnect("<ENTER DSN NAME HERE>")
    
    # run a SQL query using the connection you created
    res <- sqlQuery(conn, "SELECT * FROM radio_sample_data")
    
    # print out the column names in the query output
    names(res) 
        
    # print out the number of rows in the query output
    nrow (res)

## <a name="connect-from-python"></a>Ligar a partir do Python

Nesta secção, utilize um IDE do Python (como INATIVO) para dados de referência disponível no Azure Databricks. Antes de começar, conclua os seguintes pré-requisitos:

* Instalar o Python de [aqui](https://www.python.org/downloads/). Também instalar o Python desta ligação instala INATIVO.

* Numa linha de comandos no computador, instalar o `pyodbc` pacote. Execute o seguinte comando:

      pip install pyodbc

Abrir INATIVO e efetue os seguintes passos:

- Importar o `pyodbc` pacote. Isto permite-lhe ligar ao Databricks do Azure utilizando o DSN que criou anteriormente.
- Estabelece uma ligação com o DSN que criou anteriormente.
-  Execute uma consulta de SQL Server utilizando a ligação que criou. No seguinte fragmento, *radio_sample_data* é uma tabela que já exista na Databricks do Azure.
- Efetue operações em consulta para verificar o resultado.

O fragmento de código seguinte executa estas tarefas:

```python
# import the `pyodbc` package:
import pyodbc

# establish a connection using the DSN you created earlier
conn = pyodbc.connect("DSN=<ENTER DSN NAME HERE>", autocommit = True)

# run a SQL query using the connection you created
cursor = conn.cursor()
cursor.execute("SELECT * FROM radio_sample_data")

# print the rows retrieved by the query.
for row in cursor.fetchall():
    print(row)

```

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre as origens de onde pode importar dados para Databricks do Azure, consulte [origens de dados do Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#)


