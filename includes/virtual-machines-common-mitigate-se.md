
 
A divulgação recente de um [nova classe de vulnerabilidades de CPU](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) conhecido como ataques de canal de lado de execução speculative resultou no questões a partir de clientes de pesquisa mais efeitos de clareza. 

 
## <a name="azure-infrastructure"></a>Infraestrutura do Azure

Os problemas descritos em divulgação a vulnerabilidade pode ser utilizados para ignorar um limite de hipervisor e permitir a divulgação de memória entre duas máquinas virtuais conjuntamente alojadas. Tal como relatado na anterior [blogue](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/), Azure tenha aplicada mitigações para proteger os clientes contra esta vulnerabilidade.  A Microsoft recomenda sempre que os clientes aplicam procedimentos recomendados de segurança para as imagens VM, incluindo a instalação de todas as atualizações de segurança do seu fornecedor de sistema operativo.

## <a name="paas-services-on-azure"></a>Serviços de PaaS no Azure
Ofertas do Azure de PaaS tem mitigações implementadas por predefinição. É necessária nenhuma ação por clientes. Consulte abaixo para exceção de Cloud Services do Azure.  


## <a name="azure-cloud-services"></a>Cloud Services do Azure

[Serviços Cloud do Azure](https://azure.microsoft.com/services/cloud-services/) com a atualização automática ativada automaticamente, recebe uma versão do SO convidado que inclui mitigações para estas vulnerabilidades. 

As seguintes versões de SO convidado incluem atualizações com proteções contra vulnerabilidades de canal de lado de execução speculative:

* WA-CONVIDADO-SO-5.15_201801-01
* WA-CONVIDADO-SO-4.50_201801-01


Um pequeno número de clientes que utilizam os Cloud Services do Azure para alojar código não fidedigno deve ativar também [Kernel Virtual endereço sombra da](#enabling-kernel-virtual-address-shadowing-on-windows-server) para além de atualizar o SO convidado. Isto proporciona proteção adicional contra a execução speculative vulnerabilidades do lado do canal. Isto pode ser conseguido através de uma tarefa de arranque. Obter mais informações sobre os quais os clientes e cenários de utilização necessitam que esta funcionalidade e como ativá-la, são fornecidas abaixo.


## <a name="azure-virtual-machines-windows--linux"></a>Máquinas virtuais do Azure (Windows e Linux)

A Microsoft recomenda sempre que os clientes instalar todas as atualizações de segurança. 

A agregação de segurança de Janeiro de 2018 contém correções para estas vulnerabilidades e muito mais. Certifique-se de que está a executar uma aplicação antivírus suportada antes de instalar atualizações do SO. Contacte o fornecedor do software antivírus para informações de compatibilidade. 

A vulnerabilidades de execução speculative de endereço, as atualizações para o Linux kernel será necessária e pode ser obtido através do seu fornecedor de distribuição quando disponível. 

Um pequeno número de clientes que utilizam máquinas virtuais do Azure (Windows) para alojar código não fidedigno deve ativar também [Kernel Virtual endereço sombra da](#enabling-kernel-virtual-address-shadowing-on-windows-server) que fornece proteção adicional contra speculative execução lado-canal vulnerabilidades.  Obter mais informações sobre quais os clientes e a utilização de cenários requerem esta funcionalidade e como o activar são fornecidas abaixo.


## <a name="enabling-kernel-virtual-address-shadowing-on-windows-server"></a>Ativar endereço Virtual de Kernel sombra no Windows Server

Os clientes que utilizam o Windows Server para executar código não fidedigno devem ativar uma funcionalidade denominada Kernel Virtual endereço sombra da que fornece proteção para os sistemas onde está a executar o código não fidedigno com privilégios de utilizador baixa.

Esta proteção adicional poderá afetar o desempenho e está desativada por predefinição. Kernel Virtual endereço sombra da protege contra a divulgação de informações do processo de processo e o processo do kernel.

As VMs, serviços em nuvem ou servidores no local estão em risco de se poderem enquadram-se uma das seguintes categorias:

* Azure aninhada os anfitriões de Virtualização Hyper-V
* Serviços de ambiente de trabalho remoto (RDSH) de anfitriões
* Máquinas virtuais ou serviços em nuvem que estiver a executar código não fidedigno como contentores ou extensões não fidedignas para a base de dados, o conteúdo web não fidedigno ou cargas de trabalho que executam o código que é fornecido a partir de origens externas.

Um exemplo de um cenário onde Kernel Virtual endereço sombra da é necessária: 

|     |
|-----|
|Uma máquina virtual do Azure executa um serviço em que os utilizadores não fidedignos podem submeter código JavaScript, que é executado com privilégios limitados. Na mesma VM, existe um processo muito privilegiado que contém dados secretos que não devem ser acessíveis aos utilizadores. Nesta situação, é necessária fornecer proteção contra a divulgação de entre os dois Kernel Virtual endereço sombra da.|
|     | 

Estão disponíveis através de instruções específicas sobre como ativar Kernel Virtual endereço sombra da [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution).


> [!NOTE]
> No momento da publicação, Kernel Virtual endereço sombra da só está disponível no Windows Server 2016, o Windows Server 2012 R2 e o Windows Server 2008 R2.  
>
>

Se estiver a executar um servidor Linux, consulte o fornecedor dos sistemas de operativos atualizações e instruções.

## <a name="branch-target-injection-mitigation-support-microcode"></a>Suporte de mitigação de inserção do ramo destino (Microcode)

Clientes que utilizam as ferramentas que detetar a existência de com base em microcode mitigações relatório que é tratam Azure. Isto está incorreto. No momento desta publicação, o suporte de mitigação de Injeção de sucursal destino não está exposto do hipervisor do Azure na Azure Virtual Machines ou Cloud Services do Azure. Isto significa que as máquinas virtuais não estiverem cientes a existência de microcode e não é possível utilizar o respetivo conjunto de instruções aumentado. Isto significa que o Azure está vulnerável a ataques de canal de lado de execução speculative cross-VM.
 
À medida que trabalha com parceiros da indústria atualizações mais podem tornar-se disponível.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, consulte [clientes proteger Azure da vulnerabilidade de CPU](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).