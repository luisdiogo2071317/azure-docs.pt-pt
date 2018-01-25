

## <a name="what-is-happening"></a>O que acontece?

Uma vulnerabilidade de segurança baseada em hardware e ao nível de toda a indústria foi [anunciada a 3 janeiro](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html). Manter os clientes segura é sempre nosso prioridade superior e estão a utilizar o Active Directory passos para garantir que nenhum cliente do Azure está exposto a estas vulnerabilidades.

Com a divulgação pública de vulnerabilidade de segurança, iremos [acelerados a temporização de manutenção planeada](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) e começou seja automaticamente reiniciado as VMs que continua a ser necessária a atualização.


## <a name="how-can-i-see-which-of-my-vms-are-already-updated"></a>Como posso ver quais das minhas VMs já são atualizados? 

Pode ver o estado das suas VMs, bem como se o reinício já foi concluída, na [lista de VMs no portal do Azure](https://aka.ms/T08tdc). As VMs estão listadas como "Já Está Atualizada", se a atualização tiver sido aplicada, ou como "Agendada", caso a atualização ainda seja necessária. Se quiser ver apenas as VMs no estado "Agendada", veja o [Azure Service Health](https://portal.azure.com/).

## <a name="can-i-find-out-exactly-when-my-vms-will-be-rebooted"></a>Pode encontrar saída exatamente quando minhas VMs serão reiniciados?

A melhor forma de obter um alerta sobre o reinício está a configurar [agendada eventos](https://docs.microsoft.com/azure/virtual-machines/windows/scheduled-events). Isto fornece uma notificação de 15 minutos da VM ficar devido a manutenção.

## <a name="can-i-manually-redeploy-now-to-perform-the-required-maintenance"></a>Pode posso manualmente Reimplementar agora a efetuar a manutenção necessária? 

Não podemos garantir que uma VM reimplementada será atribuída para um anfitrião atualizado. Sempre que possível, os recursos de infraestrutura do Azure irão tentar alocar VMs a anfitriões que já estão atualizados. É possível que voltar a implementar uma VM foi apresentado num anfitrião não é atualizada, nesse caso, pode estar sujeita a uma segunda reinicialização, forçada como parte da manutenção agendada. Como resultado, redeploys manuais não são recomendadas como uma solução.

## <a name="how-long-will-the-reboot-take"></a>Quanto tempo demorará a reinicialização? 

A maioria dos reinícios estão a demorar, aproximadamente, **30 minutos**.

## <a name="does-the-guest-os-need-to-be-updated"></a>É o convidado SO tem de ser atualizados? 

Esta atualização de infraestrutura do Azure aborda a vulnerabilidade disclosed ao nível do hipervisor e não necessita de uma atualização para as imagens do Windows ou a VM com Linux. No entanto, como sempre, deve continuar a aplicar procedimentos recomendados de segurança para as imagens VM. Consulte o fornecedor dos sistemas de operativos atualizações e obter instruções, conforme necessário. Para clientes com VM do Windows Server, a documentação de orientação já foi publicada e está disponível [aqui](../articles/virtual-machines/windows/mitigate-se.md).

## <a name="will-there-be-a-performance-impact-as-a-result-of-resolving-this-update"></a>Haverá um impacto no desempenho devido a resolver esta atualização?

A maioria dos clientes do Azure não ter visto um impacto evidente no desempenho com esta atualização. Temo-nos esforçado por otimizar o caminho de E/S de disco e CPU e não temos detetado qualquer mudança significativa no desempenho após a aplicação da correção. Um pequeno conjunto de clientes poderá sentir alguma mudança no desempenho de rede. Isto pode ser resolvido utilizando acelerados redes do Azure, para [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) ou [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli), que é uma capacidade livre disponível para todos os clientes do Azure.

## <a name="i-follow-your-recommendations-for-high-availability-will-my-environment-remain-highly-available-during-the-reboot"></a>Posso siga as recomendações para elevada disponibilidade, os meus ambiente permanecerá altamente disponível durante o reinício?

Sim, máquinas virtuais implementadas na disponibilidade de um conjunto ou conjuntos de dimensionamento de máquina virtual tem de construção de domínios de atualização (UD). Quando efetuar a manutenção, o Azure honra a restrição UD e não irá reiniciar as máquinas virtuais de diferentes UD (no mesmo conjunto de disponibilidade). Para obter mais informações sobre como de elevada disponibilidade, consulte [gerir a disponibilidade das virtual machines Windows no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) ou [gerir a disponibilidade das máquinas virtuais do Linux no Azure](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

## <a name="i-have-architected-my-business-continuitydisaster-recovery-plan-using-region-pairs-will-reboots-to-my-vms-occur-in-region-pairs-at-the-same-time"></a>Posso ter criado o meu plano de recuperação de continuidade/desastre de negócio utilizando pares de região. Reinícios às minhas VMs ocorrerá pares de região ao mesmo tempo?

Normalmente, os eventos de manutenção planeada do Azure são implementados em regiões emparelhadas um de cada vez para minimizar o risco de interrupção em ambas as regiões. No entanto, devido à natureza urgente desta atualização de segurança, iremos são disponibilizando a atualização para todas as regiões em simultâneo.

## <a name="what-about-paas-services-on-azure"></a>O que sobre os serviços de PaaS no Azure?  

Os serviços de plataforma do Azure, incluindo web & mobile, serviços de dados, IoT, sem servidor, etc. têm endereçada a vulnerabilidade. Não há nenhuma ação necessária para clientes que utilizam estes serviços.

## <a name="intel-released-additional-guidance-on-january-22-2018-related-to-the-security-vulnerabilities--will-this-guidance-cause-any-additional-maintenance-activities-by-azure"></a>Intel lançadas orientações adicionais em 22 de Janeiro de 2018 relacionadas com as vulnerabilidades de segurança.  Esta orientação fará com que quaisquer atividades de manutenção adicionais pelo Azure?  

Não são afetadas pelo Azure mitigações previamente anunciadas no 3 de Janeiro de 2018 o [instruções atualizadas](https://newsroom.intel.com/news/root-cause-of-reboot-issue-identified-updated-guidance-for-customers-and-partners/) da Intel. Não haverá nenhuma atividade manutenção adicionais no cliente VMs devido a estas informações de novo.
 

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, consulte [clientes proteger Azure da vulnerabilidade de CPU](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
