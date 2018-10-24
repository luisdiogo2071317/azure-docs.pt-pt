---
title: Tutorial Windows ampliar servidores de ficheiros com o Azure File Sync | Documentos da Microsoft
description: Saiba como servidores de ficheiros de estender o Windows com o Azure File Sync, do início ao fim.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 10/23/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 4c3ceead792f60ceac7c5eddb64dc4644d662f83
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49960676"
---
<!---Customer intent: As a IT Administrator, I want see how to extend Windows file servers with Azure File Sync, so I can evaluate the process for extending storage capacity of my Windows Servers.
--->

# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Tutorial: Ampliar servidores de ficheiros do Windows com o Azure File Sync
Utilizar o Azure File Sync centralizar as partilhas de ficheiros da sua organização e para expandir a capacidade de armazenamento de um servidor de ficheiros no local. O Azure File Sync transforma o Windows Server numa cache rápida da sua partilha de ficheiros do Azure.

Neste tutorial, vamos mostrar as etapas básicas para expandir a capacidade de armazenamento do Windows Server com o Azure File Sync. Estamos a utilizar uma VM do Windows Server do Azure para este tutorial, mas normalmente faria o processo para os seus servidores no local. Se estiver pronto para implementar o Azure File Sync em seu próprio ambiente, utilize o [implementar Azure File Sync](storage-sync-files-deployment-guide.md) artigo em vez disso.

> [!div class="checklist"]
> * Implementar o serviço de sincronização de armazenamento
> * Preparar o Windows Server para utilizar com o Azure File Sync
> * Instalar o agente de sincronização de ficheiros do Azure
> * Registre-se o Windows Server com o serviço de sincronização de armazenamento
> * Criar um grupo de sincronização e um ponto final da cloud
> * Criar um ponto final do servidor

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="prepare-your-environment-for-the-tutorial"></a>Prepare seu ambiente para o tutorial
Existem algumas coisas que precisa colocar no local para este tutorial antes de implementar o Azure File Sync. Juntamente com a criação de uma conta de armazenamento do Azure e o ficheiro da partilha, irá criar uma VM do Windows Server 2016 Datacenter e preparar nesse servidor para o Azure File Sync.

### <a name="create-a-folder-and-txt-file"></a>Crie um ficheiro de pasta e. txt

No seu computador local, crie uma nova pasta chamada *FilesToSync* e adicione um arquivo de texto chamado *mytestdoc.txt*. deverá carregar esse ficheiro para a partilha de ficheiros mais tarde neste tutorial.

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Criar uma partilha de ficheiros
Em seguida, criar uma partilha de ficheiros.

1. Quando a implementação da conta de armazenamento do Azure estiver concluída, clique em **Ir para recurso**.
1. Clique em **ficheiros** partir do painel de conta de armazenamento.

    ![Clique em ficheiros](./media/storage-sync-files-extend-servers/click-files.png)

1. Clique em **+ partilha de ficheiros**.

    ![Clique no botão de partilha de ficheiros de adicionar](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Nomeie a nova partilha de ficheiros *afsfileshare* e introduza "1" para o **Quota**, em seguida, clique em **criar**. A quota pode ter um máximo de 5 TiB, mas precisa apenas 1 GB para este tutorial.

    ![Forneça um nome e a quota para a nova partilha de ficheiros](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Selecione a nova partilha de ficheiros, em seguida, na localização de partilha de ficheiros, clique em **carregar**.

    ![Carregar um ficheiro](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. Navegue para o *FilesToSync* pasta onde criou o seu ficheiro. txt, selecione *mytestdoc.txt* e clique em **carregar**.

    ![Procurar a partilha de ficheiros](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

Neste momento, criou uma conta de armazenamento do Azure e uma partilha de ficheiros com um ficheiro na mesma no Azure. Agora, vai criar a VM do Azure com o Windows Server 2016 Datacenter para representar o servidor no local neste tutorial.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>Implementar uma VM e anexar um disco de dados

1. Em seguida, expanda o menu no lado esquerdo do portal e selecione **criar um recurso** no canto superior esquerdo do portal do Azure.
1. Na caixa de pesquisa acima da lista de **do Azure Marketplace** recursos, procure e selecione **do Windows Server 2016 Datacenter**, em seguida, escolha **criar**.
1. Na **Noções básicas** separador, em **detalhes do projeto**, selecione o grupo de recursos que criou para este tutorial.

   ![Introduza as informações básicas sobre a VM no painel do portal](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. Sob **detalhes de instância**, forneça um nome VM, tal como *myVM*.
1. Deixe as predefinições para **região**, **opções de disponibilidade**, **imagem**, e **tamanho**.
1. Sob **conta de administrador**, forneça um **nome de utilizador** e **palavra-passe** para a VM.
1. Em **Regras da porta de entrada**, selecione **Permitir portas selecionadas** e, em seguida, selecione **RDP (3389)** e **HTTP** na lista pendente.

   Antes de criar a VM, terá de criar um disco de dados.

1. Clique em **próxima: discos**

   ![Adicionar discos de dados](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

1. Na **discos** separador, em **opções de disco**, deixe as predefinições. 
1. Sob **discos de dados**, clique em **criar e anexar um disco novo**.

1. Deixe as predefinições, exceto alteração do **tamanho (GiB)** ao **1GB** para este tutorial.

   ![Detalhes do disco de dados](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

1. Clique em **OK**.
1. Clique em **Rever + criar**.
1. Clique em **Criar**.

   Pode clicar a **notificações** ícone para ver a **progresso da implementação**. Criar uma nova VM irá demorar alguns minutos a concluir.

1. Uma vez concluída a implementação de VM, clique em **Ir para recurso**.

   ![Ir para recurso](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

   Neste momento, criou uma nova máquina virtual e um disco de dados anexado. Agora precisa de ligar à VM.

### <a name="connect-to-your-vm"></a>Ligar à sua VM

1. No portal do Azure, clique em **Connect** na página de propriedades da máquina virtual.

   ![Ligar a uma VM do Azure a partir do portal](./media/storage-sync-files-extend-servers/connect-vm.png)

1. Na **ligar à máquina virtual** página, mantenha as opções predefinidas para ligar ao **endereço IP** mais porta 3389 e clique em **ficheiro RDP transferir**.

   ![Transferir o ficheiro RDP](./media/storage-sync-files-extend-servers/download-rdp.png)

1. Abra o ficheiro RDP transferido e clique em **Ligar** quando lhe for pedido.
1. Na janela **Segurança do Windows**, selecione **Mais escolhas** e **Utilizar uma conta diferente**. Escreva o nome de utilizador como *localhost\username*, introduza a palavra-passe que criou para a máquina virtual e, em seguida, clique em **OK**.

   ![Mais opções](./media/storage-sync-files-extend-servers/local-host2.png)

1. Poderá receber um aviso de certificado durante o processo de início de sessão. Clique em **Sim** ou **Continuar** para criar a ligação.

### <a name="prepare-the-windows-server"></a>Preparar o servidor do Windows
Para o **Windows Server 2016 Datacenter** servidor, desative **configuração de segurança avançada do Internet Explorer**. Este passo é necessário apenas para o registo inicial do servidor. Pode voltar a ativá-lo Depois do servidor foi registado.

Na **Windows Server 2016 Datacenter** VM, **Gestor de servidor** será aberta automaticamente.  Se **Gestor de servidor** não abrir por predefinição, procure-o no Explorer.

1. Na **Gestor de servidores** clique em **servidor Local**.

   !["Servidor local", no lado esquerdo da IU do Gestor de servidores](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. Sobre o **propriedades** subpane, selecione a ligação para **configuração de segurança avançada do IE**.  

    ![O painel "Configuração avançada do IE segurança" na IU do Gestor de servidores](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. Na **a configuração de segurança avançada do Internet Explorer** caixa de diálogo, selecione **desativar** para **administradores** e **utilizadores**:  

    ![A janela de pop da configuração de segurança avançada do Internet Explorer com "Off" selecionado](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

   Agora pode adicionar o disco de dados para a VM.

### <a name="add-the-data-disk"></a>Adicionar o disco de dados

1. Enquanto estiver da **Windows Server 2016 Datacenter** VM, clique em **ficheiros e serviços de armazenamento** > **Volumes** > **discos** .

    ![Disco de dados](media/storage-sync-files-extend-servers/your-disk.png)

1. Com o botão direito do disco de 1 GB denominado **disco Virtual da Msft** e clique em **novo volume**.
1. Conclua o assistente deixar as predefinições no lugar, observar a letra de unidade atribuída e clique em **criar**.
1. Clique em **Fechar**.

   Neste momento, tive colocado o disco online e criado um volume. Pode confirmar se adicionar o disco de dados foi bem-sucedida abrindo o Explorer na VM e confirmar que a nova unidade está presente.

1. No Explorador de na VM, expanda **este PC** e faça duplo clique em nova unidade. É a unidade f: neste exemplo.
1. Com o botão direito e selecione **New** > **pasta**. Nome da pasta *FilesToSync*.
1. Clique duas vezes o **FilesToSync** pasta.
1. Com o botão direito e selecione **New** > **documento de texto**. Nomeie o arquivo de texto *MyTestFile*.

    ![Adicione um novo ficheiro de texto](media/storage-sync-files-extend-servers/new-file.png)

1. Fechar **Explorer** e **Gestor de servidor**.

### <a name="download-the-azurerm-powershell-module"></a>Transferir o módulo AzureRM PowerShell
Em seguida, no **Windows Server 2016 Datacenter** VM, instale o **módulo AzureRM PowerShell** no servidor.

1. Na VM, abra uma janela elevada do PowerShell
1. Execute o seguinte comando:

   ```powershell
   Install-Module -Name AzureRM -AllowClobber
   ```

   > [!NOTE]
   > Se tiver uma versão mais antiga do que a 2.8.5.201 do NuGet, será pedido que transfira e instale a versão mais recente do NuGet.

   Por predefinição, a galeria do PowerShell não está configurada como um repositório fidedigno para o PowerShellGet. Na primeira vez que utilizar o PSGallery verá a seguinte linha de comandos:

   ```output
   Untrusted repository

   You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet.

   Are you sure you want to install the modules from 'PSGallery'?
   [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
   ```

1. Responda `Yes` ou `Yes to All` para continuar com a instalação.

O módulo `AzureRM` é um módulo de rollup para os cmdlets do Azure PowerShell. Instalá-lo transfere todos os módulos disponíveis do Azure Resource Manager e torna seus cmdlets disponíveis para utilização.

Neste ponto, terminar de configurar o ambiente para o tutorial e está pronto para começar a implementar o **serviço de sincronização de armazenamento**.

## <a name="deploy-the-storage-sync-service"></a>Implementar o serviço de sincronização de armazenamento 
Implementar Azure File Sync começa com a colocação de um **serviço de sincronização de armazenamento** recursos num grupo de recursos para a sua subscrição selecionada. O serviço de sincronização de armazenamento herda as permissões de acesso da subscrição e grupo de recursos a implementá-la em.

1. No portal do Azure, clique em **criar um recurso** e, em seguida, procure **Azure File Sync**.
1. Nos resultados da pesquisa, selecione **do Azure File Sync**.
1. Selecione **Create** para abrir o **implementar sincronização de armazenamento** separador.

   ![Implementar Sincronização de Armazenamento](media/storage-sync-files-extend-servers/afs-info.png)

   No painel que se abre, introduza as seguintes informações:

   | Valor | Descrição |
   | ----- | ----- |
   | **Nome** | Um nome exclusivo (por subscrição) para o serviço de sincronização de armazenamento.<br><br>Neste tutorial, está a utilizar *afssyncservice02*. |
   | **Subscrição** | A subscrição que está a utilizar para este tutorial. |
   | **Grupo de recursos** | O grupo de recursos que está a utilizar para este tutorial.<br><br>Neste tutorial, está a utilizar *afsresgroup101918*. |
   | **Localização** | EUA Leste |

1. Quando tiver terminado, selecione **Create** para implementar a **serviço de sincronização de armazenamento**.
1. Clique nas **notificações** separador > **Ir para recurso**.

## <a name="install-the-azure-file-sync-agent"></a>Instalar o agente de sincronização de ficheiros do Azure
O agente de sincronização de ficheiros do Azure é um pacote disponível para download que permite ao Windows Server a ser sincronizada com uma partilha de ficheiros do Azure.

1. Mude novamente para o **Windows Server 2016 Datacenter** VM e abrir **do Internet Explorer**
1. Vá para o [Centro de transferências da Microsoft](https://go.microsoft.com/fwlink/?linkid=858257). Desloque para baixo para o **do Azure File Sync Agent** secção e clique em **transferir**.

   ![Transferência do agente de sincronização](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. Marque a caixa **StorageSyncAgent_V3_WS2016. EXE** e clique em **próxima**.

   ![Selecionar agente](media/storage-sync-files-extend-servers/select-agent.png)

1. **Permitir que uma vez** > **execute** > **aberto** o ficheiro.
1. Se ainda não o fez, feche a janela do PowerShell.
1. Aceite as predefinições no **Assistente de configuração de agente de sincronização de armazenamento**.
1. Clique em **Instalar**.
1. Clique em **Concluir**.

Implementou o serviço de sincronização do Azure e instalado o agente do **Windows Server 2016 Datacenter** VM. Agora precisa de registar a VM com o **serviço de sincronização de armazenamento**.

## <a name="register-windows-server-with-storage-sync-service"></a>Registre-se o Windows Server com o serviço de sincronização de armazenamento
Registar o seu Windows Server com um serviço de sincronização de armazenamento estabelece uma relação de confiança entre o servidor (ou cluster) e o serviço de sincronização de armazenamento. Um servidor só podem ser registrado para um serviço de sincronização de armazenamento e se podem sincronizar com outros servidores e do Azure associadas com o mesmo serviço de sincronização de armazenamento de partilhas de ficheiros.

A interface do Usuário de registo do servidor deverá abrir automaticamente depois de instalar o **agente do Azure File Sync**. Se isso não acontecer, pode abri-la manualmente da localização do ficheiro: C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe.

1. Quando abrir a interface do Usuário de registo do servidor na VM, clique em **OK**.
1. Clique em **início de sessão** para começar.
1. Inicie sessão com as credenciais da conta do Azure e clique em **início de sessão**.
1. Forneça as seguintes informações:

   ![Uma captura de ecrã da interface do Usuário do servidor de registo](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Valor | Descrição |
   | **Subscrição do Azure** | A subscrição que contém o serviço de sincronização de armazenamento para este tutorial. |
   | **Grupo de Recursos** | O grupo de recursos que contém o serviço de sincronização de armazenamento para este tutorial. Usamos *afsresgroup101918* neste tutorial. |
   | **Serviço de sincronização de armazenamento** | O nome do serviço de sincronização de armazenamento utilizada para este tutorial. Usamos *afssyncservice02* neste tutorial. |

1. Clique em **registar** para concluir o registo do servidor.
1. Como parte do processo de registo, lhe for pedido um adicional para início de sessão. Iniciar sessão e clique em **seguinte**.
1. Clique em **OK**

## <a name="create-a-sync-group-and-a-cloud-endpoint"></a>Criar um grupo de sincronização e um ponto final da cloud
Um grupo de sincronização define a topologia de sincronização para um conjunto de arquivos. Um grupo de sincronização tem de conter um ponto final da cloud, que representa uma partilha de ficheiros do Azure e um ou mais pontos finais do servidor. Um ponto de final de servidor representa um caminho de um servidor registado.

1. Para criar um grupo de sincronização, o [portal do Azure](https://portal.azure.com/), selecione **+ grupo de sincronização** do serviço de sincronização de armazenamento que criou para este tutorial. Usamos *afssyncservice02* como o exemplo neste tutorial.

   ![Criar um novo grupo de sincronização no portal do Azure](media/storage-sync-files-extend-servers/add-sync-group.png)

1. No painel que se abre, introduza as seguintes informações para criar um grupo de sincronização com um ponto final da cloud:

   | Valor | Descrição |
   | ----- | ----- |
   | **Nome do grupo de sincronização** | Este nome tem de ser exclusivo no âmbito do serviço de sincronização de armazenamento, mas pode ser qualquer nome que seja lógica para. Neste tutorial, estamos usando *afssyncgroup*.|
   | **Subscrição** | A subscrição em que implementou o serviço de sincronização de armazenamento para este tutorial. |
   | **Conta de armazenamento** |Clique em **selecione a conta de armazenamento**. No painel que aparece, selecione a conta de armazenamento que tenha a partilha de ficheiros do Azure que criou para este tutorial. Usamos *afsstoracct101918*. |
   | **Partilha de ficheiros do Azure** | O nome da partilha de ficheiros do Azure que criou para este tutorial. Usamos *afsfileshare*. |

1. Clique em **Criar**.

Se selecionar o grupo de sincronização, verá que agora tem um **ponto final da cloud**.

## <a name="add-a-server-endpoint"></a>Adicionar um ponto final do servidor
Um ponto de final de servidor representa uma localização específica num servidor registado, por exemplo, uma pasta num volume do servidor.

1. Para adicionar um ponto final do servidor, selecione o grupo de sincronização recém-criado e, em seguida, selecione **adicionar ponto final de servidor**.

   ![Adicionar um novo ponto de final de servidor no painel do grupo de sincronização](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. Na **adicionar ponto final de servidor** painel, introduza as seguintes informações para criar um ponto final do servidor:

   | | |
   | ----- | ----- |
   | Valor | Descrição |
   | **Servidor registado** | O nome do servidor que criou para este tutorial. Usamos *afsvm101918* neste tutorial ||
   | **Caminho** | O caminho do Windows Server para a unidade que criou para este tutorial. No nosso exemplo, é *f:\filestosync*. |
   | **Em camada de cloud** | Deixe desativado para este tutorial. |
   | **Espaço livre do volume** | Deixe em branco para este tutorial. |

1. Clique em **Criar**.

Os ficheiros agora são mantidos em sincronia entre a sua partilha de ficheiros do Azure e no Windows Server.

![Armazenamento do Azure sincronizado com êxito](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu as etapas básicas para expandir a capacidade de armazenamento de um Windows Server com o Azure File Sync. Siga esta ligação para uma visão mais completa de planear uma implementação do Azure File Sync.

> [!div class="nextstepaction"]
> [Planear a implementação do Azure File Sync](./storage-sync-files-planning.md)