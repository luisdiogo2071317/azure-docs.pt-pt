---
title: "Azure Cosmos DB: Criar uma aplicação Web Flask com Python e a API MongoDB do Azure Cosmos DB | Microsoft Docs"
description: "Apresenta um exemplo de código Python Flask que pode utilizar para ligar e consultar a API MongoDB do Azure Cosmos DB"
services: cosmos-db
documentationcenter: 
author: hshapiro
manager: scicoria
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 10/2/2017
ms.author: hshapiro
ms.openlocfilehash: f86c6cce82812e02f373d7307c76ace26ea3e99b
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="azure-cosmos-db-build-a-flask-app-with-the-mongodb-api"></a>Azure Cosmos DB: Criar uma aplicação Flask com a API MongoDB

O Azure Cosmos DB é um serviço de bases de dados com vários modelos e distribuído globalmente da Microsoft. Pode criar e consultar rapidamente o documento, a chave/valor e as bases de dados de gráficos, que beneficiam de capacidades de escalamento horizontal e distribuição global no centro do Azure Cosmos DB.

Este guia de introdução, utiliza o seguinte [exemplo de Flask](https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample) e demonstra como criar uma aplicação Flask de tarefas simples com o [Emulador do Azure Cosmos DB](/local-emulator.md) em vez do MongoDB.

## <a name="prerequisites"></a>Pré-requisitos

- Transfira o [Emulador do Azure Cosmos DB](/local-emulator.md). Atualmente, o emulador é suportado apenas em Windows. O exemplo mostra como utilizar o exemplo com uma chave de produção do Azure, o que pode ser feito em qualquer plataforma.

- Se ainda não tiver o Visual Studio Code instalado, pode instalar rapidamente o [VS Code](https://code.visualstudio.com/Download) para a sua plataforma (Windows, Mac, Linux).

- Certifique-se de que adiciona suporte de Linguagem Python, instalando uma das extensões Python populares.
    1. Selecione uma extensão.
    2. Instale a extensão, escrevendo `ext install` Paleta de Comandos `Ctrl+Shift+P`.

    O exemplo neste documento utiliza a [Extensão Python](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python) popular e com todas as funcionalidades de Don Jayamanne.

## <a name="clone-the-sample-application"></a>Clonar a aplicação de exemplo

Agora, vamos clonar uma aplicação API Flask-MongoDB a partir do GitHub, definir a cadeia de ligação e executá-la. Vai ver como é fácil trabalhar com dados programaticamente.

1. Abra uma janela de terminal do git, tal como git bash, e `cd` para um diretório de trabalho.
2. Execute o seguinte comando para clonar o repositório de exemplo.

    ```bash
    git clone https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git
    ```
3. Execute o seguinte comando para instalar os módulos de python.

    ```bash 
    pip install -r .\requirements.txt
    ```
4. Abra a pasta no Visual Studio Code.

## <a name="review-the-code"></a>Rever o código

Vamos fazer uma breve revisão do que está a acontecer na aplicação. Abra o ficheiro **app.py** no diretório de raiz e verá que estas linhas de código criam a ligação do Azure Cosmos DB. O código seguinte utiliza a cadeia de ligação para o Emulador do Azure Cosmos DB local. A palavra-passe tem de ser dividida, como pode ver baixo, para adaptar-se às barras que não podem ser analisadas de outra forma.

* Inicialize o cliente MongoDB, obtenha a base de dados e faça a autenticação.

    ```python
    client = MongoClient("mongodb://127.0.0.1:10250/?ssl=true") #host uri
    db = client.test    #Select the database
    db.authenticate(name="localhost",password='C2y6yDjf5' + r'/R' + '+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw' + r'/Jw==')
    ```

* Obtenha a coleção ou crie-a, caso ainda não exista.

    ```python
    todos = db.todo #Select the collection
    ```

* Criar a aplicação

    ```Python
    app = Flask(__name__)
    title = "TODO with Flask"
    heading = "ToDo Reminder"
    ```
    
## <a name="run-the-web-app"></a>Executar a aplicação Web

1. Certifique-se de que o Emulador do Azure Cosmos DB está em execução.

2. Abra uma janela de terminal e `cd` para o diretório onde a aplicação está guardada.

3. Em seguida, defina a variável de ambiente para a aplicação Flask com `set FLASK_APP=app.py` ou `export FLASK_APP=app.py` se estiver a utilizar um Mac.

4. Execute a aplicação com `flask run` e navegue até [http://127.0.0.1:5000/](http://127.0.0.1:5000/).

5. Adicione e remova tarefas e veja-as adicionadas e alteradas na coleção.

## <a name="create-a-database-account"></a>Criar uma conta de base de dados

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Atualizar a cadeia de ligação

Se quiser testar o código numa Conta do Azure Cosmos DB ativa, aceda ao portal do Azure para criar uma conta e obter as informações da sua cadeia de ligação. Em seguida, copie-as para a aplicação.

1. No [portal do Azure](http://portal.azure.com/), na sua conta do Azure Cosmos DB, na navegação da esquerda, clique em **Cadeia de Ligação** e em **Chaves de leitura/escrita**. Vai utilizar os botões de copiar no lado direito do ecrã para copiar o Nome de Utilizador, a Palavra-passe e o Anfitrião para o ficheiro Dal.cs no próximo passo.

2. Abra o ficheiro **app.py** no diretório de raiz.

3. Copie o valor de **nome de utilizador** a partir do portal (com o botão Copiar) e faça deste o valor de **nome** no ficheiro **app.py**.

4. Em seguida, copie o valor da sua **cadeia de ligação** a partir do portal e faça deste o valor do MongoClient no ficheiro **app.py**.

5. Por fim, copie o valor de **palavra-passe** a partir do portal e faça deste o valor de **palavra-passe** no ficheiro **app.py**.

Atualizou agora a sua aplicação com todas as informações necessárias para comunicar com o Azure Cosmos DB. Pode executá-la tal como fez anteriormente.

## <a name="deploy-to-azure"></a>Implementar no Azure

Para implementar esta aplicação, pode criar uma nova aplicação Web no Azure e ativar a implementação contínua com um fork deste repositório do github. Siga este [tutorial](https://docs.microsoft.com/azure/app-service-web/app-service-continuous-deployment) para configurar a implementação contínua com o Github no Azure.

Ao implementar no Azure, deve remover as chaves da aplicação e certificar-se de que a secção abaixo não está comentada:

```python
    client = MongoClient(os.getenv("MONGOURL"))
    db = client.test    #Select the database
    db.authenticate(name=os.getenv("MONGO_USERNAME"),password=os.getenv("MONGO_PASSWORD"))
```

Em seguida, tem de adicionar o seu MONGOURL, MONGO_PASSWORD e MONGO_USERNAME às definições da aplicação. Pode seguir este [tutorial](https://docs.microsoft.com/azure/app-service-web/web-sites-configure#application-settings) para saber mais sobre as Definições da Aplicação nas Aplicações Web do Azure.

Se não quiser criar um fork deste repositório, também pode clicar no botão Implementar no Azure abaixo. Em seguida, deve entrar no Azure e configurar as definições da aplicação com as informações da sua conta do Cosmos DB.

<a href="https://deploy.azure.com/?repository=https://github.com/heatherbshapiro/To-Do-List---Flask-MongoDB-Example" target="_blank">
<img src="http://azuredeploy.net/deploybutton.png"/>
</a>

> [!NOTE]
> Se pretender armazenar o código no Github ou outras opções de controlo de origem, certifique-se de que remove as cadeias de ligação do código. Em alternativa, podem ser definidas com as definições da aplicação para a aplicação Web.

## <a name="review-slas-in-the-azure-portal"></a>Rever os SLAs no portal do Azure

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Se não pretender continuar a utilizar esta aplicação, elimine todos os recursos criados com este guia de introdução no portal do Azure com os seguintes passos:

1. No menu do lado esquerdo do portal do Azure, clique em **Grupos de recursos** e, em seguida, clique no nome de recurso que criou.
2. Na página do grupo de recursos, clique em **Eliminar**, escreva o nome do recurso a eliminar na caixa de texto e, em seguida, clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, aprendeu a criar uma conta do Azure Cosmos DB e a executar uma aplicação Flask com a API do MongoDB. Agora, pode importar dados adicionais para a sua conta do Cosmos DB.

> [!div class="nextstepaction"]
> [Import data into Azure Cosmos DB for the MongoDB API](mongodb-migrate.md) (Importar dados para o Azure Cosmos DB para a MongoDB API)
