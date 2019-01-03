---
title: Configurar a sincronização de dados SQL do Azure | Documentos da Microsoft
description: Este tutorial mostra-lhe como configurar a sincronização de dados SQL do Azure
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 11/07/2018
ms.openlocfilehash: 7a009d310fe29ce67a16d48edb86a7e652088ac6
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53727926"
---
# <a name="tutorial-set-up-sql-data-sync-to-sync-data-between-azure-sql-database-and-sql-server-on-premises"></a>Tutorial: Configurar a sincronização de dados SQL para sincronizar dados entre a base de dados do Azure SQL e SQL Server no local

Neste tutorial, saiba como configurar a sincronização de dados SQL do Azure através da criação de um grupo de sincronização de híbrida que contém instâncias de base de dados do Azure SQL e SQL Server. O novo grupo de sincronização está completamente configurado e sincroniza-se num agendamento definido por.

Este tutorial parte do princípio de que tem pelo menos alguma experiência anterior com a base de dados SQL e com o SQL Server.

Para obter uma descrição geral da Sincronização de Dados SQL, veja [Sincronizar dados em várias bases de dados na cloud e no local com a Sincronização de Dados SQL do Azure](sql-database-sync-data.md).

Para exemplos do PowerShell completos que mostram como configurar a sincronização de dados SQL, veja os artigos seguintes:

- [Utilizar o PowerShell para sincronizar entre várias bases de dados SQL do Azure](scripts/sql-database-sync-data-between-sql-databases.md)
- [Utilizar o PowerShell para sincronizar entre uma Base de Dados SQL do Azure e uma base de dados do SQL Server no local](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!IMPORTANT]
> Sincronização de dados SQL do Azure faz **não** suporta a instância gerida da base de dados SQL do Azure neste momento.

## <a name="step-1---create-sync-group"></a>Passo 1 – criar o grupo de sincronização

### <a name="locate-the-data-sync-settings"></a>Localizar as definições de sincronização de dados

1. No seu browser, navegue para o portal do Azure.

2. No portal, localize as bases de dados SQL a partir do seu Dashboard ou a partir do ícone de bases de dados SQL na barra de ferramentas.

    ![Lista de bases de dados SQL do Azure](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3. Sobre o **bases de dados SQL** , selecione a base de dados SQL existente que pretende utilizar como a base de dados de hub para sincronização de dados. É aberta a página de base de dados SQL.

    A base de dados de hub é o ponto de final central da topologia de sincronização, no qual um grupo de sincronização tem vários pontos de extremidade do banco de dados. Todos os outros da base de dados pontos finais a sincronização de grupo - ou seja, todos os membro bases de dados - sync mesmo com a base de dados de hub.

4. Na página de banco de dados SQL para a base de dados selecionada, selecione **sincronização para outros bancos de dados**. É aberta a página de sincronização de dados.

    ![Sincronizar a outra opção de bases de dados](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Criar um novo grupo de sincronização

1. Na página de sincronização de dados, selecione **novo grupo de sincronização**. O **novo grupo de sincronização** com o passo 1, é aberta a página **criar o grupo de sincronização**, realçado. O **criar grupo de sincronização de dados** é aberta a página também.

2. Sobre o **criar grupo de sincronização de dados** página, efetue os seguintes procedimentos:

   1. Na **nome do grupo de sincronização** , insira um nome para o novo grupo de sincronização.

   2. Na **base de dados de metadados de sincronização** secção, escolha se pretende criar uma nova base de dados (recomendado) ou utilizar uma base de dados existente.

        > [!NOTE]
        > A Microsoft recomenda que crie um novo banco de dados vazio para utilizar como a base de dados de metadados de sincronização. Sincronização de dados cria as tabelas nesta base de dados e executa uma carga de trabalho frequente. Esta base de dados é partilhada automaticamente como a base de dados de metadados de sincronização para todos os grupos de sincronização na região selecionada. Não é possível alterar a base de dados de metadados de sincronização ou o respetivo nome sem remover todos os grupos de sincronização e agentes de sincronização na região.

        Se escolheu **nova base de dados**, selecione **criar nova base de dados.** O **base de dados SQL** é aberta a página. Sobre o **base de dados SQL** página, dê um nome e configurar a nova base de dados. Em seguida, selecione **OK**.

        Se escolheu **utilizar base dados existente**, selecione a base de dados a partir da lista.

   3. Na **sincronização automática** secção, primeiro selecione **no** ou **desativar**.

        Se escolheu **no**, na **frequência de sincronização** secção, introduza um número e selecione segundos, minutos, horas ou dias.

        ![Especifique a frequência de sincronização](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

   4. Na **resolução de conflitos** secção, selecione "Hub wins" ou "Wins de membro".

        "Hub wins" significa que, quando ocorrer um conflito, os dados na base de dados de hub substitui os dados em conflito na base de dados do membro. "Wins de membro" significa que, quando ocorrer um conflito, os dados na base de dados membro substitui os dados em conflito na base de dados de hub.

       ![Especifique a forma como são resolvidos os conflitos](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

   5. Selecione **OK** e espere até que o novo grupo de sincronização a ser criada e implementada.

## <a name="step-2---add-sync-members"></a>Passo 2 - adicionar membros de sincronização

Depois do novo grupo de sincronização é criado e implementado, passo 2, **adicionar membros de sincronização**, está realçado na **novo grupo de sincronização** página.

Na **base de dados de Hub** , digite as credenciais existentes para o servidor de base de dados SQL, onde está localizada a base de dados de hub. Não introduza *novo* credenciais nesta secção.

![Base de dados de hub foi adicionado ao grupo de sincronização](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

### <a name="add-an-azure-sql-database"></a>Adicionar uma base de dados SQL do Azure

Na **base de dados do membro** secção, opcionalmente, adicione uma base de dados do SQL do Azure para o grupo de sincronização ao selecionar **adicionar uma base de dados do Azure**. O **configurar a base de dados do Azure** é aberta a página.

Sobre o **configurar a base de dados do Azure** página, efetue os seguintes procedimentos:

1. Na **nome de membro de sincronização** campo, forneça um nome para o novo membro de sincronização. Este nome é diferente do nome da base de dados em si.

2. Na **subscrição** campo, selecione a subscrição do Azure associada para fins de faturação.

3. Na **servidor SQL do Azure** campo, selecione o servidor de base de dados SQL existente.

4. Na **base de dados do Azure SQL** campo, selecione a base de dados SQL existente.

5. Na **direções de sincronização** campo, selecione sincronização bidirecional, para o Hub ou de Hub.

    ![Adicionar um novo membro de sincronização de base de dados SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6. Na **nome de utilizador** e **palavra-passe** campos, introduza as credenciais existentes para o servidor de base de dados SQL, onde está localizada a base de dados do membro. Não introduza *novo* credenciais nesta secção.

7. Selecione **OK** e espere até que o novo membro de sincronização ser criado e implementado.

    ![Foi adicionado o novo membro de sincronização de base de dados SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

### <a name="add-on-prem"></a> Adicionar uma base de dados do SQL Server no local

Na **base de dados do membro** secção, opcionalmente, adicione um servidor de SQL no local para o grupo de sincronização ao selecionar **adicionar uma base de dados no local**. O **configurar no local** é aberta a página.

Sobre o **configurar no local** página, efetue os seguintes procedimentos:

1. Selecione **escolha o Gateway do agente de sincronização**. O **selecionar agente de sincronização** é aberta a página.

    ![Escolha o gateway do agente de sincronização](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2. Sobre o **escolha o Gateway do agente de sincronização** página, escolha se pretende utilizar um agente existente ou criar um novo agente.

    Se escolheu **agentes existentes**, selecione o agente existente na lista.

    Se escolheu **criar um novo agente**, efetue os seguintes procedimentos:

   1. Transferir o software cliente do agente de sincronização da ligação fornecida e instalá-lo no computador onde está localizado o SQL Server. Também pode transferir o agente de sincronização de dados diretamente a partir [agente de sincronização de dados do SQL Azure](https://www.microsoft.com/download/details.aspx?id=27693).

        > [!IMPORTANT]
        > Terá de abrir a porta de saída TCP 1433 na firewall para permitir que o agente do cliente comunicar com o servidor.

   2. Introduza um nome para o agente.

   3. Selecione **criar e gerar chave**.

   4. Copie a chave de agente na área de transferência.

        ![Criar um novo agente de sincronização](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

   5. Selecione **OK** para fechar a **selecionar agente de sincronização** página.

   6. No computador do SQL Server, localize e execute a aplicação de agente de sincronização do cliente.

        ![Aplicação cliente do agente de sincronização de dados](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

   7. Na aplicação do agente de sincronização, selecione **chave de agente submeter**. O **configuração de base de dados de metadados de sincronização** é aberta a caixa de diálogo.

   8. Na **configuração de base de dados de metadados de sincronização** caixa de diálogo, cole a chave de agente que copiou do portal do Azure. Também fornece as credenciais existentes para o servidor da SQL Database do Azure onde está localizada a base de dados de metadados. (Se tiver criado uma nova base de dados de metadados, esta base de dados está no mesmo servidor da base de dados de hub.) Selecione **OK** e espere até que a configuração concluir.

        ![Introduza as credenciais de chave e o servidor de agente](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        > [!NOTE]
        > Se obtiver um erro de firewall neste momento, tem de criar uma regra de firewall no Azure para permitir tráfego de entrada do computador do SQL Server. Pode criar manualmente a regra no portal, mas talvez ache mais fácil para criá-lo no SQL Server Management Studio (SSMS). No SSMS, tentando estabelecer ligação à base de dados de hub no Azure. Introduza o respetivo nome como < hub_database_name >. database.windows.net. Para configurar a regra de firewall do Azure, siga os passos na caixa de diálogo. Em seguida, volte para a aplicação de agente de sincronização do cliente.

   9. Na aplicação do agente de sincronização do cliente, clique em **registar** para registar uma base de dados do SQL Server com o agente. O **configuração do SQL Server** é aberta a caixa de diálogo.

        ![Adicionar e configurar uma base de dados do SQL Server](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

   10. Na **configuração do SQL Server** diálogo caixa, escolha se pretende ligar utilizando a autenticação do SQL Server ou a autenticação do Windows. Se escolheu a autenticação do SQL Server, introduza as credenciais existentes. Forneça o nome do SQL Server e o nome da base de dados que pretende sincronizar. Selecione **Testar ligação** para testar as suas definições. Em seguida, selecione **Guardar**. A base de dados registado aparece na lista.

        ![Base de dados do SQL Server está agora registado](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

   11. Agora, pode fechar a aplicação de agente de sincronização do cliente.

   12. No portal, sobre o **configurar no local** página, selecione **selecione a base de dados.** O **selecionar base de dados** é aberta a página.

   13. No **selecionar base de dados** página, além do **nome do membro de sincronização** campo, forneça um nome para o novo membro de sincronização. Este nome é diferente do nome da base de dados em si. Selecione a base de dados a partir da lista. Na **direções de sincronização** campo, selecione sincronização bidirecional, para o Hub ou de Hub.

        ![Selecione a base de dados no local](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

   14. Selecione **OK** para fechar a **selecionar base de dados** página. Em seguida, selecione **OK** para fechar a **configurar no local** página e espere até que o novo membro de sincronização ser criado e implementado. Por fim, clique em **OK** para fechar a **selecionar membros de sincronização** página.

        ![Na base de dados locais adicionado ao grupo de sincronização](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3. Para ligar a sincronização de dados SQL e o agente local, adicione o seu nome de utilizador à função `DataSync_Executor`. Sincronização de dados cria esta função na instância do SQL Server.

## <a name="step-3---configure-sync-group"></a>Passo 3 - configurar o grupo de sincronização

Depois dos novos membros do grupo de sincronização são criados e implementados, passo 3, **configurar o grupo de sincronização**, está realçado na **novo grupo de sincronização** página.

1. Sobre o **tabelas** página, selecione uma base de dados da lista de membros do grupo de sincronização e, em seguida, selecione **esquema de atualização**.

2. Na lista de tabelas disponíveis, selecione as tabelas que pretende sincronizar.

    ![Selecionar tabelas a sincronizar](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3. Por predefinição, todas as colunas na tabela estão selecionadas. Se não quiser sincronizar todas as colunas, desative a caixa de verificação para as colunas que não pretende sincronizar. Certifique-se de que deixar a coluna de chave primária selecionada.

    ![Selecionar campos a sincronizar](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4. Por fim, selecione **guardar**.

## <a name="faq-about-setup-and-configuration"></a>FAQ sobre a instalação e configuração

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>A frequência com que a sincronização de dados pode sincronizar os meus dados

A duração mínima entre acionar as sincronizações é de cinco minutos.

### <a name="does-sql-data-sync-fully-create-and-provision-tables"></a>Sincronização de dados SQL totalmente criar e aprovisionar tabelas

Se as tabelas do esquema de sincronização já não forem criadas na base de dados de destino, a sincronização de dados SQL cria-as com as colunas que selecionou. No entanto, esse comportamento não resulta num esquema de fidelidade total, pelos seguintes motivos:

- Apenas as colunas que selecionou são criadas na tabela de destino. Se algumas colunas nas tabelas de origem não fazem parte do grupo de sincronização, essas colunas não são aprovisionadas nas tabelas de destino.
- Os índices são criados apenas para as colunas selecionadas. Se o índice de tabela de origem tem colunas que não fazem parte do grupo de sincronização, esses índices não são aprovisionados nas tabelas de destino.
- Índices em colunas de tipo XML não são aprovisionados.
- Restrições de verificação não são aprovisionadas.
- Os acionadores existentes em tabelas de origem não são aprovisionados.
- As vistas e procedimentos armazenados não são criados na base de dados de destino.

Devido a essas limitações, recomendamos os seguintes procedimentos:

- Para ambientes de produção, Aprovisione o esquema de fidelidade total por conta própria.
- Para experimentar o serviço, a funcionalidade de aprovisionamento automático de sincronização de dados SQL funciona bem.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Por que vejo as tabelas que eu não criei

Sincronização de dados cria tabelas do lado da base de dados de registo de alterações. Não elimine-os ou sincronização de dados deixa de funcionar.

### <a name="is-my-data-convergent-after-a-sync"></a>É o meus dados convergent depois de uma sincronização

Não necessariamente. Num grupo de sincronização com um hub- and -spoke de três (A, B e C), as sincronizações são Hub para um Hub para o B e Hub para C. Se for feita uma alteração à base de dados A *depois de* Hub para uma sincronização, o que a alteração não gravada na base de dados B ou C da base de dados até que a tarefa de sincronização seguinte.

### <a name="how-do-i-get-schema-changes-into-a-sync-group"></a>Como posso obter as alterações de esquema num grupo de sincronização

Terá de fazer e propagar manualmente a todas as alterações de esquema.

1. Replicar as alterações de esquema manualmente para o hub e a todos os membros de sincronização.
2. Atualize o esquema de sincronização.

**Adicionar novas tabelas e colunas**.

Novas tabelas e colunas não tenham impacto sobre a sincronização atual. Sincronização de dados ignora as novas tabelas e colunas até adicionar ao esquema de sincronização. Quando adiciona novos objetos de base de dados, esta é a sequência de melhor a seguir:

1. Adicione as novas tabelas ou colunas para o hub e a todos os membros de sincronização.
2. Adicione as novas tabelas ou colunas ao esquema de sincronização.
3. Começa a inserir valores para as novas tabelas e colunas.

**Alterar o tipo de dados de uma coluna**.

Quando altera o tipo de dados de uma coluna existente, sincronização de dados continua a funcionar, desde que os novos valores de acordo com o tipo de dados original, definido no esquema de sincronização. Por exemplo, se alterar o tipo de base de dados de origem **int** ao **bigint**, sincronização de dados continua a funcionar até que inserir um valor que é demasiado grande para o **int** tipo de dados . Para concluir a alteração, replique a alteração de esquema manualmente para o hub e a todos os membros de sincronização e, em seguida, atualize o esquema de sincronização.

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Como exportar e importar uma base de dados com a sincronização de dados

Depois de exportar uma base de dados como um `.bacpac` de ficheiro e importe-o para criar uma nova base de dados, tem de efetue os seguintes dois procedimentos para utilizar a sincronização de dados na nova base de dados:

1. Limpar os objetos de sincronização de dados e tabelas de lado no **nova base de dados** utilizando [este script](https://github.com/vitomaz-msft/DataSyncMetadataCleanup/blob/master/Data%20Sync%20complete%20cleanup.sql). Este script elimina todos os objetos de sincronização de dados necessários da base de dados.
2. Recrie o grupo de sincronização com a nova base de dados. Se já não precisar do antigo grupo de sincronização, elimine-o.

## <a name="faq-about-the-client-agent"></a>FAQ sobre o agente do cliente

Para perguntas mais frequentes sobre o agente do cliente, consulte [FAQ do agente](sql-database-data-sync-agent.md#agent-faq).

## <a name="next-steps"></a>Passos Seguintes

Parabéns! Criou um grupo de sincronização que inclui uma instância de base de dados SQL e uma base de dados do SQL Server.

Para obter mais informações sobre a Sincronização de Dados SQL, veja:

-   Descrição geral - [sincronizar dados em várias bases de dados na cloud e no local com sincronização de dados SQL do Azure](sql-database-sync-data.md)
-   Configurar a sincronização de dados
    - Com o PowerShell
        -  [Utilizar o PowerShell para sincronizar entre várias bases de dados SQL do Azure](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [Utilizar o PowerShell para sincronizar entre uma Base de Dados SQL do Azure e uma base de dados do SQL Server no local](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Agente de - de sincronização de dados [agente de sincronização de dados SQL do Azure de sincronização de dados](sql-database-data-sync-agent.md)
-   Melhores práticas - [melhores práticas para a sincronização de dados SQL do Azure](sql-database-best-practices-data-sync.md)
-   Monitor de - [monitorizar a sincronização de dados SQL com o Log Analytics](sql-database-sync-monitor-oms.md)
-   Resolução de problemas - [solucionar problemas com a sincronização de dados SQL do Azure](sql-database-troubleshoot-data-sync.md)
-   Atualizar o esquema de sincronização
    -   Com o Transact-SQL - [automatizar a replicação das alterações de esquema na sincronização de dados SQL do Azure](sql-database-update-sync-schema.md)
    -   Com o PowerShell - [utilize o PowerShell para atualizar o esquema de sincronização num grupo de sincronização existente](scripts/sql-database-sync-update-schema.md)

Para obter mais informações sobre a Base de Dados SQL, veja:

- [Descrição Geral da Base de Dados SQL](sql-database-technical-overview.md)
- [Gestão do Ciclo de Vida da Base de Dados](https://msdn.microsoft.com/library/jj907294.aspx)
