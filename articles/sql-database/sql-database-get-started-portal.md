---
title: 'Portal do Azure: Criar uma única base de dados SQL | Microsoft Docs'
description: Crie um servidor lógico da Base de Dados SQL, regras de firewall ao nível do servidor e uma base de dados no portal do Azure e realize consultas à mesma.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 09/07/2018
ms.openlocfilehash: 0e7ea33fa775bfba934d68d7cbcdd754880c3e55
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165012"
---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Criar uma base de dados SQL do Azure no portal do Azure

Este início rápido explica como criar uma base de dados SQL no Azure com o [modelo de compra com base na DTU](sql-database-service-tiers-dtu.md). A Base de Dados SQL do Azure é uma oferta de "Base de dados como Serviço" que lhe permite executar e dimensionar bases de dados do SQL Server altamente disponíveis na cloud. Este guia de início rápido mostra-lhe como começar a criar e a consultar uma base de dados SQL através do portal do Azure.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

  >[!NOTE]
  >Este tutorial utiliza o modelo de compra baseado em DTU, mas o [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md) também está disponível.

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Criar uma base de dados SQL

É criada uma base de dados SQL do Azure com um conjunto definido de [recursos de armazenamento e computação](sql-database-service-tiers-dtu.md). A base de dados é criada num [Grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) e num [servidor lógico da Base de Dados SQL do Azure](sql-database-features.md).

Siga estes passos para criar uma base de dados SQL com os dados de exemplo da Adventure Works LT.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.

2. Selecione **Bases de Dados** na página **Novo** e selecione **Criar na Base de Dados SQL** em **Base de Dados SQL** na página **Novo**.

   ![criar base de dados-1](./media/sql-database-get-started-portal/create-database-1.png)

3. Preencha o formulário da Base de Dados SQL com as informações seguintes, conforme mostrado na imagem anterior:   

   | Definição       | Valor sugerido | Descrição |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Nome da base de dados** | mySampleDatabase | Para nomes de bases de dados válidos, veja [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Identificadores de Bases de Dados). |
   | **Subscrição** | A sua subscrição  | Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições). |
   | **Grupo de recursos**  | myResourceGroup | Para nomes de grupo de recursos válidos, veja [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições). |
   | **Selecionar origem** | Exemplo (AdventureWorksLT) | Carrega o esquema e os dados AdventureWorksLT na nova base de dados |

   > [!IMPORTANT]
   > Tem de selecionar a base de dados de exemplo neste formulário porque é utilizada no resto deste início rápido.
   >

4. No **Servidor**, clique em **Configurar definições necessárias** e preencha o formulário do servidor SQL (servidor lógico) com as informações seguintes, conforme mostrado na imagem anterior:   

   | Definição       | Valor sugerido | Descrição |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Nome do servidor** | Qualquer nome globalmente exclusivo | Para nomes de servidores válidos, veja [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições). |
   | **Início de sessão de administrador do servidor** | Qualquer nome válido | Para nomes de início de sessão válidos, veja [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Identificadores de Bases de Dados). |
   | **Palavra-passe** | Qualquer palavra-passe válida | A sua palavra-passe deve ter, pelo menos, oito carateres e deve conter carateres de três das seguintes categorias: carateres maiúsculos, carateres minúsculos, números e carateres não alfanuméricos. |
   | **Subscrição** | A sua subscrição | Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições). |
   | **Grupo de recursos** | myResourceGroup | Para nomes de grupo de recursos válidos, veja [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições). |
   | **Localização** | Nenhuma localização válida | Para obter mais informações sobre regiões, veja [Azure Regions](https://azure.microsoft.com/regions/) (Regiões do Azure). |

   > [!IMPORTANT]
   > O início de sessão e a palavra-passe de administrador de servidor que especificar aqui serão necessários para iniciar sessão no servidor e nas respetivas bases de dados mais tarde neste início rápido. Lembre-se ou grave estas informações para utilização posterior.
   >  

   ![criar servidor de base de dados](./media/sql-database-get-started-portal/create-database-server.png)

5. Quando concluir o formulário, clique em **Selecionar**.

6. Clique em **Escalão de preço** para especificar a camada de serviço, o número de DTUs e a quantidade de armazenamento. Explore as opções para a quantidade de DTUs e de armazenamento que tem disponível para cada camada de serviço.

   > [!IMPORTANT]
   > No escalão Premium, está atualmente disponível mais de 1 TB de armazenamento em todas as regiões, exceto nas seguintes: Alemanha Central, E.U.A. Centro-Oeste, Leste da China, Nordeste da Alemanha, Norte da China, Norte do Reino Unido, Sul2 do Reino Unido, US DoD – Centro, US DoD – Leste, US Gov – Centro-Sul, US Gov – Leste, US Gov – Sudoeste. Noutras regiões, o armazenamento máximo no escalão Premium está limitado a 1 TB. Ver [Limitações Atuais P11-P15]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

7. Para este início rápido, selecione o escalão de preço **Standard** e, em seguida, utilize o controlo de deslize para selecionar **10 DTUs (S0)** e **1** GB de armazenamento.

   ![criar base de dados-s1](./media/sql-database-get-started-portal/create-database-s1.png)

8. Aceite os termos de pré-visualização para utilizar a opção **Adicionar ao Armazenamento**.

   > [!IMPORTANT]
   > No escalão Premium, está atualmente disponível mais de 1 TB de armazenamento em todas as regiões, exceto nas seguintes: Alemanha Central, E.U.A. Centro-Oeste, Leste da China, Nordeste da Alemanha, Norte da China, US DoD – Centro, US DoD – Leste, US Gov – Iowa, US Gov – Sudoeste. Noutras regiões, o armazenamento máximo no escalão Premium está limitado a 1 TB. Ver [Limitações Atuais P11-P15]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

9. Depois de selecionar o escalão de servidor, o número de DTUs e a quantidade de armazenamento, clique em **Aplicar**.  

10. Agora que concluiu o formulário da Base de Dados SQL do Servidor, clique em **Criar** para aprovisionar a base de dados. O aprovisionamento demora alguns minutos.

11. Na barra de ferramentas, clique em **Notificações** para monitorizar o processo de implementação.

     ![notificação](./media/sql-database-get-started-portal/notification.png)

## <a name="query-the-sql-database"></a>Consultar a base de dados SQL

Agora que criou uma base de dados de exemplo no Azure, vamos utilizar a ferramenta de consulta incorporada no portal do Azure para confirmar que se consegue ligar à base de dados e consultar os dados.

1. Na página Base de Dados SQL da sua base de dados, clique em **Editor de consultas (pré-visualização)** no menu à esquerda e, em seguida, clique em **Iniciar sessão**.

   ![início de sessão](./media/sql-database-get-started-portal/query-editor-login.png)

2. Selecione a autenticação do servidor SQL, forneça as informações necessárias de início de sessão e, em seguida, clique em **OK** para iniciar sessão.

3. Quando estiver autenticado como **ServerAdmin**, escreva a consulta seguinte no painel do editor de consultas.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

4. Clique em **Executar** e, em seguida, reveja os resultados da consulta no painel **Resultados**.

   ![Resultados do editor de consultas](./media/sql-database-get-started-portal/query-editor-results.png)

5. Feche a página **Editor de consultas**, clique em **OK** para eliminar as edições não guardadas.

## <a name="clean-up-resources"></a>Limpar recursos

Guarde estes recursos se pretender aceder aos [Próximos passos](#next-steps) e saiba como ligar e consultar a base de dados utilizando um número de métodos diferentes. Se, no entanto, pretender eliminar os recursos que criou neste início rápido, utilize os seguintes passos.


1. No menu do lado esquerdo no portal do Azure, clique em **Grupos de recursos** e, em seguida, clique em **myResourceGroup**.
2. Na página do grupo de recursos, clique em **Eliminar**, escreva **myResourceGroup** na caixa de texto e, em seguida, clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

- Agora que tem uma base de dados, tem de criar uma regra de firewall ao nível do servidor para ligar à mesma a partir das suas ferramentas no local. Veja [Criar uma regra de firewall ao nível do servidor](sql-database-get-started-portal-firewall.md)
- Se criar uma regra de firewall ao nível do servidor, pode [ligar e consultar](sql-database-connect-query.md) através de uma das suas ferramentas ou linguagens favoritas, incluindo
  - [Ligar e consultar com o SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Ligar e consultar com o Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Para criar bases de dados com a CLI do Azure, veja [Exemplos da CLI do Azure](sql-database-cli-samples.md)
- Para criar bases de dados com o Azure PowerShell, veja [Exemplos do Azure PowerShell](sql-database-powershell-samples.md)