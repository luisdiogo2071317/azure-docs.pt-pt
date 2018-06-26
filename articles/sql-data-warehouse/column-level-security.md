---
title: Segurança de nível de coluna do armazém de dados SQL do Azure | Microsoft Docs
description: Segurança ao nível da coluna (conformidade com CLS) permite aos clientes controlar o acesso às colunas da tabela de base de dados com base no contexto de execução de um utilizador ou a respetiva associação a grupos. Conformidade com CLS simplifica a programação de segurança na sua aplicação e a estrutura. Conformidade com CLS permite-lhe implementar restrições de acesso através da coluna.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 06/15/2018
ms.author: kavithaj
ms.reviewer: igorstan, carlrab
ms.openlocfilehash: 5a916132f705f3c517ee6789b61a3972b2445b62
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938466"
---
# <a name="column-level-security"></a>Segurança ao nível da coluna 
Segurança ao nível da coluna (conformidade com CLS) permite aos clientes controlar o acesso às colunas da tabela de base de dados com base no contexto de execução de um utilizador ou a respetiva associação a grupos.  

Conformidade com CLS simplifica a programação de segurança na sua aplicação e a estrutura. Conformidade com CLS permite-lhe implementar restrições de acesso através da coluna para proteger os dados confidenciais. Por exemplo, garantindo que utilizadores específicos podem aceder a determinados as colunas de uma tabela relevante para o respetivo departamento. A lógica de restrição de acesso está localizado na camada de base de dados em vez de away dos dados na camada da aplicação em outro. A base de dados aplica-se as restrições de acesso sempre que esse acesso de dados é tentado a partir de qualquer camada. Isto torna o seu sistema de segurança mais fiável e robusta, reduzindo a área de superfície do seu sistema de segurança geral. Além disso, este também elimina a necessidade de introduzir vistas para filtrar colunas para impor restrições de acesso de utilizadores. 

Pode implementar a conformidade com CLS com o [conceder](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) instrução T-SQL. Este mecanismo de autenticação do SQL Server e o Azure Active Directory (AAD) são suportados.

![conformidade com CLS](./media/column-level-security/cls.png)

## <a name="syntax"></a>Sintaxe 

```sql
GRANT <permission> [ ,...n ] ON    
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]   
    TO <database_principal> [ ,...n ]    
    [ WITH GRANT OPTION ]   
    [ AS <database_principal> ]   
<permission> ::=   
    SELECT 
  | UPDATE  
<database_principal> ::=    
      Database_user    
    | Database_role    
    | Database_user_mapped_to_Windows_User    
    | Database_user_mapped_to_Windows_Group    
```

## <a name="example"></a>Exemplo 
O exemplo seguinte mostra como restringir 'TestUser' acedam a coluna 'SSN' da tabela 'Associação': 

Crie a tabela de 'Associação' com a coluna de SSN utilizada para armazenar os números de segurança social:

```sql
CREATE TABLE Membership   
  (MemberID int IDENTITY,   
   FirstName varchar(100) NULL,   
   SSN char(9) NOT NULL, 
   LastName varchar(100) NOT NULL,   
   Phone varchar(12) NULL,   
   Email varchar(100) NULL);  
```

Permitir 'TestUser' para aceder a todas as colunas exceto coluna SSN que tenha dados confidenciais: 

```sql  
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;   
``` 

Consultas executadas como 'TestUser' irá falhar se incluem a coluna de SSN:

```sql  
SELECT * FROM Membership;

Msg 230, Level 14, State 1, Line 12
The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'. 
``` 

## <a name="use-cases"></a>Casos de utilização
Alguns exemplos de como CLS está a ser utilizada hoje em dia: 
- Um firm de serviços financeiros permite única conta de gestores da ter acesso aos números de segurança social de cliente (SSN), os números de telefone e outras informações de identificação pessoal (PII).
- Um fornecedor de saúde permite apenas doctors e nurses a ter acesso aos registos médicas confidenciais enquanto não permitir que os membros do departamento de faturação ver estes dados.
