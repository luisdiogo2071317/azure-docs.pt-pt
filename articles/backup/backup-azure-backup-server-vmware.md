---
title: Fazer backup de servidores de VMware no servidor de cópia de segurança do Azure
description: Utilize o servidor de cópia de segurança do Azure para criar cópias de segurança um servidores vCenter/ESXi do VMware para o Azure ou o disco. Este artigo fornece passo = instruções passo a passo para a cópia de segurança (ou proteger) suas cargas de trabalho do VMware.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/24/2017
ms.author: adigan
ms.openlocfilehash: e39e5d12610164ca4a1372830cf25ea203fd382c
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968839"
---
# <a name="back-up-a-vmware-server-to-azure"></a>Criar cópias de segurança um servidor VMware para o Azure

Este artigo explica como configurar o servidor de cópia de segurança do Azure para ajudar a proteger cargas de trabalho do VMware server. Este artigo pressupõe que já tiver instalado o Azure Backup Server. Se não tiver instalado o Azure Backup Server, consulte [preparar a cópia de segurança de cargas de trabalho com o Azure Backup Server](backup-azure-microsoft-azure-backup.md).

O servidor de cópia de segurança do Azure pode criar cópias de segurança ou ajudar a proteger VMware vCenter Server versão 6.5, 6.0 e 5.5.


## <a name="create-a-secure-connection-to-the-vcenter-server"></a>Criar uma ligação segura ao vCenter Server

Por predefinição, o servidor de cópia de segurança do Azure se comunica com cada servidor do vCenter através de um canal HTTPS. Para ativar a comunicação segura, recomendamos que instale o certificado de autoridade de certificado de VMware (AC) no servidor de cópia de segurança do Azure. Se não exige uma comunicação segura e prefere desativar o requisito de HTTPS, consulte [desativar o protocolo de comunicação segura](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol). Para criar uma ligação segura entre o servidor de cópia de segurança do Azure e o vCenter Server, importe o certificado fidedigno no servidor de cópia de segurança do Azure.

Normalmente, utilize um browser no computador do servidor de cópia de segurança do Azure para ligar ao vCenter Server por meio do vSphere Web Client. Na primeira vez que usa o navegador de servidor de cópia de segurança do Azure para ligar ao vCenter Server, a ligação não segura. A imagem seguinte mostra uma ligação não segura.

![Exemplo de uma ligação não segura para o servidor VMware](./media/backup-azure-backup-server-vmware/unsecure-url.png)

Para corrigir este problema e criar uma ligação segura, transfira os certificados de AC de raiz fidedigna.

1. No browser no servidor de cópia de segurança do Azure, introduza o URL para o cliente Web do vSphere. É apresentada a página de início de sessão de cliente Web do vSphere.

    ![vSphere Web cliente](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    Na parte inferior das informações para administradores e desenvolvedores, localize a **Download fidedigna de certificados de AC de raiz** ligação.

    ![Ligação para transferir os certificados de AC de raiz fidedigna](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  Se não vir a página de início de sessão de cliente Web do vSphere, verifique as definições de proxy do seu browser.

2. Clique em **Download fidedigna de certificados de AC de raiz**.

    O vCenter Server transfere um ficheiro para o computador local. Nome do ficheiro é denominado **transferir**. Dependendo do seu browser, receberá uma mensagem que perguntar se pretende abrir ou guardar o ficheiro.

    ![Transferir a mensagem quando os certificados são transferidos](./media/backup-azure-backup-server-vmware/download-certs.png)

3. Guarde o ficheiro para uma localização no servidor de cópia de segurança do Azure. Quando salvar o arquivo, adicione a extensão de nome de ficheiro. zip.

    O ficheiro é um ficheiro. zip que contém as informações sobre os certificados. Com a extensão. zip, pode usar as ferramentas de extração.

4. Com o botão direito **download.zip**e, em seguida, selecione **extrair tudo** para extrair o conteúdo.

    O ficheiro. zip extrai os respetivos conteúdos para uma pasta denominada **certificados**. São apresentados dois tipos de ficheiros na pasta de certificados. O ficheiro de certificado de raiz tem uma extensão que começa com uma sequência numerada como.0 e.1.

    O ficheiro CRL tem uma extensão que começa com uma seqüência como .r0 ou .r1. O ficheiro CRL está associado um certificado.

    ![Transferir ficheiro extraído localmente ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. Na **certificados** pasta, faça duplo clique no ficheiro de certificado de raiz e, em seguida, clique em **mudar o nome**.

    ![Mudar o nome do certificado de raiz ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    Altere a extensão do certificado de raiz para. crt. Quando for solicitado que se tiver a certeza de que pretende alterar a extensão, clique em **Sim** ou **OK**. Caso contrário, alterar função do ficheiro pretendido. O ícone para o ficheiro é alterado para um ícone que representa um certificado de raiz.

6. Faça duplo clique o certificado de raiz e a partir do menu de pop-up, selecione **instalar certificado**.

    O **Assistente para importar certificados** é apresentada a caixa de diálogo.

7. Na **Assistente para importar certificados** caixa de diálogo, selecione **máquina Local** como o destino para o certificado e, em seguida, clique **seguinte** para continuar.

    ![Opções de destino de armazenamento de certificado ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    Se lhe forem pedidas se quiser permitir alterações para o computador, clique em **Sim** ou **OK**, para todas as alterações.

8. Sobre o **Store de certificado** página, selecione **colocar todos os certificados no seguinte arquivo**e, em seguida, clique em **procurar** para escolher o arquivo de certificados.

    ![Colocar os certificados num local de armazenamento específico](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    O **selecione certificado Store** é apresentada a caixa de diálogo.

    ![Hierarquia de pasta de armazenamento de certificados](./media/backup-azure-backup-server-vmware/cert-store.png)

9. Selecione **autoridades de certificação de raiz fidedigna** como a pasta de destino para os certificados e, em seguida, clique **OK**.

    ![Pasta de destino de certificado](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    O **autoridades de certificação de raiz fidedigna** pasta foi confirmada como o arquivo de certificados. Clique em **Seguinte**.

    ![Pasta de arquivo de certificados](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. Sobre o **a concluir o Assistente de importação de certificado** página, certifique-se de que o certificado está na pasta pretendida e, em seguida, clique em **concluir**.

    ![Verifique se o certificado está na pasta apropriada](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    É apresentada uma caixa de diálogo, a importação de certificado bem-sucedida foi confirmada.

11. Inicie sessão no vCenter Server para confirmar se a ligação está segura.

  Se a importação de certificado não for bem-sucedida, e não é possível estabelecer uma ligação segura, consulte a documentação do VMware vSphere nos [obter certificados de servidor](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).

  Se o ter limites seguros na sua organização e não pretender ativar o protocolo HTTPS, utilize o procedimento seguinte para desativar as proteger as comunicações.

### <a name="disable-secure-communication-protocol"></a>Desativar o protocolo de comunicação segura

Se sua organização não requer o protocolo HTTPS, utilize os seguintes passos para desativar o HTTPS. Para desativar o comportamento predefinido, crie uma chave de registo que ignora o comportamento padrão.

1. Copie e cole o seguinte texto num ficheiro. txt.

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. Guarde o ficheiro para o seu computador de servidor de cópia de segurança do Azure. Para o nome de ficheiro, utilize DisableSecureAuthentication.reg.

3. Faça duplo clique no ficheiro para ativar a entrada de registo.


## <a name="create-a-role-and-user-account-on-the-vcenter-server"></a>Criar uma conta de utilizador e a função no vCenter Server

No vCenter Server, uma função é um conjunto predefinido de privilégios. Um administrador do servidor vCenter cria as funções. Para atribuir permissões, o administrador emparelha as contas de utilizador com uma função. Para estabelecer as credenciais de utilizador necessários para criar cópias de segurança do computador do servidor vCenter, cria uma função com privilégios específicos e, em seguida, associar a conta de utilizador com a função.

O servidor de cópia de segurança do Azure utiliza um nome de utilizador e palavra-passe para autenticar com o vCenter Server. Servidor de cópia de segurança do Azure utiliza estas credenciais como autenticação para todas as operações de cópia de segurança.

Para adicionar uma função de servidor vCenter e os respetivos privilégios de administrador de cópias de segurança:

1. Inicie sessão para o vCenter Server e, em seguida, no vCenter Server **Navigator** painel, clique em **administração**.

    ![Opção de administração no painel de navegador do servidor vCenter](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. Na **administração** selecionar **funções**e, em seguida, no **funções** painel clique no ícone de função de adicionar (o símbolo +).

    ![Adicionar função](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    O **criar função** é apresentada a caixa de diálogo.

    ![Criar função](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. Na **criar função** caixa de diálogo a **nome da função** , introduza *BackupAdminRole*. O nome da função pode ser que quiser, mas deve ser reconhecível para finalidade da função.

4. Selecione os privilégios para a versão adequada do vCenter e, em seguida, clique em **OK**. A tabela seguinte identifica os privilégios necessários para o vCenter 6.0 e o vCenter 5.5.

  Quando seleciona os privilégios, clique no ícone junto à etiqueta principal para expandir o pai e ver os privilégios de subordinados. Para selecionar os privilégios de VirtualMachine, terá de ir a vários níveis na hierarquia principal subordinado. Não precisa de selecionar todos os privilégios de filho dentro de um privilégio principal.

  ![Hierarquia de privilégio do principal subordinado](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  Depois de clicar em **OK**, a nova função aparece na lista no painel de funções.

|Privilégios para o vCenter 6.0 e 6.5| Privilégios para o vCenter 5.5|
|----------------------------------|---------------------------|
|Datastore.AllocateSpace   | Datastore.AllocateSpace|
|Global.ManageCustomFields | Global.ManageCustomerFields|
|Global.SetCustomFields    |   |
|Host.Local.CreateVM       | Network.Assign |
|Network.Assign            |  |
|Resource.AssignVMToPool   |  |
|VirtualMachine.Config.AddNewDisk  | VirtualMachine.Config.AddNewDisk   |
|VirtualMachine.Config.AdvanceConfig| VirtualMachine.Config.AdvancedConfig|
|VirtualMachine.Config.ChangeTracking| VirtualMachine.Config.ChangeTracking |
|VirtualMachine.Config.HostUSBDevice||
|VirtualMachine.Config.QueryUnownedFiles|    |
|VirtualMachine.Config.SwapPlacement| VirtualMachine.Config.SwapPlacement |
|VirtualMachine.Interact.PowerOff| VirtualMachine.Interact.PowerOff |
|VirtualMachine.Inventory.Create| VirtualMachine.Inventory.Create |
|VirtualMachine.Provisioning.DiskRandomAccess| |
|VirtualMachine.Provisioning.DiskRandomRead|VirtualMachine.Provisioning.DiskRandomRead |
|VirtualMachine.State.CreateSnapshot| VirtualMachine.State.CreateSnapshot|
|VirtualMachine.State.RemoveSnapshot|VirtualMachine.State.RemoveSnapshot |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a>Criar uma conta de utilizador do servidor vCenter e permissões

Depois de configurar a função com privilégios, crie uma conta de utilizador. A conta de utilizador tem um nome e uma palavra-passe, que fornece as credenciais que são utilizadas para autenticação.

1. Para criar uma conta de utilizador, no vCenter Server **Navigator** painel, clique em **utilizadores e grupos**.

    ![Opção de utilizadores e grupos](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    O **vCenter utilizadores e grupos** painel é apresentada.

    ![Painel de utilizadores e grupos do vCenter](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. Na **vCenter utilizadores e grupos** painel, selecione a **utilizadores** separador e, em seguida, clique no ícone de utilizadores de adicionar (o símbolo +).

    O **novo utilizador** é apresentada a caixa de diálogo.

3. Na **novo utilizador** caixa de diálogo caixa, adicione as informações do utilizador e, em seguida, clique em **OK**. Neste procedimento, o nome de utilizador é BackupAdmin.

    ![Caixa de diálogo do novo utilizador](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    A nova conta de utilizador é apresentado na lista.

4. Para associar a conta de utilizador com a função, o **Navigator** painel, clique em **permissões globais**. Na **permissões globais** painel, selecione a **gerir** separador e, em seguida, clique no ícone de adicionar (o símbolo +).

    ![Painel de permissões global](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    O **Global de permissões de raiz - adicionar permissão** é apresentada a caixa de diálogo.

5. Na **Global de permissão de raiz - adicionar permissão** caixa de diálogo, clique em **Add** para escolher o utilizador ou grupo.

    ![Escolha o utilizador ou grupo](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    O **selecionar os utilizadores/grupos** é apresentada a caixa de diálogo.

6. Na **selecionar os utilizadores/grupos** caixa de diálogo caixa, escolha **BackupAdmin** e, em seguida, clique em **Add**.

    Na **usuários**, o *domínio \ nomedeutilizador* formato é usado para a conta de utilizador. Se pretender utilizar um domínio diferente, selecione-o a partir da **domínio** lista.

    ![Adicionar utilizador BackupAdmin](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    Clique em **OK** para adicionar os utilizadores selecionados para o **adicionar permissão** caixa de diálogo.

7. Agora que identificou o utilizador, atribua o utilizador à função. Na **função atribuída**, na lista pendente, selecione **BackupAdminRole**e, em seguida, clique em **OK**.

    ![Atribuir utilizador à função](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  Sobre o **gerir** separador a **permissões globais** painel, a nova conta de utilizador e a função associada são apresentados na lista.


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a>Estabelecer as credenciais do servidor vCenter no servidor de cópia de segurança do Azure

Antes de adicionar o servidor VMware para o servidor de cópia de segurança do Azure, instale [atualização 1 para o Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).

1. Para abrir o servidor de cópia de segurança do Azure, faça duplo clique no ícone na área de trabalho de servidor de cópia de segurança do Azure.

    ![Ícone de servidor de cópia de segurança do Azure](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    Se não encontrar o ícone na área de trabalho, abra o servidor de cópia de segurança do Azure na lista de aplicações instaladas. O nome da aplicação do Azure Backup Server é chamado de cópia de segurança do Microsoft Azure.

2. Na consola do servidor de cópia de segurança do Azure, clique em **gerenciamento**, clique em **servidores de produção**e, em seguida, no Friso, clique em **gerir VMware**.

    ![Consola do Azure Backup Server](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    O **gerir credenciais** é apresentada a caixa de diálogo.

    ![Caixa de diálogo de credenciais de gerir o servidor de cópia de segurança do Azure](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. Na **gerir credenciais** caixa de diálogo, clique em **Add** para abrir o **adicionar credencial** caixa de diálogo.

4. Na **adicionar credencial** caixa de diálogo, introduza um nome e uma descrição para a nova credencial. Em seguida, especifique o nome de utilizador e palavra-passe. O nome, *credenciais do Contoso Vcenter* é utilizado para identificar a credencial no próximo procedimento. Utilize o mesmo nome de utilizador e palavra-passe que é utilizado para o vCenter Server. Se o servidor vCenter e o servidor de cópia de segurança do Azure não estão no mesmo domínio, no **nome de utilizador**, especifique o domínio.

    ![Caixa de diálogo Adicionar credencial do servidor de cópia de segurança do Azure](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    Clique em **adicionar** para adicionar a nova credencial ao servidor de cópia de segurança do Azure. A nova credencial aparece na lista na **gerir credenciais** caixa de diálogo.

    ![Caixa de diálogo de credenciais de gerir o servidor de cópia de segurança do Azure](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. Para fechar a **gerir credenciais** caixa de diálogo, clique nas **X** no canto superior direito.


## <a name="add-the-vcenter-server-to-azure-backup-server"></a>Adicionar o vCenter Server para o servidor de cópia de segurança do Azure

Assistente de adição do servidor de produção é utilizado para adicionar o vCenter Server para o servidor de cópia de segurança do Azure.

Para abrir o Assistente de adição do servidor de produção, execute o seguinte procedimento:

1. Na consola do servidor de cópia de segurança do Azure, clique em **gerenciamento**, clique em **servidores de produção**e, em seguida, clique em **Add**.

    ![Assistente de adição do servidor de produção aberto](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    O **Assistente de adição do servidor de produção** é apresentada a caixa de diálogo.

    ![Assistente de adição do servidor de produção](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. Sobre o **tipo de selecionar o servidor de produção** , selecione **servidores VMware**e, em seguida, clique em **seguinte**.

3. Na **servidor de nome/endereço IP**, especifique o nome de domínio completamente qualificado (FQDN) ou o endereço IP do servidor VMware (o servidor anfitrião ESXi). Se todos os servidores do ESXi são geridos pelo mesmo vCenter, pode utilizar o nome do vCenter.

    ![Especifique o endereço IP ou FQDN do servidor de VMware](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. Na **porta SSL**, introduza a porta que é utilizada para comunicar com o servidor VMware. Utilize a porta 443, que é a porta predefinida, a menos que saiba que é necessária a uma porta diferente.

5. Na **especificar credenciais**, selecione a credencial que criou anteriormente.

    ![Especificar credenciais](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Clique em **Add** para adicionar o servidor VMware à lista de **adicionados servidores de VMware**e, em seguida, clique em **seguinte** para mover para a página seguinte do assistente.

    ![Adicionar servidor VMWare e as credenciais](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. Na **resumo** página, clique em **Add** para adicionar o servidor VMware especificado para o servidor de cópia de segurança do Azure.

    ![Adicionar servidor VMware para o servidor de cópia de segurança do Azure](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  A cópia de segurança do servidor do VMware é uma cópia de segurança sem agente e o novo servidor é adicionado imediatamente. O **concluir** página mostra os resultados.

  ![Página de conclusão](./media/backup-azure-backup-server-vmware/summary-screen.png)

  Para adicionar várias instâncias do vCenter Server ao servidor de cópia de segurança do Azure, repita os passos anteriores desta secção.

Depois de adicionar o vCenter Server para o servidor de cópia de segurança do Azure, a próxima etapa é criar um grupo de proteção. O grupo de proteção Especifica os diversos detalhes para a retenção de curto ou longo prazo, e é onde define e aplica a política de cópia de segurança. A política de cópia de segurança é a agenda para quando ocorrem as cópias de segurança e o que é uma cópia de segurança.


## <a name="configure-a-protection-group"></a>Configurar um grupo de proteção

Se não tiver utilizado o System Center Data Protection Manager ou o servidor de cópia de segurança do Azure antes de, veja [planear para cópias de segurança do disco](https://technet.microsoft.com/library/hh758026.aspx) para preparar o ambiente de hardware. Depois que verifique se tem armazenamento adequado, utilize o Assistente Criar novo grupo de proteção para adicionar máquinas virtuais VMware.

1. Na consola do servidor de cópia de segurança do Azure, clique em **proteção**e no Friso de ferramentas, clique em **New** para abrir o Assistente Criar novo grupo de proteção.

    ![Abrir o Assistente Criar novo grupo de proteção](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    O **criar novo grupo de proteção** é apresentada a caixa de diálogo do assistente.

    ![Caixa de diálogo do Assistente Criar novo grupo de proteção](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    Clique em **próxima** para avançar para o **selecione o tipo de grupo de proteção** página.

2. Sobre o **tipo de grupo de proteção de selecionar** , selecione **servidores** e, em seguida, clique em **seguinte**. O **selecionar membros do grupo** é apresentada a página.

3. Sobre o **selecionar membros do grupo** página, os membros disponíveis e os membros selecionados aparecem. Selecione os membros que pretende proteger e, em seguida, clique em **seguinte**.

    ![Selecionar Membros do grupo](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    Quando seleciona um membro, se selecionar uma pasta que contém a VMs ou outras pastas, essas VMs e pastas também são selecionadas. A inclusão de pastas e VMs na pasta principal é chamada de proteção ao nível da pasta. Para remover uma pasta ou a VM, desmarque a caixa de verificação.

    Se uma VM ou uma pasta que contém uma VM, já está protegida para o Azure, não é possível selecionar essa VM novamente. Ou seja, depois de uma VM é protegida para o Azure, este não pode ser protegido novamente, que impede que o pontos de recuperação duplicados a ser criada para uma VM. Se quiser ver a instância de servidor de cópia de segurança do Azure protege já um membro, aponte para o membro para ver o nome do servidor de proteção.

4. Sobre o **selecionar método de proteção de dados** página, introduza um nome para o grupo de proteção. São selecionados proteção a curto prazo (para o disco) e a proteção online. Se pretender utilizar a proteção online (para o Azure), tem de utilizar proteção de curta duração no disco. Clique em **seguinte** para avançar para o intervalo de proteção de curta duração.

    ![Selecionar método de proteção de dados](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. Sobre o **especificar objetivos a curto prazo** página, para **período de retenção**, especifique o número de dias que pretende manter os pontos de recuperação que estão *armazenados no disco*. Se pretender alterar a hora e dias após a pontos de recuperação são obtidos, clique em **modificar**. Os pontos de recuperação de curto prazo são cópias de segurança completas. Não são cópias de segurança incrementais. Quando estiver satisfeito com os objetivos de curto prazo, clique em **seguinte**.

    ![Especificar objetivos de curto prazo](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. Sobre o **rever atribuição do disco** página, reveja e se necessário, modifique o espaço em disco para as VMs. As alocações de disco recomendado baseiam-se no período de retenção especificado no **especificar objetivos a curto prazo** página, o tipo de carga de trabalho e o tamanho dos dados protegidos (identificados na etapa 3).  

  - **Tamanho dos dados:** tamanho dos dados no grupo de proteção.
  - **Espaço em disco:** a quantidade de espaço em disco para o grupo de proteção recomendada. Se pretender modificar esta definição, deve alocar espaço total ligeiramente maior do que a quantidade que estimar o que aumenta a cada origem de dados.
  - **Colocalizar dados:** se ativar a partilha de localização, do ponto de dados de várias origens na proteção podem ser mapeado para uma única réplica e de recuperação volume. A colocalização não é suportada para todas as cargas de trabalho.
  - **Aumentar automaticamente:** se ativar esta definição, caso os dados no grupo de proteção superem a alocação inicial, o System Center Data Protection Manager tenta aumentar o tamanho de disco em 25 por cento.
  - **Detalhes do agrupamento de armazenamento:** mostra o estado do agrupamento de armazenamento, incluindo total e restante o tamanho do disco.

    ![Rever atribuição do disco](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    Quando estiver satisfeito com a alocação de espaço, clique em **seguinte**.

7. Sobre o **Choose Replica Creation Method** , especifique como pretende gerar a cópia inicial ou a réplica, dos dados protegidos no servidor de cópia de segurança do Azure.

    A predefinição é **automaticamente através da rede** e **agora**. Se utilizar a predefinição, é recomendável que especificar um horário fora de pico. Escolher **mais tarde** e especifique um dia e hora.

    Para grandes quantidades de dados ou condições de rede aquém do ideal, considere replicar os dados offline usando mídia removível.

    Depois de fazer suas escolhas, clique em **seguinte**.

    ![Escolher método de criação de réplica](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. Sobre o **opções de verificação de consistência** página, selecione como e quando automatizar as verificações de consistência. Pode executar verificações de consistência quando os dados de réplica se tornar inconsistentes ou numa agenda definida.

    Se não quiser configurar verificações de consistência automáticas, pode executar uma verificação manual. Na área de proteção da consola do servidor de cópia de segurança do Azure, com o botão direito do grupo de proteção e, em seguida, selecione **efetuar verificação de consistência**.

    Clique em **seguinte** para mover para a página seguinte.

9. Sobre o **especificar dados da proteção Online** , selecione uma ou mais origens de dados que pretende proteger. Pode selecionar os membros individualmente, ou clique em **Selecionar tudo** escolher todos os membros. Depois de escolher os membros, clique em **seguinte**.

    ![Especificar dados da proteção online](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. Sobre o **Especificar agenda de cópia de segurança Online** , especifique a agenda para gerar pontos de recuperação a partir da cópia de segurança do disco. Depois do ponto de recuperação é gerado, é transferido para o Cofre de serviços de recuperação no Azure. Quando estiver satisfeito com a agenda de cópia de segurança online, clique em **seguinte**.

    ![Especifique a agenda de cópia de segurança online](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. Sobre o **especificar política de retenção Online** página, indique o período de tempo pretende manter os dados de cópia de segurança no Azure. Depois da política estiver definida, clique em **seguinte**.

    ![Especificar Política de retenção online](./media/backup-azure-backup-server-vmware/retention-policy.png)

    Não existe nenhum limite de tempo para o tempo que pode manter os dados no Azure. Quando armazena os dados de ponto de recuperação no Azure, o único limite é de que não pode ter mais de 9999 pontos de recuperação por instância protegida. Neste exemplo, a instância protegida é o servidor VMware.

12. Sobre o **resumo** página, reveja os detalhes para os membros do grupo de proteção e as definições e, em seguida, clique em **criar grupo**.

    ![Membro do grupo de proteção e o resumo de definição](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a>Passos Seguintes
Se utilizar o Azure Backup Server para proteger cargas de trabalho do VMware, poderá estar interessado em utilizar o Azure Backup Server para ajudar a proteger um [servidor do Microsoft Exchange](./backup-azure-exchange-mabs.md), uma [farm do SharePoint do Microsoft](./backup-azure-backup-sharepoint-mabs.md), ou uma [Base de dados do SQL Server](./backup-azure-sql-mabs.md).

Para obter informações sobre problemas com o agente a registar, configurar o grupo de proteção, ou fazer backup das tarefas, consulte [resolver problemas relacionados com o Azure Backup Server](./backup-azure-mabs-troubleshoot.md).
