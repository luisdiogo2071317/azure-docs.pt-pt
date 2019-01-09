---
title: Versões suportadas na base de dados do Azure para MySQL
description: Descreve as versões suportadas na base de dados do Azure para MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/07/2019
ms.openlocfilehash: ecd6466d8d7a7e4497d076ced0c9f2375d5dfb7f
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106041"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Suporte base de dados do Azure para versões de servidor MySQL
Base de dados do Azure para MySQL foi desenvolvido a partir [MySQL Community Edition](https://www.mysql.com/products/community/), usando o mecanismo de InnoDB.

MySQL usa o esquema de nomenclatura de x.y. z. X é a versão principal, Y é a versão secundária e Z é a versão de correção de erros. Para obter mais informações sobre o esquema, consulte a [MySQL documentação](https://dev.mysql.com/doc/refman/5.7/en/which-version.html).

Atualmente, a base de dados do Azure para MySQL suporta as seguintes versões:

## <a name="mysql-version-56"></a>Versão do MySQL 5.6

Versão de correção de erro: 5.6.39

Consulte o MySQL [notas de versão](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html) para saber mais sobre as melhorias e correções em MySQL 5.6.39.

## <a name="mysql-version-57"></a>Versão do MySQL 5.7

Versão de correção de erro: 5.7.21

Consulte o MySQL [notas de versão](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html) para saber mais sobre as melhorias e correções em MySQL 5.7.21.

> [!NOTE]
> No serviço, um gateway é utilizado para redirecionar as ligações para instâncias de servidor. Depois da ligação é estabelecida, o cliente do MySQL apresenta a versão do MySQL definida no gateway, não a versão real em execução na sua instância do servidor MySQL. Para determinar a versão da sua instância do servidor MySQL, utilize o `SELECT VERSION();` comando na linha de comandos MySQL.

## <a name="managing-updates-and-upgrades"></a>Gerenciando atualizações e atualizações
O serviço gere automaticamente a aplicação de patches para atualizações de versões de correção de erros. Por exemplo, 5.7.20 para 5.7.21.  

Atualmente, as atualizações das versões principais e secundárias não são suportadas. Por exemplo, a atualização do MySQL 5.6 para MySQL 5.7 não é suportada. Se desejar atualizar de 5.6 para 5.7, dê uma [capturar e restaurar](./concepts-migrate-dump-restore.md) -lo para um servidor que foi criado com a nova versão do motor.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre recursos específicos quotas e limitações com base no seu **escalão de serviço**, consulte [escalões de serviço](./concepts-pricing-tiers.md)
