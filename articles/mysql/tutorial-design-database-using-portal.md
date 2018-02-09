---
title: Conceber a sua primeira Base de Dados do Azure para a base de dados MySQL - Portal do Azure | Microsoft Docs
description: "Este tutorial explica como criar e gerir a Base de Dados do Azure para o servidor MySQL e a base de dados através do Portal do Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: tutorial
ms.date: 11/03/2017
ms.custom: mvc
ms.openlocfilehash: 76cccf9e2ce0a1e59b43646c43ac165d46dade4a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Conceber a sua primeira Base de Dados do Azure para a base de dados MySQL
A Base de Dados do Azure para MySQL é um serviço gerido que lhe permite executar, gerir e dimensionar as bases de dados MySQL de alta disponibilidade na cloud. No portal do Azure, pode facilmente gerir o seu servidor e criar uma base de dados.

Neste tutorial, irá utilizar o Portal do Azure para aprender a:

> [!div class="checklist"]
> * Criar uma Base de Dados do Azure para MySQL
> * Configurar a firewall do servidor
> * Utilize a ferramenta da linha de comandos mysql para criar uma base de dados
> * Carregar os dados de exemplo
> * Consultar dados
> * Atualizar dados
> * Restaurar dados

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Abra o browser favorito e visite o [portal do Microsoft Azure](https://portal.azure.com/). Introduza as suas credenciais para iniciar sessão no portal. A vista predefinida é o dashboard de serviço.

## <a name="create-an-azure-database-for-mysql-server"></a>Criar uma Base de Dados do Azure para o servidor MySQL
É criada uma Base de Dados do Azure para o servidor MySQL com um conjunto definido de [recursos de armazenamento e computação](./concepts-compute-unit-and-storage.md). O servidor é criado dentro de um [grupo de recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1. Navegue para **Bases de Dados** > **Base de Dados do Azure para MySQL**. Se não encontrar o servidor MySQL na categoria **Bases de Dados**, clique em **Ver tudo**, para mostrar todos os serviços de bases de dados disponíveis. Também pode escrever **Base de Dados do Azure para MySQL** na caixa de pesquisa para encontrar rapidamente o serviço.
![2-1 Navegar para o MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Clique no mosaico **Base de Dados do Azure para MySQL** e, em seguida, clique em **Criar**.

Neste exemplo, preencha o formulário Base de Dados do Azure para MySQL com as informações seguintes:

| **Definição** | **Valor sugerido** | **Descrição do Campo** |
|---|---|---|
| *Nome do servidor* | myserver4demo  | O nome do servidor deve ser globalmente exclusivo. |
| *Subscrição* | mysubscription | Selecione a subscrição na lista pendente. |
| *Grupo de recursos* | myResourceGroup | Crie um grupo de recursos ou utilize um existente. |
| *Início de sessão de administrador do servidor* | myadmin | Configurar nome da conta de administrador. |
| *Palavra-passe* |  | Defina uma palavra-passe forte para a conta de administrador. |
| *Confirmar palavra-passe* |  | Confirme a palavra-passe da conta de administrador. |
| *Localização* |  | Selecione uma região disponível. |
| *Versão* | 5.7 | Escolha a versão mais recente. |
| *Configurar o desempenho* | Básico, 50 unidades de computação, 50 GB  | Escolha **Escalão de preço**, **Unidades de Computação**, **Armazenamento (GB)** e clique em **OK**. |
| *Afixar ao Dashboard* | Marcar | Recomendamos que marque esta caixa para encontrar facilmente o servidor posteriormente |
Em seguida, clique em **Criar**. Num ou dois minutos, uma nova Base de Dados do Azure para o servidor MySQL estará em execução na cloud. Pode clicar no botão **Notificações** na barra de ferramentas para monitorizar o processo de implementação.

## <a name="configure-firewall"></a>Configurar a firewall
As Bases de Dados do Azure para MySQL estão protegidas por uma firewall. Por predefinição, todas as ligações ao servidor e as bases de dados dentro do servidor são rejeitadas. Antes de ligar à Base de Dados do Azure para MySQL pela primeira vez, configure a firewall para adicionar o endereço IP de rede público do computador do cliente (ou um intervalo de endereços IP).

1. Clique no servidor acabado de criar e clique em **Segurança da ligação**.
   ![3-1 Segurança da ligação](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. Pode **Adicionar o Meu IP** ou configurar regras de firewall aqui. Não se esqueça de clicar em **Guardar** depois de criar a regras.
Já pode ligar ao servidor através da ferramenta de linha de comandos mysql ou da ferramenta da GUI MySQL Workbench.

> [!TIP]
> A Base de Dados do Azure para o servidor MySQL comunica através da porta 3306. Se estiver a tentar ligar a partir de uma rede empresarial, o tráfego de saída através da porta 3306 poderá não ser permitido pela firewall da rede. Se assim for, não poderá ligar ao servidor MySQL do Azure, a menos que o departamento de TI abra a porta 3306.

## <a name="get-connection-information"></a>Obter informações da ligação
Obtenha o **Nome do servidor** completamente qualificado e o **Nome de início de sessão de administração do servidor** para a sua Base de Dados do Azure para o servidor MySQL a partir do portal do Azure. Utilize o nome de servidor totalmente qualificado para ligar ao seu servidor com a ferramenta de linha de comandos mysql. 

1. No [portal do Azure](https://portal.azure.com/), clique em **Todos os recursos**, no menu do lado esquerdo, escreva o nome e pesquise a sua Base de Dados do Azure para o servidor MySQL. Selecione o nome do servidor para ver os detalhes.

2. No cabeçalho Definições, clique em **Propriedades**. Aponte **NOME DO SERVIDOR** e **NOME DO INÍCIO DE SESSÃO DE ADMINISTRADOR DO SERVIDOR**. Pode clicar no botão Copiar, junto a cada campo, para copiar para a área de transferência.
   ![4-2 propriedades do servidor](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

Neste exemplo, o nome do servidor é *myserver4demo.mysql.database.azure.com* e o início de sessão de administrador do servidor é *myadmin@myserver4demo*.

## <a name="connect-to-the-server-using-mysql"></a>Ligar ao servidor com o mysql
Utilize a [ferramenta de linha de comandos do mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) para estabelecer uma ligação à base de dados do Azure para o servidor MySQL. Pode executar a ferramenta de linha de comandos do mysql a partir do Azure Cloud Shell no navegador ou a partir do seu computador com as ferramentas do mysql instaladas localmente. Para iniciar o Azure Cloud Shell, clique no botão `Try It` num bloco de código neste artigo ou visite o portal do Azure e clique no ícone `>_` da barra de ferramentas superior à direita. 

Escreva o comando para estabelecer a ligação:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Criar uma base de dados vazia
Assim que estiver ligado ao servidor, crie uma base de dados vazia para trabalhar.
```sql
CREATE DATABASE mysampledb;
```

Na linha de comandos, execute o comando seguinte para mudar a ligação para a base de dados recentemente criada:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Criar tabelas na base de dados
Agora que sabe como ligar à Base de Dados do Azure para MySQL, pode concluir algumas tarefas básicas:

Em primeiro lugar, crie uma tabela e carregue-a com alguns dados. Vamos criar uma tabela que armazena informações de inventário.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Carregar dados para as tabelas
Agora que tem uma tabela, insira alguns dados na mesma. Na janela da linha de comandos aberta, execute a seguinte consulta para inserir algumas linhas de dados.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Tem agora duas linhas de dados de exemplo nas tabelas que criou anteriormente.

## <a name="query-and-update-the-data-in-the-tables"></a>Consultar e atualizar os dados nas tabelas
Execute a seguinte consulta para obter informações da tabela de base de dados.
```sql
SELECT * FROM inventory;
```

Também pode atualizar os dados nas tabelas.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

A linha é atualizada em conformidade ao obter dados.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Restaurar uma base de dados para um ponto anterior no tempo
Imagine que eliminou acidentalmente uma tabela de base de dados importante e não consegue recuperar os dados facilmente. A Base de Dados do Azure para MySQL permite restaurar o servidor num momento específico, ao criar uma cópia das bases de dados no novo servidor. Pode utilizar este servidor novo para recuperar os dados eliminados. Os passos seguintes restauram o servidor de exemplo para um ponto antes da tabela ter sido adicionada.

1. No portal do Azure, encontre a Base de Dados do Azure para MySQL. Na página **Descrição geral**, clique em **Restaurar** na barra de ferramentas. É aberta a página Restaurar.

   ![10-1 restaurar uma base de dados](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Preencha o formulário **Restaurar** com as informações necessárias.
   
   ![10-2 formulário de restauro](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Ponto de restauro**: selecione um momento específico em que pretende restaurar, durante o período de tempo listado. Certifique-se de que converte o fuso horário local para UTC.
   - **Restaurar para novo servidor**: indique um novo nome de servidor para o qual pretende restaurar.
   - **Localização**: a região é a mesma que o servidor de origem e não pode ser alterada.
   - **Escalão de preço**: o escalão de preço é o mesmo que o servidor de origem e não pode ser alterado.
   
3. Clique em **OK** para restaurar o servidor para [ e restaurar para um momento específico](./howto-restore-server-portal.md) antes da tabela ter sido eliminada. Restaurar um servidor cria uma nova cópia do servidor, a partir do momento específico que especificar. 

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, utilizou o portal do Azure para aprender a:

> [!div class="checklist"]
> * Criar uma Base de Dados do Azure para MySQL
> * Configurar a firewall do servidor
> * Utilize a ferramenta da linha de comandos mysql para criar uma base de dados
> * Carregar os dados de exemplo
> * Consultar dados
> * Atualizar dados
> * Restaurar dados

> [!div class="nextstepaction"]
> [Como ligar aplicações à Base de Dados do Azure para MySQL](./howto-connection-string.md)
