---
title: Introdução à integração do registo do Azure | Microsoft Docs
description: Saiba como instalar o serviço de integração de registo do Azure e integrar registos a partir do armazenamento do Azure, os registos de auditoria do Azure e alertas do Centro de segurança do Azure.
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 05/25/2018
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 8ceffb666eb18ee7b087ad1e1dbc27b57388ee49
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641201"
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Integração de registo do Azure com o registo de diagnóstico do Azure e o reencaminhamento de eventos do Windows

Só deve utilizar a integração de registos do Azure se um [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) conector não está disponível a partir do seu fornecedor de incidente de segurança e Event Management (SIEM).

Integração de registo do Azure disponibiliza registos do Azure para o SIEM para que possa criar um dashboard unificado de segurança para todos os seus recursos.
Para obter mais informações sobre o estado de um conector de Monitor do Azure, contacte o fabricante do SIEM.

> [!IMPORTANT]
> Se o seu interesse primário é recolher registos de máquina virtual, a maioria dos fornecedores SIEM incluir esta opção na sua solução. Utilizar o SIEM conector do fornecedor é sempre a alternativa preferencial.

Este artigo ajuda-o a começar a utilizar a integração de registo do Azure. Concentra-se sobre como instalar o serviço de integração de registo do Azure e integrar o serviço de diagnóstico do Azure. O serviço de integração de registo do Azure, em seguida, recolhe informações de registo de eventos do Windows do canal de eventos de segurança do Windows a partir de máquinas virtuais implementadas numa infraestrutura do Azure como um serviço. Isto é semelhante à *reencaminhamento de eventos* que poderá utilizar um sistema local.

> [!NOTE]
> Integrar o resultado de integração de registo do Azure com um SIEM, é necessário o SIEM próprio. Para obter mais informações, consulte [integrar a integração de registo do Azure com o SIEM no local](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/).

O serviço de integração de registo do Azure é executado um físico ou um computador virtual com o Windows Server 2008 R2 ou posterior (Windows Server 2016 ou o Windows Server 2012 R2 é preferencial).

Um computador físico pode ser executado no local ou num site de alojamento. Se optar por executar o serviço de integração de registo do Azure numa máquina virtual, a máquina virtual pode estar localizada no local ou numa nuvem pública, tal como no Microsoft Azure.

A máquina física ou virtual a executar o serviço de integração de registo do Azure necessita de conectividade de rede na nuvem pública do Azure. Este artigo fornece detalhes sobre a configuração necessária.

## <a name="prerequisites"></a>Pré-requisitos

No mínimo, instalar a integração de registo do Azure requer os seguintes itens:

* Um **subscrição do Azure**. Se não tiver uma, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/free/).
* A **conta de armazenamento** que podem ser utilizados para o Windows Azure Diagnostics (WAD) registo. Pode utilizar uma conta de armazenamento pré-configuradas ou criar uma nova conta de armazenamento. Neste artigo, vamos descrevem como configurar a conta de armazenamento.

  > [!NOTE]
  > Dependendo do seu cenário, uma conta de armazenamento não poderá ser necessária. Para o cenário de diagnóstico do Azure abordado neste artigo, é necessária uma conta de armazenamento.

* **Dois sistemas**: 
  * Um computador que executa o serviço de integração de registo do Azure. Esta máquina recolhe as informações de registo que mais tarde são importadas para o SIEM. Este sistema:
    * Pode ser no local ou alojado no Microsoft Azure.  
    * Tem de ser executado um x64 versão do Windows Server 2008 R2 SP1 ou posterior, e ter o Microsoft .NET 4.5.1 instalado. Para determinar a versão do .NET instalada, consulte [determinar que versões do .NET Framework estão instaladas](https://msdn.microsoft.com/library/hh925568).  
    * Tem de ter conectividade para a conta de armazenamento do Azure que utilizou para o registo de diagnóstico do Azure. Neste artigo, vamos descrevem como confirmar a conetividade.
  * Uma máquina que pretende monitorizar. Esta é uma VM em execução como um [máquina virtual do Azure](../virtual-machines/virtual-machines-windows-overview.md). As informações de registo desta máquina são enviadas para a máquina de serviço de integração de registo do Azure.

Para obter uma demonstração rápida de como criar uma máquina virtual utilizando o portal do Azure, veja o vídeo seguinte:<br /><br />


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]

## <a name="deployment-considerations"></a>Considerações sobre implementação

Durante o teste, pode utilizar qualquer sistema que cumpra os requisitos mínimos do sistema operativo. Num ambiente de produção, a carga pode implicar planear como aumentar verticalmente ou aumentar horizontalmente.

Pode executar várias instâncias do serviço de integração de registo do Azure. No entanto, pode executar apenas uma instância do serviço por máquina física ou virtual. Além disso, pode contas de armazenamento de diagnóstico do Azure de balanceamento de carga para WAD. O número de subscrições para fornecer às instâncias de baseiam-se a sua capacidade.

> [!NOTE]
> Atualmente, não temos recomendações específicas sobre quando aumentar horizontalmente instâncias das máquinas de integração de registo do Azure (ou seja, as máquinas que executa o serviço de integração de registo do Azure) ou para contas de armazenamento ou subscrições. Tome decisões de dimensionamento, com base na sua as observações de desempenho em cada uma destas áreas.

Para ajudar a melhorar o desempenho, também tem a opção para dimensionar o serviço de integração de registo do Azure. As métricas de desempenho seguintes podem ajudar a dimensionar as máquinas que optar por executar o serviço de integração de registo do Azure:

* Numa máquina 8-processador (principal), uma única instância de integração de registo do Azure pode processar cerca de 24 milhões de eventos por dia (cerca de 1 milhão de eventos por hora).
* Numa máquina 4-processador (principal), uma única instância de integração de registo do Azure pode processar cerca de 1,5 milhões de eventos por dia (aproximadamente 62,500 eventos por hora).

## <a name="install-azure-log-integration"></a>Instalar a integração de registos do Azure

Para instalar a integração de registo do Azure, transfira o [a integração de registo do Azure](https://www.microsoft.com/download/details.aspx?id=53324) ficheiro de instalação. Conclua o processo de configuração. Escolha se pretende fornecer informações de telemetria para a Microsoft.

O serviço de integração de registo do Azure recolhe dados de telemetria a partir do computador no qual está instalado.  

Os dados de telemetria que são recolhidos incluem o seguinte:

* Exceções que ocorrem durante a execução de integração de registo do Azure.
* Métricas sobre o número de consultas e eventos processados.
* Estatísticas sobre qual Azlog.exe Opções da linha de comandos são utilizadas. 

> [!NOTE]
> Recomendamos que permitem à Microsoft recolher dados de telemetria. Pode desativar a recolha de dados de telemetria ao desmarcar a **permite à Microsoft recolher dados de telemetria** caixa de verificação.
>

![Captura de ecrã do painel de instalação, com a caixa de verificação de telemetria selecionado](./media/security-azure-log-integration-get-started/telemetry.png)


O processo de instalação é abrangido no vídeo seguinte:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]

## <a name="post-installation-and-validation-steps"></a>Passos de validação e de pós-instalação

Depois de concluir a configuração básica, está pronto para efetuar os passos de validação e de pós-instalação:

1. Abra o PowerShell como um Administrador. Em seguida, aceda a C:\Program Files\Microsoft integração de registo do Azure.
2. Importe os cmdlets de integração de registo do Azure. Para importar os cmdlets, execute o script `LoadAzlogModule.ps1`. Introduza `.\LoadAzlogModule.ps1`, e, em seguida, prima Enter (tenha em atenção a utilização de **.\\**  neste comando). Deverá ver algo semelhante que é apresentado na figura seguinte:

  ![Captura de ecrã da saída do comando LoadAzlogModule.ps1](./media/security-azure-log-integration-get-started/loaded-modules.png)
3. Em seguida, configure a integração de registo do Azure para utilizar um ambiente do Azure específico. Um *ambiente do Azure* é o tipo do Centro de dados de nuvem do Azure pretende trabalhar. Embora não existam vários ambientes do Azure, atualmente, as opções relevantes são **AzureCloud** ou **AzureUSGovernment**. Executar o PowerShell como administrador, certifique-se de que está em C:\Program Files\Microsoft Azure registo Integration\. Em seguida, execute este comando:

  `Set-AzlogAzureEnvironment -Name AzureCloud` (para **AzureCloud**)
  
  Se pretender utilizar a nuvem de US Government Azure, utilize **AzureUSGovernment** para o **-nome** variável. Atualmente, não são suportadas outras nuvens do Azure.  

  > [!NOTE]
  > Não receber comentários quando o comando for bem sucedida. 

4. Para poder monitorizar um sistema, terá do nome da conta de armazenamento que é utilizado para o Azure Diagnostics. No portal do Azure, aceda a **máquinas virtuais**. Procure a máquina virtual do Windows que irá monitorizar. No **propriedades** secção, selecione **definições de diagnóstico**.  Em seguida, selecione **agente**. Anote o nome da conta de armazenamento que é especificado. É necessário este nome de conta para o passo posterior.

  ![Captura de ecrã do painel de definições de diagnóstico do Azure](./media/security-azure-log-integration-get-started/storage-account-large.png) 

  ![Captura de ecrã de ativar a monitorização botão de ao nível do convidado](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)

  > [!NOTE]
  > Se a monitorização não foi ativada quando a máquina virtual foi criada, pode ativá-la conforme mostrado na imagem anterior.

5. Agora, volte para a máquina de integração de registo do Azure. Verifique se tem conectividade para a conta de armazenamento do sistema onde instalou a integração de registo do Azure. O computador que executa o serviço de integração de registo do Azure necessita de acesso à conta de armazenamento para obter informações ao diagnóstico do Azure com sessão iniciada cada um dos sistemas monitorizados. Para verificar a conectividade: 
  1. [Transferir o Explorador de armazenamento do Azure](http://storageexplorer.com/).
  2. Conclua a configuração.
  3. Quando a instalação estiver concluída, selecione **seguinte**. Deixe o **iniciar o Explorador de armazenamento do Microsoft Azure** caixa de verificação selecionada.  
  4. Inicie sessão no Azure.
  5. Certifique-se de que pode ver a conta de armazenamento que configurou para o Azure Diagnostics: 

   ![Captura de ecrã de contas do storage no Explorador de armazenamento](./media/security-azure-log-integration-get-started/storage-explorer.png)

  6. Algumas opções são apresentados em contas de armazenamento. Em **tabelas**, deverá ver uma tabela chamada **WADWindowsEventLogsTable**.

  Se a monitorização não foi ativada quando a máquina virtual foi criada, pode ativá-la, conforme descrito anteriormente.


## <a name="integrate-windows-vm-logs"></a>Integrar os registos VM do Windows

Neste passo, configure o computador que executa o serviço de integração de registo do Azure para ligar à conta de armazenamento que contém os ficheiros de registo.

Para concluir este passo, terá de algumas coisas:  
* **FriendlyNameForSource**: um nome amigável que pode aplicar a conta de armazenamento que configurou para a máquina virtual armazenar as informações de diagnóstico do Azure.
* **StorageAccountName**: O nome da conta do storage que especificou quando configurou o diagnóstico do Azure.  
* **StorageKey**: A chave de armazenamento para a conta de armazenamento onde as informações de diagnóstico do Azure são armazenadas para esta máquina virtual.  

Para obter a chave de armazenamento, execute os seguintes passos:
1. Aceda ao [Portal do Azure](http://portal.azure.com).
2. No painel de navegação, selecione **todos os serviços**.
3. No **filtro** box, introduza **armazenamento**. Em seguida, selecione **contas do Storage**.

  ![Captura de ecrã que mostra as contas de armazenamento em todos os serviços](./media/security-azure-log-integration-get-started/filter.png)

4. É apresentada uma lista de contas do storage. Faça duplo clique em conta que atribuiu ao armazenamento de registo.

  ![Captura de ecrã que mostra uma lista de contas de armazenamento](./media/security-azure-log-integration-get-started/storage-accounts.png)

5. Em **Definições**, selecione **Chaves de acesso**.

  ![Captura de ecrã que mostra a opção de chaves de acesso no menu](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)

6. Cópia **chave1**e, em seguida, guarde-o numa localização segura que tem acesso para o passo seguinte.
7. No servidor onde instalou a integração de registo do Azure, abra uma janela de linha de comandos como administrador. (Lembre-se de que abra uma janela da linha de comandos como administrador e não do PowerShell).
8. Aceda a C:\Program Files\Microsoft Azure registo integração.
9. Execute este comando: `Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>`.
 
  Exemplo:
  
  `Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

  Se pretender que o ID de subscrição apareçam de eventos XML, acrescente o ID de subscrição para o nome amigável:

  `Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`
  
  Exemplo:
  
  `Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

> [!NOTE]
> Aguarde até 60 minutos e, em seguida, veja os eventos que são solicitados da conta do storage. Para ver os eventos no registo a integração do Azure, selecione **Visualizador de eventos** > **registos do Windows** > **eventos reencaminhados**.

O seguinte vídeo aborda os passos anteriores:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="if-data-isnt-showing-up-in-the-forwarded-events-folder"></a>Se os dados não estão ser apresentado na pasta eventos reencaminhados
Se os dados não estão ser apresentado na pasta eventos reencaminhados depois de uma hora, conclua estes passos:

1. Verifique o computador que está a executar o serviço de integração de registo do Azure. Certifique-se de que se pode aceder ao Azure. Para testar a conectividade, num browser, experimente ir para o [portal do Azure](http://portal.azure.com).
2. Certifique-se de que a conta de utilizador Azlog tem permissão de escrita para a pasta users\Azlog.
  1. Abra o Explorador de Ficheiros.
  2. Aceda à C:\users.
  3. Clique com botão direito C:\users\Azlog.
  4. Selecione **segurança**.
  5. Selecione **NT Service\Azlog**. Verifique as permissões da conta. Se a conta está em falta a partir deste separador, ou se não estiverem mostrar as permissões adequadas, pode conceder as permissões de conta neste separador.
3. Quando executa o comando `Azlog source list`, certifique-se de que o armazenamento da conta que foi adicionado no comando `Azlog source add` está listado no resultado.
4. Para ver se estão reportados erros do serviço de integração de registo do Azure, aceda à **Visualizador de eventos** > **registos do Windows** > **aplicação**.

Caso se depare com problemas durante a instalação e configuração, pode criar um [pedido de suporte](../azure-supportability/how-to-create-azure-support-request.md). Para o serviço, selecione **integração de registo**.

Outra opção de suporte é o [fórum MSDN do Azure registo integração](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). No fórum do MSDN, a Comunidade pode fornecer o suporte, as respostas a questões e partilhar sugestões e truques sobre como obter o máximo partido da integração do registo do Azure. A equipa de integração de registo do Azure também monitoriza neste fórum. Ajudam a sempre que podem.

## <a name="integrate-azure-activity-logs"></a>Integrar os registos de atividade do Azure

O registo de atividade do Azure é um registo de subscrição que fornece informações sobre os eventos de nível de subscrição ocorridos no Azure. Isto inclui uma série de dados, a partir dos dados operacionais do Azure Resource Manager para atualizações de eventos de estado de funcionamento do serviço. Alertas do Centro de segurança do Azure também estão incluídos neste registo.
> [!NOTE]
> Antes de tentar os passos neste artigo, tem de consultar o [começar](security-azure-log-integration-get-started.md) artigo e conclua os passos não existe.

### <a name="steps-to-integrate-azure-activity-logs"></a>Passos para integrar os registos de atividade do Azure

1. Abra a linha de comandos e execute este comando:  ```cd c:\Program Files\Microsoft Azure Log Integration```
2. Execute este comando:  ```azlog createazureid```

    Este comando pede-lhe o início de sessão do Azure. O comando, em seguida, cria um Azure Active Directory principal de serviço em inquilinos do Azure AD que alojam as subscrições do Azure na qual o utilizador com sessão iniciada é um administrador, coadministrador ou um proprietário. O comando irá falhar se o utilizador com sessão iniciada é apenas um utilizador convidado no inquilino do Azure AD. Autenticação do Azure é feita através do Azure AD. Criar um principal de serviço para a integração de registo do Azure cria a identidade do Azure AD, que é dado acesso ao ler a partir de subscrições do Azure.
3.  Execute o seguinte comando para autorizar o principal de serviço de integração de registo do Azure criado no acesso do passo anterior para a leitura do registo de atividade para a subscrição. Tem de ser um proprietário da subscrição para executar o comando.

    ```Azlog.exe authorize subscriptionId``` Exemplo:

```AZLOG.exe authorize ba2c2367-d24b-4a32-17b5-4443234859```
4.  Verifique as seguintes pastas para confirmar que os ficheiros de JSON do registo de auditoria do Azure Active Directory são criados nos mesmos:
    - C:\Users\azlog\AzureResourceManagerJson
    - C:\Users\azlog\AzureResourceManagerJsonLD

> [!NOTE]
> Para obter instruções específicas sobre colocar as informações dos ficheiros JSON em sistemas security information and system management (SIEM) de eventos, contacte o fabricante do SIEM.

A assistência de Comunidade está disponível através de [fórum MSDN do Azure registo integração](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Neste fórum permite que as pessoas da Comunidade a integração de registo do Azure para suportar entre si com perguntas, respostas, sugestões e truques. Além disso, a equipa de integração de registo do Azure monitoriza neste fórum e ajuda a sempre que possível.

Também pode abrir um [pedido de suporte](../azure-supportability/how-to-create-azure-support-request.md). Selecione a integração de registo que o serviço para o qual está a pedir suporte.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a integração de registo do Azure, consulte os artigos seguintes: antes de tentar os passos neste artigo, tem de rever o artigo de introdução do Get e conclua os passos não existe.

* [Integração de registo do Azure para os registos do Azure](https://www.microsoft.com/download/details.aspx?id=53324). O Centro de transferências inclui detalhes, requisitos de sistema e as instruções de instalação para a integração de registo do Azure.
* [Introdução à integração de registos do Azure](security-azure-log-integration-overview.md). Este artigo apresenta-lhe a integração de registo do Azure, as suas capacidades principais e como funciona.
* [Passos de configuração do parceiro](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/). Este blogue mostra-lhe como configurar a integração de registo do Azure para trabalhar com soluções de parceiros Splunk, HP ArcSight e IBM QRadar. Descreve a nossa documentação de orientação atual sobre como configurar os componentes do SIEM. Consulte o fornecedor do SIEM para obter detalhes adicionais.
* [Integração de registo do Azure perguntas mais frequentes (FAQ)](security-azure-log-integration-faq.md). Estas FAQ responde a questões recorrentes sobre a integração de registo do Azure.
* [Integração de alertas do Centro de segurança do Azure com a integração de registo do Azure](../security-center/security-center-integrating-alerts-with-log-integration.md). Este artigo mostra-lhe como sincronizar alertas do Centro de segurança e eventos de segurança de máquinas virtuais que são recolhidos por diagnósticos do Azure e a atividade do Azure registos. Sincronizar os registos ao utilizar a sua solução de análise de registos do Azure ou SIEM.
* [Registos de auditoria de novas funcionalidades de diagnóstico do Azure e Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/). Esta mensagem de blogue apresenta-lhe os registos de auditoria do Azure e outras funcionalidades que podem ajudar a obter conhecimentos aprofundados sobre as operações dos seus recursos Azure.
