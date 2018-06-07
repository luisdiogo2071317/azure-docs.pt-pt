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
ms.date: 05/23/2018
ms.openlocfilehash: 4402867c93d9eb3f0d11a156da6045e758ac661a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34639797"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Suportado base de dados do Azure para as versões de servidor MySQL
Base de dados do Azure para MySQL tem foi desenvolvida de [MySQL Comunidade edição](https://www.mysql.com/products/community/), utilizando o motor de InnoDB.  Base de dados do Azure para MySQL atualmente suporta as seguintes versões:

## <a name="mysql-version-5639"></a>Versão de MySQL 5.6.39
Consulte o MySQL [documentação](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html) para saber mais sobre os melhoramentos e correções MySQL 5.6.39.

## <a name="mysql-version-5721"></a>Versão de MySQL 5.7.21
Consulte o MySQL [documentação](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html) para saber mais sobre os melhoramentos e correções em MySQL 5.7.21.

> [!NOTE]
> O serviço, um gateway é utilizado para redirecionar as ligações para instâncias do servidor. Depois da ligação for estabelecida, o cliente de MySQL apresenta a versão do MySQL definido no gateway, não a versão real em execução na sua instância de servidor MySQL. Para determinar a versão da sua instância de servidor MySQL, utilize o `SELECT VERSION();` comando na linha de MySQL. 

## <a name="managing-updates-and-upgrades"></a>Gestão de atualizações e melhoramentos
O serviço gere automaticamente a aplicação de patches para atualizações de versões de secundárias. As atualizações de versão principal não são suportadas (por ex. Atualizar do MySQL 5.6 para MySQL 5.7).

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre recursos específico quotas e limitações com base na sua **camada de serviço**, consulte [escalões de serviço](./concepts-pricing-tiers.md)
