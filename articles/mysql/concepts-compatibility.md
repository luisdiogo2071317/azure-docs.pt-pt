---
title: Drivers de MySQL e compatibilidade de ferramentas de gestão
description: Este artigo descreve os drivers de MySQL e ferramentas de gestão que são compatíveis com a base de dados do Azure para MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 11/21/2018
ms.openlocfilehash: 7bb5f861676517d709f59c1bf50d77c4d9cc49a4
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548056"
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>Controladores do MySQL e ferramentas de gestão compatíveis com a base de dados do Azure para MySQL
Este artigo descreve os drivers e ferramentas de gestão que são compatíveis com a base de dados do Azure para MySQL.

## <a name="mysql-drivers"></a>Controladores do MySQL
Base de dados do Azure para MySQL utiliza a edição da Comunidade mais popular do mundo da base de dados MySQL. Por conseguinte, é compatível com uma grande variedade de idiomas e drivers de programação. O objetivo é oferecer suporte as três versões mais recentes MySQL drivers e esforços com os autores da Comunidade de código-fonte aberto para aperfeiçoar constantemente a funcionalidade e a usabilidade de drivers de MySQL continuam. É fornecida uma lista de controladores que foram testadas e consideradas compatíveis com a base de dados do Azure para MySQL 5.6 e 5.7 na tabela a seguir:

| **Controlador** | **Ligações** | **Versões compatíveis** | **Versões incompatíveis** | **Notas** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | Para a ligação de PHP 7.0 com SSL MySQLi, adicione MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT na cadeia de ligação. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> Conjunto PDO: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` opção como falso.|
| .Net | [MySqlConnector no GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Pacote de instalação do Nuget](https://www.nuget.org/packages/MySqlConnector/) | 0.27 e o depois | 0.26.5 e antes | |
| MySQL Connector/NET | [MySQL Connector/NET](https://github.com/mysql/mysql-connector-net) | 8.0, 7.0, 6.10 |  | Um bug de codificação pode fazer com que as ligações a falha em alguns sistemas do Windows não UTF8. |
| Nodejs |  [MySQLjs no GitHub](https://github.com/mysqljs/mysql/) <br> Pacote de instalação a partir do NPM:<br> Executar `npm install mysql` partir do NPM | 2.15 | 2.14.1 e antes | |
| GO | https://github.com/go-sql-driver/mysql/releases | 1.3 | 1.2 e antes | Utilizar allowNativePasswords = true na cadeia de ligação |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 e antes | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1, 2.0, 1.6 | 1.5.5 e antes | |

## <a name="management-tools"></a>Ferramentas de Gestão
A vantagem de compatibilidade estende a ferramentas de gestão de base de dados também. Suas ferramentas existentes devem continuar a trabalhar com a base de dados do Azure para MySQL, desde que a manipulação de base de dados funciona nos limites das permissões de utilizador. Três ferramentas de gestão da base de dados comuns que foram testadas e consideradas compatíveis com a base de dados do Azure para MySQL 5.6 e 5.7 estão listadas na tabela a seguir:

|                                     | **MySQL Workbench 6.x e até** | **Navicat 12** | **PHPMyAdmin 4.x e até** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Criar, atualizar, ler, escrever, eliminar | X | X | X |
| Ligação de SSL | X | X | X |
| Conclusão de automática de consulta SQL | X | X |  |
| Importar e exportar dados | X | X | X |
| Exportar para vários formatos | X | X | X |
| Cópia de Segurança e Restauro |  | X |  |
| Apresentar os parâmetros do servidor | X | X | X |
| Exibir as conexões de cliente | X | X | X |
