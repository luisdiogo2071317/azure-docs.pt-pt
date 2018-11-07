---
title: Bibliotecas de ligação para base de dados SQL | Documentos da Microsoft
description: Fornece links para downloads de módulos que permitem a ligação ao SQL Server e base de dados SQL a partir de uma ampla variedade de linguagens de programação de cliente.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: d7c0a2060d1ec084e575edaf128d72495b1d7b85
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259650"
---
# <a name="connectivity-libraries-and-frameworks-for-sql-server"></a>Bibliotecas de conectividade e estruturas para o SQL Server

Confira nosso [introdução aos tutoriais](https://aka.ms/sqldev) para começar rapidamente com linguagens como c#, Java, node. js, PHP e Python. Em seguida, crie uma aplicação com o SQL Server no Linux ou o Windows ou o Docker no macOS.

A tabela seguinte apresenta uma lista de bibliotecas de conectividade ou *drivers* que aplicações cliente podem utilizar a partir de uma variedade de idiomas para ligar e utilizar o SQL Server em execução no local ou na cloud. Pode usá-los no Linux, Windows ou no Docker e usá-los para ligar à base de dados do Azure SQL e o Azure SQL Data Warehouse. 

| Idioma | Plataforma | Recursos adicionais | Transferência | Introdução |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET para o SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Transferência](https://www.microsoft.com/net/download/) | [Introdução](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Controlador Microsoft JDBC para SQL Server](https://msdn.microsoft.com/library/mt484311.aspx) | [Transferência](https://go.microsoft.com/fwlink/?linkid=852460) |  [Introdução](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [Controlador de SQL de PHP para SQL Server](https://msdn.microsoft.com/library/dn865013.aspx) | Sistema operativo: <br/> \* [Windows](https://www.microsoft.com/download/details.aspx?id=55642) <br/> \* [Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \* [macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Introdução](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [Controlador de node. js para SQL Server](https://msdn.microsoft.com/library/mt652093.aspx) | [Instalar](https://msdn.microsoft.com/library/mt652094.aspx) |  [Introdução](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Controlador de Python SQL](https://msdn.microsoft.com/library/mt652092.aspx) | Opções de instalação: <br/> \* [pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \* [pyodbc](https://msdn.microsoft.com/library/mt763257.aspx) |  [Introdução](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Controladores Ruby para SQL Server](https://msdn.microsoft.com/library/mt691981.aspx) | [Instalar](https://msdn.microsoft.com/library/mt711041.aspx) | [Introdução](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Controlador Microsoft ODBC para SQL Server](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) | [Transferência](https://msdn.microsoft.com/library/mt654048(v=sql.1).aspx) |  

A tabela seguinte lista exemplos de estruturas de mapeamento objeto-relacional (ORM) e estruturas da web que as aplicações de cliente podem utilizar com o SQL Server em execução no local ou na cloud. Pode utilizar as estruturas no Linux, Windows ou no Docker e usá-los para ligar à base de dados SQL e SQL Data Warehouse. 

| Idioma | Plataforma | ORM(s) |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/ef)<br>[Entity Framework Core](https://docs.microsoft.com/ef/core/index) |
| Java | Windows, Linux, macOS |[Hibernar ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloqüente)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](http://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](http://rubyonrails.org/) |
||||

## <a name="related-links"></a>Ligações relacionadas
- [Controladores do SQL Server](https://msdn.microsoft.com/library/mt654049.aspx) que são utilizadas para ligar aplicações cliente
- Ligar à base de dados SQL:
    - [Ligar à Base de Dados SQL com o .NET (C#)](sql-database-connect-query-dotnet.md)
    - [Ligar à Base de Dados SQL com PHP](sql-database-connect-query-php.md)
    - [Ligar à Base de Dados SQL com o Node.js](sql-database-connect-query-nodejs.md)
    - [Ligar à Base de Dados SQL com Java](sql-database-connect-query-java.md)
    - [Ligar à Base de Dados SQL com o Python](sql-database-connect-query-python.md)
    - [Ligar à Base de Dados SQL com Ruby](sql-database-connect-query-ruby.md)
- Exemplos de código da lógica de repetição:
    - [Ligar de forma resiliente ao SQL Server com ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
    - [Ligar de forma resiliente ao SQL Server com PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

