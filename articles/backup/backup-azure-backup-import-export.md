---
title: Cópia de segurança do Azure - cópia de segurança Offline ou propagação inicial através do serviço do Azure para importar/exportar
description: Saiba como a cópia de segurança do Azure permite-lhe enviar dados fora da rede utilizando o serviço importar/exportar do Azure. Este artigo explica a propagação offline dos dados de cópia de segurança iniciais, ao utilizar o serviço de exportação de importação do Azure.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: saurse
ms.openlocfilehash: 5ef44ccf87bc5e40b57dc7fc997c9a827c93484b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831465"
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Fluxo de trabalho de cópias de segurança offline no Azure Backup
Cópia de segurança do Azure tem vários resulta numa eficiência incorporada que reduzir os custos de armazenamento e de rede durante as cópias de segurança completas iniciais dos dados no Azure. As cópias de segurança completas iniciais normalmente transferem grandes quantidades de dados e necessitam de mais largura de banda quando comparado com as cópias de segurança subsequentes transferir apenas as diferenças/incrementais. Durante o processo de propagação offline, cópia de segurança do Azure pode utilizar discos para carregar os dados de cópia de segurança offline para o Azure.

O Azure Backup processo offline seeding está totalmente integrado com o [serviço importar/exportar do Azure](../storage/common/storage-import-export-service.md) que permite a transferência de dados de cópia de segurança iniciais para o Azure através da utilização de discos. Se tiver terabytes (TBs) de dados de cópia de segurança iniciais que têm de ser transferidos através de uma rede de latência alta e baixa largura de banda, pode utilizar o fluxo de trabalho offline propagação para enviar a cópia de segurança inicial, num ou mais discos rígidos, para um datacenter do Azure. A imagem seguinte fornece uma descrição geral dos passos no fluxo de trabalho.

  ![Descrição geral do processo de fluxo de trabalho de importação offline](./media/backup-azure-backup-import-export/offlinebackupworkflowoverview.png)

O processo de cópia de segurança offline envolve os seguintes passos:

1. Em vez de enviar os dados de cópia de segurança através da rede, escreva os dados de cópia de segurança para uma localização de transição.
2. Utilize o utilitário de AzureOfflineBackupDiskPrep para escrever os dados na localização de transição para um ou mais discos SATA.
3. Como parte do trabalho preparatórios, o utilitário de AzureOfflineBackupDiskPrep cria uma tarefa de importação do Azure. Enviar as utilizem unidades SATA ao centro de dados do Azure mais próximo e a tarefa de importação para ligar as atividades de referência.
4. No Centro de dados do Azure, os dados nos discos são copiados para uma conta de armazenamento do Azure.
5. Cópia de segurança do Azure copia os dados de cópia de segurança da conta do storage para o Cofre dos serviços de recuperação e cópias de segurança incrementais.

## <a name="supported-configurations"></a>Configurações suportadas 
As seguintes funcionalidades de cópia de segurança do Azure ou cargas de trabalho suportam a utilização da cópia de segurança Offline.

> [!div class="checklist"]
> * Cópia de segurança de ficheiros e pastas com o agente de serviços de recuperação do Azure (MARS) da Microsoft, também referido como o agente do Backup do Azure. 
> * Cópia de segurança de todos os ficheiros com o System Center Data Protection Manager (SC DPM) e cargas de trabalho 
> * Cópia de segurança de todos os ficheiros e cargas de trabalho com o servidor de cópia de segurança do Microsoft Azure <br/>

   > [!NOTE]
   > Cópia de segurança offline não é suportada para cópias de segurança do Estado do sistema feitas com o agente de cópia de segurança do Azure. 

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="prerequisites"></a>Pré-requisitos

  > [!NOTE]
  > Os seguintes pré-requisitos e fluxo de trabalho aplicam-se apenas a cópia de segurança Offline dos ficheiros e pastas utilizando o [agente mais recente do MARS](https://aka.ms/azurebackup_agent). Para efetuar cópias de segurança Offline para cargas de trabalho com o System Center DPM ou o servidor de cópia de segurança do Azure, consulte [neste artigo](backup-azure-backup-server-import-export-.md). 

Antes de iniciar o fluxo de trabalho de cópia de segurança Offline, conclua os seguintes pré-requisitos: 
* Criar um [cofre dos serviços de recuperação](backup-azure-recovery-services-vault-overview.md). Para criar um cofre, consulte os passos descritos para [neste artigo](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Certifique-se de que apenas o [versão mais recente do agente do Backup do Azure](https://aka.ms/azurebackup_agent) foi instalado no cliente Windows Server/Windows, conforme aplicável e o computador está registado com o Cofre de serviços de recuperação.
* O Azure PowerShell 3.7.0 é necessária no computador com o agente do Backup do Azure. É recomendado que transfira e [versão de instalar o 3.7.0 do Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v3.7.0-March2017).
* No computador que executa o agente do Backup do Azure, certifique-se está instalado o Microsoft Edge ou o Internet Explorer 11 e é o JavaScript ativado. 
* Crie uma conta de armazenamento do Azure na mesma subscrição que o Cofre dos serviços de recuperação. 
* Certifique-se de que tem o [as permissões necessárias](../azure-resource-manager/resource-group-create-service-principal-portal.md) ao criar a aplicação do Azure Active Directory. O fluxo de trabalho de cópia de segurança Offline cria uma aplicação do Azure Active Directory na subscrição associada à conta de armazenamento do Azure. O objetivo da aplicação é fornecer cópia de segurança do Azure com o acesso seguro e âmbito para o serviço de importação do Azure, necessário para o fluxo de trabalho de cópia de segurança Offline. 
* Registe o fornecedor de recursos Microsoft.ImportExport com a subscrição que contém a conta de armazenamento do Azure. Para registar o fornecedor de recursos:
    1. No menu principal, clique em **subscrições**.
    2. Se tem subscritos várias subscrições, selecione a subscrição que está a utilizar para a cópia de segurança offline. Se utilizar apenas uma subscrição, é apresentada a subscrição.
    3. No menu de subscrição, clique em **fornecedores de recursos** para ver a lista de fornecedores.
    4. Na lista de fornecedores de deslocamento para baixo para Microsoft.ImportExport. Se o estado é NotRegistered, clique em **registar**.
    ![registar o fornecedor de recursos](./media/backup-azure-backup-import-export/registerimportexport.png)
* Uma localização de transição, que pode ser uma partilha de rede ou de qualquer unidade adicional no computador, interno ou externo, com espaço em disco suficiente para conter a cópia inicial, é criada. Por exemplo, se estiver a tentar fazer uma cópia de segurança de um servidor de ficheiros de 500 GB, certifique-se de que a área de transição é pelo menos de 500 GB. (Uma quantidade menor é utilizada devido a compressão.)
* Quando enviar discos para o Azure, utilize apenas polegada 2,5 SSD ou 2.5 polegadas ou 3.5 polegadas SATA II/III interno unidades de disco rígido. Pode utilizar unidades de disco rígido até 10 TB. Verifique o [documentação do serviço do Azure para importar/exportar](../storage/common/storage-import-export-requirements.md#supported-hardware) para o conjunto de discos com o serviço suporta mais recente.
* As unidades SATA tem de estar ligadas a um computador (referido como um *computador cópia*) a partir de onde a cópia dos dados de cópia de segurança do *localização de transição* para o SATA unidades é efetuada. Certifique-se de que o Bitlocker está ativado no *computador cópia*.

## <a name="workflow"></a>Fluxo de trabalho
Esta secção descreve o fluxo de trabalho de cópia de segurança offline, para que os dados podem ser fornecidos para um datacenter do Azure e carregados para o armazenamento do Azure. Se tiver dúvidas sobre o serviço de importação ou em qualquer aspeto do processo, consulte o [importar documentação de descrição geral do serviço](../storage/common/storage-import-export-service.md).

## <a name="initiate-offline-backup"></a>Iniciar a cópia de segurança offline
1. Quando agendar uma cópia de segurança no agente MARS, verá o ecrã seguinte.

    ![Ecrã de importação](./media/backup-azure-backup-import-export/offlinebackup_inputs.png)

  A descrição das entradas é o seguinte:

    * **Localização de transição**: A localização de armazenamento temporário para o qual a cópia de segurança inicial é escrita. Localização de transição podem estar num computador local ou uma partilha de rede. Se o computador de cópia e o computador de origem são diferentes, é recomendado que especifique o caminho de rede completa de localização de transição.
    * **Conta de armazenamento do Azure Resource Manager**: O nome da conta de armazenamento de tipo de Gestor de recursos em qualquer subscrição do Azure.
    * **Contentor de armazenamento do Azure**: O nome do blob de armazenamento de destino na conta do Storage do Azure onde os dados de cópia de segurança são importados antes de ser copiado para o Cofre dos serviços de recuperação.
    * **ID de subscrição do Azure**: O ID da subscrição do Azure onde será criada a conta de armazenamento do Azure.
    * **Nome da tarefa de importação do Azure**: O nome exclusivo que importar do Azure Backup do Azure e do serviço controlam a transferência de dados enviados em discos para o Azure. 
  
  Forneça as entradas no ecrã e clique em **seguinte**. Guardar fornecido *localização de transição* e *nome de tarefa de importação do Azure*, uma vez que esta informação é necessária para preparar os discos.

2. Quando lhe for solicitado, inicie sessão na sua subscrição do Azure. Tem de iniciar sessão, para que a cópia de segurança do Azure pode criar a aplicação do Azure Active Directory e fornecer as permissões necessárias para aceder ao serviço de importação do Azure.

    ![Cópia de segurança agora](./media/backup-azure-backup-import-export/azurelogin.png)

3. Concluir o fluxo de trabalho e, na consola de agente do Backup do Azure, clique em **cópia de segurança agora**.

    ![Cópia de segurança agora](./media/backup-azure-backup-import-export/backupnow.png)

4. Na página de confirmação do assistente, clique em **cópia de segurança**. A cópia de segurança inicial é escrita para a área de transição como parte da configuração.

   ![Confirme que está pronto para cópia de segurança agora](./media/backup-azure-backup-import-export/backupnow-confirmation.png)

    Depois de concluída a operação, a localização de transição está pronta para ser utilizado para a preparação do disco.

   ![Cópia de segurança agora](./media/backup-azure-backup-import-export/opbackupnow.png)

## <a name="prepare-sata-drives-and-ship-to-azure"></a>Preparar utilizem unidades SATA e são enviados para o Azure
O *AzureOfflineBackupDiskPrep* utilitário prepara as utilizem unidades SATA que são enviadas para o datacenter do Azure mais próximo. Este utilitário está disponível no diretório de instalação de agente do Backup do Azure (no seguinte caminho):

   *\Microsoft azure Recovery Services Agent\Utils\\*

1. Vá para o diretório e copie o **AzureOfflineBackupDiskPrep** diretório para outro computador em que a utilizem unidades SATA estão ligadas. No computador com os utilizem unidades SATA ligados, certifique-se:

    * O computador de cópia pode aceder a localização de transição para o fluxo de trabalho offline seeding utilizando o mesmo caminho de rede que foi fornecido o **iniciar cópia de segurança offline** fluxo de trabalho.
    * O BitLocker está ativado no computador de cópia.
    * O Azure PowerShell 3.7.0 está instalado.
    * Browsers mais recentes compatíveis (Edge ou o Internet Explorer 11) estão instalados e é o JavaScript ativado. 
    * O computador de cópia pode aceder ao portal do Azure. Se necessário, o computador de cópia pode ser o mesmo que o computador de origem.
    
    > [!IMPORTANT] 
    > Se o computador de origem for uma máquina virtual, em seguida, o computador de cópia tem de ser outro servidor físico ou máquina cliente do computador de origem.

2. Abra uma linha de comandos elevada no computador de cópia com o *AzureOfflineBackupDiskPrep* diretório utilitário como o diretório atual e execute o seguinte comando:

    ```.\AzureOfflineBackupDiskPrep.exe s:<Staging Location Path>```

    | Parâmetro | Descrição |
    | --- | --- |
    | s:&lt;*caminho de localização de transição*&gt; |Entrada obrigatória utilizada para fornecer o caminho para a localização de transição que introduziu no **iniciar cópia de segurança offline** fluxo de trabalho. |
    | p:&lt;*caminho PublishSettingsFile*&gt; |Entrada opcional que é utilizada para fornecer o caminho para o **definições de publicação do Azure** ficheiro que introduziu no **iniciar cópia de segurança offline** fluxo de trabalho. |

    Quando executar o comando, o utilitário de pedidos a seleção da tarefa de importação do Azure que corresponde às unidades que precisam para estar preparado. Se apenas uma tarefa de importação único está associada a com a localização de transição fornecida, verá um ecrã de como a que se segue.

    ![Entrada de ferramenta de preparação de disco do Azure](./media/backup-azure-backup-import-export/diskprepconsole0_1.png) <br/>

3. Introduza a letra de unidade da vírgula decimal para o disco montado que pretende preparar para a transferência para o Azure. 
4. Indique confirmação para a formatação da unidade quando lhe for pedido.
5. Lhe for pedido para iniciar sessão na sua subscrição do Azure. Indique as suas credenciais.

    ![Entrada de ferramenta de preparação de disco do Azure](./media/backup-azure-backup-import-export/signindiskprep.png) <br/>

    Em seguida, inicia a ferramenta preparar o disco e copiar os dados de cópia de segurança. Terá de anexar discos adicionais quando lhe for pedido pela ferramenta no caso do disco fornecido não tem espaço suficiente para os dados de cópia de segurança. <br/>

    No final da execução bem sucedida da ferramenta, a linha de comandos fornece três informações:
    1. Um ou mais discos que forneceu estão preparados para envio para o Azure. 
    2. Receberá uma confirmação que a tarefa de importação foi criada. A tarefa de importação utiliza o nome fornecido.
    3. A ferramenta apresenta o endereço de envio para o datacenter do Azure.

    ![Preparação de disco do Azure completa](./media/backup-azure-backup-import-export/console2.png)<br/>

6. No final da execução de comandos, pode atualizar as informações de envio.

7. São enviados os discos para o endereço que a ferramenta fornecida e mantenha o número de controlo para consulta futura.

   > [!IMPORTANT] 
   > Não existem duas tarefas de importação do Azure pode ter o mesmo número de controlo. Certifique-se de que as unidades preparadas o utilitário de uma única tarefa de importação do Azure são fornecidas em conjunto num único pacote e se existe um número único controlo exclusivo para o pacote. Não é combine unidades preparadas como parte das tarefas de importação do Azure separadas num único pacote.



## <a name="update-shipping-details-on-the-azure-import-job"></a>Atualizar detalhes do envio na tarefa de importação do Azure

O seguinte procedimento atualiza os detalhes de envio de tarefa de importação do Azure. Estas informações incluem detalhes sobre:
* o nome da operadora que disponibiliza os discos do Azure
* devolver o envio de detalhes para os discos

1. Inicie sessão sua subscrição do Azure.
2. No menu principal, clique em **todos os serviços** e em todos os serviços caixa de diálogo de, tipo de importação. Quando vir **as tarefas de importação/exportação**, clique no mesmo.
    ![Introduzir informações de envio](./media/backup-azure-backup-import-export/search-import-job.png)<br/>

    A lista de **as tarefas de importação/exportação** é aberto o menu e é apresentada a lista de todas as tarefas de importação/exportação na subscrição selecionada. 

3. Se tiver várias subscrições, lembre-se de que selecione a subscrição utilizada para importar os dados de cópia de segurança. Em seguida, selecione a tarefa de importação recentemente criada para abrir os respetivos detalhes.

    ![Reveja as informações de envio](./media/backup-azure-backup-import-export/import-job-found.png)<br/>

4. No menu de definições para a tarefa de importação, clique em **gerir informações de envio** e introduza os detalhes de envio de retorno.

    ![Armazenar informações de envio](./media/backup-azure-backup-import-export/shipping-info.png)<br/>

5. Quando tiver o número de controlo do seu envio operadora, clique na faixa na página de descrição geral da tarefa de importação do Azure e introduza os seguintes detalhes:

   > [!IMPORTANT] 
   > Certifique-se de que as informações de carrier e o controlo número são atualizadas dentro de duas semanas da criação da tarefa de importação do Azure. Falha ao verificar a estas informações no prazo de duas semanas pode resultar no trabalho a ser eliminado e as unidades não processadas.

   ![Armazenar informações de envio](./media/backup-azure-backup-import-export/joboverview.png)<br/>

   ![Armazenar informações de envio](./media/backup-azure-backup-import-export/tracking-info.png)

### <a name="time-to-process-the-drives"></a>Tempo para processar as unidades 
A quantidade de tempo que demora a processar uma tarefa de importação do Azure varia dependendo de fatores diferentes, tais como o tempo de envio, tipo, tipo e tamanho dos dados que está a ser copiados e o tamanho dos discos fornecido da tarefa. O serviço importar/exportar do Azure não tem um SLA mas depois dos discos são recebidos o serviço strives concluir a cópia de dados de cópia de segurança à sua conta de armazenamento do Azure dentro de 7 a 10 dias. 

### <a name="monitoring-azure-import-job-status"></a>Monitorização de estado da tarefa de importação do Azure
Pode monitorizar o estado da tarefa de importação do portal do Azure, navegando até o **as tarefas de importação/exportação** página e selecionando a tarefa. Para obter mais informações sobre o estado das tarefas de importação, consulte o [exportar de importação de armazenamento serviço](../storage/common/storage-import-export-service.md) artigo.

### <a name="complete-the-workflow"></a>Concluir o fluxo de trabalho
Depois da tarefa de importação foi concluída com êxito, os dados de cópia de segurança iniciais estão disponíveis na sua conta de armazenamento. No momento da cópia de segurança agendada seguinte, cópia de segurança do Azure copia o conteúdo dos dados da conta do storage para o Cofre dos serviços de recuperação, conforme mostrado abaixo: 

   ![Copiar dados para o Cofre de serviços de recuperação](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

No momento da cópia de segurança agendada seguinte, cópia de segurança do Azure efetua a cópia de segurança incremental.

### <a name="cleaning-up-resources"></a>A limpar os recursos
Assim que a cópia de segurança inicial estiver concluída, pode eliminar em segurança os dados importados para o contentor de armazenamento do Azure e os dados de cópia de segurança na localização de transição.

## <a name="next-steps"></a>Passos Seguintes
* Para quaisquer perguntas sobre o fluxo de trabalho de importação/exportação do Azure, consulte [utilizar o serviço de importação/exportação do Microsoft Azure para transferir dados para o Blob storage](../storage/common/storage-import-export-service.md).
* Consulte a secção de cópia de segurança offline da cópia de segurança do Azure de [FAQ](backup-azure-backup-faq.md) para quaisquer perguntas sobre o fluxo de trabalho.
