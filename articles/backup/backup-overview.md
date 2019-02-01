---
title: O que é o Backup do Azure?
description: Fornece uma descrição geral do serviço de cópia de segurança do Azure e como implementá-la como parte da sua estratégia de recuperação (BCDR) de desastre e continuidade empresariais.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 298c9fabca9d1994e0b952fdf8b48b70370c3ec2
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490675"
---
# <a name="what-is-azure-backup"></a>O que é o Backup do Azure?

O serviço de cópia de segurança do Azure faz o backup de dados para a cloud do Microsoft Azure. Pode fazer backup de máquinas no local e cargas de trabalho e máquinas virtuais do Azure (VMs).


## <a name="why-use-azure-backup"></a>Porquê utilizar o Backup do Azure?

O Azure Backup fornece estas principais vantagens:

- **A descarga de cópia de segurança no local**: O Azure Backup oferece uma solução simple para cópia de segurança de seus recursos no local para a cloud. Obtenha cópia de segurança de curta e longo prazo, sem a necessidade de implementar complexas soluções no local cópia de segurança. Elimine a necessidade de banda ou uma cópia de segurança externa.
- **Cópia de segurança de VMs IaaS do Azure**: O Azure Backup fornece cópias de segurança de forma independentes e isoladas para proteger contra destruição acidental de dados originais. As cópias de segurança são armazenadas num cofre dos serviços de recuperação com interna gerenciada de pontos de recuperação. Configuração e a escalabilidade é simples, as cópias de segurança são otimizadas e pode facilmente restaurar conforme necessário.
- **Dimensione facilmente** - cópia de segurança do Azure utiliza o poder subjacente e o dimensionamento ilimitado da nuvem do Azure para fornecer elevada disponibilidade - com nenhuma manutenção ou monitorização sobrecarga. Pode configurar alertas para fornecer informações sobre eventos, mas não precisa de se preocupar com a elevada disponibilidade dos seus dados na nuvem.
- **Obter a transferência de dados ilimitada** - cópia de segurança do Azure não limita a quantidade de entrada ou saídos de transferência de dados. O Azure Backup também não cobra por dados que sejam transferidos. No entanto, se utilizar o serviço Importar/Exportar do Azure para importar grandes quantidades de dados, existe um custo associado aos dados de entrada. Para mais informações sobre este custo, veja [Fluxo de trabalho de cópia de segurança offline no Azure Backup](backup-azure-backup-import-export.md). Os dados de saída são os dados transferidos a partir de um cofre dos Serviços de Recuperação durante uma operação de restauro.
- **Manter os dados seguros**: Encriptação de dados permite a transmissão segura e o armazenamento dos seus dados na cloud pública. A frase de acesso de encriptação é armazenada por si localmente, e nunca é transmitida ou armazenada no Azure. Se for necessário restaurar quaisquer dados, é o único a ter a frase de acesso ou a chave de encriptação.
- **Obter cópias de segurança consistente com a aplicação**: Uma cópia de segurança consistentes com aplicações significa que um ponto de recuperação tem todos os dados necessários para restaurar a cópia de segurança. O Azure Backup fornece cópias de segurança consistentes com as aplicações, o que garante que não são necessárias correções adicionais para restaurar os dados. Restaurar dados consistentes com as aplicações reduz o tempo de restauro, permitindo-lhe voltar rapidamente a um estado de execução.
- **Obter a retenção de curta e longo prazo**: **Retenção a longo prazo** – Pode utilizar os cofres dos Serviços de Recuperação para a retenção de dados a curto e longo prazo. O Azure não limita o período de tempo durante o qual os dados podem permanecer num cofre dos Serviços de Recuperação. Pode manter os dados num cofre o tempo que pretender. O Azure Backup tem um limite de 9999 pontos de recuperação por instância protegida. Consulte a secção [Backup and retention (Cópia de segurança e retenção)](backup-introduction-to-azure-backup.md#backup-and-retention) neste artigo para obter uma explicação sobre como este limite pode afetar as suas necessidades de cópia de segurança.
- **Gestão de armazenamento automática** - os ambientes híbridos necessitam frequentemente de armazenamento heterogéneo - alguns no local e outros na nuvem. Com o Azure Backup, não existe nenhum custo para a utilização de dispositivos de armazenamento no local. O Azure Backup atribui automaticamente e gere o armazenamento de cópia de segurança, e utiliza um modelo de pagamento enquanto utiliza. O pagamento enquanto utiliza significa que paga apenas pelo armazenamento que consumir. Para mais informações, veja o artigo [Preços do Azure](https://azure.microsoft.com/pricing/details/backup).
- **Várias opções de armazenamento** - um aspeto de elevada disponibilidade é a replicação do armazenamento. O Azure Backup oferece dois tipos de replicação: [armazenamento localmente redundante](../storage/common/storage-redundancy-lrs.md) e [armazenamento georredundante](../storage/common/storage-redundancy-grs.md). Escolha a opção de armazenamento de cópia de segurança com base na necessidade:
    - O armazenamento localmente redundante (LRS) replica os seus dados três vezes (cria três cópias dos dados) numa unidade de escala de armazenamento de um datacenter. Todas as cópias dos dados existem na mesma região. O LRS é uma opção de baixo custo para proteger os dados contra falhas de hardware locais.
    - O armazenamento georredundante (GRS) é a predefinição e a opção de replicação recomendada. O GRS replica os dados para uma região secundária (a centenas de quilómetros da localização primária da origem de dados). O GRS custa mais do que o LRS, mas o GRS proporciona um nível mais elevado de durabilidade aos seus dados, mesmo se ocorrer uma indisponibilidade regional.


## <a name="whats-the-difference-between-azure-backup-and-azure-site-recovery"></a>O que é a diferença entre a cópia de segurança do Azure e o Azure Site Recovery?

Os serviços da cópia de segurança do Azure e o Azure Site Recovery contribuem para uma estratégia de recuperação (BCDR) de desastres e continuidade do negócio da sua empresa. BCDR consiste em dois objetivos gerais:

- Mantenha os dados empresariais seguros e recuperáveis quando ocorrem falhas.
- Mantenha as suas aplicações e cargas de trabalho em funcionamento durante os tempos de inatividade planejados quanto não planejados.

Ambos os serviços fornecem funcionalidades diferentes mas complementares.

- **Azure Site Recovery**: Recuperação de sites fornece uma solução de recuperação após desastre para máquinas no local e para as VMs do Azure. Replicar máquinas a partir de uma localização primária para uma secundária. Momentos de desastre, pós-falha de máquinas para a localização secundária e aceder aos mesmos a partir daí. Quando tudo estiver em execução normalmente novamente, falhar máquinas back para recuperá-los no site primário.
- **O Azure Backup**: O serviço de cópia de segurança do Azure faz uma cópia dos dados a partir de máquinas no local e VMs do Azure. Dados podem ser uma cópia de segurança e recuperar num nível granular, incluindo a cópia de segurança de ficheiros, pastas, estado do sistema de máquina e cópia de segurança de dados com suporte para a aplicação. Cópia de segurança do Azure processa dados num nível mais granular do que o Site Recovery. Por exemplo, se uma apresentação no computador portátil se danificasse, pode utilizar o Azure Backup para restaurar a apresentação. Se quiser manter uma configuração de VM e os dados seguros e acessível, poderia usar o Site Recovery.  

Utilize os pontos de tabela para ajudar a descobrir as suas necessidades BCDR. 

**Objetivo** | **Detalhes** | **Comparação**
--- | --- | --- | --- |
**Cópia de segurança/retenção de dados** | Dados de cópia de segurança podem ser mantidos e armazenados durante dias, meses ou até anos a fio se for necessário numa perspetiva de conformidade. | Soluções de cópia de segurança, como a cópia de segurança do Azure permitem-lhe escolher os dados que pretende criar cópias de segurança e fazer um ajuste as políticas de cópia de segurança e retenção.<br/><br/> Recuperação de sites não permite a mesma ajuste fino.
**Objetivo de ponto de recuperação (RPO)** | A quantidade de perda de dados aceitável se uma recuperação tiver de ser efetuada. | As cópias de segurança têm mais variável RPO.<br/><br/> Cópias de segurança VM normalmente têm um RPO de um dia, enquanto as cópias de segurança da base de dados têm RPOs tão baixos como 15 minutos.<br/><br/> Recuperação de sites fornece um RPO baixo, uma vez que a replicação é contínua ou com frequência, para que o delta entre a cópia de origem e de réplica é pequeno.
**Objetivo de tempo de recuperação (RTO)** |A quantidade de tempo que demora a concluir uma recuperação ou um restauro. | Devido ao RPO maior, a quantidade de dados que uma solução de cópia de segurança necessita para processar é, normalmente, muito mais elevada, o que resulta em RTOs maiores. Por exemplo, pode demorar dias para restaurar dados de bandas, consoante o tempo que demora a transportar a banda a partir de uma localização fora das instalações. | Soluções de recuperação após desastre, como a recuperação de Site têm um RPO baixo, uma vez que a replicação contínua/frequentes, geralmente, significa que o destino é mais elevada sincronizado com a origem. |

## <a name="what-backup-scenarios-are-supported"></a>Quais cenários de cópia de segurança são suportados?

Cópia de segurança do Azure pode fazer uma cópia de segurança de máquinas no local e VMs do Azure.

**Machine** | **Criar cópias de segurança cenário**
--- | ---
**Máquinas no local (físicos/virtuais)** |  Pode criar cópias de segurança individuais máquinas no local.<br/><br/>Pode fazer uma cópia de segurança de máquinas no local que estão protegidas pelo System Center Data Protection Manager (DPM)<br/><br/> Pode fazer backup de máquinas no local protegidas pelo Microsoft Azure Backup Server (MABS)....
**VMs do Azure** | Pode fazer backup de VMs do Azure individuais.<br/><br/> Pode fazer uma cópia de segurança de VMs do Azure protegidos pelo DPM ou MABS.

### <a name="back-up-servers"></a>Fazer backup de servidores

Pode querer servidores no local e cargas de trabalho, ou VMs do Azure e as cargas de trabalho, cópia de segurança pela primeira vez a um servidor de cópia de segurança e, em seguida, para dos serviços de recuperação cofre. 

**Servidor de cópia de segurança** | **Detalhes**
--- | ---
**O System Center Data Protection Manager (DPM)** | Pode utilizar o Azure Backup para fazer backup de dados protegidos pelo DPM:<br/><br/> – O DPM pode estar em execução no local (físico ou virtual) ou no Azure.<br/><br/> -Pode proteger diferentes tipos de dados em execução em máquinas no local e VMs do Azure através de segurança os dados para o servidor do DPM.<br/><br/> Por sua vez o servidor do DPM pode ser copiado para um cofre de serviços de recuperação utilizando o serviço de cópia de segurança do Azure.<br/><br/> O servidor DPM e as máquinas que protege tem de ser na mesma rede. As máquinas no local só podem ser protegidas por um servidor do DPM no local. Da mesma forma, as VMs do Azure só pode ser protegidas pelo DPM em execução no Azure.
**Servidor de cópia de segurança do Microsoft Azure (MABS)** | Pode utilizar o Azure Backup para fazer uma cópia dos dados protegidos pelo MABS:<br/><br/> -MABS pode estar em execução no local (físico ou virtual) ou no Azure.<br/><br/> -É proteger diferentes tipos de dados em execução em máquinas no local e VMs do Azure através de segurança de dados até MABS.<br/><br/> -Por sua vez MABS podem ser copiadas para um cofre de serviços de recuperação utilizando o serviço de cópia de segurança do Azure.<br/><br/> -MABS fornece uma funcionalidade semelhante ao DPM, exceto pelo fato de não é possível criar cópias de segurança em banda com o MABS. MABS não requer uma licença do System Center.<br/><br/> Como o DPM, as máquinas no local só podem ser protegidas por um MABS no local. VMs do Azure só podem ser protegidas por um MABS no Azure.

As vantagens de fazer cópias de segurança primeiro para o DPM/MABS e, em seguida, para um cofre são os seguintes:

- O backup em DPM/MAB fornece cópias de segurança com suporte para a aplicação otimizadas para aplicações comuns, como o SQL Server, Exchange e SharePoint, no adicionais para cópias de segurança do ficheiro/pasta/volume e backups de estado do computador (bare-metal, o estado do sistema).
- Não precisa de instalar o agente de cópia de segurança do Azure em cada máquina que pretende criar cópias de segurança. Cada máquinas executa o agente de proteção do DPM/MABS e o agente de serviços de recuperação do Azure Backup Microsoft Azure é executado sobre o DPM server/MABS apenas.
- Tem mais flexibilidade e opções de agendamento granulares para a execução de cópias de segurança.
- Pode gerir as cópias de segurança para várias máquinas que agrupa em grupos de proteção numa única consola. Isto é particularmente útil quando as aplicações são dispostos em camadas através de várias máquinas e pretende que o backup deles em conjunto.

## <a name="what-can-be-backed-up"></a>O que pode ser uma cópia de segurança?

**Machine** | **Servidor de cópia de segurança** | **Fazer cópia de segurança**
--- | --- | ---
VMs do Windows no local | Não uma cópia de segurança para DPM ou MABS | Cópia de segurança de ficheiros, pastas, estado do sistema.
VMs do Azure (Windows e Linux) | Não uma cópia de segurança para DPM ou MABS | Cópia de segurança de ficheiros, pastas, estado do sistema.<br/><br/> As cópias de segurança são aplicações com suporte para máquinas Windows e arquivo com suporte para máquinas do Linux.
VMs do VMs/Azure no local | Protegidos pelo DPM | Cópia de segurança tudo o que é protegido pelo DPM, incluindo ficheiros/pastas/partilhas/volumes e dados específicos da aplicação. [Saiba](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-1807) o que o DPM pode criar cópias de segurança.
VMs do VMs/Azure no local | Protegido pelo MABS | Cópia de segurança tudo o que é protegido pelo MABS, incluindo ficheiros/pastas/partilhas/volumes e dados específicos da aplicação. [Saiba](backup-mabs-protection-matrix.md) o MABS pode criar cópias de segurança.

## <a name="what-backup-agents-do-i-need"></a>Os agentes de cópia de segurança é necessário?
**Cenário** | **Agente** | **Detalhes**
--- | --- | ---
(Nenhum servidor de cópia de segurança) de máquinas no local | Agente de serviços de recuperação do Azure (MARS) da Microsoft é executado na máquina do Windows. | Transfira e instale o agente de MARS durante a implementação de cópia de segurança do Azure.<br/><br/> Suporte para apenas a máquinas do Windows.
VMs do Azure (nenhum servidor de cópia de segurança) | Nenhum agente explícito obrigado. Extensão VM do Azure para cópia de segurança é executado na VM do Azure. | A extensão está instalada ao executar a primeira cópia de segurança de VM do Azure.<br/><br/> Suporte para o suporte do Windows e Linux.
VMs no local máquinas/Azure protegidas pelo DPM. | O agente de MARS é executado no servidor do DPM. | Não é necessário o agente de MARS em máquinas individuais.<br/><br/> Quando implementar o DPM, o agente de proteção do DPM está instalado em cada máquina que protege. 
Fazer uma cópia de segurança de máquinas no local e VMs do Azure protegidos pelo MABS | O agente de MARS é executado sobre o MABS. | Não é necessário o agente de MARS em máquinas individuais.<br/><br/> Quando implementa o MABS, o agente de proteção do MABS é instalado em cada máquina que protege. 


## <a name="which-backup-agent-should-i-use"></a>O agente de cópia de segurança devo utilizar?

**Cópia de segurança** | **Solução** | **Limitação**
--- | --- | ---
Eu quero fazer cópias de máquinas do Windows no local. As máquinas não estão protegidas pelo DPM ou MABS | Instale o agente de MARS na máquina. | Pode fazer backup de arquivos, pastas e estado do sistema para o Azure. As cópias de segurança não têm consciência de aplicação.
Eu quero fazer cópias de segurança de máquinas do Linux no local. As máquinas não estão protegidas pelo DPM ou MABS. | Precisa implantar o DPM ou MABS para criar cópias de segurança para o Azure.
Eu quero fazer cópias de segurança de aplicações em execução em máquinas do Windows no local | Para cópias de segurança de aplicação com suporte para máquinas Windows têm de ser protegidas pelo DPM ou MABS.
Eu quero fazer uma cópia de segurança de VMs do Azure | Execute uma cópia de segurança com o Azure Backup. A extensão de cópia de segurança serão automaticamente configurada no Windows ou VM do Linux do Azure. | Os discos VM são uma cópia de segurança.<br/><br/> Para VMs do Windows, a cópia de segurança é consistente com a aplicação. para Linux, a cópia de segurança é consistente com ficheiros. Se precisar de com suporte para a aplicação terá de configurar esta opção com scripts personalizados.
Eu quero fazer cópias de segurança de VMs do Azure com flexibilidade granular das definições de cópia de segurança e recuperação | Protege VMs do Azure com o DPM ou MABS em execução no Azure para flexibilidade adicional para o agendamento de cópia de segurança e total flexibilidade para proteger e restaurar ficheiros, pastas, volumes, aplicações e estado do sistema.


## <a name="next-steps"></a>Passos Seguintes

- [Revisão](backup-architecture.md) a arquitetura e componentes para diferentes cenários de cópia de segurança.
- [Certifique-se de](backup-support-matrix.md) suportado recursos e configurações para cópia de segurança.

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png

