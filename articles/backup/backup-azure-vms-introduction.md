---
title: Sobre a cópia de segurança de VM do Azure
description: Saiba mais sobre a cópia de segurança de VM do Azure e tenha em atenção algumas práticas recomendadas.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: raynew
ms.openlocfilehash: 9a80671a72f059e24a8cebc5de803af9261ad829
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53743957"
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
4. A extensão de cópia de segurança tira um instantâneo consistente com a aplicação, de nível de armazenamento.
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
**Consistente do sistema de ficheiros** | Sim (apenas Windows) |  Cópias de segurança de ficheiros fornecem cópias de segurança de ficheiros de disco através de um instantâneo de todos os ficheiros ao mesmo tempo.<br/><br/> Pontos de recuperação de cópia de segurança do Azure são ficheiros consistente para:<br/><br/> -Linux VMs cópias de segurança que não tenham scripts pré/pós ou que tem o script que falhou.<br/><br/> -Cópias de segurança de VM do Windows onde o VSS falhou. | Ao recuperar com um instantâneo consistente com ficheiros, a VM arranca. Não existe danos em dados ou perda. Aplicações tem de implementar seu próprio mecanismo de "correção de segurança" para se certificar de que os dados restaurados são consistentes.
**Consistentes de falhas** | Não | Consistência de falhas sempre acontece quando uma VM do Azure é desligado no momento da cópia de segurança.  Apenas os dados que já existe no disco no momento da cópia de segurança são capturados e uma cópia de segurança.<br/><br/> Um ponto de recuperação consistentes com falhas não garante a consistência de dados para o sistema operativo ou a aplicação. | Há garantias de nenhum, mas normalmente as inicializações VM e forma com um disco de verificação para corrigir erros de danos. Quaisquer dados na memória ou de escrita que não foram transferida para o disco são perdidos. Aplicações implementam a verificação de seus próprios dados. Por exemplo, para uma aplicação de base de dados, se um registo de transações tem entradas que não estão na base de dados, o software de base de dados agrega até que os dados são consistentes.


## <a name="service-and-subscription-limits"></a>Limites de serviço e subscrição

O Azure Backup tem um número de limites em torno de subscrições e cofres.

[!INCLUDE [azure-backup-limits](../../includes/azure-backup-limits.md)]


## <a name="backup-performance"></a>Desempenho de cópia de segurança

### <a name="disk-considerations"></a>Considerações de disco

Cópia de segurança tenta concluir o mais rápido possível, consumindo a quantidade de recursos que ele pode.

- Numa tentativa de maximizar a velocidade do mesmo, o processo de cópia de segurança tenta fazer cópias de segurança a cada um dos discos da VM em paralelo.
- Por exemplo, se uma VM tem quatro discos, o serviço tenta fazer cópias de segurança de todos os discos de quatro em paralelo.
- O número de discos a cópia de segurança é o fator mais importante na determinação de tráfego de cópia de segurança de conta de armazenamento.
- Todas as operações de e/s estão limitadas pela *taxa de transferência de destino para o Blob único*, que tem um limite de 60 MB por segundo.
- Para cada disco a cópia de segurança, cópia de segurança do Azure lê os blocos no disco e armazena apenas os dados alterados (cópia de segurança incremental). Pode utilizar os valores de débito médio abaixo para estimar a quantidade de tempo necessária para fazer backup de um disco de um determinado tamanho.

    **Operação** | **Melhor débito**
    --- | ---
    Cópia de segurança inicial | 160 Mbps |
    Cópia de segurança incremental | 640 Mbps <br><br> Débito cai significativamente se os dados delta estão dispersos entre o disco.|



### <a name="scheduling-considerations"></a>Considerações da programação

Agendamento de cópia de segurança tem impacto no desempenho.

- Se configurar as políticas, para todas as VMs são uma cópia de segurança ao mesmo tempo, tiver agendado uma confusão de tráfego, como o o processo de cópia de segurança tenta fazer cópias de segurança de todos os discos em paralelo.
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


- **Cópia de segurança inicial para um disco adicionado recentemente a uma VM já protegido**: Se uma VM está passando por cópia de segurança incremental, quando é adicionado um novo disco, em seguida, a cópia de segurança pode perder o SLA de um dia, dependendo do tamanho do disco novo.
- **Aplicação fragmentada**: Se uma aplicação está mal configurada pode não ser ideal para o armazenamento:
    - Se o instantâneo contém muitas gravações de pequenas e fragmentadas, o serviço gasta mais tempo a processar os dados escritos pelas aplicações.
    - Para aplicações em execução dentro da VM, o bloco de recomendada de gravações de aplicativo mínimo é de 8 KB. Se o aplicativo usar um bloco de menos de 8 KB, o desempenho de cópia de segurança é afetado.
- **Conta de armazenamento sobrecarregada**: Foi possível agendar uma cópia de segurança quando a aplicação está em execução na produção, ou se tiver mais do que cinco a dez discos estão alojados na mesma conta de armazenamento.
- **Modo de verificação (CC) de consistência**: Para discos superiores a 1TB de discos a cópia de segurança pode ser no modo CC para vários motivos:
    - Move o disco gerido como parte do reinício da VM.
    - Promove o instantâneo de blob de base.


## <a name="restore-considerations"></a>Restaurar considerações

Uma operação de restauro consiste em duas tarefas principais: copiar dados do cofre para a conta de armazenamento escolhida e criar a máquina virtual. O tempo necessário para copiar dados a partir do cofre depende de onde as cópias de segurança são armazenadas no Azure e a localização da conta de armazenamento. Tempo decorrido para copiar dados depende de:

- **Tempo de espera na fila**: Uma vez que os processos de serviço restaurar trabalhos a partir de várias contas de armazenamento ao mesmo tempo, os pedidos de restauro são colocados numa fila.
- **Tempo de cópia de dados**: Dados são copiados do cofre para a conta de armazenamento. Restaurar tempo depende de IOPS e débito de conta de armazenamento selecionada que utiliza o serviço de cópia de segurança do Azure. Para reduzir o tempo de cópia durante o processo de restauração, selecione uma conta de armazenamento não carregá-lo com outras aplicações escritas e leituras.

## <a name="best-practices"></a>Melhores práticas

Sugerimos que seguindo essas práticas ao configurar cópias de segurança VM:

- Não agende cópias de segurança para mais de 100 VMs a partir de um cofre, ao mesmo tempo.
- Agende cópias de segurança VM durante o horário de pico. Desta forma o serviço de cópia de segurança utiliza o IOPS para a transferência de dados da conta de armazenamento para o cofre.
- Se estiver fazendo backup discos geridos, o serviço de cópia de segurança do Azure processa a gestão de armazenamento. Se estiver fazendo o backup discos não geridos:
    - Certifique-se aplicar uma política de cópia de segurança para VMs espalhadas em várias contas de armazenamento.
    - Não mais de 20 discos a partir de uma única conta de armazenamento devem ser protegidos pela mesma agenda de cópia de segurança.
    - Se tiver mais de 20 discos numa conta de armazenamento, distribuir essas VMs por várias políticas para obter o IOPS necessário durante a fase de transferência do processo de cópia de segurança.
    - Não restaure uma VM em execução no armazenamento premium para a mesma conta de armazenamento. Se o processo de operação de restauro coincide com a operação de cópia de segurança, reduz o IOPS disponíveis para cópia de segurança.
    - Para cópia de segurança de Premium VM na pilha de cópia de segurança de VM V1, deve alocar apenas 50% de espaço de conta de armazenamento total para que o serviço de cópia de segurança possa copiar o instantâneo para a conta de armazenamento e transferência de dados da conta de armazenamento para o cofre.
- Recomenda-se para utilizar contas de armazenamento diferentes em vez de usar as mesmas contas de armazenamento para restaurar VMs a partir de um único cofre. Isso evitar a limitação e resultar no sucesso do restauro de 100% com bom desempenho.
- A restauração da camada de armazenamento 1 do escalão serão concluídas em minutos em relação as restaurações de armazenamento de camada 2 que demora algumas horas. Recomendamos que utilize [funcionalidade de RP instantâneas](backup-upgrade-to-vm-backup-stack-v2.md) para restauros mais rápidos. Este é apenas aplicável gerido VMs do Azure.


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

- Transfira o [planilha do Excel de planeamento de capacidade](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) para experimentar o disco e números de agendamento de cópia de segurança.
- [Saiba mais sobre](../virtual-machines/windows/premium-storage-performance.md) otimização de aplicações para otimizar o desempenho com o armazenamento do Azure. O foco do artigo no armazenamento premium, mas também é aplicável para discos de armazenamento standard.
- [Introdução ao](backup-azure-arm-vms-prepare.md) com cópia de segurança ao rever as limitações e suporte VM, criar um cofre e preparar para cópia de segurança de VMs.
