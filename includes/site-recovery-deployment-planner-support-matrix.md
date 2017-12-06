# <a name="support-matrix"></a>Matriz de suporte

| | **VMware para o Azure** |**Hyper-V para o Azure**|**Azure para o Azure**|**Hyper-V para o site secundário**|**VMware para o site secundário**
--|--|--|--|--|--
Cenários suportados |Sim|Sim|Não|Sim*|Não
Versão suportada | vCenter 6.5, 6.0 ou 5.5| Windows Server 2016, Windows Server 2012 R2 | ND |Windows Server 2016, Windows Server 2012 R2|ND
Configuração suportada|vCenter, ESXi| Cluster Hyper-V, o anfitrião Hyper-V|ND|Cluster Hyper-V, o anfitrião Hyper-V|ND|
Número de servidores que podem ser profiled por executar a instância do Planeador de implementação do Azure Site Recovery |Único (podem profiled VMs que pertencem a um vCenter Server ou um servidor de ESXi a uma hora)|Vários (VMs em vários anfitriões ou clusters de anfitriões podem ser o perfil de cada vez)| ND |Vários (VMs em vários anfitriões ou clusters de anfitriões podem ser o perfil de cada vez)| ND

* A ferramenta é principalmente para o Hyper-V para o cenário de recuperação de desastre do Azure. Para o Hyper-V para recuperação de desastre do site secundário, esta só pode ser utilizada para compreender origem recomendações lado como largura de banda de rede necessários, espaço de armazenamento livre necessário em cada um dos servidores de Hyper-V de origem e a replicação inicial batch e números de criação de batches definições.  Ignore as recomendações do Azure e custos do relatório. Além disso, a operação de débito obter não é aplicável para o Hyper-V para o cenário de recuperação de desastre do site secundário.
