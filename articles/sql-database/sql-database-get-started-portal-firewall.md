---
title: 'Portal do Azure: Criar uma regra de firewall da Base de Dados SQL | Microsoft Docs'
description: Criar uma regra de firewall ao nível do servidor da Base de Dados SQL
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 09/06/2018
ms.openlocfilehash: ea843344e70d400f05c2767675bacb466c5b3fbd
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161918"
---
# <a name="create-a-server-level-firewall-rule-for-your-sql-database-using-the-azure-portal"></a>Criar uma regra de firewall ao nível do servidor para a base de dados SQL com o portal do Azure

Este guia de início rápido explica como criar uma regra de firewall ao nível do servidor para uma base de dados SQL do Azure que lhe permite ligar à mesma a partir de um recurso no local.

## <a name="prerequisites"></a>Pré-requisitos

Este guia de início rápido utiliza como ponto de partida os recursos criados neste guia de início rápido: [Criar uma base de dados SQL do Azure no portal do Azure](sql-database-get-started-portal.md)

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule"></a>Criar uma regra de firewall ao nível do servidor

O serviço da Base de Dados SQL cria uma firewall ao nível do servidor que impede que as aplicações e ferramentas estabeleçam uma ligação ao servidor ou a todas as bases de dados no servidor, a menos que seja criada uma regra de firewall para abrir a firewall. Para uma ligação de um endereço IP fora do Azure, crie uma regra de firewall para um endereço IP específico ou um intervalo de endereços. Siga estes passos para criar uma [regra de firewall ao nível do servidor da Base de Dados SQL](sql-database-firewall-configure.md) para o endereço IP do seu cliente, para permitir a conectividade externa através da firewall da Base de Dados SQL apenas para o seu endereço IP.

> [!NOTE]
> A Base de Dados SQL comunica através da porta 1433. Se estiver a tentar ligar a partir de uma rede empresarial, o tráfego de saída através da porta 1433 poderá não ser permitido pela firewall da rede. Se assim for, não poderá ligar ao servidor da Base de Dados SQL do Azure, a menos que o departamento de TI abra a porta 1433.
>

1. Depois de concluída a implementação, clique em **Bases de dados SQL** no menu do lado esquerdo e, em seguida, clique em **mySampleDatabase** na página **Bases de dados SQL**. É aberta uma página de descrição geral para a base de dados que mostra o nome de servidor completamente qualificado (como **mynewserver-20170824.database.windows.net**) e oferece opções para configuração adicional.

2. Copie este nome de servidor totalmente qualificado para utilizar para ligar ao seu servidor e às respetivas bases de dados nos seguintes inícios rápidos.

   ![nome do servidor](./media/sql-database-get-started-portal/server-name.png)

3. Clique em **Configurar firewall do servidor** na barra de ferramentas, conforme mostrado na imagem anterior. É aberta a página **Definições da firewall** do servidor da Base de Dados SQL.

   ![regra de firewall do servidor](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Clique em **Adicionar IP de cliente**, na barra de ferramentas, para adicionar o seu endereço IP atual a uma nova regra de firewall. Uma regra de firewall consegue abrir a porta 1433 para um único endereço IP ou para um intervalo de endereços IP.

5. Clique em **Guardar**. É criada uma regra de firewall ao nível do servidor para a sua porta de abertura 1433 do endereço IP atual no servidor lógico.

6. Clique em **OK** e, em seguida, feche a página **Definições da firewall**.

Pode agora ligar ao servidor da Base de Dados SQL e às respetivas bases de dados com o SQL Server Management Studio ou outra ferramenta à sua escolha a partir deste endereço IP com a conta de administrador de servidor criada anteriormente.

> [!IMPORTANT]
> Por predefinição, o acesso através da firewall da Base de Dados SQL está ativado para todos os serviços do Azure. Clique em **DESATIVAR** nesta página para desativar todos os serviços do Azure.
>

## <a name="clean-up-resources"></a>Limpar recursos

Guarde estes recursos se pretender aceder aos [Próximos passos](#next-steps) e saiba como ligar e consultar a base de dados utilizando um número de métodos diferentes. Se, no entanto, pretender eliminar os recursos que criou neste início rápido, utilize os seguintes passos.


1. No menu do lado esquerdo no portal do Azure, clique em **Grupos de recursos** e, em seguida, clique em **myResourceGroup**.
2. Na página do grupo de recursos, clique em **Eliminar**, escreva **myResourceGroup** na caixa de texto e, em seguida, clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

- Agora que tem uma base de dados, pode [ligar e consultar](sql-database-connect-query.md) com uma das suas ferramentas ou linguagens favoritas, incluindo
  - [Ligar e consultar com o SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Ligar e consultar com o Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Para saber como criar a sua primeira base de dados, criar tabelas e inserir dados, veja um destes tutoriais:
 - [Criar a sua primeira base de dados SQL do Azure com o SSMS](sql-database-design-first-database.md)
 - [Criar uma base de dados SQL do Azure e estabelecer ligação com C# e ADO.NET](sql-database-design-first-database-csharp.md)
