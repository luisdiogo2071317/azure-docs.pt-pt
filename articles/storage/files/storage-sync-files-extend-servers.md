---
title: Tutorial sobre como expandir os servidores de ficheiros do Windows com o Azure File Sync | Microsoft Docs
description: Saiba como expandir os servidores de ficheiros do Windows com o Azure File Sync, do princípio ao fim.
services: storage
author: wmgries
ms.service: storage
ms.topic: tutorial
ms.date: 10/23/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: cc34411cc27870dbd9c707a34ebf34b96c7253dc
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986122"
---
# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Tutorial: Expandir os servidores de ficheiros do Windows com o Azure File Sync
Neste tutorial, vamos mostrar os passos básicos para expandir a capacidade de armazenamento de um Windows Server com o Azure File Sync. Embora estejamos a utilizar uma VM do Azure do Windows Server neste tutorial, este processo é, geralmente, feito para os servidores no local. Se estiver pronto para implementar o Azure File Sync no seu ambiente, utilize o artigo [Deploy Azure File Sync](storage-sync-files-deployment-guide.md) (Implementar o Azure File Sync).

> [!div class="checklist"]
> * Implementar o Serviço de Sincronização de Armazenamento
> * Preparar o Windows Server para ser utilizado com o Azure File Sync
> * Instalar o agente do Azure File Sync
> * Registar o Windows Server no Serviço de Sincronização de Armazenamento
> * Criar um grupo de sincronização e um ponto final na cloud
> * Criar um ponto final de servidor

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="prepare-your-environment"></a>Preparar o ambiente
Antes de implementar o Azure File Sync, é necessário configurar algumas coisas no âmbito deste tutorial. Juntamente com a criação de uma conta de Armazenamento e uma partilha de ficheiros do Azure, vai criar uma VM do Windows Server 2016 Datacenter e preparar esse servidor para o Azure File Sync.

### <a name="create-a-folder-and-txt-file"></a>Criar uma pasta e um ficheiro .txt

No computador local, crie uma pasta nova com o nome *FilesToSync* e adicione um ficheiro de texto denominado *mytestdoc.txt*. Este ficheiro vai ser carregado para a partilha de ficheiros mais adiante no tutorial.

### <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Criar uma partilha de ficheiros
Depois, crie uma partilha de ficheiros.

1. Quando a implementação da conta de armazenamento do Azure estiver concluída, clique em **Ir para recurso**.
1. Clique em **Ficheiros**, no painel da conta de armazenamento.

    ![Clique em “Ficheiros”](./media/storage-sync-files-extend-servers/click-files.png)

1. Clique em **+ File Share** (+ Partilha de Ficheiros).

    ![Clique no botão “adicionar partilha de ficheiros”](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Dê à nova partilha de ficheiros o nome *afsfileshare*, introduza "1" em **Quota** e clique em **Create** (Criar). A quota pode ter um máximo de 5 TiB, mas só precisa de 1 GB neste tutorial.

    ![Indique um nome e uma quota para a partilha de ficheiros nova](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Selecione a partilha de ficheiros nova e, na localização da partilha de ficheiros, clique em **Upload** (Carregar).

    ![Carregar um ficheiro](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. Navegue para a pasta *FilesToSync* em que criou o ficheiro .txt, selecione *mytestdoc.txt* e clique em **Upload** (Carregar).

    ![Navegar para a partilha de ficheiros](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

Nesta fase, já criou uma conta de Armazenamento do Azure e uma partilha de ficheiros com um ficheiro no Azure. Agora, vai criar a VM do Azure com o Windows Server 2016 Datacenter para representar o servidor no local neste tutorial.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>Implementar uma VM e anexar um disco de dados

1. Em seguida, expanda o menu no lado esquerdo do portal e escolha **Criar um recurso**, no canto superior esquerdo do portal do Azure.
1. Na caixa de pesquisa acima da lista de recursos do **Azure Marketplace**, procure e selecione **Windows Server 2016 Datacenter** e escolha **Criar**.
1. No separador **Noções Básicas**, em **Detalhes do projeto**, selecione o grupo de recursos que criou para o tutorial.

   ![Introduza as informações básicas sobre a VM no painel do portal](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. Em **Detalhes da instância**, indique um nome para a VM, como *myVM*.
1. Mantenha as predefinições para **Região**, **Opções de disponibilidade**, **Imagem** e **Tamanho**.
1. Em **Conta de administrador**, indique um **Nome de utilizador** e uma **palavra-passe** para a VM.
1. Em **Regras da porta de entrada**, selecione **Permitir portas selecionadas** e, em seguida, selecione **RDP (3389)** e **HTTP** na lista pendente.

   Antes de criar a VM, tem de criar um disco de dados.

1. Clique em **Next:Disks**

   ![Adicionar discos de dados](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

1. No separador **Discos**, em **Opções do disco**, mantenha as predefinições.
1. Em **DISCOS DE DADOS**, clique em **Criar e anexar um disco novo**.

1. Mantenha as predefinições, mas altere **Tamanho (GiB)** para **1 GB**, no âmbito deste tutorial.

   ![Detalhes do disco de dados](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

1. Clique em **OK**.
1. Clique em **Rever + criar**.
1. Clique em **Criar**.

   Pode clicar no ícone **Notificações** para ver o **Progresso da implementação**. A criação da VM nova irá demorar alguns minutos a concluir.

1. Após a conclusão da implementação da VM, clique em **Ir para recurso**.

   ![Ir para recurso](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

   Nesta fase, já criou uma máquina virtual nova e anexou um disco de dados. Agora, é preciso ligar à VM.

### <a name="connect-to-your-vm"></a>Ligar à VM

1. No portal do Azure, clique no botão **Ligar** na página de propriedades da máquina virtual.

   ![Ligar a uma VM do Azure a partir do portal](./media/storage-sync-files-extend-servers/connect-vm.png)

1. Na página **Ligar à máquina virtual**, mantenha as opções predefinidas para ligar através de **endereço IP** na porta 3389 e clique em **Transferir ficheiro RDP**.

   ![Transferir o ficheiro RDP](./media/storage-sync-files-extend-servers/download-rdp.png)

1. Abra o ficheiro RDP transferido e clique em **Ligar** quando lhe for pedido.
1. Na janela **Segurança do Windows**, selecione **Mais escolhas** e **Utilizar uma conta diferente**. Escreva o nome de utilizador como *localhost\username*, introduza a palavra-passe que criou para a máquina virtual e clique em **OK**.

   ![Mais opções](./media/storage-sync-files-extend-servers/local-host2.png)

1. Poderá receber um aviso de certificado durante o processo de início de sessão. Clique em **Sim** ou **Continuar** para criar a ligação.

### <a name="prepare-the-windows-server"></a>Preparar o Windows Server
Relativamente ao servidor **Windows Server 2016 Datacenter**, desative a **Configuração de Segurança Avançada do Internet Explorer**. Este passo só é necessário para o registo inicial do servidor. Pode reativá-la depois de o servidor estar registado.

Na VM do **Windows Server 2016 Datacenter**, o **Gestor de Servidor** abre-se automaticamente.  Se **Gestor de Servidor** não se abrir por predefinição, procure-o no Explorer.

1. No **Gestor de Servidor**, clique em **Servidor Local**.

   ![“Servidor Local” no lado esquerdo da IU do Gestor de Servidor](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. No subpainel **Propriedades**, selecione a ligação para **Configuração de Segurança Avançada do IE**.  

    ![Painel “Configuração de Segurança Avançada do IE”, na IU do Gestor de Servidor](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. Na caixa de diálogo **Configuração de Segurança Avançada do Internet Explorer**, selecione **Desativado** em **Administradores** e **Utilizadores**.

    ![Janela pop-up da Configuração de Segurança Avançada do Internet Explorer com “Desativado” selecionado](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

   Agora, pode adicionar o disco de dados à VM

### <a name="add-the-data-disk"></a>Adicionar o disco de dados

1. Ainda na VM do **Windows Server 2016 Datacenter**, clique em **Serviços de ficheiros e armazenamento** > **Volumes** > **Discos**.

    ![Disco de dados](media/storage-sync-files-extend-servers/your-disk.png)

1. Clique com o botão direito do rato no disco de 1 GB com o nome **Msft Virtual Disk** e clique em **Novo volume**.
1. Conclua o assistente, mantenha as predefinições, aponte a letra de unidade atribuída e clique em **Criar**.
1. Clique em **Fechar**.

   Nesta fase, pôs o disco online e criou um volume. Para confirmar se a adição do disco de dados foi bem-sucedida, abra o Explorer na VM e verifique se a unidade nova está presente.

1. No Explorer na VM, expanda **Este PC** e faça duplo clique na unidade nova. Neste exemplo, a unidade é a F:.
1. Clique com botão direito do rato e selecione **Novo** > **Pasta**. Dê à pasta o nome *FilesToSync*.
1. Faça duplo clique na pasta **FilesToSync**.
1. Faça duplo clique e selecione **Novo** > **Documento de Texto**. Dê ao ficheiro de texto o nome *MyTestFile*.

    ![Adicionar um ficheiro de texto novo](media/storage-sync-files-extend-servers/new-file.png)

1. Feche o **Explorer** e o **Gestor de Servidor**.

### <a name="download-the-azurerm-powershell-module"></a>Transferir o módulo do AzureRM do PowerShell
Em seguida, na VM do **Windows Server 2016 Datacenter**, instale o **módulo do AzureRM do PowerShell** no servidor.

1. Na VM, abra uma janela do PowerShell elevada
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

O módulo `AzureRM` é um módulo de rollup para os cmdlets do Azure PowerShell. A instalação do mesmo transfere todos os módulos disponíveis do Azure Resource Manager e disponibiliza os respetivos cmdlets para utilização.

Nesta fase, já concluiu a configuração do ambiente para o tutorial e está pronto para começar a implementar o **Serviço de Sincronização de Armazenamento**.

## <a name="deploy-the-service"></a>Implementar o serviço 
A implementação do Azure File Sync começa com a colocação de um recurso do **Serviço de Sincronização de Armazenamento** num grupo de recursos da subscrição que selecionou. O Serviço de Sincronização de Armazenamento herda as permissões de acesso da subscrição e do grupo de recursos no qual o implementar.

1. No portal do Azure, clique em **Criar um recurso** e procure por **Azure File Sync**.
1. Nos resultados da pesquisa, selecione **Azure File Sync**.
1. Selecione **Criar** para abrir o separador **Implementar Sincronização de Armazenamento**.

   ![Implementar Sincronização de Armazenamento](media/storage-sync-files-extend-servers/afs-info.png)

   No painel que se abre, introduza as informações seguintes:

   | Valor | Descrição |
   | ----- | ----- |
   | **Nome** | Um nome exclusivo (por subscrição) para o Serviço de Sincronização de Armazenamento.<br><br>Neste tutorial, estamos a utilizar *afssyncservice02*. |
   | **Subscrição** | A subscrição que está a utilizar neste tutorial. |
   | **Grupo de recursos** | O grupo de recursos que está a utilizar neste tutorial.<br><br>Neste tutorial, estamos a utilizar *afsresgroup101918*. |
   | **Localização** | EUA Leste |

1. Quando tiver terminado, selecione **Criar** para implementar o **Serviço de Sincronização do Armazenamento**.
1. Clique no separador **Notificações** > **Ir para recurso**.

## <a name="install-the-agent"></a>Instalar o agente
O agente do Azure File Sync é um pacote transferível que permite a sincronização do Windows Server com uma partilha de ficheiros do Azure.

1. Mude para a VM do **Windows Server 2016 Datacenter** e abra o **Internet Explorer**.
1. Aceda ao [Centro de Transferências da Microsoft](https://go.microsoft.com/fwlink/?linkid=858257). Navegue para a secção **Azure File Sync Agent** (Agente do Azure File Sync) e clique em **Transferir**.

   ![Transferência do agente do Sync](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. Assinale a caixa para **StorageSyncAgent_V3_WS2016.EXE** e clique em **Seguinte**.

   ![Selecionar o agente](media/storage-sync-files-extend-servers/select-agent.png)

1. **Permitir uma vez** > **Executar** > **Abrir** o ficheiro.
1. Se ainda não tiver fechado a janela do PowerShell, feche-a.
1. Aceite as predefinições do **Assistente de Configuração do Agente de Sincronização de Armazenamento**.
1. Clique em **Instalar**.
1. Clique em **Concluir**.

Implementou o Serviço de Sincronização do Azure e instalou o agente na VM do **Windows Server 2016 Datacenter**. Agora, tem de registar a VM no **Serviço de Sincronização de Armazenamento**.

## <a name="register-windows-server"></a>Registar o Windows Server
Registar o Windows Server num Serviço de Sincronização de Armazenamento estabelece uma relação de confiança entre o servidor (ou cluster) e aquele serviço. Os servidores só podem ser registados num Serviço de Sincronização de Armazenamento e podem ser sincronizados com outros servidores e com partilhas de ficheiros do Azure associados ao mesmo Serviço de Sincronização de Armazenamento.

Após a instalação do **agente do Azure File Sync**, a IU do Registo do Servidor deverá abrir-se automaticamente. Se isso não acontecer, pode abri-la manualmente na localização do ficheiro, em C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe.

1. Quando a IU do Registo do Servidor se aberta na VM, clique em **OK**.
1. Clique em **Iniciar sessão**, para começar.
1. Inicie sessão com as credenciais da sua conta do Azure e clique em **Iniciar sessão**.
1. Forneça as seguintes informações:

   ![Uma captura de ecrã da IU do Registo do Servidor](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Valor | Descrição |
   | **Subscrição do Azure** | A subscrição que contém o Serviço de Sincronização de Armazenamento neste tutorial. |
   | **Grupo de Recursos** | O grupo de recursos que contém o Serviço de Sincronização de Armazenamento neste tutorial. No decurso deste tutorial, utilizámos *afsresgroup101918*. |
   | **Serviço de Sincronização de Armazenamento** | O nome do Serviço de Sincronização de Armazenamento utilizado neste tutorial. No decurso deste tutorial, utilizámos *afssyncservice02*. |

1. Clique em **Registar** para concluir o registo do servidor.
1. Como parte do processo de registo, é-lhe pedido que volte a iniciar sessão novamente. Inicie-a e clique em **Seguinte**.
1. Clique em **OK**.

## <a name="create-a-sync-group"></a>Criar um grupo de sincronização
Os grupos de sincronização definem a topologia da sincronização para um conjunto de ficheiros. Os grupos de sincronização têm de conter um ponto final da cloud, que representa uma partilha de ficheiros do Azure e um ou mais pontos finais de servidor. Os pontos finais de servidor representam um caminho num servidor registado.

1. Para criar um grupo de sincronização, no [portal do Azure](https://portal.azure.com/), selecione **+ Sync group** (+ Grupo de sincronização) a partir do Serviço de Sincronização de Armazenamento que criou no âmbito do tutorial. No tutorial, utilizámos *afssyncservice02* como o exemplo.

   ![Criar um grupo de sincronização novo no portal do Azure](media/storage-sync-files-extend-servers/add-sync-group.png)

1. No painel que se abre, introduza as informações seguintes para criar um grupo de sincronização com um ponto final da cloud:

   | Valor | Descrição |
   | ----- | ----- |
   | **Nome do grupo de sincronização** | Este nome tem de ser exclusivo no Serviço de Sincronização de Armazenamento, mas pode ser qualquer nome que lhe pareça lógico. Neste tutorial, estamos a utilizar *afssyncgroup*.|
   | **Subscrição** | A subscrição na qual implementou o Serviço de Sincronização de Armazenamento neste tutorial. |
   | **Conta de armazenamento** |Clique em **Selecionar conta de armazenamento**. No painel que se abre, selecione a conta de armazenamento que tem a partilha de ficheiros do Azure que foi criada para o tutorial. Utilizámos *afsstoracct101918*. |
   | **Partilha de ficheiros do Azure** | O nome da partilha de ficheiros do Azure que criou para este tutorial. Utilizámos *afsfileshare*. |

1. Clique em **Criar**.

Se selecionar o grupo de sincronização, pode ver que tem agora um **ponto final da cloud**.

## <a name="add-a-server-endpoint"></a>Adicionar um ponto final de servidor
Os pontos finais de servidor representam uma localização específica num servidor registado, como uma pasta num volume do servidor.

1. Para adicionar um ponto final de servidor, selecione o grupo de sincronização recém-criado e selecione **Adicionar ponto final de servidor**.

   ![Adicionar um ponto final de servidor novo no painel do grupo de sincronização](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. No painel **Adicionar ponto final de servidor**, introduza as informações seguintes para criar um ponto final de servidor:

   | | |
   | ----- | ----- |
   | Valor | Descrição |
   | **Servidor registado** | O nome do servidor que criou para este tutorial. No tutorial, utilizámos *afsvm101918* |
   | **Caminho** | O caminho do Windows Server para a unidade que criou neste tutorial. No nosso exemplo, é *f:\filestosync*. |
   | **Arrumo na Cloud** | Deixe desativado para o tutorial. |
   | **Espaço Livre do Volume** | Deixe em branco para o tutorial. |

1. Clique em **Criar**.

Os ficheiros estão agora sincronizados na partilha de ficheiros do Azure e no Windows Server.

![Armazenamento do Azure sincronizado com êxito](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, aprendeu os passos básicos para expandir a capacidade de armazenamento de um Windows Server com o Azure File Sync. Siga esta ligação para ver um planeamento mais minucioso para uma implementação do Azure File Sync.

> [!div class="nextstepaction"]
> [Plan for Azure File Sync deployment](./storage-sync-files-planning.md) (Planear uma implementação do Azure File Sync)