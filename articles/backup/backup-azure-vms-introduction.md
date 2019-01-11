---
title: Sobre a cópia de segurança de VM do Azure
description: Saiba mais sobre a cópia de segurança de VM do Azure e tenha em atenção algumas práticas recomendadas.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: raynew
ms.openlocfilehash: 67d81387a347bb2061457bfd24553f304e965f38
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198767"
---
# <a name="about-azure-vm-backup"></a>Sobre a cópia de segurança de VM do Azure

Este artigo descreve como o [serviço de cópia de segurança do Azure](backup-introduction-to-azure-backup.md) efetua uma cópia de segurança de VMs do Azure.

## <a name="backup-process"></a>Processo de cópia de segurança

Eis como cópia de segurança do Azure é concluída uma cópia de segurança para VMs do Azure.

1. Para as VMs do Azure que estão selecionadas para cópia de segurança, o serviço de cópia de segurança do Azure inicia uma tarefa de cópia de segurança em conformidade com o agendamento de cópia de segurança que especificar.
2. O serviço aciona a extensão de cópia de segurança.
    - Utilização de VMs do Windows a _VMSnapshot_ extensão.
    - VMs do Linux utilizar o _VMSnapshotLinux_ extensão.
    - A extensão é instalada durante a primeira cópia de segurança VM.
    - Para instalar a extensão, tem de executar a VM.
    - Se a VM não estiver em execução, o serviço Backup cria um instantâneo do armazenamento subjacente (uma vez que não ocorrem escritas da aplicação enquanto a VM está parada).
4. A extensão de cópia de segurança tira um instantâneo de consistente/ficheiro-consistente de falhas de nível de armazenamento.
5. Depois do instantâneo é tirado os dados são transferidos para o cofre. Para maximizar a eficiência, o serviço identifica e transfere apenas os blocos de dados que foram alterados desde a cópia de segurança anterior (o delta).
5. Quando a transferência de dados estiver concluída, o instantâneo é removido e é criado um ponto de recuperação.

![Arquitetura de cópia de segurança de máquina virtual do Azure](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="data-encryption"></a>Encriptação de dados

O Azure Backup não encriptam os dados como parte do processo de cópia de segurança. O Azure Backup suporta cópias de segurança de VMs do Azure são encriptadas com o Azure Disk Encryption.

- Cópia de segurança de VMs encriptadas com Key(BEK) de encriptação do Bitlocker só e é suportada a BEK em conjunto com a chave de encriptação Key(KEK), geridos e das VMs do Azure.
- O BEK(secrets) e KEK(keys) uma cópia de segurança são encriptados para que possam ser lidos e utilizados apenas quando restaurada para o Cofre de chaves, os utilizadores autorizados.
- Uma vez que a BEK é também uma cópia de segurança, em cenários em que a BEK é perdido, os utilizadores autorizados podem restaurar a BEK para o Cofre de chaves e recuperar a VM encriptada. Chaves e segredos de VMs encriptadas são uma cópia de segurança no formato encriptado, pelo que nem os utilizadores não autorizados nem para o Azure pode ler ou utilize uma cópia de chaves e segredos de segurança. Apenas os utilizadores com o nível certo de permissões podem criar cópias de segurança e restaurar VMs encriptadas, bem como as chaves e segredos.

## <a name="snapshot-consistency"></a>Consistência de instantâneo

Para tirar instantâneos enquanto aplicações estão em execução, o Azure Backup tira instantâneos consistentes com a aplicação.

- **VMs do Windows**: Para VMs do Windows, o serviço de cópia de segurança, as coordenadas com o serviço do Volume de cópia de sombra (VSS) para obter um instantâneo consistente com os discos de VM.
    - Por predefinição, o Azure Backup permite cópias de segurança completas do VSS. [Saiba mais](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx).
    - Se pretender alterar a definição para que as cópias de segurança do Azure utiliza cópias de segurança de cópia VSS, defina a seguinte chave de registo:
        ```
        [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
        ""USEVSSCOPYBACKUP"="TRUE"
        ```
- **VMs do Linux**: Para garantir que suas VMs do Linux são consistentes com a aplicação quando o Azure Backup tira um instantâneo, pode utilizar a estrutura de script anterior e script posterior do Linux. Pode escrever seus próprios scripts personalizados para garantir a consistência quando um instantâneo de VM.
    -  Cópia de segurança do Azure apenas invoca o Pre- e pós-scripts, escritos por.
    - Se o script prévio e os pós-scripts de executam com êxito, o Azure Backup marca o ponto de recuperação consistentes com aplicações como. No entanto, está, por fim, responsável pela consistência de aplicação ao utilizar scripts personalizados.
    - [Saiba mais](backup-azure-linux-app-consistent.md) sobre a configuração de scripts.


#### <a name="consistency-types"></a>Tipos de consistência

A tabela seguinte explica os diferentes tipos de consistência.

**instantâneo** | **Com base em VSS** | **Detalhes** | **Recuperação**
--- | --- | --- | ---
**Consistentes com aplicações** | Sim (apenas Windows) | As cópias de segurança consistente com a aplicação capturam a memória conteúda e operações de e/s pendentes. Instantâneos consistentes com a aplicação utilizam o escritor VSS (ou script prévio/posterior para Linux) que assegurar a consistência dos dados da aplicação antes de ocorre uma cópia de segurança. | Ao recuperar com um instantâneo consistente com a aplicação, a VM arranca. Não existe danos em dados ou perda. Os aplicativos começam num estado consistente.
**Consistente do sistema de ficheiros** | Sim (apenas Windows) |  Cópias de segurança de ficheiros fornecem cópias de segurança de ficheiros de disco através de um instantâneo de todos os ficheiros ao mesmo tempo.<br/><br/> Pontos de recuperação de cópia de segurança do Azure são ficheiros consistente para:<br/><br/> -Linux VMs cópias de segurança que não tenham scripts pré/pós ou que tem o script que falhou.<br/><br/> -Cópias de segurança de VM do Windows onde o VSS falhou. | Ao recuperar com um instantâneo consistente com ficheiros, a VM arranca. Não existe danos em dados ou perda. As aplicações necessitam implementar seu próprio mecanismo de "correção de segurança" para se certificar de que os dados restaurados são consistentes.
**Consistentes de falhas** | Não | Consistência de falhas sempre acontece quando uma VM do Azure é desligado no momento da cópia de segurança.  Apenas os dados que já existe no disco no momento da cópia de segurança são capturados e uma cópia de segurança.<br/><br/> Um ponto de recuperação consistentes com falhas não garante a consistência de dados para o sistema operativo ou a aplicação. | Há garantias de nenhum, mas normalmente as inicializações VM e forma com um disco de verificação para corrigir erros de danos. Quaisquer dados na memória ou de escrita que não foram transferida para o disco são perdidos. Aplicações implementam a verificação de seus próprios dados. Por exemplo, para uma aplicação de base de dados, se um registo de transações tem entradas que não estão na base de dados, o software de base de dados agrega até que os dados são consistentes.


## <a name="service-and-subscription-limits"></a>Limites de serviço e subscrição

O Azure Backup tem um número de limites em torno de subscrições e cofres.

[!INCLUDE [azure-backup-limits](../../includes/azure-backup-limits.md)]


## <a name="backup-performance"></a>Desempenho de cópia de segurança

### <a name="disk-considerations"></a>Considerações de disco

Operação de cópia de segurança otimiza fazendo o backup de cada um disco da VM em paralelo. Por exemplo, se uma VM tem quatro discos, o serviço tenta fazer cópias de segurança de todos os discos de quatro em paralelo. Para cada disco a cópia de segurança, cópia de segurança do Azure lê os blocos no disco e armazena apenas os dados alterados (cópia de segurança incremental).


### <a name="scheduling-considerations"></a>Considerações da programação

Agendamento de cópia de segurança tem impacto no desempenho.

- Se configurar as políticas, para que todas as VMs são uma cópia de segurança ao mesmo tempo, tiver agendado uma confusão de tráfego, como o processo de cópia de segurança tenta fazer cópias de segurança de todos os discos em paralelo.
- Para reduzir o tráfego de cópia de segurança, cópia de segurança diferentes VMs em diferente altura do dia, sem sobreposição.


### <a name="time-considerations"></a>Considerações de tempo

Embora mais de cópia de segurança tempo é gasto leitura e a cópia de dados, adicionar outras operações para o tempo total necessário para fazer uma cópia de segurança de uma VM:

- **Instalar a extensão de cópia de segurança**: Tempo necessário para instalar ou atualizar a extensão de cópia de segurança.
- **Instantâneo de Acionador**: Tempo decorrido para acionar um instantâneo. Os instantâneos são acionados próximo da hora de cópia de segurança agendada.
- **Tempo de espera na fila**: Desde as cópia de segurança processos tarefas do serviço de várias contas de armazenamento de clientes ao mesmo tempo, dados de instantâneos podem não imediatamente ser copiados para o Cofre dos serviços de recuperação. Em períodos de pico de carga, pode demorar até oito horas antes das cópias de segurança são processadas. No entanto, o tempo de cópia de segurança de VM total é inferior a 24 horas para políticas de cópia de segurança diárias.
- **Cópia de segurança inicial**: Embora o tempo de cópia de segurança total de menos de 24 horas é válido para cópias de segurança incrementais, pode não ser para a primeira cópia de segurança. Tempo necessário dependerá de tamanho dos dados e quando é feita a cópia de segurança.
- **Tempo de transferência de dados**: Tempo necessário para o serviço de cópia de segurança calcular as alterações incrementais de cópia de segurança anterior e essas alterações de transferência para o Cofre de armazenamento.

### <a name="factors-affecting-backup-time"></a>Fatores que afetam o tempo de cópia de segurança

Cópia de segurança consiste em duas fases, instantâneos e a transferência de instantâneos para o cofre. Otimiza o serviço de cópia de segurança para armazenamento.

- Ao transferir dados de instantâneos para um cofre, o serviço apenas transfere as alterações incrementais partir do instantâneo anterior.
- Para determinar as alterações incrementais, o serviço calcula a soma de verificação dos blocos.
    - Se um bloco for alterado, o bloco é identificado como um bloco sejam enviados para o cofre.
    - As explorações de serviço adicional em cada um dos blocos identificados, procurando por oportunidades minimizar os dados a transferir.
    - Depois de avaliar todos os blocos alterados, o serviço une as alterações e envia-os para o cofre.

Situações que podem afetar o tempo de cópia de segurança incluem o seguinte:

- **Cópia de segurança inicial para um disco adicionado recentemente a uma VM já protegido**: Se uma VM está em cópia de segurança incremental e um disco novo é adicionado a esta VM, a duração de cópia de segurança pode ir além de 24 horas, uma vez que o disco adicionado recentemente tem de passar por uma replicação inicial, juntamente com a replicação de diferenças de discos existentes.
- **Fragmentação**: Produto de cópia de segurança verifica a existência de alterações incrementais entre as operações de duas cópias de segurança. Operações de cópia de segurança são mais rápidas quando as alterações no disco sejam colocadas em comparação com as alterações são spread entre, em seguida, o disco. 
- **Alterações a dados**: Volume de alterações diária (para a replicação incremental) por mais de 200 GB de disco pode tirar maior do que cerca de 8 horas para concluir a operação. Se a VM tem mais de um disco e um destes discos está a demorar mais tempo a cópia de segurança, em seguida, ela pode afetar a operação de cópia de segurança global (ou pode resultar numa falha). 
- **Modo de comparação (CC) de soma de verificação**: O modo CC é comparativamente mais lento do que o modo otimizado utilizado pela RP instantânea. Se já estiver a utilizar o RP instantânea e ter eliminado os instantâneos de escalão 1, em seguida, cópia de segurança muda para o modo CC, fazendo com que a operação de cópia de segurança ter mais de 24 horas (ou falha).

## <a name="restore-considerations"></a>Restaurar considerações

Uma operação de restauro consiste em duas tarefas principais: copiar dados do cofre para a conta de armazenamento escolhida e criar a máquina virtual. O tempo necessário para copiar dados a partir do cofre depende de onde as cópias de segurança são armazenadas no Azure e a localização da conta de armazenamento. Tempo decorrido para copiar dados depende de:

- **Tempo de espera na fila**: Uma vez que os processos de serviço restaurar trabalhos a partir de várias contas de armazenamento ao mesmo tempo, os pedidos de restauro são colocados numa fila.
- **Tempo de cópia de dados**: Dados são copiados do cofre para a conta de armazenamento. Restaurar tempo depende de IOPS e débito da conta de armazenamento selecionada, o que utiliza o serviço de cópia de segurança do Azure. Para reduzir o tempo de cópia durante o processo de restauração, selecione uma conta de armazenamento não carregá-lo com outras aplicações escritas e leituras.

## <a name="best-practices"></a>Melhores práticas

Sugerimos que seguindo essas práticas ao configurar cópias de segurança VM:

- Considere modificar o tempo de política que está predefinido (para ex. Se o seu tempo de política predefinida é 12:00 AM considere incrementando-o em minutos) quando os instantâneos de dados são tomados para garantir a recursos são utilizados de forma otimizada.
- Para a Premium VM a cópia de segurança no recurso de não - instantâneas RP aloca aproximadamente 50% de espaço de conta de armazenamento total. Serviço de cópia de segurança requer este espaço para copiar o instantâneo para a mesma conta de armazenamento e para transferi-los para o cofre.
- Para restaurar VMs a partir de um único cofre, é altamente recomendável usar diferentes [contas de armazenamento v2](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) para garantir que a conta de armazenamento de destino não é limitada. Por exemplo, cada VM tem de ter conta de armazenamento diferente (se as 10 VMs são restauradas, em seguida, considere a utilização de 10 contas de armazenamento diferente).
- A restauração da camada de armazenamento de escalão 1 (instantâneo) será concluída em minutos (uma vez que é a mesma conta de armazenamento) em relação a camada de armazenamento de nível 2 (cofre) que pode demorar horas. Recomendamos que utilize [restaurar instantâneas](backup-instant-restore-capability.md) funcionalidade para restauros mais rápidos, caso em que os dados estão disponíveis na camada 1 (que tem os dados a ser restaurada do cofre, em seguida, irá demorar tempo).
- O limite do número de discos por conta de armazenamento é relativo ao peso os discos estão a ser acedidos por aplicações em execução na VM de IaaS. Certifique-se de que se vários discos estão alojados numa única conta de armazenamento. Como uma prática geral, se os discos de 5 a 10 ou mais estiverem presentes numa única conta de armazenamento, balancear a carga ao mover alguns discos para separar as contas de armazenamento.

## <a name="backup-costs"></a>Custos de cópia de segurança

VMs do Azure, uma cópia de segurança com o Azure Backup estão sujeitos aos [preços de cópia de segurança do Azure](https://azure.microsoft.com/pricing/details/backup/).

- Faturação não inicia até concluir a primeira cópia de segurança com êxito. Neste momento, começa a faturação para armazenamento e instâncias protegidas.
- Faturação continua, desde que não há quaisquer dados de cópia de segurança armazenados num cofre para a máquina virtual. Se parar a proteção na máquina virtual, mas os dados de cópia de segurança da máquina virtual existem num cofre, continua a faturação.
- Deixa de faturação de uma VM especificada apenas se a proteção é parada e todos os dados de cópia de segurança é eliminada.
- Quando parar a proteção e não há nenhuma tarefa de cópia de segurança Active Directory, o tamanho do último backup bem-sucedido de VM torna-se o tamanho de instância protegida utilizado para a fatura mensal.
- O cálculo de instâncias protegidas baseia-se sobre o *real* tamanho da máquina virtual, que é a soma de todos os dados na máquina virtual, excluindo o armazenamento temporário.
- Preço baseia-se em dados reais armazenados no disco de dados.
- Preços do backup das VMs não se baseia no tamanho máximo suportado para cada disco de dados anexado à máquina virtual.
- Da mesma forma, a fatura de armazenamento de cópia de segurança baseia-se na quantidade de dados que são armazenados em cópia de segurança do Azure, que é a soma dos dados reais em cada ponto de recuperação.

Por exemplo, dê uma máquina virtual com tamanho A2 padrão, que tem dois discos de dados adicionais com um tamanho máximo de 4 TB cada. A tabela seguinte fornece os dados reais armazenados em cada um destes discos:

| Tipo de disco | Tamanho máximo | Dados reais presentes |
| --------- | -------- | ----------- |
| Disco do sistema operativo |4095 GB |17 DE GB |
| Disco local / temporária disco |135 GB |5 GB (não incluído para cópia de segurança) |
| Disco de dados 1 |4095 GB |30 GB |
| Disco de dados 2 |4095 GB |0 GB |

- Neste caso, o tamanho real da máquina de virtual VM é 17 GB + 30 GB + 0 GB = 47 GB.
- Este tamanho de instância protegida (47 GB) torna-se a base para a fatura mensal.
- Pelo contrário em conformidade à medida que aumenta a quantidade de dados na máquina virtual, o tamanho de instância protegida utilizado para alterações de cobrança.


## <a name="next-steps"></a>Passos Seguintes

Depois de rever o processo de cópia de segurança e considerações de desempenho, faça o seguinte:

- [Saiba mais sobre](../virtual-machines/windows/premium-storage-performance.md) otimização de aplicações para otimizar o desempenho com o armazenamento do Azure. O foco do artigo no armazenamento premium, mas também é aplicável para discos de armazenamento standard.
- [Introdução ao](backup-azure-arm-vms-prepare.md) com cópia de segurança ao rever as limitações e suporte VM, criar um cofre e preparar para cópia de segurança de VMs.
