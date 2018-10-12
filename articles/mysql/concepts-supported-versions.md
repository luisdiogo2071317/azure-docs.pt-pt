---
title: Versões suportadas na base de dados do Azure para MySQL
description: Descreve as versões suportadas na base de dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: f2a9348e267ad6a5929fda64cc08dcd837243e71
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093345"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Suporte base de dados do Azure para versões de servidor MySQL
Base de dados do Azure para MySQL foi desenvolvido a partir [MySQL Community Edition](https://www.mysql.com/products/community/), usando o mecanismo de InnoDB. Atualmente, a base de dados do Azure para MySQL suporta as seguintes versões:

## <a name="mysql-version-5639"></a>Versão do MySQL 5.6.39
Consulte o MySQL [documentação](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html) para saber mais sobre as melhorias e correções em MySQL 5.6.39.

## <a name="mysql-version-5721"></a>Versão do MySQL 5.7.21
Consulte o MySQL [documentação](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html) para saber mais sobre as melhorias e correções em MySQL 5.7.21.

> [!NOTE]
> No serviço, um gateway é utilizado para redirecionar as ligações para instâncias de servidor. Depois da ligação é estabelecida, o cliente do MySQL apresenta a versão do MySQL definida no gateway, não a versão real em execução na sua instância do servidor MySQL. Para determinar a versão da sua instância do servidor MySQL, utilize o `SELECT VERSION();` comando na linha de comandos MySQL.

## <a name="managing-updates-and-upgrades"></a>Gerenciando atualizações e atualizações
O serviço gere automaticamente a aplicação de patches para atualizações de versões de correção de erros. Atualmente, a atualização de versão secundária não é suportada. Por exemplo, a atualização do MySQL 5.6 para o MySQL 5.7 não é suportada. Se gostaria de atualizar para a próxima versão secundária, dê uma [capturar e restaurar](./concepts-migrate-dump-restore.md) -lo para um servidor que foi criado com a nova versão do motor.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre recursos específicos quotas e limitações com base no seu **escalão de serviço**, consulte [escalões de serviço](./concepts-pricing-tiers.md)
