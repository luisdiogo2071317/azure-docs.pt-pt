---
title: Versões suportadas na base de dados do Azure para MySQL
description: Descreve as versões suportadas na base de dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/22/2018
ms.openlocfilehash: 53c8d51ddf9b7465a99b8b0685d7f6ce177fc526
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Suportado base de dados do Azure para as versões de servidor MySQL
Base de dados do Azure para MySQL tem foi desenvolvida de [MySQL Comunidade edição](https://www.mysql.com/products/community/), utilizando o motor de InnoDB.  Base de dados do Azure para MySQL atualmente suporta as seguintes versões:

## <a name="mysql-version-5638"></a>Versão de MySQL 5.6.38
Consulte o MySQL [documentação](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-38.html) para saber mais sobre os melhoramentos e correções MySQL 5.6.38.

## <a name="mysql-version-5720"></a>Versão de MySQL 5.7.20
Consulte o MySQL [documentação](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-20.html) para saber mais sobre os melhoramentos e correções em MySQL 5.7.20.

> [!NOTE]
> O serviço, um gateway é utilizado para redirecionar as ligações para instâncias do servidor. Depois da ligação for estabelecida, o cliente de MySQL apresenta a versão do MySQL definido no gateway, não a versão real em execução na sua instância de servidor MySQL. Para determinar a versão da sua instância de servidor MySQL, utilize o `SELECT VERSION();` comando na linha de MySQL. 

## <a name="managing-updates-and-upgrades"></a>Gestão de atualizações e melhoramentos
O serviço gere automaticamente a aplicação de patches para atualizações de versões de secundárias. As atualizações de versão principal não são suportadas (por ex. Atualizar do MySQL 5.6 para MySQL 5.7).

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre recursos específico quotas e limitações com base na sua **camada de serviço**, consulte [escalões de serviço](./concepts-pricing-tiers.md)
