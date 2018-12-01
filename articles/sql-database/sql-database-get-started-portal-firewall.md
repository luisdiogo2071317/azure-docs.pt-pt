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
ms.date: 12/01/2018
ms.openlocfilehash: cab92539b5019d4807ddefb2b84279c844f53016
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721906"
---
# <a name="quickstart-create-a-server-level-firewall-rule-for-your-sql-database-using-the-azure-portal"></a>Início Rápido: Criar uma regra de firewall ao nível do servidor para a base de dados SQL com o portal do Azure

Este guia de início rápido explica como criar uma regra de firewall ao nível do servidor para uma base de dados SQL do Azure que lhe permite ligar à mesma a partir de um recurso no local.

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido utiliza os recursos criados [criar uma base de dados SQL do Azure no portal do Azure](sql-database-get-started-portal.md) como ponto de partida.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule"></a>Criar uma regra de firewall ao nível do servidor

O serviço de base de dados SQL cria uma firewall ao nível do servidor. Esta firewall impede que aplicações e ferramentas de se ligar ao servidor ou a quaisquer bases de dados do servidor, a menos que crie uma regra de firewall para abrir a firewall. Para uma ligação de um endereço IP fora do Azure, crie uma regra de firewall para um endereço IP específico ou um intervalo de endereços. Para obter mais informações sobre regras de firewall, consulte [regra de firewall ao nível do servidor de base de dados SQL](sql-database-firewall-configure.md).

> [!NOTE]
> A Base de Dados SQL comunica através da porta 1433. Se estiver a tentar ligar a partir de uma rede empresarial, o tráfego de saída através da porta 1433 poderá não ser permitido pela firewall da rede. Se assim for, não é possível ligar ao seu servidor de base de dados do Azure SQL, a menos que o departamento de TI abra a porta 1433.
>

Siga estes passos para criar uma regra de firewall ao nível do servidor para o endereço IP do seu cliente e permitir a conectividade externa através da firewall de base de dados SQL para apenas o seu endereço IP.

1. Depois do [pré-requisitos base de dados SQL do Azure](#prerequisites) implementação estiver concluída, selecione **bases de dados SQL** no menu esquerdo e, em seguida, escolha **mySampleDatabase** no  **Bases de dados SQL** página. É aberta uma página de descrição geral para a base de dados que mostra o nome de servidor completamente qualificado (como **mynewserver-20170824.database.windows.net**) e oferece opções para configuração adicional.

2. Copie este nome de servidor completamente qualificado para utilizar quando se liga ao seu servidor e respetivas bases de dados em outros guias de introdução.

   ![nome do servidor](./media/sql-database-get-started-portal/server-name.png)

3. Selecione **definir firewall do servidor** na barra de ferramentas. É aberta a página **Definições da firewall** do servidor da Base de Dados SQL.

   ![regra de firewall do servidor](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Escolher **Adicionar IP de cliente** na barra de ferramentas para adicionar o seu endereço IP atual a uma nova regra de firewall. Uma regra de firewall consegue abrir a porta 1433 para um único endereço IP ou para um intervalo de endereços IP.

   > [!IMPORTANT]
   > Por predefinição, o acesso através da firewall da Base de Dados SQL está ativado para todos os serviços do Azure. Escolher **OFF** nesta página para desativar todos os serviços Azure.
   >

5. Selecione **Guardar**. É criada uma regra de firewall ao nível do servidor para a sua porta de abertura 1433 do endereço IP atual no servidor lógico.

6. Fechar o **definições da Firewall** página.

Utilizar o SQL Server Management Studio ou outra ferramenta à sua escolha, pode agora ligar para o servidor de base de dados SQL e respetivas bases de dados deste endereço IP com a conta de administrador de servidor criada anteriormente.

## <a name="clean-up-resources"></a>Limpar recursos

Guarde estes recursos se pretender aceder aos [Próximos passos](#next-steps) e saiba como ligar e consultar a base de dados utilizando um número de métodos diferentes. Se, no entanto, que pretende eliminar os recursos que criou neste início rápido, utilize os seguintes passos.


1. No menu esquerdo no portal do Azure, selecione **grupos de recursos** e, em seguida, selecione **myResourceGroup**.
2. Na página do grupo de recursos, selecione **elimine**, tipo **myResourceGroup** na caixa de texto e, em seguida, selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

- Agora que tem uma base de dados, pode [ligar e consultar](sql-database-connect-query.md) com uma das suas ferramentas ou linguagens favoritas, incluindo
  - [Ligar e consultar com o SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Ligar e consultar com o Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Para saber como criar a sua primeira base de dados, criar tabelas e inserir dados, veja um destes tutoriais:
  - [Criar a sua primeira base de dados SQL do Azure com o SSMS](sql-database-design-first-database.md)
  - [Criar uma base de dados SQL do Azure e estabelecer ligação com C# e ADO.NET](sql-database-design-first-database-csharp.md)
