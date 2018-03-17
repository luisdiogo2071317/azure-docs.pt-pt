---
title: "Cópia de segurança do Azure - cópia de segurança Offline ou propagação inicial através do serviço de importação/exportação do Azure | Microsoft Docs"
description: "Saiba como a cópia de segurança do Azure permite-lhe enviar dados fora da rede utilizando o serviço importar/exportar do Azure. Este artigo explica a propagação offline dos dados de cópia de segurança iniciais, ao utilizar o serviço de exportação de importação do Azure."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 2/6/2018
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: 7af2623a25f73f6d9062d476309ecd53da542f70
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Fluxo de trabalho de cópias de segurança offline no Azure Backup
Cópia de segurança do Azure tem vários resulta numa eficiência incorporada que reduzir os custos de armazenamento e de rede durante as cópias de segurança completas iniciais dos dados no Azure. As cópias de segurança completas iniciais normalmente transferem grandes quantidades de dados e necessitam de mais largura de banda quando comparado com as cópias de segurança subsequentes transferir apenas as diferenças/incrementais. Cópia de segurança do Azure comprime as cópias de segurança iniciais. Durante o processo de propagação offline, cópia de segurança do Azure pode utilizar discos para carregar o comprimido inicial cópia de segurança de dados offline para o Azure.  

O processo offline propagação da cópia de segurança do Azure está totalmente integrado com o [serviço importar/exportar do Azure](../storage/common/storage-import-export-service.md) que permite-lhe transferir dados para o Azure através da utilização de discos. Se tiver terabytes (TBs) de dados de cópia de segurança iniciais que têm de ser transferidos através de uma rede de latência alta e baixa largura de banda, pode utilizar o fluxo de trabalho offline propagação para enviar a cópia de segurança inicial num ou mais unidades de disco rígido para um datacenter do Azure. Este artigo fornece uma descrição geral e obter os passos de detalhes que conclua este fluxo de trabalho.


## <a name="overview"></a>Descrição geral
Com a capacidade de cópia de segurança do Azure e do Azure para importar/exportar offline seeding, é simple carregar os dados offline para o Azure através da utilização de discos. O processo de cópia de segurança Offline envolve os seguintes passos:

> [!div class="checklist"]
> * Os dados de cópia de segurança, em vez de a ser enviados através da rede, são escritos um *localização de transição*
> * Os dados no *localização de transição* , em seguida, é escrito um ou mais discos SATA utilizando o *AzureOfflineBackupDiskPrep* utilitário
> * Uma tarefa de importação do Azure é criada automaticamente pelo utilitário
> * As unidades SATA, em seguida, são enviadas para o datacenter do Azure mais próximo
> * Após a conclusão do carregamento dos dados de cópia de segurança para o Azure, cópias de segurança do Azure copia os dados de cópia de segurança para o Cofre de cópia de segurança e as cópias de segurança incrementais.

## <a name="supported-configurations"></a>Configurações suportadas 
Cópia de segurança offline é suportada para todos os modelos de implementação do Backup do Azure que dados de cópia de segurança noutro local de local para o Microsoft Cloud. Isto inclui

> [!div class="checklist"]
> * Cópia de segurança de ficheiros e pastas com o agente de serviços de recuperação do Azure (MARS) da Microsoft ou o agente do Backup do Azure. 
> * Cópia de segurança de todos os ficheiros com o System Center Data Protection Manager (SC DPM) e cargas de trabalho 
> * Cópia de segurança de todos os ficheiros e cargas de trabalho com o servidor de cópia de segurança do Microsoft Azure <br/>

   > [!NOTE]
   > Cópia de segurança offline não é suportada para cópias de segurança do Estado do sistema feitas com o agente de cópia de segurança do Azure. 

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="prerequisites"></a>Pré-requisitos
Certifique-se de que os seguintes pré-requisitos são cumpridos antes de iniciar o fluxo de trabalho de cópia de segurança Offline
* A [cofre dos serviços de recuperação](backup-azure-recovery-services-vault-overview.md) foi criado. Para criar um, consulte os passos descritos para [neste artigo](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Agente do Backup do Azure ou servidor de cópia de segurança do Azure ou SC DPM foi instalado no cliente ou Windows Server/Windows, conforme aplicável e o computador está registado com o Cofre de serviços de recuperação. Certifique-se de que apenas o [versão mais recente da cópia de segurança do Azure](https://go.microsoft.com/fwlink/?linkid=229525) é utilizado. 
* [Transfira o ficheiro de definições de publicação do Azure](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) no computador a partir do qual planeia efetuar uma cópia dos seus dados. A partir dos quais transferir o ficheiro de definições de publicação de subscrição pode ser diferente da subscrição que contém o Cofre de serviços de recuperação. Se a sua subscrição está no sovereign nuvens do Azure, em seguida, utilize as seguintes ligações conforme apropriado para transferir o ficheiro de definições de publicação do Azure.

    | Região de nuvem sovereign | Ligação do ficheiro de definições de publicação do Azure |
    | --- | --- |
    | Estados Unidos | [Link](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |
    | China | [Link](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |

* Uma conta de armazenamento do Azure com *clássico* foi criado o modelo de implementação na subscrição do que transferiu o ficheiro de definições de publicação conforme mostrado abaixo: 

 ![Criar uma conta de armazenamento clássico](./media/backup-azure-backup-import-export/storageaccountclassiccreate.png)

* Uma localização de transição, que pode ser uma partilha de rede ou de qualquer unidade adicional no computador, interno ou externo, com espaço em disco suficiente para conter a cópia inicial, é criada. Por exemplo, se estiver a tentar fazer uma cópia de segurança de um servidor de ficheiros de 500 GB, certifique-se de que a área de transição é pelo menos de 500 GB. (Uma quantidade menor é utilizada devido a compressão.)
* Relativamente a discos que serão enviados para o Azure, certifique-se de que apenas 2,5 polegada SSD ou 2.5 polegadas ou 3.5 polegadas SATA II/III unidades de disco rígido internas são utilizadas. Pode utilizar unidades de disco rígido até 10 TB. Verifique o [documentação do serviço do Azure para importar/exportar](../storage/common/storage-import-export-service.md#hard-disk-drives) para o conjunto de discos com o serviço suporta mais recente.
* As unidades SATA tem de estar ligado a um computador (referido como um *computador cópia*) a partir de onde a cópia dos dados de cópia de segurança do *localização de transição* para o SATA unidades é efetuada. Certifique-se de que o Bitlocker está ativado no *computador de cópia* 

## <a name="workflow"></a>Fluxo de trabalho
As informações nesta secção ajuda-o a concluir o fluxo de trabalho de cópia de segurança offline, para que os dados podem ser fornecidos para um datacenter do Azure e carregados para o armazenamento do Azure. Se tiver dúvidas sobre o serviço de importação ou em qualquer aspeto do processo, consulte o [descrição geral do serviço de importação](../storage/common/storage-import-export-service.md) documentação referenciada anteriormente.

### <a name="initiate-offline-backup"></a>Iniciar a cópia de segurança offline
1. Quando agendar uma cópia de segurança, verá o ecrã seguinte (no Windows Server, o cliente do Windows ou o System Center Data Protection Manager).

    ![Ecrã de importação](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Eis o ecrã correspondente no System Center Data Protection Manager: <br/>
    ![Ecrã de importação de servidor do DPM de SC e cópia de segurança do Azure](./media/backup-azure-backup-import-export/dpmoffline.png)

    A descrição das entradas é o seguinte:

    * **Localização de transição**: A localização de armazenamento temporário para o qual a cópia de segurança inicial é escrita. Localização de transição podem estar num computador local ou uma partilha de rede. Se o computador de cópia e o computador de origem são diferentes, é recomendado que especifique o caminho de rede completa de localização de transição.
    * **Nome da tarefa de importação do Azure**: O nome exclusivo que importar do Azure Backup do Azure e do serviço controlam a transferência de dados enviados em discos para o Azure.
    * **Definições de publicação do Azure**: forneça o caminho local para o ficheiro de definições de publicação.
    * **ID de subscrição do Azure**: ID de subscrição do Azure para a subscrição de onde transferiu o ficheiro de definições de publicação do Azure. 
    * **Conta de armazenamento do Azure**: O nome da conta de armazenamento na subscrição do Azure associada ao ficheiro de definições de publicação do Azure.
    * **Contentor de armazenamento do Azure**: O nome do blob de armazenamento de destino na conta do storage do Azure onde os dados de cópia de segurança foi importados.

     Guardar o *localização de transição* e *nome de tarefa de importação do Azure* fornecidas conforme for necessário para preparar os discos.  
     
2. Concluir o fluxo de trabalho e, para iniciar a cópia da cópia de segurança offline, clique em **cópia de segurança agora** na consola de gestão de agente cópia de segurança do Azure. A cópia de segurança inicial é escrita para a área de transição como parte deste passo.

    ![Cópia de segurança agora](./media/backup-azure-backup-import-export/backupnow.png)

    Para concluir o fluxo de trabalho correspondente no servidor do System Center Data Protection Manager ou a cópia de segurança do Azure, clique com botão direito do **grupo de proteção**e, em seguida, escolha o **criar ponto de recuperação** opção. Em seguida, escolha o **proteção Online** opção.

    ![O DPM SC e o servidor de cópia de segurança do Azure fazer cópias de segurança agora](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Depois de concluída a operação, a localização de transição está pronta para ser utilizado para a preparação do disco.

    ![Progresso da cópia de segurança](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-sata-drives-and-ship-to-azure"></a>Preparar utilizem unidades SATA e são enviados para o Azure
O *AzureOfflineBackupDiskPrep* utilitário é utilizado para preparar as utilizem unidades SATA que são enviadas para o Centro de dados do Azure mais próximo. Este utilitário está disponível no diretório de instalação do agente dos serviços de recuperação no caminho seguinte:

   *\Microsoft* *Azure* *Recovery* *Services* *Agent\Utils\*

1. Vá para o diretório e copie o **AzureOfflineBackupDiskPrep** diretório para um computador de cópia no qual estão ligadas as utilizem unidades SATA para estar preparado. Certifique-se as seguintes relativamente a computador cópia:

    * O computador de cópia pode aceder a localização de transição para o fluxo de trabalho offline seeding utilizando o mesmo caminho de rede que foi fornecido o **iniciar cópia de segurança offline** fluxo de trabalho.
    * O BitLocker está ativado no computador de cópia.
    * O computador de cópia pode aceder ao portal do Azure.

    Se necessário, o computador de cópia pode ser o mesmo que o computador de origem. 
    
    > [!IMPORTANT] 
    > Se o computador de origem for uma máquina virtual, em seguida, é obrigatório para utilizar um servidor físico diferente ou máquina cliente como o computador de cópia.
    
    
2. Abra uma linha de comandos elevada no computador de cópia com o *AzureOfflineBackupDiskPrep* diretório utilitário como o diretório atual e execute o seguinte comando:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to AzurePublishSettingsFile*>]`

    | Parâmetro | Descrição |
    | --- | --- |
    | s:&lt;*caminho de localização de transição*&gt; |Entrada obrigatória que é utilizada para fornecer o caminho para a localização de transição que introduziu no **iniciar cópia de segurança offline** fluxo de trabalho. |
    | p:&lt;*caminho PublishSettingsFile*&gt; |Entrada opcional que é utilizada para fornecer o caminho para o **definições de publicação do Azure** ficheiro que introduziu no **iniciar cópia de segurança offline** fluxo de trabalho. |

    > [!NOTE]
    > O &lt;caminho AzurePublishSettingFile&gt; valor é obrigatório quando o computador de cópia e o computador de origem são diferentes.
    >
    >

    Quando executar o comando, o utilitário de pedidos a seleção da tarefa de importação do Azure que corresponde às unidades que precisam para estar preparado. Se apenas uma tarefa de importação único está associada a com a localização de transição fornecida, verá um ecrã de como a que se segue.

    ![Entrada de ferramenta de preparação de disco do Azure](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
    
3. Introduza a letra de unidade da vírgula decimal para o disco montado que pretende preparar para a transferência para o Azure. Indique confirmação para a formatação da unidade quando lhe for pedido.

    Em seguida, inicia a ferramenta preparar o disco e copiar os dados de cópia de segurança. Terá de anexar discos adicionais quando lhe for pedido pela ferramenta no caso do disco fornecido não tem espaço suficiente para os dados de cópia de segurança. <br/>

    No final da execução bem sucedida da ferramenta, um ou mais discos que forneceu estão preparados para envio para o Azure. Além disso, uma tarefa de importação com o nome que forneceu durante a **iniciar cópia de segurança offline** fluxo de trabalho é criado no Azure. Por fim, a ferramenta apresenta o endereço de envio para o datacenter do Azure em que os discos têm de ser fornecidos.

    ![Preparação de disco do Azure completa](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>
    
4. No final da execução de comando Consulte também a opção para atualizar as informações de envio, conforme mostrado abaixo:

    ![Atualizar a opção de informações de envio](./media/backup-azure-backup-import-export/updateshippingutility.png)<br/>

5. Pode introduzir os detalhes de imediato. A ferramenta orienta-o processo de uma série de entradas. No entanto se não dispõe de informações como o número ou outros detalhes relacionados com o envio de registo, pode terminar a sessão. Os passos para atualizar os detalhes de envio posterior são fornecidos neste artigo. 

6. São enviados os discos para o endereço que a ferramenta fornecida e mantenha o número de controlo para consulta futura.

   > [!IMPORTANT] 
   > Não existem duas tarefas de importação do Azure pode ter o mesmo número de controlo. Certifique-se de que as unidades preparadas o utilitário de uma única tarefa de importação do Azure são fornecidas em conjunto num único pacote e se existe um número único controlo exclusivo para o pacote. Não combinar unidades preparadas como parte da **diferentes** tarefas de importação do Azure num pacote único. 

5. Se tiver o controlo número informação, vá para o computador de origem, que está a aguardar a conclusão da tarefa de importação e execute o seguinte comando numa linha de comandos elevada com *AzureOfflineBackupDiskPrep* diretório do utilitário de diretório atual: 

   `*.\AzureOfflineBackupDiskPrep.exe*  u:`

   Pode, opcionalmente, executar o seguinte comando num computador diferente, tal como o *computador cópia*, com *AzureOfflineBackupDiskPrep* diretório do utilitário do diretório atual:
   
   `*.\AzureOfflineBackupDiskPrep.exe*  u:  s:<*Staging Location Path*>   p:<*Path to AzurePublishSettingsFile*>`

    | Parâmetro | Descrição |
    | --- | --- |
    | u: | Entrada obrigatória utilizada para atualizar detalhes do envio de uma tarefa de importação do Azure |
    | s:&lt;*caminho de localização de transição*&gt; | Quando o comando não é executado no computador de origem de entrada obrigatória. Utilizado para fornecer o caminho para a localização de transição que introduziu no **iniciar cópia de segurança offline** fluxo de trabalho. |
    | p:&lt;*caminho PublishSettingsFile*&gt; | Quando o comando não é executado no computador de origem de entrada obrigatória. Utilizado para fornecer o caminho para o **definições de publicação do Azure** ficheiro que introduziu no **iniciar cópia de segurança offline** fluxo de trabalho. |
    
    O utilitário Deteta automaticamente a tarefa de importação que o computador de origem está a aguardar ou as tarefas de importação associada com a localização de transição, quando o comando é executado num computador diferente. Em seguida, fornece a opção para atualizar informações de envio através de uma série de entradas, como mostrado abaixo: 
    
    ![Introduzir informações de envio](./media/backup-azure-backup-import-export/shippinginputs.png)<br/>

6. Depois de todas as entradas são fornecidas, reveja cuidadosamente os detalhes e consolidar as informações de envio que forneceu escrevendo *Sim*. 

    ![Reveja as informações de envio](./media/backup-azure-backup-import-export/reviewshippinginformation.png)<br/>

7. Em Atualizar as informações de envio com êxito, o utilitário fornece uma localização local onde os detalhes de envio introduzidos por si são armazenados como mostrado abaixo 

    ![Armazenar informações de envio](./media/backup-azure-backup-import-export/storingshippinginformation.png)<br/>

   > [!IMPORTANT] 
   > Certifique-se de que as unidades de contactar o Centro de dados do Azure dentro de duas semanas de fornecer a informação de envio utilizando o *AzureOfflineBackupDiskPrep* utilitário. Falha ao fazê-lo pode resultar em unidades não processadas.  

Depois de concluir os passos acima, o Centro de dados do Azure está pronto para receber as unidades e continuar a processá-los para transferir os dados de cópia de segurança das unidades para a conta de armazenamento do Azure clássico tipo que criou. 

### <a name="time-to-process-the-drives"></a>Tempo para processar as unidades 
A quantidade de tempo que demora a processar uma tarefa de importação do Azure varia dependendo de fatores diferentes, tais como o tempo de envio, tipo, tipo e tamanho dos dados que está a ser copiados e o tamanho dos discos fornecido da tarefa. O serviço importar/exportar do Azure não tem um SLA mas depois dos discos são recebidos o serviço strives concluir a cópia de dados de cópia de segurança à sua conta de armazenamento do Azure dentro de 7 a 10 dias. A secção seguinte descreve em detalhe como pode monitorizar o estado da tarefa de importação do Azure. 

### <a name="monitoring-azure-import-job-status"></a>Monitorização de estado da tarefa de importação do Azure
Enquanto a unidades estão em trânsito ou datacenter do Azure para ser copiado para a conta de armazenamento, o agente do Backup do Azure ou SC DPM ou a consola do servidor de cópia de segurança do Azure no computador de origem mostra o estado da tarefa seguinte para as cópias de segurança agendadas. 

  `Waiting for Azure Import Job to complete. Please check on Azure Management portal for more information on job status`

Siga os passos abaixo, para verificar o estado da tarefa de importação. 
1. Abra uma linha de comandos elevada no computador de origem e execute o seguinte comando:
    
     `AzureOfflineBackupDiskPrep.exe u:`
    
2.  O resultado mostra o estado atual da tarefa de importação, conforme mostrado abaixo: 

    ![A verificar o estado da tarefa de importação](./media/backup-azure-backup-import-export/importjobstatusreporting.png)<br/>

Para obter mais informações sobre os vários Estados da tarefa de importação do Azure, consulte [neste artigo](../storage/common/storage-import-export-service.md#how-does-the-azure-importexport-service-work)

### <a name="complete-the-workflow"></a>Concluir o fluxo de trabalho
Após a conclusão da tarefa de importação, os dados de cópia de segurança iniciais estão disponíveis na sua conta de armazenamento. No momento da cópia de segurança agendada seguinte, o backup do Azure copia o conteúdo dos dados da conta do storage para o Cofre dos serviços de recuperação conforme mostrado abaixo: 

   ![Copiar dados para o Cofre de serviços de recuperação](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

No momento da cópia de segurança agendada seguinte, cópia de segurança do Azure efetua a cópia de segurança incremental através de cópia de segurança inicial.

## <a name="next-steps"></a>Passos Seguintes
* Para quaisquer perguntas sobre o fluxo de trabalho de importação/exportação do Azure, consulte [utilizar o serviço de importação/exportação do Microsoft Azure para transferir dados para o Blob storage](../storage/common/storage-import-export-service.md).
* Consulte a secção de cópia de segurança offline da cópia de segurança do Azure de [FAQ](backup-azure-backup-faq.md) para quaisquer perguntas sobre o fluxo de trabalho.
