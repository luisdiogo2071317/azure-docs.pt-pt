---
title: Perguntas comuns - recuperação após desastre do Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo resume as perguntas mais comuns ao configurar a recuperação após desastre de VMs do Azure para aonther região do Azure com o Azure Site Recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: 6fe7152e43640a809ab9f4de39b1c6b599975a20
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969952"
---
# <a name="common-questions---azure-to-azure-replication"></a>Perguntas comuns - replicação do Azure para o Azure

Este artigo fornece respostas a perguntas comuns que vemos ao implementar a recuperação após desastre de VMs do Azure para outra região do Azure. Se tiver questões depois de ler este artigo, publique o [fórum de serviços de recuperação do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Geral
### <a name="how-is-site-recovery-priced"></a>Como é que o Site Recovery é cobrado?
Revisão [preços do Azure Site Recovery](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) detalhes.

### <a name="how-to-configure-site-recovery-on-azure-vms-what-are-the-best-practices"></a>Como configurar a recuperação de sites em VMs do Azure. Quais são as melhores práticas?
1. [Compreender a arquitetura do Azure para o Azure](azure-to-azure-architecture.md)
2. [Reveja as configurações suportadas e não suportadas](azure-to-azure-support-matrix.md)
3. [Configurar a recuperação após desastre para VMs do Azure](azure-to-azure-how-to-enable-replication.md)
4. [Executar uma ativação pós-falha de teste](azure-to-azure-tutorial-dr-drill.md)
5. [A ativação pós-falha e reativação pós-falha para a região primária](azure-to-azure-tutorial-failover-failback.md)

## <a name="replication"></a>Replicação

### <a name="can-i-replicate-azure-disk-encryption-enabled-vms"></a>Posso replicar Azure disco VMs de encriptação ativada?
Sim, pode replicá-los. Consultar [artigo](azure-to-azure-how-to-enable-replication-ade-vms.md) para ativar a replicação de encriptação de disco do Azure (ADE) ativada VMs. Tenha em atenção que apenas as VMs do Azure com o SO Windows e ativado para a encriptação com a aplicação do Azure AD são atualmente suportadas pelo Azure Site Recovery.

### <a name="can-i-replicate-vms-to-another-subscription"></a>Pode replicar VMs para outra subscrição?
Sim, pode replicar VMs do Azure para uma subscrição diferente no mesmo inquilino do Azure Active Directory.
Configurar DR [entre subscrições](https://azure.microsoft.com/blog/cross-subscription-dr) é simples. Pode selecionar outra subscrição no momento da replicação.

### <a name="can-i-replicate-zone-pinned-azure-vms-to-another-region"></a>Pode replicar VMs do afixadas por zona do Azure para outra região.
Sim, pode [replicar VMs de afixadas por zona](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region) para outra região.

### <a name="can-i-exclude-disks"></a>Pode excluir discos?

Sim, pode excluir discos no momento da proteção utilizando a power shell. Consultar [orientações do powershell](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine) para excluir o disco

###<a name="how-often-can-i-replicate-to-azure"></a>Com que frequência posso replicar para o Azure?
A replicação é contínua ao replicar VMs do Azure para outra região do Azure. Verifique os [do Azure para o Azure](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-architecture#replication-process) arquitetura da replicação para entender os detalhes.

### <a name="can-i-replicate-virtual-machines-within-a-same-region-i-need-this-to-migrate-vms"></a>Pode replicar máquinas virtuais dentro de uma mesma região? Esta opção para migrar VMs necessário?
Não é possível utilizar a solução de DR do Azure para o Azure para replicar VMs dentro de uma mesma região.

### <a name="can-i-replicate-vms-to-any-azure-region"></a>Pode replicar VMs para qualquer região do Azure?
Com o Site Recovery, pode replicar e recuperar VMs entre quaisquer duas regiões dentro do mesmo cluster geográfico. Clusters geográficas são definidos tendo a latência de dados e soberania de dados em mente. Para obter mais informações, consulte o Site Recovery [matriz de suporte de região](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support).

### <a name="does-site-recovery-require-internet-connectivity"></a>Recuperação de sites necessitam de conectividade à internet?

Não, recuperação de sites não necessita de conectividade à internet, mas o acesso aos intervalos IP e URLs de recuperação de Site como mencionado neste [artigo](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges)

## <a name="replication-policy"></a>Política de Replicação

### <a name="what-is-a-replication-policy"></a>O que é uma política de replicação?
Define as definições de recuperação ponto retenção histórico e aplicação de frequência de instantâneo consistente. Por predefinição, o Azure Site Recovery cria uma nova política de replicação com as predefinições dos ' 24 horas para retenção do ponto de recuperação e "60 minutos para a frequência de instantâneo consistente da aplicação.

### <a name="what-is-crash-consistent-recovery-point"></a>O que é o ponto de recuperação consistente com a falha?
Ponto de recuperação consistente com a falha representa os dados no disco, como se a VM falhou ou o cabo de alimentação foi obtido a partir do servidor no momento do instantâneo foi tirado. Ele não inclui tudo o que estava na memória, quando o instantâneo foi tirado. Hoje em dia, a maioria dos aplicativos pode recuperar bem a partir de instantâneos consistentes com falhas. Um ponto de recuperação consistentes com falhas geralmente é suficiente para não sistemas operacionais de base de dados e aplicações, como servidores de arquivos, servidores DHCP, servidores de impressão e assim por diante.

### <a name="what-is-application-consistent-recovery-point"></a>O que é o ponto de recuperação consistentes com aplicações? 
Pontos de recuperação consistentes com a aplicação são criados a partir de instantâneos consistentes com a aplicação que capturam os mesmos dados como instantâneos consistentes com falhas, com a adição de todos os dados na memória e todas as transações no processo. Devido aos conteúdos adicionais, os instantâneos consistentes com aplicações mais estejam e segue há mais tempo para executar. Pontos de recuperação consistentes com aplicações são recomendados para sistemas operacionais de base de dados e aplicações, tais como o SQL.

### <a name="how-are-recovery-points-generated-and-saved"></a>Como pontos de recuperação gerados e guardados?
Para compreender como o Site Recovery gera os pontos de recuperação permite colocar um exemplo de política de replicação, que tem a janela de retenção de 24 horas e o instantâneo de frequência consistente de aplicação de 1 hora de ponto de recuperação.
Site Recovery cria ponto consistente com falhas a cada 5 minutos e o utilizador não tem qualquer controle para alterar essa frequência. Por conseguinte, para o último utilizador de uma hora terá 12 pontos consistente com a falha e 1 ponto consistente de aplicação à sua escolha. Conforme o andamento do tempo, prunes de recuperação de sites do ponto de todos os pontos de recuperação além da última 1 hora e apenas salvar uma recuperação por hora.
Para fins de ilustração, na captura de ecrã abaixo:


1. Por período inferior a última 1 hora, existem pontos de recuperação com a frequência de 5 minutos.
2. Por tempo além da última 1 hora, podemos ver que apenas um ponto de recuperação por hora é mantido.

  ![geração de pontos de recuperação](./media/azure-to-azure-troubleshoot-errors/recoverypoints.png)


### <a name="how-far-back-can-i-recover"></a>Qual a amplitude posso recuperar?
O ponto de recuperação mais antigo, que pode usar é 72 horas.

### <a name="what-will-happen-if-i-have-a-replication-policy-of-24-hours-and-there-is-no-recovery-points-generation-due-to-some-issue-for-more-than-24-hours-does-my-previous-recovery-points-will-be-pruned"></a>O que acontecerá se eu tiver uma política de replicação de 24 horas e não existe nenhum geração de pontos de recuperação devido a algum problema durante mais de 24 horas. É meu serão eliminados de pontos de recuperação anterior?
Não, Site Recovery irá manter os todos os pontos de recuperação anteriores neste caso. 

### <a name="after-replication-is-enabled-on-a-vm-how-do-i-change-the-replication-policy"></a>Após a replicação estiver ativada numa VM, como posso alterar a política de replicação? 
Aceda ao Cofre de recuperação de sites > infraestrutura do Site Recovery > políticas de replicação. Selecione a política que pretende editar e guardar as alterações. Qualquer alteração será aplicada a todas as replicações existentes também. 

### <a name="are-all-the-recovery-points-complete-copy-of-the-vm-or-differential"></a>São todos os pontos de recuperação de cópia completa da VM ou diferenciais?
Em caso de inicial replicação o primeiro ponto de recuperação, que obtém gerado terão a cópia completa e quaisquer pontos de recuperação sucessivas terão alterações delta.

### <a name="does-increasing-recovery-points-retention-windows-increases-the-storage-cost"></a>Aumentar janelas de retenção de pontos de recuperação aumenta o custo de armazenamento?
Sim, se aumentar o período de retenção de 24 horas para 72 horas, em seguida, o Site Recovery irá guardar os pontos de recuperação para adição a 48 horas que será incorrido-custos de armazenamento. Para o exemplo se um ponto de recuperação de único tem alterações de delta de 10 GB e custo por GB é 0.16 us $ por mês, em seguida, o que seria US $1.6 * 48 encargos adicionais por mês.

## <a name="failover"></a>Ativação pós-falha

### <a name="is-failover-automatic"></a>A ativação pós-falha é automática?

A ativação pós-falha não é automática. Inicia as ativações pós-falha com um clique único no portal ou pode utilizar o Site Recovery [PowerShell](azure-to-azure-powershell.md) para acionar uma ativação pós-falha. 

### <a name="can-i-retain-public-ip-address-after-failover"></a>Posso manter o endereço IP público após a ativação pós-falha?

Endereço IP público da aplicação de produção **não podem ser mantidas na ativação pós-falha**. Cargas de trabalho colocadas como parte do processo de ativação pós-falha deve ser atribuído um recurso de IP público do Azure disponível na região de destino. Este passo pode ser feito manualmente ou é automatizado com planos de recuperação. Consultar [artigo](https://docs.microsoft.com/azure/site-recovery/concepts-public-ip-address-with-site-recovery#public-ip-address-assignment-using-recovery-plan) para atribuir o endereço IP público através do plano de recuperação.  

### <a name="can-i-retain-private-ip-address-during-failover"></a>Posso manter o endereço IP privado durante a ativação pós-falha?
Sim, pode manter o endereço IP privado. Por predefinição, quando ativar a recuperação após desastre para VMs do Azure, o Site Recovery cria os recursos de destino com base nas definições de recursos de origem. Para as VMs do Azure configurada com endereços IP estáticos, o Site Recovery tenta aprovisionar o mesmo endereço IP da VM, de destino se não está em utilização. Consultar [artigo](site-recovery-retain-ip-azure-vm-failover.md) para manter o endereço IP privado em condições diferentes.

### <a name="after-failover-the-server-does-not-have-the-same-ip-address-as-the-source-vm-why-is-it-assigned-with-a-new-ip-address"></a>Após a ativação pós-falha, o servidor não tem o mesmo endereço IP da VM de origem. Por que é atribuída com um novo endereço IP-lo?

Recuperação de site tenta fornecer o endereço IP na base de melhor esforço no momento da ativação pós-falha. No caso de está a ser colocada por alguma outra máquina virtual, o endereço IP disponível seguinte está definido como o destino. Para obter uma explicação completa de como o Site Recovery trata endereçamento, [rever este artigo.](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-network-mapping#set-up-ip-addressing-for-target-vms)

### <a name="what-does-latestlowest-rpo-recovery-points-means"></a>O que faz a recuperação da versão mais recente (RPO mais baixo) aponta significa?
Esta opção processa primeiro todos os dados que tenham sido enviados para o serviço de recuperação de Site, para criar um ponto de recuperação para cada VM antes de realizar a ativação pós-falha para o mesmo. Esta opção disponibiliza o último RPO (objetivo de ponto de recuperação), uma vez que a VM criada após a ativação pós-falha terá todos os dados replicados para o Site Recovery quando a ativação pós-falha foi acionada.

### <a name="does-latest-lowest-rpo-recovery-points-have-impact-on-failover-rto"></a>Pontos de recuperação mais recente (RPO mais baixo) tem impacto no RTO de ativação pós-falha?
Sim, como o Site Recovery irá processar todos os dados pendentes antes de realizar a ativação pós-falha, esta opção terão um RTO superior em comparação com outras pessoas.

### <a name="what-does-latest-processed-option-in-recovery-points-mean"></a>O que faz mais recente processado opção na média de pontos de recuperação?
Esta opção faz a ativação pós-falha de todas as VMs no plano do ponto de recuperação mais recente processado pelo Site Recovery. Para ver o ponto para uma VM específica de recuperação mais recente, verifique os pontos de recuperação mais recentes nas definições de VM. Esta opção proporciona um RTO (Objetivo de Tempo de Recuperação) baixo, porque não é despendido tempo ao processar os dados não processados.

### <a name="if-im-replicating-between-two-azure-regions-what-happens-if-my-primary-region-experiences-an-unexpected-outage"></a>Se estiver a replicar entre duas regiões do Azure, o que acontece se minha região primária sofre uma falha inesperada?
Pode acionar uma ativação pós-falha após a falha. Recuperação de sites não necessita de conectividade da região primária para efetuar a ativação pós-falha.

## <a name="recovery-plan"></a>Plano de recuperação

### <a name="what-is-a-recovery-plan-"></a>O que é um plano de recuperação?
Planos de recuperação no Site Recovery orquestrar a recuperação de ativação pós-falha de VMs. Ele ajuda a tornar a recuperação consistentemente precisos, repetíveis e automatizadas. Um plano de recuperação aborda as seguintes necessidades para o utilizador:

- Definir um grupo de virtual em conjunto a máquinas que a ativação pós-falha.
- Definindo as dependências entre as máquinas virtuais para que o aplicativo é exibido com precisão.
- Automatizar a recuperação juntamente com as ações manuais personalizadas para que as tarefas que não seja a ativação pós-falha das máquinas virtuais também podem ser alcançadas.

[Saiba mais](site-recovery-create-recovery-plans.md) sobre os planos de recuperação.

### <a name="how-does-sequencing-is-achieved-in-a-recovery-plan"></a>Como faz o sequenciamento é obtido de um plano de recuperação?

No plano de recuperação, pode criar vários grupos para alcançar a sequenciação. Cada grupo de ativação pós-falha de uma vez, o que significa que as VMs que fazem parte do mesmo grupo efetuará a ativação pós-falha em conjunto seguida de outro grupo. Verifique como [aplicação de modelo com o plano de recuperação.](https://review.docs.microsoft.com/azure/site-recovery/recovery-plan-overview?branch=pr-en-us-61681#model-apps) 

### <a name="how-can-i-find-the-rto-of-a-recovery-plan"></a>Como posso encontrar o RTO de um plano de recuperação?
Para verificar o RTO de um plano de recuperação, ativação pós-falha do plano de recuperação de teste e vá para as tarefas de recuperação de Site.
Por exemplo conforme mostrado abaixo, plano de recuperação de teste de SAP demorou 8 segundos de 59 minutos a ativação pós-falha de todas as máquinas virtuais e execute quaisquer ações especificadas.

  ![Erro de com](./media/azure-to-azure-troubleshoot-errors/recoveryplanrto.PNG)

### <a name="can-i-add-automation-runbooks-to-the-recovery-plan"></a>Pode adicionar runbooks de automatização para o plano de recuperação?
Sim, pode integrar os runbooks de automatização do Azure no seu plano de recuperação. [Saiba mais](site-recovery-runbook-automation.md)

## <a name="reprotect-and-failback"></a>Voltar a proteger e a reativação pós-falha 

### <a name="after-doing-a-failover-from-primary-region-to-disaster-recovery-region-does-vms-in-a-dr-region-gets-protected-automatically"></a>Depois de efetuar uma ativação pós-falha da região primária para a região de recuperação após desastre faz VMs numa região DR obtém automaticamente protegido?
Não, quando [ativação pós-falha](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) as VMs do Azure de uma região para outra, as VMs efetuar o arranque na região DR num estado desprotegido. Para efetuar a reativação pós-falha de VMs para a região primária, precisa [voltar a proteger](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect) as VMs na região secundária.

### <a name="at-the-time-of-reprotection-does-site-recovery-replicate-complete-data-from-secondary-region-to-primary-region"></a>No momento da nova proteção é a recuperação de Site replicado completa de dados de região secundária para a região primária?
Depende da situação, por exemplo, se a região de origem VM existe, em seguida, apenas as alterações entre o disco de origem e o disco de destino são sincronizadas. Os diferenciais são calculados comparando a ambos os discos e, em seguida, transferidas. Normalmente, esta ação demorar algumas horas a concluir. Consultar [artigo]( https://docs.microsoft.com/azure/site-recovery/azure-to-azure-how-to-reprotect#what-happens-during-reprotection) para obter detalhes sobre o que acontece durante a nova proteção.

### <a name="how-much-time-does-it-take-to-failback"></a>Quanto tempo isso efetuar a reativação pós-falha?
Após a conclusão da nova proteção, em seguida, normalmente, é a quantidade de tempo semelhante a ativação pós-falha da região primária para a região secundária. 

## <a name="security"></a>Segurança
### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Os dados de replicação são enviados para o serviço de Recuperação de Sites?
Não, a recuperação de sites não interceta os dados replicados e não tem quaisquer informações sobre o que está em execução nas suas máquinas virtuais. Apenas os metadados necessários para orquestrar a replicação e a ativação pós-falha são enviados para o serviço de Recuperação de Sites.  
Site Recovery é ISO 27001:2013, 27018, HIPAA, DPA certified e está no processo de SOC2 e FedRAMP JAB.

### <a name="does-site-recovery-encrypt-replication"></a>A Recuperação de Sites faz encriptação de replicação?
Sim, ambas as encriptação em trânsito e [encriptação no Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) são suportados.

## <a name="next-steps"></a>Passos Seguintes
* [Revisão](azure-to-azure-support-matrix.md) dar suporte aos requisitos.
* [Configurar](azure-to-azure-tutorial-enable-replication.md) replicação do Azure para o Azure.
