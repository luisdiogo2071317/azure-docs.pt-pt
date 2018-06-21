---
title: Configura a replicação de dados para replicar os dados na base de dados do Azure para MySQL.
description: Este artigo descreve como configurar dados na replicação da base de dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/20/2018
ms.openlocfilehash: e099597eae419653a2a40c7f01ee7abbbc4657f0
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294426"
---
# <a name="how-to-configure-azure-database-for-mysql-data-in-replication"></a>Como configurar a base de dados do Azure para replicação de dados MySQL

Neste artigo, irá aprender a configurar a replicação de dados na base de dados do Azure para o serviço de MySQL ao configurar os servidores primário e réplica. Replicação de dados permite-lhe sincronizar os dados de um servidor principal do MySQL, em execução no local, em máquinas virtuais ou serviços de base de dados alojados por outros fornecedores de nuvem para uma réplica na base de dados do Azure para o serviço de MySQL. 

Este artigo pressupõe que tem, pelo menos, de alguma experiência anterior com servidores MySQL e bases de dados.

## <a name="create-a-mysql-server-to-be-used-as-replica"></a>Criar um servidor de MySQL a utilizar como réplica

1. Criar uma nova base de dados do Azure para o servidor de MySQL

   Criar um novo servidor de MySQL (ex. "replica.mysql.database.azure.com"). Consulte [criar uma base de dados do Azure para o servidor de MySQL, utilizando o portal do Azure](quickstart-create-mysql-server-database-using-azure-portal.md) para a criação do servidor. Este servidor é o servidor de "réplica" na replicação de dados.

   > [!IMPORTANT]
   > A base de dados do Azure para o servidor de MySQL têm de ser criado os escalões de preços de finalidade geral ou de otimização de memória.
   > 

2. Criar contas de utilizador mesmo e privilégios correspondentes

   Contas de utilizador não são replicadas a partir do servidor primário para o servidor de réplica. Se pretender fornecer aos utilizadores acesso ao servidor de réplica, terá de criar manualmente todas as contas e privilégios correspondentes neste criado recentemente a base de dados do Azure para o servidor de MySQL.

## <a name="configure-the-primary-server"></a>Configurar o servidor primário
Os seguintes passos preparam e configurem o MySQL server alojado no local, uma máquina virtual ou o serviço de base de dados alojado por outros fornecedores de nuvem para replicação de dados. Este servidor encontra-se a "primário" na replicação de dados. 

1. Ativar o registo binário

   Verifique se o registo binário tenha sido ativado principal executando o seguinte comando: 

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Se a variável [ `log_bin` ](https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin) é devolvido com o valor "ON", o registo binário está ativado no seu servidor. 

   Se `log_bin` é devolvido com o valor "OFF", ative binário registo editando o ficheiro my.cnf assim que `log_bin=ON` e reinicie o servidor para que a alteração surta efeito.

2. Definições do servidor primário

   Replicação de dados requer o parâmetro `lower_case_table_names` para estar consistente entre os servidores primário e réplica. Este parâmetro é 1 por predefinição na base de dados do Azure para MySQL. 

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

3. Criar uma nova função de replicação e configurar a permissão

   Crie uma conta de utilizador no servidor primário que está configurado com privilégios de replicação. Isto pode ser feito através de comandos do SQL Server ou uma ferramenta como o MySQL Workbench. Considere se planeia replicar com SSL, esta terá de ser especificado ao criar o utilizador. Consulte a documentação do MySQL, para compreender como [adicionar contas de utilizador](https://dev.mysql.com/doc/refman/5.7/en/adding-users.html) no seu servidor primário. 

   Os comandos abaixo, a nova função de replicação criada é capaz de aceder principal a partir de qualquer máquina, não apenas a máquina que aloja o principal de si próprio. Isto é feito através da especificação de "syncuser@'%'" no comando de criação do utilizador. Consulte a documentação de MySQL para saber mais sobre [especificando os nomes das contas](https://dev.mysql.com/doc/refman/5.7/en/account-names.html).

   **Comando SQL**

   *Replicação com SSL*

   Para exigir SSL para todas as ligações de utilizador, utilize o seguinte comando para criar um utilizador: 

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
   ```

   *Replicação sem SSL*

   Se o SSL não é necessário para todas as ligações, utilize o seguinte comando para criar um utilizador:

   ```sql
   CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
   GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
   ```

   **MySQL Workbench**

   Para criar a função de replicação no MySQL Workbench, abra o **utilizadores e privilégios** painel do **gestão** painel. Em seguida, clique em **adicionar conta**. 
 
   ![Os utilizadores e privilégios](./media/howto-data-in-replication/users_privileges.png)

   Escreva o nome de utilizador para o **nome de início de sessão** campo. 

   ![Utilizador de sincronização](./media/howto-data-in-replication/syncuser.png)
 
   Clique em de **funções administrativas** painel e, em seguida, selecione **replicação multisservidor** da lista de **privilégios Global**. Em seguida, clique em **aplicar** para criar a função de replicação.

   ![A replicação multisservidor](./media/howto-data-in-replication/replicationslave.png)


4. Definir o servidor principal para o modo só de leitura

   Antes de começar a informação do Estado da saída da base de dados, o servidor tem de ser colocado no modo só de leitura. Enquanto estiver no modo só de leitura, o principal será não é possível processar quaisquer transações de escrita. Avaliar o impacto ao seu negócio e agendar a janela de só de leitura numa hora fora de pico, se necessário.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

5. Obter nome de ficheiro de registo binário e deslocamento

   Execute o [ `show master status` ](https://dev.mysql.com/doc/refman/5.7/en/show-master-status.html) o comando para determinar o nome de ficheiro de registo binário atual e o desvio.
    
   ```sql
   show master status;
   ```
   Os resultados deverão ser semelhante. Certifique-se de que tome nota do nome de ficheiro binário que será utilizado em passos posteriores.

   ![Resultados de estado do mestre](./media/howto-data-in-replication/masterstatus.png)
 
## <a name="dump-and-restore-primary-server"></a>Informação do Estado e restaurar o servidor primário

1. Informação do Estado todas as bases de dados do servidor primário

   Pode utilizar mysqldump para bases de dados de captura do servidor primário. Para obter detalhes, consulte [informação do Estado da & Restaurar](concepts-migrate-dump-restore.md). É desnecessário informação do Estado da biblioteca de MySQL e biblioteca de teste.

2. Definir o servidor primário para o modo de leitura/escrita

   Assim que tiver sido capturar a base de dados, alterar o principal anterior de servidor MySQL para modo de leitura/escrita.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Restaurar o ficheiro de informação para o novo servidor

   Restaure o ficheiro de captura para o servidor criada na base de dados do Azure para o serviço de MySQL. Consulte [informação do Estado da & Restaurar](concepts-migrate-dump-restore.md) como restaurar um ficheiro de informação para um servidor de MySQL. Se o ficheiro de captura for grande, carregá-lo a uma máquina virtual no Azure na mesma região que o servidor de réplica. Restaurá-lo para a base de dados do Azure para o servidor de MySQL da máquina virtual.

## <a name="link-primary-and-replica-servers-to-start-data-in-replication"></a>Ligar os servidores primário e réplica para iniciar a replicação de dados

1. Servidor primário do conjunto

   Todas as funções do dados na replicação são efetuadas pela procedimentos armazenados. Pode encontrar todos os procedimentos em [dados na replicação procedimentos armazenados](reference-data-in-stored-procedures.md). Os procedimentos armazenados podem ser executados na shell de MySQL ou MySQL Workbench. 

   Para ligar os dois servidores e iniciar a replicação, início de sessão para o servidor de réplica de destino na base de dados do Azure para o serviço de MySQL e defina a instância externa que o servidor principal. Isto é feito utilizando o `mysql.az_replication_change_primary` procedimento armazenado da base de dados do Azure para o servidor de MySQL.

   ```sql
   CALL mysql.az_replication_change_primary('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```

   - master_host: nome de anfitrião do servidor primário
   - master_user: nome de utilizador para o servidor primário
   - master_password: palavra-passe para o servidor primário
   - master_log_file: nome de ficheiro de registo binário a execução `show master status`
   - master_log_pos: posição registo binário a execução `show master status`
   - master_ssl_ca: contexto de certificado de AC. Se não utilizar SSL, passe uma cadeia vazia.
       - Recomenda-se passar este parâmetro como uma variável. Consulte os exemplos seguintes para obter mais informações.

   **Exemplos**

   *Replicação com SSL*

   A variável `@cert` é criada executando os seguintes comandos de MySQL: 

   ```sql
   SET @cert = '-----BEGIN CERTIFICATE-----
   PLACE YOUR PUBLIC KEY CERTIFICATE’S CONTEXT HERE
   -----END CERTIFICATE-----'
   ```

   A replicação com o SSL é configurada entre um servidor primário alojado no domínio "companya.com" e um servidor de réplica alojados na base de dados do Azure para MySQL. Este procedimento armazenado é executado na réplica. 

   ```sql
   CALL mysql.az_replication_change_primary('primary.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, @cert);
   ```
   *Replicação sem SSL*

   Replicação sem SSL está configurada entre um servidor primário alojado no domínio "companya.com" e um servidor de réplica alojados na base de dados do Azure para MySQL. Este procedimento armazenado é executado na réplica.

   ```sql
   CALL mysql.az_replication_change_primary('primary.companya.com', 'syncuser', 'P@ssword!', 3306, 'mysql-bin.000002', 120, '');
   ```

2. Iniciar a replicação

   Chamar o `mysql.az_replication_start` procedimento para iniciar a replicação armazenado.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Verifique o estado de replicação

   Chamar o [ `show slave status` ](https://dev.mysql.com/doc/refman/5.7/en/show-slave-status.html) comando no servidor de réplica para ver o estado de replicação.
    
   ```sql
   show slave status;
   ```

   Se o estado de `Slave_IO_Running` e `Slave_SQL_Running` são "Sim" e o valor da `Seconds_Behind_Master` é "0", a replicação está a funcionar corretamente. `Seconds_Behind_Master` indica como dinâmico a réplica. Se o valor não é "0", significa que a réplica está a processar as atualizações. 

## <a name="other-stored-procedures"></a>Outros procedimentos armazenados

### <a name="stop-replication"></a>Parar replicação

Para parar a replicação entre o servidor primário e réplica, utilize o seguinte procedimento armazenado:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-replication-relationship"></a>Remova a relação de replicação

Para remover a relação entre o servidor primário e réplica, utilize o seguinte procedimento armazenado:

```sql
CALL mysql.az_replication_remove_primary;
```

### <a name="skip-replication-error"></a>Ignorar erro de replicação

Para ignorar a um erro de replicação e permitir que a replicação continuar, utilize o seguinte procedimento armazenado:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [replicação de dados](concepts-data-in-replication.md) para a base de dados do Azure para MySQL. 