---
title: Resolver problemas da sincronização de dados SQL do Azure | Documentos da Microsoft
description: Saiba como resolver problemas comuns com sincronização de dados SQL do Azure.
services: sql-database
ms.date: 07/16/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
manager: craigg
ms.custom: data-sync
ms.openlocfilehash: 2853310df1183a8b32450c5b5c79289b78d1a26d
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422506"
---
# <a name="troubleshoot-issues-with-sql-data-sync"></a>Resolver problemas com a sincronização de dados SQL

Este artigo descreve como resolver problemas conhecidos com sincronização de dados SQL do Azure. Se existir uma resolução para um problema, ele é fornecido aqui.

Para obter uma descrição geral da Sincronização de Dados SQL, veja [Sincronizar dados em várias bases de dados na cloud e no local com a Sincronização de Dados SQL do Azure](sql-database-sync-data.md).

## <a name="sync-issues"></a>Problemas de sincronização

- [Falha de sincronização no portal da interface do Usuário para bases de dados no local que estão associados com o agente do cliente](#sync-fails)

- [Meu grupo de sincronização está bloqueado no estado de processamento](#sync-stuck)

- [Vejo incorretas que tenham dados nas minhas tabelas](#sync-baddata)

- [Posso ver os dados de chave primários inconsistentes após uma sincronização com êxito](#sync-pkdata)

- [Vejo uma degradação significativa do desempenho](#sync-perf)

- [Posso ver esta mensagem: "não é possível inserir o valor NULL na coluna <column>. Coluna não permite valores NULL." O que significa isto e como posso corrigi-lo?](#sync-nulls)

- [Como a sincronização de dados com referências circulares? Ou seja, quando os mesmos dados são sincronizados em vários grupos de sincronização e mantém a alteração assim?](#sync-circ)

### <a name="sync-fails"></a> Falha de sincronização no portal da interface do Usuário para bases de dados no local que estão associados com o agente do cliente

Sincronização falha no portal do SQL Data Sync da interface do Usuário para bases de dados no local que estão associados com o agente do cliente. No computador local que está a executar o agente, consulte os erros de System.IO.IOException no registo de eventos. Os erros de dizem que o disco tem espaço suficiente.

- **Causa**. A unidade não tem espaço suficiente.

- **Resolução**. Crie mais espaço na unidade em que está localizado no diretório % TEMP %.

### <a name="sync-stuck"></a> Meu grupo de sincronização está bloqueado no estado de processamento

Um grupo de sincronização na sincronização de dados SQL está o estado de processamento por muito tempo. Ele não responder para o **parar** comando e os registos não mostram nenhuma nova entrada.

Qualquer uma das seguintes condições poderá resultar num grupo de sincronização a ser bloqueado no estado de processamento:

- **Causa**. O agente do cliente está offline

- **Resolução**. Certifique-se de que o agente do cliente está online e, em seguida, tente novamente.

- **Causa**. O agente do cliente é desinstalada ou em falta.

- **Resolução**. Se o agente do cliente é desinstalada ou de outra forma de em falta:

    1. Remova o ficheiro XML do agente da pasta de instalação de sincronização de dados SQL, se o ficheiro existe.
    1. Instale o agente num computador no local (pode ser a mesma ou num computador diferente). Em seguida, envie a chave de agente que é gerada no portal para o agente que está a aparecer como offline.

- **Causa**. O serviço de sincronização de dados SQL está parado.

- **Resolução**. Reinicie o serviço de sincronização de dados SQL.

    1. Na **começar** menu, procure **serviços**.
    1. Nos resultados da pesquisa, selecione **serviços**.
    1. Encontrar o **sincronização de dados SQL** serviço.
    1. Se o estado do serviço for **parado**, faça duplo clique o nome do serviço e, em seguida, selecione **iniciar**.

> [!NOTE]
> Se as informações anteriores não mover o seu grupo de sincronização para fora do Estado de processamento, Support da Microsoft pode repor o estado do seu grupo de sincronização. Ter o estado do grupo de sincronização repor, além da [fórum de base de dados do Azure SQL](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted), criar uma publicação. A postagem, incluem o seu ID de subscrição e o ID de grupo de sincronização para o grupo que tem de ser reposto. Um engenheiro de Support da Microsoft irá responder à sua postagem e irá informá-lo quando o estado foi reposto.

### <a name="sync-baddata"></a> Vejo incorretas que tenham dados nas minhas tabelas

Se tabelas que têm o mesmo nome, mas que são de esquemas de banco de dados diferentes estão incluídas numa sincronização, verá incorretas que tenham dados nas tabelas após a sincronização.

- **Causa**. O processo de aprovisionamento do SQL Data Sync utiliza as mesmo tabelas de controlo para tabelas que têm o mesmo nome, mas que estão em esquemas diferentes. Por este motivo, as alterações de ambas as tabelas são refletidas na mesma tabela de controle. Isso faz com que as alterações de dados de errado durante a sincronização.

- **Resolução**. Certifique-se de que os nomes das tabelas envolvidas numa sincronização são diferentes, mesmo que as tabelas pertencem a diferentes esquemas numa base de dados.

### <a name="sync-pkdata"></a> Posso ver os dados de chave primários inconsistentes após uma sincronização com êxito

Uma sincronização é comunicada como concluída com êxito e o registo mostra sem linhas com falhas ou ignoradas, mas observar que os dados de chave primários são inconsistentes entre as bases de dados no grupo de sincronização.

- **Causa**. Este resultado é propositado. As alterações em qualquer coluna de chave primária resultaram em dados inconsistentes nas linhas em que a chave primária foi alterada.

- **Resolução**. Para evitar este problema, certifique-se de que não existem dados numa coluna de chave primária são alterados. Para corrigir este problema após ela ter ocorrido, elimine a linha com dados inconsistentes de todos os pontos finais no grupo de sincronização. Em seguida, reinserir a linha.

### <a name="sync-perf"></a> Vejo uma degradação significativa do desempenho

O desempenho degrada significativamente, possivelmente ao ponto em que ainda não é possível abrir a interface do Usuário da sincronização de dados.

- **Causa**. A causa mais provável é um loop de sincronização. Um loop de sincronização ocorre quando um grupo de sincronização ao sincronizar A aciona um grupo de sincronização por sincronização B, que dispara uma grupo de sincronização ao sincronizar A. A situação real pode ser mais complexa, e pode envolver a mais de dois grupos de sincronização no loop. O problema é que existe um acionar circular de sincronização que é causado por grupos de sincronização sobreposição entre si.

- **Resolução**. A melhor correção é prevenção. Certifique-se de que não tem referências circulares nos seus grupos de sincronização. Não não possível sincronizar todas as linhas que são sincronizadas por um grupo de sincronização por outro grupo de sincronização.

### <a name="sync-nulls"></a> Posso ver esta mensagem: "não é possível inserir o valor NULL na coluna <column>. Coluna não permite valores NULL." O que significa isto e como posso corrigi-lo? 
Esta mensagem de erro indica que tenha ocorrido um dos dois problemas seguintes:
-  Uma tabela não tem uma chave primária. Para corrigir este problema, adicione uma chave primária para todas as tabelas que se estiver a sincronizar.
-  Há uma cláusula WHERE na instrução CREATE INDEX. Sincronização de dados não lida com esta condição. Para corrigir este problema, remova a cláusula WHERE ou manualmente, faça as alterações a todas as bases de dados. 
 
### <a name="sync-circ"></a> Como a sincronização de dados com referências circulares? Ou seja, quando os mesmos dados são sincronizados em vários grupos de sincronização e mantém a alteração assim?
Sincronização de dados não lida com referências circulares. Certifique-se de que evitá-las. 

## <a name="client-agent-issues"></a>Problemas de agente do cliente

- [O agente do cliente de instalar, desinstalar ou repare a falha](#agent-install)

- [O agente do cliente não funciona depois de cancelar a desinstalação](#agent-uninstall)

- [Meu banco de dados não está listado na lista de agente](#agent-list)

- [Agente de cliente não começa (erro 1069)](#agent-start)

- [Não consigo submeter a chave de agente](#agent-key)

- [Não é possível eliminar o agente do cliente no portal se é possível contactar o seu banco de dados associadas no local](#agent-delete)

- [Aplicação local do agente de sincronização não é possível ligar ao serviço de sincronização local](#agent-connect)

### <a name="agent-install"></a> O agente do cliente de instalar, desinstalar ou repare a falha

- **Causa**. Muitos cenários poderão causar esta falha. Para determinar a causa específica para esta falha, consulte os registos.

- **Resolução**. Para encontrar a causa específica da falha, gerar e ver os registos do instalador do Windows. Pode ativar o registo no prompt de comando. Por exemplo, se o ficheiro transferido do AgentServiceSetup.msi LocalAgentHost.msi, gerar e examinar os ficheiros de registo utilizando as seguintes linhas de comando:

    -   Para instalações: `msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`
    -   Para desinstala: `msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

    Também pode ativar o registo para todas as instalações que são executadas pelo Windows Installer. O artigo da Base de dados de conhecimento da Microsoft [como ativar o registo do Windows Installer](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) fornece uma solução de um clique para ativar o registo para o Windows Installer. Ele também fornece a localização dos registos.

### <a name="agent-uninstall"></a> O agente do cliente não funciona depois de cancelar a desinstalação

O agente do cliente não funciona, mesmo depois de cancelar a sua desinstalação.

- **Causa**. Isto ocorre porque o agente de cliente de sincronização de dados SQL não armazena as credenciais.

- **Resolução**. Pode experimentar essas duas soluções:

    -   Utilize o Services. msc para reintroduzir as credenciais para o agente do cliente.
    -   Desinstalar este agente de cliente e, em seguida, instale um novo. Transfira e instale o agente de cliente mais recente do [Centro de transferências](http://go.microsoft.com/fwlink/?linkid=221479).

### <a name="agent-list"></a> Meu banco de dados não está listado na lista de agente

Quando tentar adicionar uma base de dados do SQL Server existente para um grupo de sincronização, a base de dados não aparece na lista de agentes.

Estes cenários podem fazer com que este problema:

- **Causa**. O grupo de agente e sincronização de cliente estiverem em datacenters diferentes.

- **Resolução**. O agente do cliente e o grupo de sincronização tem de estar no mesmo datacenter. Para configurar estas definições, tem duas opções:

    -   Crie um novo agente no Centro de dados onde está localizado o grupo de sincronização. Em seguida, registe-se a base de dados com que o agente.
    -   Elimine o grupo de sincronização atual. Em seguida, voltar a criar o grupo de sincronização no Centro de dados onde o agente está localizado.

- **Causa**. Lista do agente de cliente de bases de dados não é atual.

- **Resolução**. Pare e reinicie o serviço de agente do cliente.

    O agente local transfere a lista de bases de dados associadas apenas no primeiro envio da chave de agente. Ele não é transferido a lista de bases de dados associadas em submissões de chave de agente subsequentes. Bases de dados que são registrados durante uma transferência do agente não aparecem na instância original do agente.

### <a name="agent-start"></a> Agente de cliente não começa (erro 1069)

Descobre que o agente não está em execução num computador que aloja o SQL Server. Quando tenta iniciar manualmente o agente, verá uma caixa de diálogo que apresenta a mensagem "erro 1069: O serviço não foram iniciados devido a uma falha de início de sessão."

![Caixa de diálogo de erro 1069 de sincronização de dados](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

- **Causa**. Uma causa provável deste erro é que a palavra-passe no servidor local foi alterado, uma vez que criar o agente e a palavra-passe do agente.

- **Resolução**. Atualize palavra-passe o agente para a sua palavra-passe atual do servidor:

  1. Localize o serviço de agente de cliente de sincronização de dados SQL.  
    a. Selecione **iniciar**.  
    b. Na caixa de pesquisa, introduza **Services. msc**.  
    c. Nos resultados da pesquisa, selecione **serviços**.  
    d. Na **serviços** janela, desloque-se para a entrada para **agente de sincronização de dados SQL**.  
  1. Com o botão direito **agente de sincronização de dados SQL**e, em seguida, selecione **parar**.
  1. Com o botão direito **agente de sincronização de dados SQL**e, em seguida, selecione **propriedades**.
  1. No **das propriedades do agente de sincronização de dados do SQL**, selecione a **iniciar sessão** separador.
  1. Na **palavra-passe** , introduza a palavra-passe.
  1. Na **Confirmar palavra-passe** caixa, reintroduza a palavra-passe.
  1. Selecione **Apply** (Aplicar) e **OK**.
  1. Na **serviços** janela, com o botão direito a **agente de sincronização de dados SQL** serviço e, em seguida, clique em **iniciar**.
  1. Fechar o **serviços** janela.

### <a name="agent-key"></a> Não consigo submeter a chave de agente

Depois de criar ou voltar a criar uma chave para um agente, tentar enviar a chave do SqlAzureDataSyncAgent de aplicação. O envio de mensagens em fila não consegue concluir.

![Sincronizar a caixa de diálogo de erro - não é possível enviar a chave de agente](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

- **Pré-requisitos**. Antes de prosseguir, verifique os pré-requisitos seguintes:

  - O serviço Windows de sincronização de dados SQL está em execução.

  - A conta de serviço do serviço Windows de sincronização de dados SQL tem acesso à rede.

  - A saída através da porta 1433 é aberto na sua regra de local firewall.

  - O ip local é adicionado para o servidor ou a regra de firewall de base de dados para a base de dados de metadados de sincronização.

- **Causa**. A chave de agente identifica exclusivamente cada agente local. A chave tem de cumprir duas condições:

  -   A chave de agente de cliente no servidor de sincronização de dados SQL e o computador local tem de ser idêntica.
  -   A chave de agente do cliente pode ser utilizada apenas uma vez.

- **Resolução**. Se o agente não está a funcionar, é porque uma ou ambas estas condições não forem cumpridas. Para obter o seu agente para funcionar novamente:

  1. Gere uma nova chave.
  1. Aplicam-se a nova chave para o agente.

  Para aplicar a nova chave para o agente:

  1. No Explorador de ficheiros, aceda ao seu diretório de instalação do agente. O diretório de instalação predefinido é c:\\Program Files (x86)\\sincronização de dados do Microsoft SQL.
  1. Faça duplo clique no subdiretório bin.
  1. Abra a aplicação de SqlAzureDataSyncAgent.
  1. Selecione **submeter a chave de agente**.
  1. No espaço fornecido, cole a chave da área de transferência.
  1. Selecione **OK**.
  1. Feche o programa.

### <a name="agent-delete"></a> Não é possível eliminar o agente do cliente no portal se é possível contactar o seu banco de dados associadas no local

Se um ponto final local (ou seja, uma base de dados) que está registado com um agente de cliente de sincronização de dados SQL ficar inacessível, não é possível eliminar o agente do cliente.

- **Causa**. Não é possível eliminar o agente local porque a base de dados inacessível ainda está registado com o agente. Ao tentar eliminar o agente, o processo de eliminação tenta aceder a base de dados, o que ocorre uma falha.

- **Resolução**. Utilize "forçar a eliminação" para eliminar a base de dados inacessível.

> [!NOTE]
> Se tabelas de metadados de sincronização permanecerem depois de uma "eliminação de forçar," utilização `deprovisioningutil.exe` limpá-los.

### <a name="agent-connect"></a> Aplicação local do agente de sincronização não é possível ligar ao serviço de sincronização local

- **Resolução**. Experimente os passos seguintes:

  1. Sair da aplicação.  
  1. Abra o painel de serviços de componente.  
    a. Na caixa de pesquisa na barra de tarefas, introduza **Services. msc**.  
    b. Nos resultados da pesquisa, faça duplo clique em **serviços**.  
  1. Parar o **sincronização de dados SQL** serviço.
  1. Reinicie o **sincronização de dados SQL** serviço.  
  1. Reabra a aplicação.

## <a name="setup-and-maintenance-issues"></a>Problemas de configuração e manutenção

- [Receber uma mensagem de "disco sem espaço"](#setup-space)

- [Não consigo eliminar o meu grupo de sincronização](#setup-delete)

- [Eu não é possível anular o registo de uma base de dados do SQL Server no local](#setup-unreg)

- [Eu não tem privilégios suficientes para iniciar serviços do sistema](#setup-perms)

- [Uma base de dados tem o estado "Desatualizado"](#setup-date)

- [Um grupo de sincronização tem o estado "Desatualizado"](#setup-date2)

- [Um grupo de sincronização não é possível eliminar dentro de desinstalação ou parar o agente de três minutos](#setup-delete2)

- [O que acontece quando restaurar uma base de dados perdido ou danificado?](#setup-restore)

### <a name="setup-space"></a> Receber uma mensagem de "disco sem espaço"

- **Causa**. A mensagem de "disco sem espaço" pode aparecer se ficheiros restantes têm de ser eliminadas. Isto pode ser causado por um software antivírus ou estiverem abertos ficheiros quando são tentadas operações de eliminação.

- **Resolução**. Elimine manualmente os ficheiros de sincronização que estão na pasta % temp % (`del \*sync\* /s`). Em seguida, elimine os subdiretórios na pasta % temp %.

> [!IMPORTANT]
> Não elimine quaisquer ficheiros enquanto a sincronização está em curso.

### <a name="setup-delete"></a> Não consigo eliminar o meu grupo de sincronização

A tentativa de eliminar um grupo de sincronização falha. Qualquer um dos seguintes cenários podem resultar numa falha ao eliminar um grupo de sincronização:

- **Causa**. O agente do cliente está offline.

- **Resolução**. Certifique-se de que o agente do cliente está online e, em seguida, tente novamente.

- **Causa**. O agente do cliente é desinstalada ou em falta.

- **Resolução**. Se o agente do cliente é desinstalada ou de outra forma de em falta:  
    a. Remova o ficheiro XML do agente da pasta de instalação de sincronização de dados SQL, se o ficheiro existe.  
    b. Instale o agente num computador no local (pode ser a mesma ou num computador diferente). Em seguida, envie a chave de agente que é gerada no portal para o agente que está a aparecer como offline.

- **Causa**. Uma base de dados está offline.

- **Resolução**. Certifique-se de que estão online todas as suas bases de dados SQL e bancos de dados do SQL Server.

- **Causa**. O grupo de sincronização é o aprovisionamento ou a sincronizar.

- **Resolução**. Aguarde até que o processo de sincronização ou de aprovisionamento estar concluído e, em seguida, repita a eliminação do grupo de sincronização.

### <a name="setup-unreg"></a> Eu não é possível anular o registo de uma base de dados do SQL Server no local

- **Causa**. Provavelmente, está a tentar anular o registo de uma base de dados que já tenha sido eliminada.

- **Resolução**. Para anular o registo de uma base de dados do SQL Server no local, selecione a base de dados e, em seguida, selecione **forçar a eliminação**.

  Se esta operação não consegue remover a base de dados do grupo de sincronização:

  1. Pare e reinicie o serviço de anfitrião de agente do cliente:  
    a. Selecione o **iniciar** menu.  
    b. Na caixa de pesquisa, introduza **Services. msc**.  
    c. Na **programas** secção da pesquisa resulta painel, faça duplo clique em **serviços**.  
    d. Com o botão direito a **sincronização de dados SQL** serviço.  
    e. Se o serviço está em execução, impedi-lo.  
    f. Com o botão direito do serviço e, em seguida, selecione **iniciar**.  
    g. Verifique se a base de dados ainda está registado. Se já não estiver registado, já está. Caso contrário, continue com a próxima etapa.
  1. Abra a aplicação de agente do cliente (SqlAzureDataSyncAgent).
  1. Selecione **editar credenciais**e, em seguida, introduza as credenciais para a base de dados.
  1. Continue com a anulação do registo.

### <a name="setup-perms"></a> Eu não tem privilégios suficientes para iniciar serviços do sistema

- **Causa**. Este erro ocorre em duas situações:
  -   O nome de utilizador e/ou a palavra-passe está incorreta.
  -   A conta de utilizador especificado não tem privilégios suficientes para iniciar sessão como um serviço.

- **Resolução**. Conceder as credenciais do registo-no-como-serviço para a conta de utilizador:

  1. Aceda a **começar** > **painel de controlo** > **ferramentas administrativas** > **política de segurança Local**  >  **Política local** > **gestão de direitos de utilizador**.
  1. Selecione **iniciar sessão como um serviço**.
  1. Na **propriedades** diálogo caixa, adicione a conta de utilizador.
  1. Selecione **Apply** (Aplicar) e **OK**.
  1. Feche todas as janelas.

### <a name="setup-date"></a> Uma base de dados tem o estado "Desatualizado"

- **Causa**. Sincronização de dados SQL remove as bases de dados que tenham sido offline do serviço de 45 dias ou mais (como contados desde o momento em que a base de dados ficou offline). Se uma base de dados está offline para 45 dias ou mais e, em seguida, fica online novamente, o respetivo estado é **desatualizados**.

- **Resolução**. Pode evitar uma **desatualizados** Estado, garantindo que nenhuma das suas bases de dados ficam offline para 45 dias ou mais.

  Se o estado de uma base de dados for **desatualizados**:

  1. Remover a base de dados que tem um **desatualizados** estado do grupo de sincronização.
  1. Adicionar a base de dados de volta para o grupo de sincronização.

  > [!WARNING]
  > Perderá todas as alterações efetuadas a esta base de dados enquanto ela estava offline.

### <a name="setup-date2"></a> Um grupo de sincronização tem o estado "Desatualizado"

- **Causa**. Se uma ou mais alterações não se aplicam durante o período de retenção inteira de 45 dias, um grupo de sincronização pode tornar-se desatualizados.

- **Resolução**. Para evitar uma **desatualizados** estado para um grupo de sincronização, examinar os resultados das suas tarefas de sincronização no Visualizador do histórico em intervalos regulares. Investigar e resolver quaisquer alterações que não se aplicam.

  Se o estado de um grupo de sincronização é **desatualizados**, elimine o grupo de sincronização e, em seguida, voltar a criá-lo.

### <a name="setup-delete2"></a> Um grupo de sincronização não é possível eliminar dentro de desinstalação ou parar o agente de três minutos

Não é possível eliminar um grupo de sincronização dentro de três minutos de desinstalação ou parar o agente de cliente de sincronização de dados SQL associado.

- **Resolução**.

  1. Remover um grupo de sincronização, enquanto os agentes de sincronização associado estão online (recomendado).
  1. Se o agente está offline, mas está instalado, colocá-la online no computador no local. Aguarde que o estado do agente a aparecer como **Online** no portal de sincronização de dados SQL. Em seguida, remova o grupo de sincronização.
  1. Se o agente estiver offline, porque foi desinstalado:  
    a.  Remova o ficheiro XML do agente da pasta de instalação de sincronização de dados SQL, se o ficheiro existe.  
    b.  Instale o agente num computador no local (pode ser a mesma ou num computador diferente). Em seguida, envie a chave de agente que é gerada no portal para o agente que está a aparecer como offline.  
    c. Tente eliminar o grupo de sincronização.

### <a name="setup-restore"></a> O que acontece quando restaurar uma base de dados perdido ou danificado?

Se restaurar uma base de dados perdido ou danificado a partir de uma cópia de segurança, poderá haver uma não-convergência dos dados nos grupos de sincronização a que pertence a base de dados.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a sincronização de dados SQL, consulte:

-   [Sincronizar dados em várias bases de dados na cloud e no local com a Sincronização de Dados SQL do Azure](sql-database-sync-data.md)  
-   [Configurar a Sincronização de Dados SQL do Azure](sql-database-get-started-sql-data-sync.md)  
-   [Melhores práticas da Sincronização de Dados SQL do Azure](sql-database-best-practices-data-sync.md)  
-   [Monitorizar a Sincronização de Dados SQL do Azure com o Log Analytics](sql-database-sync-monitor-oms.md)  
-   Conclua os exemplos do PowerShell que mostram como configurar a Sincronização de Dados SQL:  
    -   [Utilizar o PowerShell para sincronizar entre várias bases de dados SQL do Azure](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [Utilizar o PowerShell para sincronizar entre uma Base de Dados SQL do Azure e uma base de dados do SQL Server no local](scripts/sql-database-sync-data-between-azure-onprem.md)  
-   [Transferir a documentação da API REST da Sincronização de Dados SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Para obter mais informações sobre a base de dados SQL, consulte:

-   [Descrição Geral da Base de Dados SQL](sql-database-technical-overview.md)
-   [Gestão do Ciclo de Vida da Base de Dados](https://msdn.microsoft.com/library/jj907294.aspx)
