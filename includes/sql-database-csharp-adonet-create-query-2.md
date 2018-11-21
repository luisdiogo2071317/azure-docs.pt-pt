---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 11/09/2018
ms.author: genemi
ms.openlocfilehash: c4329b9efef3cdb2911466e64ac6c9f07a1e9b31
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52271690"
---
<a name="cs_0_csharpprogramexample_h2"/>

## <a name="c-program-example"></a>C#exemplo de programa

As próximas seções deste artigo presente um C# programa que utiliza o ADO.NET para enviar instruções Transact-SQL para a base de dados SQL. O C# programa executa as seguintes ações:

1. [Liga-se ao nosso banco de dados do SQL usando o ADO.NET](#cs_1_connect).
2. [Cria tabelas](#cs_2_createtables).
3. [Povoa as tabelas com dados, através da emissão de instruções T-SQL INSERT](#cs_3_insert).
4. [Atualiza os dados através de uma junção](#cs_4_updatejoin).
5. [Elimina os dados através de uma junção](#cs_5_deletejoin).
6. [Seleciona as linhas de dados através de uma junção](#cs_6_selectrows).
7. Fecha a ligação (que ignora qualquer tabelas temporárias provenientes de tempdb).

O C# programa contém:

- C#código para ligar à base de dados.
- Métodos que retornam o código-fonte T-SQL.
- Dois métodos que submeter o T-SQL na base de dados.

#### <a name="to-compile-and-run"></a>Para compilar e executar

Isso C# programa é logicamente um arquivo. cs. Mas aqui o programa fisicamente é dividido em vários blocos de código, para que cada bloco mais fácil ver e compreender. Para compilar e executar este programa, faça o seguinte:

1. Criar um C# projeto no Visual Studio.
    - O tipo de projeto deve ser um *consola* aplicação, de algo como a seguinte hierarquia: **modelos** > **Visual C#**  > **Ambiente de trabalho clássico do Windows** > **aplicação da consola (.NET Framework)**.
3. No ficheiro **Program.cs**, apagar as linhas de starter pequena de código.
3. Em Program.cs, copie e cole cada um dos seguintes blocos, na mesma sequência em que são apresentados aqui.
4. Em Program.cs, edite os seguintes valores na **Main** método:

   - **CB. Origem de dados**
   - **CD. ID de utilizador**
   - **CB. Palavra-passe**
   - **InitialCatalog**

5. Certifique-se de que o assembly **dll** é referenciado. Para verificar, expanda o **referências** nó a **Explorador de soluções** painel.
6. Para criar o programa no Visual Studio, clique nas **criar** menu.
7. Para executar o programa a partir do Visual Studio, clique nas **iniciar** botão. O resultado do relatório é apresentado numa janela cmd.exe.

> [!NOTE]
> Tem a opção de editar o T-SQL para adicionar um líder **#** para os nomes de tabela, que cria tabelas temporárias como no **tempdb**. Isso pode ser útil para fins de demonstração, quando nenhuma base de dados de teste está disponível. Tabelas temporárias são eliminadas automaticamente quando a ligação será fechada. Todas as referências para chaves externas não são impostas para tabelas temporárias.
>

<a name="cs_1_connect"/>
### <a name="c-block-1-connect-by-using-adonet"></a>C#bloquear 1: ligar com o ADO.NET

- [Seguinte](#cs_2_createtables)


```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll 
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
   class Program
   {
      static void Main(string[] args)
      {
         try
         {
            var cb = new SqlConnectionStringBuilder();
            cb.DataSource = "your_server.database.windows.net";
            cb.UserID = "your_user";
            cb.Password = "your_password";
            cb.InitialCatalog = "your_database";

            using (var connection = new SqlConnection(cb.ConnectionString))
            {
               connection.Open();

               Submit_Tsql_NonQuery(connection, "2 - Create-Tables",
                  Build_2_Tsql_CreateTables());

               Submit_Tsql_NonQuery(connection, "3 - Inserts",
                  Build_3_Tsql_Inserts());

               Submit_Tsql_NonQuery(connection, "4 - Update-Join",
                  Build_4_Tsql_UpdateJoin(),
                  "@csharpParmDepartmentName", "Accounting");

               Submit_Tsql_NonQuery(connection, "5 - Delete-Join",
                  Build_5_Tsql_DeleteJoin(),
                  "@csharpParmDepartmentName", "Legal");

               Submit_6_Tsql_SelectEmployees(connection);
            }
         }
         catch (SqlException e)
         {
            Console.WriteLine(e.ToString());
         }
         Console.WriteLine("View the report output here, then press any key to end the program...");
         Console.ReadKey();
      }
```


<a name="cs_2_createtables"/>
### <a name="c-block-2-t-sql-to-create-tables"></a>C#bloquear 2: T-SQL para criar tabelas

- [Anterior](#cs_1_connect) &nbsp;  /  &nbsp; [seguinte](#cs_3_insert)

```csharp
      static string Build_2_Tsql_CreateTables()
      {
         return @"
DROP TABLE IF EXISTS tabEmployee;
DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.


CREATE TABLE tabDepartment
(
   DepartmentCode  nchar(4)          not null
      PRIMARY KEY,
   DepartmentName  nvarchar(128)     not null
);

CREATE TABLE tabEmployee
(
   EmployeeGuid    uniqueIdentifier  not null  default NewId()
      PRIMARY KEY,
   EmployeeName    nvarchar(128)     not null,
   EmployeeLevel   int               not null,
   DepartmentCode  nchar(4)              null
      REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
);
";
      }
```

#### <a name="entity-relationship-diagram-erd"></a>Diagrama de relações de entidade (ERD)

As instruções CREATE TABLE anteriores envolvem a **referências** palavra-chave para criar um *chave estrangeira* relação (FK) entre duas tabelas.  Se estiver a utilizar tempdb, comente o `--REFERENCES` usando dois travessões líderes de palavra-chave.

Em seguida, há um ERD que mostra a relação entre as duas tabelas. Os valores no #tabEmployee.DepartmentCode *subordinado* coluna estão limitados aos valores presentes no #tabDepartment.Department *principal* coluna.

![Chave estrangeira do ERD que mostra](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)


<a name="cs_3_insert"/>
### <a name="c-block-3-t-sql-to-insert-data"></a>C#bloquear 3: T-SQL para inserir dados

- [Anterior](#cs_2_createtables) &nbsp;  /  &nbsp; [seguinte](#cs_4_updatejoin)


```csharp
      static string Build_3_Tsql_Inserts()
      {
         return @"
-- The company has these departments.
INSERT INTO tabDepartment
   (DepartmentCode, DepartmentName)
      VALUES
   ('acct', 'Accounting'),
   ('hres', 'Human Resources'),
   ('legl', 'Legal');

-- The company has these employees, each in one department.
INSERT INTO tabEmployee
   (EmployeeName, EmployeeLevel, DepartmentCode)
      VALUES
   ('Alison'  , 19, 'acct'),
   ('Barbara' , 17, 'hres'),
   ('Carol'   , 21, 'acct'),
   ('Deborah' , 24, 'legl'),
   ('Elle'    , 15, null);
";
      }
```


<a name="cs_4_updatejoin"/>
### <a name="c-block-4-t-sql-to-update-join"></a>C#bloquear 4: T-SQL para associação a atualização

- [Anterior](#cs_3_insert) &nbsp;  /  &nbsp; [seguinte](#cs_5_deletejoin)


```csharp
      static string Build_4_Tsql_UpdateJoin()
      {
         return @"
DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';


-- Promote everyone in one department (see @parm...).
UPDATE empl
   SET
      empl.EmployeeLevel += 1
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName1;
";
      }
```


<a name="cs_5_deletejoin"/>
### <a name="c-block-5-t-sql-to-delete-join"></a>C#bloquear 5: T-SQL para associação a delete

- [Anterior](#cs_4_updatejoin) &nbsp;  /  &nbsp; [seguinte](#cs_6_selectrows)


```csharp
      static string Build_5_Tsql_DeleteJoin()
      {
         return @"
DECLARE @DName2  nvarchar(128);
SET @DName2 = @csharpParmDepartmentName;  --'Legal';


-- Right size the Legal department.
DELETE empl
   FROM
      tabEmployee   as empl
      INNER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   WHERE
      dept.DepartmentName = @DName2

-- Disband the Legal department.
DELETE tabDepartment
   WHERE DepartmentName = @DName2;
";
      }
```



<a name="cs_6_selectrows"/>
### <a name="c-block-6-t-sql-to-select-rows"></a>C#bloquear 6: T-SQL para selecionar as linhas

- [Anterior](#cs_5_deletejoin) &nbsp;  /  &nbsp; [seguinte](#cs_6b_datareader)


```csharp
      static string Build_6_Tsql_SelectEmployees()
      {
         return @"
-- Look at all the final Employees.
SELECT
      empl.EmployeeGuid,
      empl.EmployeeName,
      empl.EmployeeLevel,
      empl.DepartmentCode,
      dept.DepartmentName
   FROM
      tabEmployee   as empl
      LEFT OUTER JOIN
      tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
   ORDER BY
      EmployeeName;
";
      }
```


<a name="cs_6b_datareader"/>
### <a name="c-block-6b-executereader"></a>C#bloquear 6b: ExecuteReader

- [Anterior](#cs_6_selectrows) &nbsp;  /  &nbsp; [seguinte](#cs_7_executenonquery)

Este método destina-se para executar a instrução T-SQL SELECT, que é criada pela **Build_6_Tsql_SelectEmployees** método.


```csharp
      static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("Now, SelectEmployees (6)...");

         string tsql = Build_6_Tsql_SelectEmployees();

         using (var command = new SqlCommand(tsql, connection))
         {
            using (SqlDataReader reader = command.ExecuteReader())
            {
               while (reader.Read())
               {
                  Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                     reader.GetGuid(0),
                     reader.GetString(1),
                     reader.GetInt32(2),
                     (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                     (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
               }
            }
         }
      }
```


<a name="cs_7_executenonquery"/>
### <a name="c-block-7-executenonquery"></a>C#bloquear 7: ExecuteNonQuery

- [Anterior](#cs_6b_datareader) &nbsp;  /  &nbsp; [seguinte](#cs_8_output)

Este método é chamado para operações que modificam o conteúdo de dados de tabelas sem devolver quaisquer linhas de dados.


```csharp
      static void Submit_Tsql_NonQuery(
         SqlConnection connection,
         string tsqlPurpose,
         string tsqlSourceCode,
         string parameterName = null,
         string parameterValue = null
         )
      {
         Console.WriteLine();
         Console.WriteLine("=================================");
         Console.WriteLine("T-SQL to {0}...", tsqlPurpose);

         using (var command = new SqlCommand(tsqlSourceCode, connection))
         {
            if (parameterName != null)
            {
               command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                  parameterName,
                  parameterValue);
            }
            int rowsAffected = command.ExecuteNonQuery();
            Console.WriteLine(rowsAffected + " = rows affected.");
         }
      }
   } // EndOfClass
}
```


<a name="cs_8_output"/>
### <a name="c-block-8-actual-test-output-to-the-console"></a>C#bloquear 8: resultado do teste real para a consola

- [Anterior](#cs_7_executenonquery)

Esta secção captura a saída que o programa enviado ao console. Apenas os valores de guid variam entre as execuções de teste.


```text
[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>> csharp_db_test.exe

=================================
Now, CreateTables (10)...

=================================
Now, Inserts (20)...

=================================
Now, UpdateJoin (30)...
2 rows affected, by UpdateJoin.

=================================
Now, DeleteJoin (40)...

=================================
Now, SelectEmployees (50)...
0199be49-a2ed-4e35-94b7-e936acf1cd75 , Alison , 20 , acct , Accounting
f0d3d147-64cf-4420-b9f9-76e6e0a32567 , Barbara , 17 , hres , Human Resources
cf4caede-e237-42d2-b61d-72114c7e3afa , Carol , 22 , acct , Accounting
cdde7727-bcfd-4f72-a665-87199c415f8b , Elle , 15 , NULL , NULL

[C:\csharp_db_test\csharp_db_test\bin\Debug\]
>>
```
