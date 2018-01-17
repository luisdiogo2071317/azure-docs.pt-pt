---
title: Azure Site Recovery Deployment Planner de VMware para o Azure | Microsoft Docs
description: "Este é o manual do utilizador do Azure Site Recovery Deployment Planner."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/04/2017
ms.author: nisoneji
ms.openlocfilehash: 2985ed0b4bf5d9525bc2274d71b703922524f5a8
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2018
---
# <a name="azure-site-recovery-deployment-planner-for-vmware-to-azure"></a>Azure Site Recovery Deployment Planner de VMware para o Azure
Este artigo é o manual do utilizador do Azure Site Recovery Deployment Planner para implementações de produção de VMware para o Azure.

## <a name="overview"></a>Descrição geral

Antes de começar a proteger máquinas virtuais do VMware (VMs) com o Site Recovery, tem de alocar largura de banda suficiente com base na sua taxa de alterações de dados diária, de modo a cumprir o objetivo de ponto de recuperação (RPO) pretendido. Certifique-se de que implementa o número certo de servidores de configuração e de servidores de processos no local.

Também tem de criar o tipo e o número certo de contas de armazenamento do Azure de destino. Vai criar contas de armazenamento standard ou premium levando em linha de conta o crescimento dos servidores de produção de origem devido ao aumento da utilização ao longo do tempo. O tipo de armazenamento é escolhido por VM, com base nas características da carga de trabalho (por exemplo, operações de leitura / escrita / E/S por segundo [IOPS] ou alterações a dados) e nos limites do Site Recovery.

O Planeador de Implementações do Azure Site Recovery é uma ferramenta de linha de comandos para os cenários de recuperação após desastre Hyper-V para o Azure e VMware para o Azure. Pode utilizar esta ferramenta para criar remotamente o perfil das VMs de VMware (sem qualquer tipo de impacto na produção) para compreender os requisitos de largura de banda e de armazenamento do Azure necessários para replicação e ativação pós-falha de teste bem-sucedidas. Pode executar a ferramenta sem instalar nenhum componente do Site Recovery no local. Contudo, para obter resultados de débito precisos, recomendamos que execute o Planner num Windows Server que cumpra os requisitos mínimos do servidor de configuração do Site Recovery que terá de, a determinada altura, implementar como um dos primeiros passos na implementação de produção.

A ferramenta disponibiliza os seguintes detalhes:

**Avaliação de compatibilidade**

* Uma avaliação de elegibilidade de VMs com base no número de discos, no tamanho do disco, em IOPS, na alteração a dados, no tipo de arranque (EFI/BIOS) e versão de SO
 
**Necessidade de largura de banda de rede vs avaliação de RPO**

* A largura de banda de rede estimada necessária para a replicação delta
* O débito que o Site Recovery consegue obter a partir do local para o Azure
* O número de VMs a colocar em lotes, com base na largura de banda estimada para concluir a replicação inicial dentro de um determinado período de tempo.
* RPO que pode ser obtido para uma determinada largura de banda
* Impacto no RPO pretendido se for aprovisionada uma largura de banda mais reduzida.

**Requisitos de infraestrutura do Azure**

* O requisito de tipo de armazenamento (conta armazenamento standard ou premium) para cada VM
* O número total de contas de armazenamento standard e premium a configurar para a replicação
* Sugestões de nomenclatura de contas de armazenamento, com base nas orientações do Armazenamento do Azure
* A colocação da conta de armazenamento para todas as VMs
* O número de núcleos do Azure a configurar antes da ativação pós-falha ou reativação pós-falha de teste na subscrição
* O tamanho recomendado da VM do Azure para cada VM no local

**Requisitos de infraestrutura no local**
* O número necessário de servidores de configuração e de servidores de processos a implementar no local

**Custo estimado de DR para o Azure**
* Custo total estimado de DR para o Azure: custo de computação, armazenamento, rede e da licença do Azure Site Recovery
* Análise detalhada do custo por VM


>[!IMPORTANT]
>
>Uma vez que a utilização é suscetível de aumentar ao longo do tempo, todos os cálculos da ferramenta anteriores são feitos presumindo um fator de crescimento de 30 por cento nas características das cargas de trabalho e utilizando um valor de percentil 95 de todas as métricas de criação de perfis (leitura/escrita, IOPS, alterações a dados e assim sucessivamente). Ambos os elementos (fator de crescimento e cálculo do percentil) são configuráveis. Para saber mais sobre o fator de crescimento, veja a secção "Considerações sobre o fator de crescimento". Para saber mais sobre o valor de percentil, veja a secção "Valor de percentil utilizado para o cálculo".
>

## <a name="support-matrix"></a>Matriz de suporte

| | **VMware para o Azure** |**Hyper-V para o Azure**|**Azure para o Azure**|**Hyper-V para um site secundário**|**VMware para um site secundário**
--|--|--|--|--|--
Cenários suportados |Sim|Sim|Não|Sim*|Não
Versão suportada | vCenter 6.5, 6.0 ou 5.5| Windows Server 2016, Windows Server 2012 R2 | ND |Windows Server 2016, Windows Server 2012 R2|ND
Configuração suportada|vCenter, ESXi| Cluster Hyper-V, anfitrião Hyper-V|ND|Cluster Hyper-V, anfitrião Hyper-V|ND|
Número de servidores para os quais podem ser criados perfis por instância de execução do Planeador de Implementações do Azure Site Recovery |Único (é possível criar um perfil de cada vez de VMs pertencentes a um vCenter Server ou um servidor ESXi)|Vários (é possível criar um perfil de cada vez de VMs em vários anfitriões ou clusters anfitriões)| ND |Vários (é possível criar um perfil de cada vez de VMs em vários anfitriões ou clusters anfitriões)| ND

*A ferramenta destina-se principalmente ao cenário de recuperação após desastre Hyper-V para o Azure. Para a recuperação após desastre de Hyper-V para um site secundário, só pode ser utilizada para compreender recomendações do lado da origem, como a largura de banda de rede necessária, espaço de armazenamento livre necessário em cada um dos servidores Hyper-V de origem e definições de batches e números de criação de batches na replicação inicial.  Ignore as recomendações e os custos do Azure do relatório. Além disso, a operação Obter Débito não é aplicável ao cenário de recuperação após desastre de Hyper-V para um site secundário.

## <a name="prerequisites"></a>Pré-requisitos
A ferramenta tem duas fases principais – a criação de perfis e a geração de relatórios. Também existe uma terceira opção, para calcular apenas o débito. Os requisitos para o servidor a partir do qual é iniciada a medição de criação de perfis e do débito são apresentados na tabela seguinte:

| Requisito do servidor | Descrição|
|---|---|
|Medição da criação de perfis e do débito| <ul><li>Sistema operativo: Microsoft Windows Server 2016 ou Microsoft Windows Server 2012 R2<br>(que corresponda idealmente, pelo menos, às [recomendações de tamanho do servidor de configuração](https://aka.ms/asr-v2a-on-prem-components))</li><li>Configuração da máquina : 8 vCPus, 16 GB de RAM, 300 GB HDD</li><li>[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://aka.ms/download_powercli)</li><li>[Microsoft Visual C++ Redistributable para Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)</li><li>Acesso à Internet para o Azure a partir deste servidor</li><li>Conta de armazenamento do Azure</li><li>Acesso de administrador no servidor</li><li>Mínimo de 100 GB de espaço livre no disco (presumindo mil VMs com uma média de três discos cada, com perfis criados para 30 dias)</li><li>As definições de nível de estatísticas do VMware vCenter devem ser definidas ao nível 2 ou superior</li><li>Permitir porta 443: o ASR Deployment Planner utiliza esta porta para ligar ao vCenter server/anfitrião ESXi</ul></ul>|
| Geração de relatórios | Um PC Windows ou Windows Server com o Microsoft Excel 2013 ou posterior |
| Permissões de utilizador | Permissão só de leitura para a conta de utilizador utilizada para aceder ao VMware vCenter Server/anfitrião ESXi do VMware vSphere durante a criação do perfil |

> [!NOTE]
>
>A ferramenta só pode criar perfis para VMs com discos VMDK e RDM. Não pode criar perfis para VMs com discos iSCSI ou NFS. O Site Recovery suporta discos iSCSI e NFS para servidores do VMware. Contudo, uma vez que o Deployment Planner não está dentro do convidado e cria perfis apenas com os contadores de desempenho do vCenter, a ferramenta não tem visibilidade para estes tipos de discos.
>

## <a name="download-and-extract-the-deployment-planner-tool"></a>Transferir e extrair a ferramenta Planeador de Implementações
1. Transfira a versão mais recente do [Azure Site Recovery Deployment Planner](https://aka.ms/asr-deployment-planner).  
A ferramenta está comprimida numa pasta .zip. A versão atual só suporta o cenário de VMware para o Azure.

2. Copie a pasta .zip para o Windows Server a partir do qual quer executar a ferramenta.  
Pode executá-la no Windows Server 2012 R2 se o servidor tiver acesso à rede para ligar ao vCenter Server/anfitrião ESXi do vSphere que contém as VMs para as quais criar perfis. No entanto, recomendamos que execute num servidor cuja configuração de hardware cumpra a [orientação de tamanho do servidor de configuração](https://aka.ms/asr-v2a-on-prem-components). Se já tiver implementado componentes do Site Recovery no local, execute a ferramenta no servidor de configuração.

 Recomendamos que a configuração do hardware seja igual à do servidor de configuração (que tem um servidor de processos incorporado) no qual vai executar a ferramenta. Esta configuração garante que o débito obtido comunicado pela ferramenta corresponde ao débito real que o Site Recovery pode obter durante a replicação. O cálculo de débito depende da largura de banda de rede disponível na configuração do servidor e do hardware (CPU, armazenamento e assim sucessivamente) do servidor. Se executar a ferramenta a partir de qualquer outro servidor, o débito é calculado a partir desse servidor para o Microsoft Azure. Além disso, uma vez que a configuração de hardware do servidor pode diferir da configuração do servidor de configuração, o débito obtido que a ferramenta comunica poderá estar incorreto.

3. Extraia a pasta .zip.  
Esta contém vários ficheiros e sub-pastas. O ficheiro executável é ASRDeploymentPlanner.exe, na pasta principal.

    Exemplo:  
    Copie o ficheiro .zip para a unidade E:\ e extraia-o.
   E:\ASR Deployment Planner_v2.1zip

    E:\ASR Deployment Planner_v2.1\ASRDeploymentPlanner.exe

### <a name="updating-to-the-latest-version-of-deployment-planner"></a>Atualizar para a versão mais recente do Planeador de Implementações
Se tiver uma versão anterior do Planeador de Implementações, execute um dos seguintes procedimentos:
 * Se a versão mais recente não contiver uma correção de criação de perfis e a criação de perfis já estiver em curso na sua versão atual da ferramenta, continue com a mesma.
 * Se a versão mais recente contiver essa correção, recomendamos-lhe que pare a criação de perfis na versão atual e que a reinicie na nova.


 >[!NOTE]
 >
 >Quando iniciar a criação de perfis com a nova versão, transmita o mesmo caminho do diretório de saída, para que a ferramenta anexe os dados da criação de perfis nos ficheiros existentes. Para gerar o relatório, vai ser utilizado um conjunto completo de dados de criação de perfis. Se transmitir um diretório de saída diferente, são criados ficheiros novos e os dados antigos não são utilizados para gerar o relatório.
 >
 >Cada Deployment Planner novo é uma atualização acumulativa do ficheiro .zip. Não tem de copiar os ficheiros mais recentes para a pasta anterior. Pode criar e utilizar uma pasta nova.


## <a name="version-history"></a>Histórico de versões
A versão mais recente da ferramenta Planeador de Implementações do ASR é 2.1.
Consulte a página [Histórico de Versões do Planeador de Implementações do ASR](https://social.technet.microsoft.com/wiki/contents/articles/51049.asr-deployment-planner-version-history.aspx) para obter as correções que foram adicionadas em cada atualização.

## <a name="next-steps"></a>Passos seguintes
* [Executar o planeador de implementação](site-recovery-vmware-deployment-planner-run.md).
