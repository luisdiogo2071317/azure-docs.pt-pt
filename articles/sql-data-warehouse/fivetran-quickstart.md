---
title: Início rápido do Fivetran com o Azure SQL Data Warehouse | Documentos da Microsoft
description: Comece rapidamente com Fivetran e Azure SQL Data Warehouse.
services: sql-data-warehouse
author: hirokib
manager: jrj
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 10/12/2018
ms.author: elbutter
ms.reviewer: craigg
ms.openlocfilehash: 8e738becfe356908af5baffc0ebf225916b2616e
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49355477"
---
# <a name="get-started-quickly-with-fivetran-and-sql-data-warehouse"></a>Comece a trabalhar rapidamente com Fivetran e o SQL Data Warehouse

Este guia de introdução pressupõe que já tem uma instância já existente do SQL Data Warehouse.

## <a name="setup-connection"></a>Ligação de configuração

1. Encontre o seu nome de servidor completamente qualificado e o nome de base de dados para ligar ao Azure SQL Data Warehouse.

   [Como encontrar o nome do servidor e nome de base de dados no portal?](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-connect-overview)

2. No Assistente de configuração, decida se pretende ligar a sua base de dados diretamente ou através de um túnel SSH.

   Se optar por ligar diretamente à sua base de dados, terá de criar uma regra de firewall para permitir o acesso. Este método é o método mais simples e mais seguro.

   Se optar por ligar através de um túnel SSH, Fivetran irá ligar-se para um servidor separado na sua rede que fornece um túnel SSH para a base de dados. Este método é necessário se a base de dados estiver numa sub-rede acessível numa rede virtual.

3. Adicionar "52.0.2.4" endereço IP na sua firewall ao nível do servidor para permitir ligações de entrada para o Azure SQL Data Warehouse de Fivetran.

   [Como adicionar firewall ao nível do servidor?](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal#create-a-server-level-firewall-rule)

## <a name="setup-user-credentials"></a>Configurar as credenciais de utilizador

Ligue ao seu Azure SQL Data Warehouse com o SQL Server Management Studio ou a ferramenta de escolha como utilizador de administrador do servidor e execute os seguintes comandos SQL para criar um utilizador para Fivetran:

Na base de dados mestra: ` CREATE LOGIN fivetran WITH PASSWORD = '<password>'; `

Na base de dados do armazém de dados SQL:

```
CREATE USER fivetran_user_without_login without login;
CREATE USER fivetran FOR LOGIN fivetran;
GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
```

Depois de criar fivetran de utilizador, conceda as seguintes permissões ao seu armazém:

```
GRANT CONTROL to fivetran;
```

Adicione uma classe de recursos adequados para o utilizador criado, dependendo dos requisitos de memória para a criação do índice columnstore. Por exemplo, integrações como o Salesforce e o Marketo tem de classe de recursos superior devido à grande escala número de colunas / maior volume de dados, que exige mais memória para criar índices columnstore.

É recomendado utilizar classes estáticas. Pode começar com a classe de recursos `staticrc20`, que aloca 200 MB para o usuário, independentemente do nível de desempenho que utiliza. Se a indexação columnstore falhar com a classe de recursos atual, temos de aumentar a classe de recursos.

```
EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
```

Para obter mais informações, veja os documentos para [limites de memória e simultaneidade](https://docs.microsoft.com/azure/sql-data-warehouse/memory-and-concurrency-limits#data-warehouse-limits) e [classes de recursos](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-memory-optimizations-for-columnstore-compression#ways-to-allocate-more-memory)

Permissão de controlo é necessário para criar as credenciais de um âmbito de base de dados que serão utilizadas ao carregar ficheiros a partir do armazenamento de Blobs com o PolyBase.

Introduza as credenciais para aceder ao Azure SQL Data Warehouse

1. Anfitrião (o nome do servidor)
2. Porta
3. Base de Dados
4. Utilizador (nome de utilizador deve estar `fivetran@<server_name>` em que `<server_name>` faz parte do seu uri de anfitrião do azure: `<server_name>.database.windows.net`)
5. Palavra-passe