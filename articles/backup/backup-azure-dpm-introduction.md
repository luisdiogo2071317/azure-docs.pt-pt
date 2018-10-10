---
title: Utilizar o DPM para efetuar cópias de segurança de cargas de trabalho para o Azure
description: Uma introdução à cópia de segurança de dados do DPM para um cofre dos serviços de recuperação do Azure.
services: backup
author: adigan
manager: nkolli
keywords: System Center Data Protection Manager, do data protection manager, a cópia de segurança do dpm
ms.service: backup
ms.topic: conceptual
ms.date: 8/22/2018
ms.author: adigan
ms.openlocfilehash: 2da5b04f56a5746fb77de6bc954bb5971eb4664b
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885175"
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Preparar a criação de cópias de segurança de cargas de trabalho para o Azure com o DPM
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

Este artigo explica como criar cópias de dados do System Center Data Protection Manager (DPM) para o Azure; incluindo:

* Como criar cópias de dados do servidor DPM para o Azure
* Os pré-requisitos para obter uma experiência positiva de cópia de segurança
* Os erros comuns encontrados e como lidar com eles
* Cenários suportados

> [!NOTE]
> O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [Resource Manager e clássica](../azure-resource-manager/resource-manager-deployment-model.md). Este artigo fornece as informações e procedimentos para restaurar VMs implementadas com o modelo do Resource Manager.
>
>

[O System Center DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview) faz o backup de dados de ficheiros e aplicativo. Podem encontrar mais informações sobre cargas de trabalho suportadas [aqui](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix). Dados de cópia de segurança para o DPM podem ser armazenados na banda, disco, ou uma cópia de segurança para o Azure com o Microsoft Azure Backup. O DPM interage com o Azure Backup da seguinte forma:

* **DPM implementado como uma máquina virtual do servidor ou no local física** — o DPM implementado como um servidor físico ou uma máquina de virtual de Hyper-V no local faz o backup de dados para um cofre de serviços de recuperação, além de disco e banda cópia de segurança.
* **DPM implementado como uma máquina virtual do Azure** — do System Center 2012 R2 com Update 3 em, pode implementar o DPM numa máquina virtual do Azure. Se o DPM é implementado como uma máquina virtual do Azure, pode criar cópias de segurança nos discos do Azure ligados à VM ou descarregar o armazenamento de dados pelo Backup num cofre dos serviços de recuperação.

## <a name="why-back-up-dpm-to-azure"></a>Por que fazer uma cópia de segurança do DPM para o Azure?
Os benefícios de negócios cópias de segurança dos servidores do DPM para o Azure incluem:

* Para a implementação do DPM no local, utilize o Azure como uma alternativa à implementação de longa duração em banda.
* Para implementar o DPM numa VM no Azure, descarregar o armazenamento do disco do Azure. Armazenar dados mais antigos no seu Cofre dos serviços de recuperação permite-lhe aumentar verticalmente o seu negócio ao armazenar novos dados no disco.

## <a name="prerequisites"></a>Pré-requisitos
Prepare a cópia de segurança do Azure para fazer backup de dados do DPM da seguinte forma:

1. **Criar um cofre dos serviços de recuperação** – criar um cofre no portal do Azure.
2. **Transferir as credenciais do cofre** — transferir as credenciais que utiliza para registar o servidor do DPM com o Cofre dos serviços de recuperação.
3. **Instalar o agente de cópia de segurança do Azure** — instale o agente em cada servidor do DPM.
4. **Registar o servidor** — registe-se o servidor do DPM com o Cofre dos serviços de recuperação.

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="key-definitions"></a>Definições de chave
Seguem-se algumas definições importantes para cópia de segurança do Azure para o DPM:

1. **Credencial de cofre** — as credenciais do cofre são necessários para autenticar a máquina para enviar dados de cópia de segurança para um cofre identificado no serviço de cópia de segurança do Azure. Ele pode ser transferido a partir do cofre e é válido durante 48 horas.
2. **Frase de acesso** — frase de acesso é utilizado para encriptar as cópias de segurança para a cloud. Guarde o ficheiro numa localização segura, pois é necessária durante uma operação de recuperação.
3. **PIN de segurança** — se tiver ativado o [definições de segurança](https://docs.microsoft.com/azure/backup/backup-azure-security-feature) do cofre, o PIN de segurança é necessária para a execução de operações de cópia de segurança críticas. Esta autenticação multifator adiciona outra camada de segurança. 
4. **Pasta de recuperação** — é a frase que as cópias de segurança da cloud são temporariamente transferidas durante as recuperações de cloud. Basicamente, seu tamanho deve ser igual ao tamanho dos itens de cópia de segurança que pretende recuperar em paralelo.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

## <a name="edit-storage-replication"></a>Editar os replicação de armazenamento

Replicação de armazenamento permite-lhe escolher entre o armazenamento com redundância geográfica e armazenamento localmente redundante. Por predefinição, o seu cofre tem um armazenamento georredundante. Se o Cofre é sua cópia de segurança primária, deixe a opção definida para o armazenamento georredundante. Se pretender que uma opção mais barata que não é tão durável, utilize o procedimento seguinte para configurar o armazenamento localmente redundante. Leia mais sobre as opções de armazenamento [georredundante](../storage/common/storage-redundancy-grs.md) e [localmente redundante](../storage/common/storage-redundancy-lrs.md) na [descrição geral da replicação de Armazenamento do Azure](../storage/common/storage-redundancy.md).

Para editar a definição de replicação de armazenamento:

> [!NOTE] 
> Configure a replicação de armazenamento antes de acionar a cópia de segurança inicial. Se optar por alterar a configuração de replicação de armazenamento após fazer backup item protegido, tem de parar proteção no cofre antes de mudar a configuração de armazenamento.
>  

1. Selecione o Cofre e abrir o dashboard do cofre.

2. Na **Manage** secção, clique em **infraestrutura de cópia de segurança**.

3. Sobre o **configuração de cópia de segurança** menu, escolha a opção de replicação de armazenamento para o cofre.

    ![Lista de cofres de cópia de segurança](./media/backup-azure-dpm-introduction/choose-storage-configuration-rs-vault.png)

    Após escolher a opção de armazenamento para o cofre, está pronto para associar a VM com o cofre. Para começar a associação, detete e registe as máquinas virtuais do Azure.

## <a name="download-vault-credentials"></a>Transferir as credenciais do cofre
O ficheiro de credenciais do cofre é um certificado gerado pelo portal para cada cofre de cópia de segurança. O portal, em seguida, carrega a chave pública para o Access Control Service (ACS). Durante o fluxo de trabalho do registo de máquina, chave privada do certificado é disponibilizada para o usuário, que autentica a máquina. Base a autenticação, o serviço de cópia de segurança do Azure envia dados para o Cofre identificado.

As credenciais do cofre são utilizadas apenas durante o fluxo de trabalho do registo. É responsabilidade do utilizador para se certificar de que o ficheiro de credenciais do Cofre não é comprometido. Se se perder o controlo sobre as credenciais, as credenciais do cofre podem ser utilizadas para registar outras máquinas para o cofre. No entanto, os dados de cópia de segurança são encriptados com uma frase de acesso que pertence ao cliente, para que os dados de cópia de segurança existentes não podem ficar comprometidos. Para atenuar esta preocupação, as credenciais do cofre expiram após 48 horas. Transferir as credenciais do cofre novo quantas vezes for necessário. No entanto, apenas o ficheiro de credenciais de cofre mais recente pode ser utilizado durante o fluxo de trabalho do registo.

O ficheiro de credenciais do Cofre é transferido através de um canal seguro do portal do Azure. O serviço de cópia de segurança do Azure não está ciente de chave privada do certificado e a chave privada não está disponível no portal do ou o serviço. Utilize os seguintes passos para transferir o ficheiro de credenciais do cofre para um computador local.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

2. Abra o Cofre de serviços de recuperação que pretende registar para um servidor do DPM.

3. Abre o menu de definições por predefinição. Se estiver fechado, clique em **definições** no dashboard do cofre para abrir o menu. Na **configurações** menu, clique em **propriedades**.

    ![Abrir o menu do cofre](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

4. Na página de propriedades, sob **credenciais de cópia de segurança** clique em **transferir**. O portal gera o arquivo de credenciais do cofre, que é disponibilizado para download.

    ![Transferência](./media/backup-azure-dpm-introduction/vault-credentials.png)

O portal gera uma credencial de cofre utilizando uma combinação de nome do cofre e a data atual. Clique em **guardar** para transferir as credenciais do cofre para a pasta de transferências da conta local ou selecione guardar como, a partir do menu Guardar para especificar uma localização para as credenciais do cofre. Irá demorar até um minuto para o ficheiro ser gerado.

### <a name="note"></a>Nota
* Certifique-se de que o ficheiro de credenciais do Cofre é guardado numa localização que pode ser acedida a partir do seu computador. Se ele é armazenado num compartilhamento de arquivo/SMB, verifique as permissões de acesso.
* O ficheiro de credenciais do Cofre é utilizado apenas durante o fluxo de trabalho do registo.
* O ficheiro de credenciais do cofre expira após 48hrs e pode ser transferido a partir do portal.

## <a name="install-backup-agent"></a>Instalar o agente de cópia de segurança
Depois de criar o Cofre de cópia de segurança do Azure, deve ser instalado um agente em cada uma das suas máquinas do Windows (Windows Server, Windows client, servidor do System Center Data Protection Manager ou máquina do servidor de cópia de segurança do Azure) que permite efetuar cópias de segurança de dados e aplicações para o Azure .

1. Abra o Cofre de serviços de recuperação que pretende registar a máquina do DPM.
2. Abre o menu de definições por predefinição. Se estiver fechado, clique em **definições** para abrir o menu de definições. No menu de definições, clique em **propriedades**.

    ![Abrir o menu do cofre](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. Na página Definições, clique em **baixe** sob **Azure Backup Agent**.

    ![Transferência](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

   Depois do agente é transferido, execute o MARSAgentInstaller.exe para iniciar a instalação do agente do Backup do Azure. Escolha a pasta de instalação e a pasta de rascunho, necessário para o agente. A localização da cache especificada tem de ter espaço livre, que é, pelo menos, 5% dos dados de cópia de segurança.

4. Se utilizar um servidor proxy para ligar à internet, o **configuração do Proxy** ecrã, introduza os detalhes do servidor proxy. Se utilizar um proxy autenticado, introduza os detalhes de nome e palavra-passe do utilizador neste ecrã.

5. O agente de cópia de segurança do Azure instala o .NET Framework 4.5 e Windows PowerShell (se ainda não estiver disponível) para concluir a instalação.

6. Depois de instalar o agente, **fechar** a janela.

   ![Fechar](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)

7. Para **registar o servidor do DPM** para o cofre, no **Management** separador, clique em **Online**. Em seguida, selecione **registar**. Isso abrirá o Assistente de configuração do registo.

8. Se utilizar um servidor proxy para ligar à internet, o **configuração do Proxy** ecrã, introduza os detalhes do servidor proxy. Se utilizar um proxy autenticado, introduza os detalhes de nome e palavra-passe do utilizador neste ecrã.

    ![Configuração de proxy](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
9. No ecrã de credenciais de cofre, navegue até e selecione o ficheiro de credenciais de cofre que foi baixado anteriormente.

    ![Credenciais do Cofre](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

    O ficheiro de credenciais do cofre só é válido para 48 horas (depois de ser transferido a partir do portal). Se encontrar qualquer erro neste ecrã (por exemplo, "as credenciais do cofre ficheiro fornecido expirou"), o início de sessão para o portal do Azure e a transferência que as credenciais do cofre ficheiro novamente.

    Certifique-se de que o ficheiro de credenciais do Cofre está disponível num local que pode ser acedido pelo aplicativo de instalação. Se ocorrerem erros relacionados de acesso, copie o ficheiro de credenciais do cofre para uma localização temporária nesta máquina e repita a operação.

    Se ocorrer um erro de credenciais de cofre inválidas (por exemplo, "Cofre inválidas as credenciais fornecidas") o ficheiro está danificado ou não ter as credenciais mais recentes associadas ao serviço de recuperação. Repita a operação depois de baixar um novo ficheiro de credenciais do cofre a partir do portal. Este erro é normalmente, é apresentado se o usuário clica no **credenciais do Cofre de Download** opção no portal do Azure, numa rápida sucessão. Neste caso, apenas o ficheiro de credenciais de cofre segundo é válido.

10. Para controlar a utilização de largura de banda de rede durante o trabalho e horas de descanso, o **definição de limitação** ecrã, pode definir os limites de utilização de largura de banda e definir as horas de trabalho e de descanso.

    ![Definição de limitação](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)

11. Na **definição da pasta de recuperação** ecrã, navegue para a pasta onde os ficheiros transferidos do Azure será testada temporariamente.

    ![Definição da pasta de recuperação](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)

12. Na **definição de encriptação** ecrã, pode gerar uma frase de acesso ou fornecer uma frase de acesso (mínimo de 16 carateres). Lembre-se guardar a frase de acesso numa localização segura.

    ![Encriptação](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Se a frase de acesso for perdida ou esquecida; Microsoft não pode ajudar a recuperar os dados de cópia de segurança. O utilizador final é o proprietário a frase de acesso de encriptação e a Microsoft não tem visibilidade para a frase de acesso utilizada pelo utilizador final. Guarde o ficheiro numa localização segura, pois é necessária durante uma operação de recuperação.
    >
    >

13. Depois de clicar o **registar** botão, a máquina está registada com êxito para o Cofre e está agora pronto para iniciar o backup do Microsoft Azure.

14. Ao utilizar o Data Protection Manager, pode modificar as definições especificadas durante o fluxo de trabalho do registo ao clicar o **configurar** opção selecionando **Online** sob o **gestão**  Separador.

## <a name="requirements-and-limitations"></a>Requisitos (e limitações)
* O DPM pode ser executado como um servidor físico ou uma máquina virtual de Hyper-V instalado no System Center 2012 SP1 ou System Center 2012 R2. Ele também pode ser executado como uma máquina virtual do Azure em execução no System Center 2012 R2 com, pelo menos, o DPM 2012 R2 Update Rollup 3 ou uma máquina virtual do Windows no VMware, pelo menos, a executar no System Center 2012 R2 com Update Rollup 5.
* Se estiver a executar o DPM com o System Center 2012 SP1 deve instalar Update Roll up 2 para o System Center Data Protection Manager SP1. Isto é necessário antes de poder instalar o agente de cópia de segurança do Azure.
* O servidor DPM deve ter o Windows PowerShell e o .net Framework 4.5 instalado.
* O DPM pode criar cópias de segurança a maioria das cargas de trabalho para o Azure Backup. Para obter uma lista completa do que oferece suporte a ver a cópia de segurança do Azure suporta itens abaixo.
* Não não possível recuperar dados armazenados em cópia de segurança do Azure com a opção "copiar para banda".
* Terá de uma conta do Azure com a funcionalidade de cópia de segurança do Azure ativada. Se não tiver uma conta, pode criar uma conta de avaliação gratuita em apenas alguns minutos. Leia sobre [preços de cópia de segurança do Azure](https://azure.microsoft.com/pricing/details/backup/).
* Utilizando o Azure Backup requer que o agente de cópia de segurança do Azure ser instalado nos servidores que pretende criar cópias de segurança. Cada servidor tem de ter, pelo menos, 5% do tamanho dos dados que estão a cópia de segurança, disponível como armazenamento local livre. Por exemplo, o backup de 100 GB de dados requer um mínimo de 5 GB de espaço livre na localização de rascunho.
* Os dados serão armazenados no armazenamento do cofre do Azure. Não há limite para a quantidade de dados, que pode criar uma cópia de segurança uma cópia de segurança do Azure do cofre, mas o tamanho de uma origem de dados (por exemplo uma máquina virtual ou a base de dados) não deve exceder os 54400 GB.

Estes tipos de ficheiro são suportados para criar uma cópia de segurança do Azure:

* Encriptados (apenas backups completos)
* Comprimidos (cópias de segurança incrementais suportadas)
* Dispersos (cópias de segurança incrementais suportadas)
* Comprimidos e dispersos (tratados como dispersos)

E estas não são suportadas:

* Não são suportados servidores em sistemas de ficheiros de maiúsculas e minúsculas.
* Ligações diretas (ignorados)
* (Ignorados) de pontos de reanálise
* Encriptados e comprimidos (ignorados)
* Encriptados e dispersos (ignorados)
* Fluxo comprimido
* Sequência dispersa

> [!NOTE]
> Do System Center DPM 2012 com SP1 e superior, pode criar cópias de segurança cargas de trabalho protegidas para o Azure.
>
>
