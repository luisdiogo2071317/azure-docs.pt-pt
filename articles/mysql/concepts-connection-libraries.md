---
title: "Bibliotecas de ligação para base de dados do Azure para MySQL"
description: "Este artigo apresenta uma lista de cada biblioteca ou controlador programas de cliente podem utilizar ao ligar à base de dados do Azure para MySQL."
services: mysql
author: mswutao
ms.author: wutao
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: c22bb9b31e7623daa635f82256e28ed466070481
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>Bibliotecas de ligação para base de dados do Azure para MySQL
Este artigo apresenta uma lista de cada biblioteca ou controlador programas de cliente podem utilizar ao ligar à base de dados do Azure para MySQL.

## <a name="client-interfaces"></a>Interfaces de cliente
MySQL oferece a conectividade de controlador de base de dados standard para utilizar o MySQL com as ferramentas que são compatíveis com as normas do sector ODBC e JDBC e aplicações. Qualquer sistema que trabalha com ODBC ou JDBC pode utilizar o MySQL.

| **Language** (Idioma) | **Platform** | **Recursos adicionais** | **Transferência** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [Controlador nativo MySQL para PHP - mysqlnd](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [Transferência](http://php.net/downloads.php) |
| ODBC | Plataformas Windows, Linux, Mac OS X e Unix | [Guia para programadores de MySQL conector/ODBC](https://dev.mysql.com/doc/connector-odbc/en/) | [Transferência](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [Guia para programadores de MySQL conector/Net](https://dev.mysql.com/doc/connector-net/en/) | [Transferência](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Independente da plataforma | [Guia para programadores de MySQL conector/J 5.1](https://dev.mysql.com/doc/connector-j/5.1/en/) | [Transferência](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sidorares/node-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [Transferência](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [Guia para programadores de MySQL conector/Python](https://dev.mysql.com/doc/connector-python/en/) | [Transferência](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [Guia para programadores de MySQL conector/C++](https://dev.mysql.com/doc/connector-cpp/en/) | [Transferência](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [Guia para programadores de MySQL conector/C](https://dev.mysql.com/doc/connector-c/en/) | [Transferência](https://dev.mysql.com/downloads/connector/c/)
| Perl | Plataformas Windows, Linux, Mac OS X e Unix | [DBD::MySQL](https://metacpan.org/pod/DBD::mysql) | [Transferência](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Passos Seguintes
Estes inícios rápidos sobre como ligar e consultar base de dados do Azure para MySQL, utilizando o seu idioma à escolha de leitura:

[PHP](./connect-php.md) | [Java](./connect-java.md) |  [.NET (c#)](./connect-csharp.md) | [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md)  |  [Ruby](./connect-ruby.md) | [C++](connect-cpp.md) | [aceda](./connect-go.md)

