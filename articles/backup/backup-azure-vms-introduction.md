---
title: Planeamento da sua infraestrutura de cópia de segurança de VM no Azure
description: Considerações importantes ao planejar a fazer uma cópia de segurança de máquinas virtuais no Azure
services: backup
author: markgalioto
manager: carmonm
keywords: cópia de segurança de vms, a cópia de segurança de máquinas virtuais
ms.service: backup
ms.topic: conceptual
ms.date: 8/29/2018
ms.author: markgal
ms.openlocfilehash: 9e2ef16cffb044409b6f7f8e7785010097bcda87
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286657"
---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>Planear a sua infraestrutura de cópias de segurança de VMs no Azure
Este artigo fornece desempenho e sugestões de recursos para o ajudar a planear a sua infraestrutura de cópia de segurança de VM. Também define os aspetos fundamentais do serviço de cópia de segurança; esses aspectos podem ser fundamentais para determinar sua arquitetura, planejamento de capacidade e agendamento. Se tiver [preparar o ambiente](backup-azure-arm-vms-prepare.md), o planejamento é a próxima etapa antes de começar [para fazer uma cópia de segurança de VMs](backup-azure-arm-vms.md). Se precisar de mais informações sobre máquinas virtuais do Azure, consulte a [documentação das máquinas virtuais](https://azure.microsoft.com/documentation/services/virtual-machines/). 

> [!NOTE]
> Este artigo é para utilização com discos geridos e não geridos. Se utilizar discos não geridos, existem recomendações de conta de armazenamento. Se usar [Managed Disks do Azure](../virtual-machines/windows/managed-disks-overview.md), não precisa se preocupar sobre problemas de utilização de desempenho ou recurso. Azure otimiza a utilização de armazenamento para si.
>

## <a name="how-does-azure-back-up-virtual-machines"></a>Como funciona o Azure backup de segurança de máquinas virtuais?
Quando o serviço de cópia de segurança do Azure inicia uma tarefa de cópia de segurança na hora agendada, os acionadores de serviço a extensão de cópia de segurança para tirar um instantâneo de ponto no tempo. O serviço de cópia de segurança do Azure utiliza o _VMSnapshot_ extensão no Windows e o _VMSnapshotLinux_ extensão no Linux. A extensão é instalada durante a primeira cópia de segurança VM. Para instalar a extensão, tem de executar a VM. Se a VM não estiver em execução, o serviço Backup cria um instantâneo do armazenamento subjacente (uma vez que não ocorrem escritas da aplicação enquanto a VM está parada).

Quando criar um instantâneo de VMs do Windows, o serviço Backup é coordenado com o Serviço de Cópia Sombra de Volumes (VSS) para obter um instantâneo consistente dos discos da máquina virtual. Se estiver fazendo o backup de VMs do Linux, pode escrever seus próprios scripts personalizados para garantir a consistência quando um instantâneo de VM. Neste artigo, são fornecidos detalhes em invocar esses scripts.

Assim que o serviço Azure Backup tira o instantâneo, os dados são transferidos para o cofre. Para maximizar a eficiência, o serviço identifica e transfere apenas os blocos de dados que foram alterados desde a cópia de segurança anterior.

![Arquitetura de cópia de segurança de máquina virtual do Azure](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Quando a transferência de dados estiver concluída, o instantâneo é removido e é criado um ponto de recuperação.

> [!NOTE]
> 1. Durante o processo de cópia de segurança, cópia de segurança do Azure não inclui o disco temporário anexado à máquina virtual. Para obter mais informações, consulte o blog sobre [armazenamento temporário](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
> 2. O Azure Backup tira um instantâneo de nível de armazenamento e transfere esse instantâneo para o cofre. Não altere as chaves de conta de armazenamento até que a tarefa de cópia de segurança é concluída.
>

### <a name="data-consistency"></a>Consistência dos dados
Fazer backup e restaurar dados críticos são complicados pelo fato de que os dados críticos da empresa tem uma cópia de segurança ao mesmo tempo as aplicações que produzem os dados de negócios estão em execução. Para resolver isso, o Azure Backup suporta cópias de segurança consistentes com aplicações para VMs do Linux e Windows
#### <a name="windows-vm"></a>VM do Windows
Cópia de segurança do Azure leva as cópias de segurança completas de VSS em VMs do Windows (Leia mais sobre [backup completo de VSS](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). Para ativar as cópias de segurança de cópia VSS, a seguinte chave de registo tem de ser definido na VM.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```

#### <a name="linux-vms"></a>VMs do Linux

Cópia de segurança do Azure fornece uma estrutura de scripts para controlar o fluxo de trabalho de cópia de segurança e o ambiente. Para garantir cópias de segurança de VM do Linux consistentes com aplicações, utilize a estrutura de scripts para criar scripts personalizados de pré e pós-scripts. Invocar o script anterior antes de tirar o instantâneo VM e, em seguida, invoque o script posterior, uma vez concluída a tarefa de instantâneo VM. Para obter mais informações, consulte o artigo [backups de VM do Linux consistentes de aplicativos](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

> [!NOTE]
> O Azure Backup invoca apenas a escrita de cliente pré e pós-scripts. Se o script prévio e os pós-scripts de executam com êxito, cópia de segurança do Azure marca o ponto de recuperação como aplicação consistente. No entanto, o cliente é responsável pela consistência de aplicação, ao utilizar scripts personalizados.
>

A tabela seguinte explica os tipos de consistência e as condições de quando estes ocorrerem.

| Consistência | Com base em VSS | Explicação e detalhes |
| --- | --- | --- |
| Consistência das aplicações |Sim para Windows|Consistência de aplicação é ideal para cargas de trabalho, pois garante que:<ol><li> A VM *arranca*. <li>Há *nenhum dano*. <li>Há *sem perda de dados*.<li> Os dados são consistentes com a aplicação que utiliza os dados, porque envolve a aplicação no momento da cópia de segurança – usando o script VSS ou pré/pós.</ol> <li>*VMs do Windows*-cargas de trabalho mais Microsoft têm escritores VSS, que são executadas ações de carga de trabalho específicas relacionadas com a consistência dos dados. Por exemplo, o escritor VSS do SQL Server garante que as escritas para o ficheiro de registo de transação e a base de dados, são realizadas corretamente. Para cópias de segurança de VM do Windows de IaaS, para criar um ponto de recuperação consistentes com aplicações, a extensão de cópia de segurança deve invocar o fluxo de trabalho do VSS e concluí-la antes de tirar o instantâneo VM. Para o instantâneo de VM do Azure ser precisa, os escritores VSS de todos os aplicativos de VM do Azure tem de concluir também. (Saiba a [Noções básicas do VSS](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx) e aprofundar nos detalhes da [seu funcionamento](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx)). </li> <li> *VMs do Linux*-os clientes podem executar [personalizado script prévio e o script posterior para garantir a consistência de aplicação](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). </li> |
| Consistência do sistema de ficheiros |Sim - para computadores baseados em Windows |Existem dois cenários em que o ponto de recuperação pode ser *sistema de ficheiros consistente*:<ul><li>Cópias de segurança de VMs do Linux no Azure, sem o pre-script/pós-script ou se pre-script/pós-script falhou. <li>Falha do VSS durante a cópia de segurança para VMs do Windows no Azure.</li></ul> Em ambos os casos, o melhor que pode ser feito é se certificar de que: <ol><li> A VM *arranca*. <li>Há *nenhum dano*.<li>Há *sem perda de dados*.</ol> Os aplicativos precisam implementar seu próprio mecanismo de "correção de segurança" sobre os dados restaurados. |
| Consistência de falhas |Não |Esta situação é equivalente a uma máquina virtual com uma "Falha" (por meio de forma recuperável ou disco rígido reset). Consistência de falhas ocorre normalmente quando a máquina virtual do Azure é desligada no momento da cópia de segurança. Um ponto de recuperação consistentes com falhas fornece sem garantias para a consistência dos dados sobre o meio de armazenamento – a partir da perspectiva do sistema operativo ou o aplicativo. Apenas os dados que já existe no disco no momento da cópia de segurança são capturados e uma cópia de segurança. <br/> <br/> Enquanto não há garantias, normalmente, o sistema de operativo é inicializado, seguido por verificação de disco procedimento, como o chkdsk, para corrigir quaisquer erros de danos. Quaisquer dados na memória ou escritas que não tenham sido transferidas para o disco serão perdidas. Normalmente, a aplicação segue com seu próprio mecanismo de verificação no caso de reversão de dados precisa ser feito. <br><br>Por exemplo, se o registo de transações tem entradas não estão presentes na base de dados, o software de base de dados agrega até que os dados são consistentes. Quando dados estão distribuídos em vários discos virtuais (como volumes expandidos), um ponto de recuperação consistentes com falhas não fornece garantias de nenhuma para a correção dos dados. |

## <a name="performance-and-resource-utilization"></a>Utilização de recursos e desempenho
Como o software de cópia de segurança que é implementado no local, deverá planear capacidade e a utilização de recursos de necessidades de backup das VMs no Azure. O [limites de armazenamento do Azure](../azure-subscription-service-limits.md#storage-limits) definir como estruturar as implementações de VM para obter o máximo de desempenho com impacto mínimo para executar cargas de trabalho.

### <a name="number-of-disks"></a>Número de discos
O processo de cópia de segurança tenta concluir uma tarefa de cópia de segurança mais depressa possível. Ao fazer isso, ele consome quantidade de recursos que ele pode. No entanto, todas as operações de e/s estão limitadas pela *taxa de transferência de destino para o Blob único*, que tem um limite de 60 MB por segundo. Numa tentativa de maximizar a velocidade do mesmo, o processo de cópia de segurança tenta fazer cópias de segurança a cada um dos discos da VM *paralelamente*. Se uma VM tem quatro discos, o serviço tenta fazer cópias de segurança de todos os discos de quatro em paralelo. O **número de discos** a cópia de segurança, é o fator mais importante na determinação de tráfego de cópia de segurança de conta de armazenamento.

### <a name="backup-schedule"></a>Agenda de cópia de segurança
Um fator adicional que tem impacto no desempenho é o **agenda de cópia de segurança**. Se configurar as políticas, para que todas as VMs são uma cópia de segurança ao mesmo tempo, tiver agendado uma confusão de tráfego. O processo de cópia de segurança tenta fazer cópias de segurança de todos os discos em paralelo. Para reduzir o tráfego de cópia de segurança, cópia de segurança diferentes VMs em diferente altura do dia, sem sobreposição.

## <a name="capacity-planning"></a>Planeamento de capacidade
Transfira o [capacidade de cópia de segurança de VM folha de cálculo do Excel planeamento](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) para ver o impacto do disco e opções de agendamento de backup.

### <a name="backup-throughput"></a>Débito de cópia de segurança
Para cada disco a cópia de segurança, cópia de segurança do Azure lê os blocos no disco e armazena apenas os dados alterados (cópia de segurança incremental). A tabela seguinte mostra os valores de débito de serviço de cópia de segurança média. Utilizar os seguintes dados, pode estimar a quantidade de tempo necessária para fazer backup de um disco de um determinado tamanho.

| Operação de cópia de segurança | Melhor débito |
| --- | --- |
| Cópia de segurança inicial |160 Mbps |
| Cópia de segurança incremental (DR) |640 Mbps <br><br> Débito cai significativamente se os dados alterados (o que precisam ser efetuada a cópia de segurança) estão dispersos entre o disco.|

## <a name="total-vm-backup-time"></a>Tempo de cópia de segurança de VM total
Embora a maior parte do tempo de cópia de segurança é gasto na leitura e a cópia de dados, outras operações de contribuem para o tempo total necessário para fazer uma cópia de segurança de uma VM:

* Tempo necessário para [instalar ou atualizar a extensão de cópia de segurança](backup-azure-arm-vms.md).
* Hora de instantâneo, que é o tempo necessário para acionar um instantâneo. Os instantâneos são acionados próximo da hora de cópia de segurança agendada.
* Tempo de espera de fila. Desde as cópia de segurança processos tarefas do serviço de vários clientes ao mesmo tempo, dados de instantâneos podem não imediatamente ser copiados para o Cofre dos serviços de recuperação. Em períodos de pico de carga, pode demorar até oito horas antes das cópias de segurança são processadas. No entanto, o tempo de cópia de segurança de VM total é inferior a 24 horas para políticas de cópia de segurança diárias.
Tempo total de cópia de segurança de menos de 24 horas é válido para cópias de segurança incrementais, mas não pode ser para a primeira cópia de segurança. Primeira hora de cópia de segurança é proporcional e pode ser superior a 24 horas, consoante o tamanho dos dados e quando é feita a cópia de segurança.
* Tempo de transferência de dados, o tempo necessário para o serviço de cópia de segurança calcular as alterações incrementais de cópia de segurança anterior e essas alterações de transferência para o Cofre de armazenamento.

### <a name="why-are-backup-times-longer-than-12-hours"></a>Por que são os tempos de backup de mais de 12 horas?

Cópia de segurança consiste em duas fases: instantâneos e a transferência de instantâneos para o cofre. Otimiza o serviço de cópia de segurança para armazenamento. Ao transferir dados de instantâneos para um cofre, o serviço apenas transfere as alterações incrementais partir do instantâneo anterior.  Para determinar as alterações incrementais, o serviço calcula a soma de verificação dos blocos. Se um bloco for alterado, o bloco é identificado como um bloco sejam enviados para o cofre. Em seguida, as explorações de serviço adicional em cada um dos blocos identificados, procurando por oportunidades minimizar os dados para transferir. Depois de avaliar todos os blocos alterados, o serviço une as alterações e envia-os para o cofre. Em alguns aplicativos herdados, as escritas pequenas, fragmentadas, não estão otimizadas para armazenamento. Se o instantâneo contém muitas gravações de pequenas e fragmentadas, o serviço gasta mais tempo a processar os dados escritos pelas aplicações. Para aplicações em execução dentro da VM, o bloco de recomendada de gravações de aplicativo mínimo é de 8 KB. Se o aplicativo usar um bloco de menos de 8 KB, o desempenho de cópia de segurança é afetado. Para obter ajuda com Ajuste seu aplicativo para melhorar o desempenho de cópia de segurança, consulte [otimização de aplicações para otimizar o desempenho com o armazenamento do Azure](../virtual-machines/windows/premium-storage-performance.md). Embora o artigo sobre o desempenho da cópia de segurança utiliza exemplos de armazenamento Premium, a documentação de orientação é aplicável para discos de armazenamento Standard.

## <a name="total-restore-time"></a>Tempo de restauração total

Uma operação de restauro consiste em duas tarefas principais: copiar dados do cofre para a conta de armazenamento do cliente escolhido e criar a máquina virtual. O tempo necessário para copiar dados a partir do cofre depende de onde as cópias de segurança são armazenadas no Azure e a localização da conta de armazenamento do cliente. Tempo decorrido para copiar dados depende de:
* Tempo de espera de fila - uma vez que os processos de serviço restaurar trabalhos a partir de vários clientes ao mesmo tempo, os pedidos de restauro são colocar numa fila.
* Cópia de dados de tempo - dados são copiados a partir do cofre para a conta de armazenamento do cliente. Restaurar tempo depende de IOPS e débito do serviço de cópia de segurança do Azure obtém na conta de armazenamento do cliente selecionado. Para reduzir o tempo de cópia durante o processo de restauração, selecione uma conta de armazenamento não carregá-lo com outras aplicações escritas e leituras.

## <a name="best-practices"></a>Melhores práticas

Sugerimos que seguindo essas práticas ao configurar cópias de segurança para todas as máquinas virtuais:

* Não agende cópias de segurança para mais de 10 VMs clássicas do mesmo serviço cloud, ao mesmo tempo. Se pretender criar uma cópia de segurança de várias VMs do mesmo serviço cloud, escalonar as horas de início de cópia de segurança por uma hora.
* Não agende cópias de segurança para mais de 100 VMs a partir de um cofre, ao mesmo tempo.
* Agende cópias de segurança VM durante o horário de pico. Desta forma o serviço de cópia de segurança utiliza o IOPS para a transferência de dados da conta de armazenamento do cliente para o cofre.
* Certifique-se de que as VMs do Linux ativado para a cópia de segurança, tem a Python versão 2.7 ou superior.

### <a name="best-practices-for-vms-with-unmanaged-disks"></a>Melhores práticas para as VMs com discos não geridos

As recomendações seguintes aplicam-se apenas a VMs com discos não geridos. Se as VMs utilizam discos geridos, o serviço de cópia de segurança processa todas as atividades de gestão de armazenamento.

* Certifique-se aplicar uma política de cópia de segurança para VMs espalhadas em várias contas de armazenamento. Não mais do que 20 discos total de uma única conta de armazenamento devem ser protegidos pela mesma agenda de cópia de segurança. Se tiver mais de 20 discos numa conta de armazenamento, distribuir essas VMs por várias políticas para obter o IOPS necessário durante a fase de transferência do processo de cópia de segurança.
* Não restaure uma VM em execução no armazenamento Premium para a mesma conta de armazenamento. Se o processo de operação de restauro coincide com a operação de cópia de segurança, reduz o IOPS disponíveis para cópia de segurança.
* Para cópia de segurança de Premium VM na pilha de cópia de segurança de VM V1, deve alocar apenas 50% de espaço de conta de armazenamento total para que o serviço de cópia de segurança possa copiar o instantâneo para a conta de armazenamento e transferência de dados da conta de armazenamento para o cofre.


## <a name="data-encryption"></a>Encriptação de dados
Cópia de segurança do Azure não encripta os dados como parte do processo de cópia de segurança. No entanto, pode encriptar dados dentro da VM e criar cópias de segurança dos dados protegidos de forma totalmente integrada (Leia mais sobre [cópia de segurança dos dados encriptados](backup-azure-vms-encryption.md)).

## <a name="calculating-the-cost-of-protected-instances"></a>Calcular o custo de instâncias protegidas
Máquinas virtuais do Azure, que são submetidas a backup através da cópia de segurança do Azure estão sujeitos aos [preços de cópia de segurança do Azure](https://azure.microsoft.com/pricing/details/backup/). O cálculo de instâncias protegidas baseia-se sobre o *real* tamanho da máquina virtual, que é a soma de todos os dados na máquina virtual, excluindo o armazenamento temporário.

Preços do backup das VMs não se baseia no tamanho máximo suportado para cada disco de dados anexado à máquina virtual. Preço baseia-se em dados reais armazenados no disco de dados. Da mesma forma, a fatura de armazenamento de cópia de segurança baseia-se na quantidade de dados que são armazenados em cópia de segurança do Azure, que é a soma dos dados reais em cada ponto de recuperação.

Por exemplo, dê uma máquina virtual com tamanho A2 padrão, que tem dois discos de dados adicionais com um tamanho máximo de 4 TB cada. A tabela seguinte fornece os dados reais armazenados em cada um destes discos:

| Tipo de disco | Tamanho máximo | Dados reais presentes |
| --------- | -------- | ----------- |
| Disco do sistema operativo |4095 GB |17 DE GB |
| Disco local / temporária disco |135 GB |5 GB (não incluído para cópia de segurança) |
| Disco de dados 1 |4095 GB |30 GB |
| Disco de dados 2 |4095 GB |0 GB |

Neste caso, o tamanho real da máquina virtual é 17 GB + 30 GB + 0 GB = 47 GB. Este tamanho de instância protegida (47 GB) torna-se a base para a fatura mensal. Pelo contrário em conformidade à medida que aumenta a quantidade de dados na máquina virtual, o tamanho de instância protegida utilizado para alterações de cobrança.

Faturação não inicia até concluir a primeira cópia de segurança com êxito. Neste momento, começa a faturação para armazenamento e instâncias protegidas. Faturação continua, desde que não há quaisquer dados de cópia de segurança armazenados num cofre para a máquina virtual. Se parar a proteção na máquina virtual, mas os dados de cópia de segurança da máquina virtual existem num cofre, continua a faturação.

A faturação de uma máquina virtual especificada será interrompido apenas se a proteção é parada e todos os dados de cópia de segurança é eliminada. Quando parar a proteção e não há nenhuma tarefa de cópia de segurança Active Directory, o tamanho do último backup bem-sucedido de VM torna-se o tamanho de instância protegida utilizado para a fatura mensal.

## <a name="questions"></a>Tem dúvidas?
Se tiver dúvidas ou se houver alguma funcionalidade que gostaria de ver incluída, [envie-nos comentários](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Passos Seguintes
* [Fazer uma cópia de segurança das máquinas virtuais](backup-azure-arm-vms.md)
* [Gerir cópias de segurança da máquina virtual](backup-azure-manage-vms.md)
* [Monitorizar máquinas virtuais](backup-azure-arm-restore-vms.md)
* [Resolver problemas de cópia de segurança de VMS](backup-azure-vms-troubleshoot.md)
