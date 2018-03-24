---
title: Base de dados SQL do Azure gerida instância auditoria | Microsoft Docs
description: Saiba como começar a utilizar com a auditoria do Azure SQL da base de dados geridos instância com T-SQL
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 03/19/2018
ms.author: giladm
ms.openlocfilehash: 3d5a4ad3f4046dfdfe6eb3f7ddd931ccb240b1a9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Introdução ao Azure SQL da base de dados geridos instância auditoria

[Instância de geridos de base de dados de SQL do Azure](sql-database-managed-instance.md) controla de auditoria da base de dados eventos e escreve-los para uma auditoria iniciar sessão na sua conta do storage do Azure. Auditoria também:
- Ajuda a manter a conformidade de regulamentação, compreender a atividade de base de dados e obter informações sobre discrepâncias e anomalias que poderão indicar preocupações para a empresa ou suspeitas de violação de segurança.
- Ativa e facilita a conformidade com as normas de conformidade, apesar de não garantir a compatibilidade. Para obter mais informações sobre o Azure programas que conformidade de padrões de suporte, consulte o [Centro de fidedignidade do Azure](https://azure.microsoft.com/support/trust-center/compliance/).


## <a name="set-up-auditing-for-your-server"></a>Configurar a auditoria do servidor

A secção seguinte descreve a configuração da auditoria na sua instância geridos.
1. Aceda ao [Portal do Azure](https://portal.azure.com).
2. Os seguintes passos criar um Storage do Azure **contentor** onde estão armazenados os registos de auditoria.

   - Navegue para o Storage do Azure onde pretende armazenar os registos de auditoria.

     > [!IMPORTANT]
     > Utilize uma conta do storage na mesma região que o servidor gerido instância para evitar por várias regiões leituras/escritas.

   - A conta de armazenamento, aceda a **descrição geral** e clique em **Blobs**.

     ![Painel de navegação][1]

   - No menu superior, clique em **+ contentor** para criar um novo contentor.

     ![Painel de navegação][2]

   - Forneça um contentor **nome**, defina acesso público nível para **privada**e, em seguida, clique em **OK**.

     ![Painel de navegação][3]

   - Na lista de contentores, clique o contentor criado recentemente e, em seguida, clique em **as propriedades do contentor**.

     ![Painel de navegação][4]

   - Copie o URL do contentor clicando no ícone de copiar e guardar o URL (por exemplo, no bloco de notas) para utilização futura. O formato de URL do contentor deve ser `https://<StorageName>.blob.core.windows.net/<ContainerName>`

     ![Painel de navegação][5]

3. Os seguintes passos geram um Storage do Azure **SAS Token** utilizados para conceder direitos de acesso gerido a auditoria de instância para a conta de armazenamento.

   - Navegue para a conta de armazenamento do Azure onde criou o contentor no passo anterior.

   - Clique em **assinatura de acesso partilhado** no menu de definições de armazenamento.

     ![Painel de navegação][6]

   - Configure a SAS da seguinte forma:
     - **Permitido serviços**: Blob
     - **Data de início**: para evitar problemas relacionados com o fuso horário, recomenda-se para utilizar a data do ontem.
     - **Data de fim**: selecione a data em que este Token SAS expira. 

       > [!NOTE]
       > Renove o token após a expiração: para evitar falhas de auditoria.

     - Clique em **Gerar SAS**.

       ![Painel de navegação][7]

   - Depois de clicar em gerar SAS, o Token SAS é apresentada na parte inferior. Copie o token ao clicar no ícone de copiar e guardá-lo (por exemplo, no bloco de notas) para utilização futura.

     > [!IMPORTANT]
     > Remover o ponto de interrogação ("?") caráter a partir do início do token.

     ![Painel de navegação][8]

4. Ligar à sua instância gerida através do SQL Server Management Studio (SSMS).

5. Execute a seguinte instrução de T-SQL para **criar uma nova credencial** utilizando o URL do contentor e SAS Token que criou nos passos anteriores:

    ```SQL
    CREATE CREDENTIAL [<container_url>]
    WITH IDENTITY='SHARED ACCESS SIGNATURE',
    SECRET = '<SAS KEY>'
    GO
    ```

6. Execute a seguinte instrução de T-SQL para criar um novo Server Audit (escolher seu próprio nome de auditoria, utilize o URL de contentor que criou nos passos anteriores):

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>]
    TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
    GO
    ```

    Se não for especificado, `RETENTION_DAYS` predefinido é 0 (ilimitada retenção).

    Para obter informações adicionais:
    - [Auditoria de diferenças entre a instância geridos, BD SQL do Azure e SQL Server](#subheading-3)
    - [CRIAR A AUDITORIA DO SERVIDOR](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
    - [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

7. Crie uma especificação de auditoria do servidor ou a especificação de auditoria de base de dados, tal como faria para o SQL Server:
    - [Criar o guia de T-SQL de especificação de auditoria de servidor](https://docs.microsoft.com/ sql/t-sql/statements/create-server-audit-specification-transact-sql)
    - [Criar o guia de T-SQL de especificação de auditoria de base de dados](https://docs.microsoft.com/ sql/t-sql/statements/create-database-audit-specification-transact-sql)

8. Ative a auditoria do servidor que criou no passo 6:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="analyze-audit-logs"></a>Analisar os registos de auditoria
Existem vários métodos que pode utilizar para ver os registos de auditoria de blob.

- Utilize a função de sistema `sys.fn_get_audit_file` (T-SQL) para devolver os dados de registo de auditoria em formato tabular. Para obter mais informações sobre como utilizar esta função, consulte o [sys.fn_get_audit_file documentação](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Para obter uma lista completa dos métodos de consumo de registo de auditoria, consulte o [começar com a auditoria de base de dados do SQL Server](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).

> [!IMPORTANT]
> O método para a visualização de registos de auditoria do portal do Azure (painel de 'Registos de auditoria') está atualmente disponível para instância geridos.

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Auditoria de diferenças entre a instância geridos, SQL Database do Azure e SQL Server

As principais diferenças entre a auditoria de SQL na instância geridos, SQL Database do Azure e SQL Server no local são:

- Na instância geridos, a auditoria de SQL funciona no nível do servidor e armazena `.xel` ficheiros de registo no conta de armazenamento de Blobs do Azure.
- Base de dados SQL do Azure, a auditoria de SQL funciona ao nível da base de dados.
- No SQL Server no local / virtual máquinas, funciona de auditoria de SQL no servidor de nível, mas os arquivos de eventos no sistema de ficheiros/windows registos de eventos.

Auditoria XEvent na instância geridos suporta destinos de armazenamento de Blobs do Azure. Os registos do ficheiro e windows são **não suportado**.

A chave de diferenças no `CREATE AUDIT` sintaxe de auditoria para o armazenamento de Blobs do Azure são:
- Uma nova sintaxe `TO URL` é fornecida e permite-lhe especificar o URL do contentor de armazenamento de Blobs do Azure onde o `.xel` são colocados ficheiros.
- A sintaxe `TO FILE` é **não suportado** porque a instância geridos não é possível aceder a partilhas de ficheiros do Windows.
- Opção de encerramento é **não suportado**.
- `queue_delay` 0 é **não suportado**.


## <a name="next-steps"></a>Passos Seguintes

- Para obter uma lista completa dos métodos de consumo de registo de auditoria, consulte o [começar com a auditoria de base de dados do SQL Server](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).
- Para obter mais informações sobre o Azure programas que conformidade de padrões de suporte, consulte o [Centro de fidedignidade do Azure](https://azure.microsoft.com/support/trust-center/compliance/).


<!--Anchors-->
[Set up auditing for your server]: #subheading-1
[Analyze audit logs]: #subheading-2
[Auditing differences between Managed Instance, Azure SQL DB and SQL Server]: #subheading-3

<!--Image references-->
[1]: ./media/sql-managed-instance-auditing/1_blobs_widget.png
[2]: ./media/sql-managed-instance-auditing/2_create_container_button.png
[3]: ./media/sql-managed-instance-auditing/3_create_container_config.png
[4]: ./media/sql-managed-instance-auditing/4_container_properties_button.png
[5]: ./media/sql-managed-instance-auditing/5_container_copy_name.png
[6]: ./media/sql-managed-instance-auditing/6_storage_settings_menu.png
[7]: ./media/sql-managed-instance-auditing/7_sas_configure.png
[8]: ./media/sql-managed-instance-auditing/8_sas_copy.png
