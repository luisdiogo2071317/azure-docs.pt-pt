---
title: Cópia de segurança do Azure pilha ficheiros e aplicações | Microsoft Docs
description: Utilize a cópia de segurança do Azure para criar cópias de segurança e recuperar ficheiros de pilha do Azure e aplicações para o seu ambiente de pilha do Azure.
services: backup
documentationcenter: ''
author: adiganmsft
manager: shivamg
editor: ''
keyword: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/9/2018
ms.author: adigan,markgal
ms.openlocfilehash: a907335ace1f6ea9ec427327d28ca9be5ce02fcc
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="back-up-files-and-applications-on-azure-stack"></a>Fazer cópias de segurança de ficheiros e aplicações na pilha do Azure
Pode utilizar o Backup do Azure para proteger (ou cópia de segurança) ficheiros e aplicações na pilha do Azure. Para fazer uma cópia de segurança de ficheiros e aplicações, instale o servidor de cópia de segurança do Microsoft Azure como uma máquina virtual em execução na pilha do Azure. Uma vez que instalou o servidor de cópia de segurança do Azure, adicione discos do Azure para aumentar o armazenamento local disponível para dados de cópia de segurança de curta duração. Servidor de cópia de segurança do Azure utiliza o armazenamento do Azure para a retenção de longo prazo.

> [!NOTE]
> Apesar do servidor de cópia de segurança do Azure e o System Center Data Protection Manager (DPM) são semelhantes, o DPM não é suportado para utilização com a pilha do Azure.
>


## <a name="azure-backup-server-protection-matrix"></a>Matriz de proteção do Azure Backup Server
Servidor de cópia de segurança do Azure protege as seguintes cargas de trabalho de máquina virtual de pilha do Azure.

| Origem de dados protegida | Proteção e recuperação |
| --------------------- | ----------------------- |
| Canal de anual do Windows Server Semiestruturados - Enterprise/Datacenter/padrão | Volumes, ficheiros, pastas |
| Windows Server 2016 - Enterprise/Datacenter/padrão | Volumes, ficheiros, pastas |
| Windows Server 2012 R2 - Enterprise/Datacenter/padrão | Volumes, ficheiros, pastas |
| Windows Server 2012 – Datacenter/Entprise/Standard | Volumes, ficheiros, pastas |
| Windows Server 2008 R2 - Enterprise/Datacenter/padrão | Volumes, ficheiros, pastas |
| SQL Server 2016 | Base de Dados |
| SQL Server 2014 | Base de Dados |
| SQL Server 2012 SP1 | Base de Dados |
| SharePoint 2013 | Farm, base de dados, front-end, servidor web |
| O SharePoint 2010 | Farm, base de dados, front-end, servidor web |
| Estado do Sistema | Estado do Sistema |
| Recuperação bare-metal (BMR) | Ficheiros de BMR, estado do sistema, pastas | 

## <a name="install-azure-backup-server"></a>Instalar o servidor de cópia de segurança do Azure
Para instalar o servidor de cópia de segurança do Azure numa máquina virtual pilha do Azure, consulte o artigo [a preparar a cópia de segurança de cargas de trabalho utilizando o servidor de cópia de segurança do Azure](backup-azure-microsoft-azure-backup.md). Antes de instalar e configurar o servidor de cópia de segurança do Azure, tenha em atenção o seguinte:

### <a name="determining-size-of-virtual-machine"></a>Determinar o tamanho da máquina virtual
Para executar o servidor de cópia de segurança do Azure numa máquina virtual do Azure pilha, utilize tamanho A2 ou superior. Para obter ajuda na escolha de um tamanho de máquina virtual, transfira o [Calculadora do tamanho da VM do Azure pilha](https://www.microsoft.com/download/details.aspx?id=56832).

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Redes virtuais em máquinas virtuais de pilha do Azure
Todas as máquinas virtuais utilizadas numa carga de trabalho de pilha do Azure têm de pertencer à mesma rede virtual do Azure e subscrição do Azure.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Armazenar dados de cópia de segurança no disco local e no Azure
Servidor do Backup do Azure armazena os dados de cópia de segurança nos discos Azure ligados à máquina virtual, para recuperação operacional. Assim que o espaço de armazenamento e discos estão anexados à máquina virtual, o servidor de cópia de segurança do Azure gere o armazenamento para si. A quantidade de armazenamento de dados de cópia de segurança depende do número e tamanho dos discos anexados a cada [máquina virtual de pilha do Azure](../azure-stack/user/azure-stack-storage-overview.md). Cada tamanho da VM de pilha do Azure tem um número máximo de discos que podem ser anexados à máquina virtual. Por exemplo, A2 é quatro discos. A3 é oito discos. A4 é 16 discos. Novamente, o tamanho e número de discos determina o agrupamento de armazenamento de cópia de segurança total.

> [!IMPORTANT]
> Deve **não** manter os dados de recuperação operacional (cópia de segurança) nos discos ligados por servidor de cópia de segurança do Azure mais de cinco dias.
>

Armazenar dados de cópia de segurança no Azure reduz a infraestrutura de cópia de segurança na pilha do Azure. Se os dados são mais de cinco dias, devem ser armazenado no Azure.

Para armazenar dados de cópia de segurança no Azure, criar ou utilizar um cofre dos serviços de recuperação. Quando preparar a cópia de segurança a carga de trabalho do servidor de cópia de segurança do Azure, poderá [configurar o Cofre dos serviços de recuperação](backup-azure-microsoft-azure-backup.md#recovery-services-vault). Depois de configurar, sempre que é executada uma tarefa de cópia de segurança, é criado um ponto de recuperação no cofre. Cada cofre dos serviços de recuperação contém até 9999 pontos de recuperação. Dependendo do número de pontos de recuperação criados e quanto são mantidos, pode manter os dados de cópia de segurança para muitos anos. Por exemplo, pode criar pontos de recuperação mensal e manter os mesmos cinco anos.
 
### <a name="using-sql-server"></a>Utilizar o SQL Server
Se pretender utilizar um SQL Server remoto para a base de dados do servidor de cópia de segurança do Azure, selecione apenas uma VM de pilha do Azure com o SQL Server.

### <a name="azure-backup-server-vm-performance"></a>Desempenho de VM do servidor de cópia de segurança do Azure
Se for partilhado com outras máquinas virtuais, o tamanho de conta de armazenamento e limites IOPS podem afetar o desempenho de máquina virtual do servidor de cópia de segurança do Azure. Por este motivo, deve utilizar uma conta de armazenamento separada para a máquina virtual do servidor de cópia de segurança do Azure. O agente de cópia de segurança do Azure em execução no servidor de cópia de segurança do Azure necessita de armazenamento temporário para:
    - uso próprio (uma localização da cache),
    - dados restaurados a partir da nuvem (área de transição local)
  
### <a name="configuring-azure-backup-temporary-disk-storage"></a>Configurar o armazenamento de disco temporário de cópia de segurança do Azure
Cada máquina virtual de pilha do Azure é fornecido com o armazenamento em disco temporário, que está disponível para o utilizador como volume d:`\`. A área de transição local necessária para a cópia de segurança do Azure pode ser configurada para estar situada em d:`\`, e a localização da cache pode ser colocada no c:`\`. Desta forma, nenhum armazenamento tem de ser carved na direção oposta aos discos de dados ligados à máquina virtual do servidor de cópia de segurança do Azure.

### <a name="scaling-deployment"></a>Implementação de dimensionamento
Se pretender dimensionar a sua implantação, tem as seguintes opções:
  - Aumentar verticalmente - aumentar o tamanho da máquina virtual do servidor de cópia de segurança do Azure, de uma série a série de D e aumentar o armazenamento local [pelas instruções de máquina virtual do Azure pilha](../azure-stack/user/azure-stack-manage-vm-disks.md).
  - Descarregar dados - envia os dados antigos para servidor de cópia de segurança do Azure e mantém apenas os dados mais recentes no armazenamento ligado ao servidor de cópia de segurança do Azure.
  - Ampliar - adicionar mais servidores de cópia de segurança do Azure para proteger as cargas de trabalho.


## <a name="bare-metal-recovery-for-azure-stack-vm"></a>Recuperação bare-Metal para pilha VM do Azure

Uma cópia de segurança de recuperação bare-metal (BMR) protege os ficheiros do sistema operativo e todos os dados de volume crítico, exceto os dados de utilizador. Uma cópia de segurança de BMR inclui uma cópia de segurança do Estado do sistema. Os procedimentos seguintes explicam como restaurar os dados da BMR.

### <a name="run-recovery-on-the-azure-backup-server"></a>Executar recuperação no servidor de cópia de segurança do Azure

Abra a consola do servidor de cópia de segurança do Azure.

1. Na consola, clique em **recuperação**, localizar a máquina que pretende recuperar e clique em **recuperação bare-Metal**.
2. Pontos de recuperação disponíveis são apresentados em negrito no calendário. Selecione a data e hora para o ponto de recuperação que pretende utilizar.
3. No **selecionar tipo de recuperação**, selecione **copiar para uma pasta de rede**.
4. No **especificar destino**, selecione de onde pretende copiar os dados. Lembre-se de que o destino selecionado tem de ter espaço suficiente para o ponto de recuperação. É sugerida que crie uma nova pasta.
5. No **especificar opções de recuperação**, selecione as definições de segurança para aplicar e escolher se pretende utilizar instantâneos de hardware baseada em SAN para acelerar a recuperação.     Os instantâneos de hardware baseados em SAN são uma opção apenas se tiver uma SAN com esta funcionalidade ativada e a capacidade para criar e dividi para o tornar gravável. Também para instantâneos de hardware baseados em SAN funcionar, o computador protegido e o servidor de cópia de segurança do Azure tem de estar ligado à mesma rede.
6. Configurar opções de notificação e clique em **recuperar** no **resumo** página.

### <a name="set-up-the-share-location"></a>A localização da partilha de multimédia
Na consola do servidor de cópia de segurança do Azure:
1. Na localização de restauro, navegue para a pasta que contém a cópia de segurança.
2. Partilhe a pasta acima de WindowsImageBackup para que a raiz da pasta partilhada seja a pasta WindowsImageBackup. Se a pasta WindowsImageBackup não está partilhada, a operação de restauro não irá localizar a cópia de segurança. Para ligar através de WinRE, precisa de uma partilha de WinRE acessível e o endereço IP correto e as credenciais.

### <a name="restore-the-machine"></a>Restaurar a máquina

1. Na máquina virtual em que pretende restaurar BMR, abra uma linha de comandos elevada e escreva os seguintes comandos. **/boottore** Especifica que Windows RE inicia automaticamente da próxima vez que o início do sistema.
```
Reagentc /boottore
shutdown /r /t 0
```

2. Na caixa de diálogo Abrir, selecione as definições de idioma e região. No **instalar** ecrã, selecione **reparar o seu computador**.
3. No **opções de recuperação de sistema** página, selecione **restaurar o computador utilizando uma imagem do sistema que criou anteriormente**.
4. No **selecionar uma cópia de segurança de imagem de sistema** página, escolha **selecionar uma imagem de sistema** > **avançadas** > **procurar uma imagem de sistema na rede**. Se aparecer um aviso, selecione **Sim**. Para escolher a imagem, vá para a partilha de rede, introduza as credenciais e selecione o ponto de recuperação. Esta ação procura cópias de segurança específicas disponíveis nesse ponto de recuperação. Selecione o ponto de recuperação.
5. No **escolha como restaurar a cópia de segurança**, selecione **formatar e dividir discos**. No ecrã seguinte, verifique as definições e clique em **concluir** para iniciar a tarefa de restauro. Um reinício conforme necessário.

## <a name="see-also"></a>Consulte também
Para obter informações sobre como utilizar o servidor de cópia de segurança do Azure para proteger outras cargas de trabalho, consulte um dos seguintes artigos:
- [Fazer cópias de segurança do farm do SharePoint](backup-azure-backup-sharepoint-mabs.md)
- [Criar cópias de segurança do SQL Server](backup-azure-sql-mabs.md)
