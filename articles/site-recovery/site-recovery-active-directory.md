---
title: Proteger o Active Directory e DNS com o Azure Site Recovery | Microsoft Docs
description: "Este artigo descreve como implementar uma solução de recuperação após desastre para o Active Directory utilizando o Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: manayar
ms.openlocfilehash: df5f40a49aa7359c082b0feb9e047818a642a871
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="use-azure-site-recovery-to-protect-active-directory-and-dns"></a>Utilizar o Azure Site Recovery para proteger o Active Directory e DNS

As aplicações empresariais, tais como o SharePoint, Dynamics AX e SAP dependem do Active Directory e uma infraestrutura DNS para funcionar corretamente. Quando configurar a recuperação após desastre para aplicações, muitas vezes necessitam de recuperar o Active Directory e DNS antes de recuperar outros componentes da aplicação, para garantir a funcionalidade de aplicação correto.

Pode utilizar [recuperação de Site](site-recovery-overview.md) para criar um plano de recuperação após desastre para o Active Directory. Se ocorre uma interrupção, pode iniciar uma ativação pós-falha. Pode ter o Active Directory cópias de segurança e em execução dentro de alguns minutos. Se tiver implementado do Active Directory para várias aplicações no seu site primário, por exemplo, para o SharePoint e SAP, poderá efetuar a ativação pós-falha completa do local. Primeiro pode efetuar a ativação pós-falha do Active Directory utilizando ite recuperação. Em seguida, efetua a ativação pós-falha de outras aplicações, utilizando os planos de recuperação específico da aplicação.

Este artigo explica como criar uma solução de recuperação após desastre para o Active Directory. Inclui os pré-requisitos e instruções de ativação pós-falha. Deve estar familiarizado com o Active Directory e a recuperação de Site antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Se estiver a replicar para o Azure, [preparar os recursos do Azure](tutorial-prepare-azure.md), incluindo uma subscrição, uma rede Virtual do Azure, uma conta do storage e um cofre dos serviços de recuperação.
* Reveja os [requisitos de suporte](site-recovery-support-matrix-to-azure.md) de todos os componentes.

## <a name="replicate-the-domain-controller"></a>Replicar o controlador de domínio

Tem de configurar [replicação do Site Recovery](#enable-protection-using-site-recovery), pelo menos uma VM que aloja um controlador de domínio ou DNS. Se tiver [vários controladores de domínio](#environment-with-multiple-domain-controllers) no seu ambiente, também tem de configurar um [controlador de domínio adicional](#protect-active-directory-with-active-directory-replication) no site de destino. O controlador de domínio adicionais pode ser no Azure ou num datacenter secundário no local.

### <a name="single-domain-controller"></a>Controlador de domínio único
Se tiver apenas algumas aplicações e um controlador de domínio, poderá querer efetuar a ativação pós-falha de todo o local em conjunto. Neste caso, recomendamos a utilização de recuperação de sites para replicar o controlador de domínio para o site de destino (a no Azure ou num datacenter secundário no local). Pode utilizar o mesmo controlador de domínio replicados ou a máquina virtual DNS para [ativação pós-falha de teste](#test-failover-considerations).

### <a name="multiple-domain-controllers"></a>Vários controladores de domínio
Se tiver várias aplicações e mais do que um controlador de domínio no seu ambiente ou, se pretender efetuar a ativação pós-falha algumas aplicações simultaneamente; além disso replicar as máquinas de controlador de domínio com a recuperação de Site, recomendamos que configure um [controlador de domínio adicional](#protect-active-directory-with-active-directory-replication) no site de destino (a no Azure ou num datacenter secundário no local). Para [ativação pós-falha de teste](#test-failover-considerations), pode utilizar o controlador de domínio que é replicado pela recuperação de sites. Para ativação pós-falha, pode utilizar o controlador de domínio adicionais no site de destino.

## <a name="enable-protection-with-site-recovery"></a>Ativar a proteção com a recuperação de Site

Pode utilizar a recuperação de sites para proteger a máquina virtual que aloja o controlador de domínio ou DNS.

### <a name="protect-the-vm"></a>Proteger a VM
O controlador de domínio que é replicado utilizando a recuperação de Site é utilizado para [ativação pós-falha de teste](#test-failover-considerations). Certifique-se de que cumpre os seguintes requisitos:

1. O controlador de domínio é um servidor de catálogo global.
2. O controlador de domínio deve ser o proprietário da função FSMO para as funções que são necessárias durante uma ativação pós-falha de teste. Caso contrário, estas funções tem de ser [seized](http://aka.ms/ad_seize_fsmo) após a ativação pós-falha.

### <a name="configure-vm-network-settings"></a>Configurar as definições de rede VM
Para a máquina virtual que aloja o controlador de domínio ou DNS, na recuperação de Site, configurar definições de rede sob o **computação e rede** definições da máquina virtual replicada. Isto garante que a máquina virtual está ligada à rede correta após a ativação pós-falha.

## <a name="protect-active-directory"></a>Proteger o Active Directory

### <a name="site-to-site-protection"></a>Proteção de site a site
Crie um controlador de domínio no site secundário. Quando promover o servidor para uma função de controlador de domínio, especifique o nome do mesmo domínio que está a ser utilizado no site primário. Pode utilizar o **serviços e locais do Active Directory** snap-in para configurar as definições no objeto de ligação de site a que são adicionados os sites. Ao configurar as definições de uma ligação de site, pode controlar quando ocorre a replicação entre dois ou mais sites e com que frequência ocorre. Para obter mais informações, consulte [agendar a replicação entre sites](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Proteção de site para o Azure
Primeiro, [criar um controlador de domínio numa Azure virtual network](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Quando promover o servidor para uma função de controlador de domínio, especifique o mesmo nome de domínio que é utilizado no site primário.

Em seguida, [reconfigurar o servidor DNS para a rede virtual](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network) para utilizar o servidor DNS no Azure.

![Rede do Azure](./media/site-recovery-active-directory/azure-network.png)

### <a name="azure-to-azure-protection"></a>Proteção do Azure para o Azure
Primeiro, [criar um controlador de domínio numa Azure virtual network](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Quando promover o servidor para uma função de controlador de domínio, especifique o mesmo nome de domínio que é utilizado no site primário.

Em seguida, [reconfigurar o servidor DNS para a rede virtual](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network) para utilizar o servidor DNS no Azure.

## <a name="test-failover-considerations"></a>Considerações de ativação pós-falha de teste
Para evitar o impacto nas cargas de trabalho de produção, a ativação pós-falha de teste ocorre numa rede que está isolada da rede de produção.

A maioria das aplicações requerem a presença de um controlador de domínio ou um servidor DNS. Por conseguinte, antes da aplicação a ativação pós-falha, terá de criar um controlador de domínio na rede isolada a ser utilizado para ativação pós-falha de teste. A forma mais fácil para o fazer consiste em utilizar a recuperação de sites para replicar uma máquina virtual que aloja um controlador de domínio ou DNS. Em seguida, execute uma ativação pós-falha de teste da máquina de virtual de controlador de domínio antes de executar uma ativação pós-falha do plano de recuperação para a aplicação. Eis como pode fazê-lo:

1. Utilizar a recuperação de Site para [replicar](vmware-azure-tutorial.md) a máquina virtual que aloja o controlador de domínio ou DNS.
2. Crie uma rede isolada. Qualquer rede virtual que criar no Azure está isolada de outras redes por predefinição. Recomendamos que utilize o mesmo intervalo de endereços IP para esta rede que utiliza na sua rede de produção. Não conseguir ative a conetividade site a site nesta rede.
3. Forneça um endereço de IP de DNS na rede isolada. Utilize o endereço IP que espera que a máquina virtual DNS para obter. Se estiver a replicar para o Azure, forneça o endereço IP para a máquina virtual que é utilizada na ativação pós-falha. Introduza o endereço IP, na máquina virtual replicada, no **computação e rede** definições, selecione o **IP de destino** definições.

    ![Rede de teste do Azure](./media/site-recovery-active-directory/azure-test-network.png)

    > [!TIP]
    > Recuperação de site tenta criar máquinas virtuais de teste numa sub-rede com o mesmo nome e utilizando o mesmo endereço IP que é fornecido no **computação e rede** definições da máquina virtual. Se uma sub-rede com o mesmo nome não está disponível na rede virtual do Azure que é fornecida para ativação pós-falha de teste, a máquina virtual de teste é criada na sub-rede primeira por ordem alfabética.
    >
    > Se o endereço IP de destino faz parte da sub-rede selecionada, recuperação de Site tenta criar a máquina virtual de ativação pós-falha de teste utilizando o endereço IP de destino. Se o IP de destino não faz parte da sub-rede selecionada, as máquinas de ativação pós-falha de teste é criada utilizando o seguinte IP disponível na sub-rede selecionada.
    >
    >

### <a name="test-failover-to-a-secondary-site"></a>Ativação pós-falha de teste para um site secundário

1. Se está a replicar para outro site no local e utilizar o DHCP, [configurar o DNS e DHCP para ativação pós-falha de teste](hyper-v-vmm-test-failover.md#prepare-dhcp).
2. Efetue uma ativação pós-falha de teste da máquina de virtual de controlador de domínio que é executado numa rede isolada. Utilize o mais recente disponível *aplicação consistente* ponto de recuperação da máquina de virtual de controlador de domínio para efetuar a ativação pós-falha de teste.
3. Execute uma ativação pós-falha de teste para o plano de recuperação contém máquinas virtuais que a aplicação é executada no.
4. Quando os testes são concluídos, *ativação pós-falha de teste de limpeza* na máquina de virtual do controlador de domínio. Este passo elimina o controlador de domínio que foi criado para ativação pós-falha de teste.


### <a name="remove-references-to-other-domain-controllers"></a>Remover as referências a outros controladores de domínio
Quando seleciona uma ativação pós-falha de teste, não incluem todos os controladores de domínio na rede de teste. Para remover as referências a outros controladores de domínio que existe no seu ambiente de produção, poderá ser necessário [obter funções de FSMO Active Directory](http://aka.ms/ad_seize_fsmo) e [limpeza de metadados](https://technet.microsoft.com/library/cc816907.aspx) faltam os controladores de domínio .


### <a name="issues-caused-by-virtualization-safeguards"></a>Problemas causados pelas proteções de Virtualização

> [!IMPORTANT]
> Algumas das configurações descritas nesta secção não são padrão ou uma predefinição configurações de controlador de domínio. Se não pretender efetuar estas alterações a um controlador de domínio de produção, pode criar um controlador de domínio que está dedicado para a recuperação de Site a utilizar para ativação pós-falha de teste. Efetue estas alterações apenas a esse controlador de domínio.  
>
>

Começando com o Windows Server 2012, [proteções adicionais incorporadas no Active Directory Domain Services (AD DS)](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Estas salvaguardas de ajudam a proteger os controladores de domínio virtualizados contra reverte USN se a plataforma de hipervisor subjacente suporta **VM-GenerationID**. Suporte do Azure **VM-GenerationID**. Por este motivo, os controladores de domínio que executam o Windows Server 2012 ou posterior no Azure máquinas virtuais têm estas salvaguardas adicionais.


Quando **VM-GenerationID** é reposta, o **InvocationID** também é possível repor o valor da base de dados do AD DS. Além disso, o conjunto de RID é rejeitado e SYSVOL está marcado como não autoritativo. Para obter mais informações, consulte [introdução à virtualização de serviços de domínio do Active Directory](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) e [em segurança virtualizar o DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).

Falha ao longo para o Azure poderá fazer com que **VM-GenerationID** repor. Repor **VM-GenerationID** aciona proteções adicionais quando as máquinas do controlador de domínio é iniciado no Azure. Isto pode resultar num *atraso significativo* no conseguir iniciar sessão na máquina de virtual do controlador de domínio.

Como este controlador de domínio é utilizado apenas numa ativação pós-falha de teste, as proteções de Virtualização não são necessárias. Para se certificar de que o **VM-GenerationID** não altera o valor para a máquina de virtual do controlador de domínio, pode alterar o valor do DWORD seguinte para **4** no controlador de domínio no local:


`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start`


#### <a name="symptoms-of-virtualization-safeguards"></a>Sintomas das proteções de Virtualização

Se as proteções de Virtualização são acionadas após uma ativação pós-falha de teste, poderá ver um ou mais dos seguintes sintomas:  

* O **GenerationID** valor alterações.

    ![Alteração do ID de geração](./media/site-recovery-active-directory/Event2170.png)

* O **InvocationID** valor alterações.

    ![Alteração do ID de invocação](./media/site-recovery-active-directory/Event1109.png)

* Partilhas de SYSVOL e NETLOGON não estão disponíveis.

    ![Partilha SYSVOL](./media/site-recovery-active-directory/sysvolshare.png)

    ![NtFrs SYSVOL](./media/site-recovery-active-directory/Event13565.png)

* Bases de dados DFSR são eliminados.

    ![Bases de dados DFSR são eliminados](./media/site-recovery-active-directory/Event2208.png)


### <a name="troubleshoot-domain-controller-issues-during-test-failover"></a>Resolver problemas de controlador de domínio durante a ativação pós-falha de teste

> [!IMPORTANT]
> Algumas das configurações descritas nesta secção não são configurações de controlador de domínio padrão ou uma predefinição. Se não pretender efetuar estas alterações a um controlador de domínio de produção, pode criar um controlador de domínio que está dedicado para ativação pós-falha de teste de recuperação de sites. Efetue as alterações apenas a esse controlador de domínio dedicado.  
>
>

1. Na linha de comandos, execute o seguinte comando para verificar se as pastas SYSVOL e NETLOGON são partilhadas:

    `NET SHARE`

2. Na linha de comandos, execute o seguinte comando para se certificar de que o controlador de domínio está a funcionar corretamente:

    `dcdiag /v > dcdiag.txt`

3. No registo de saída, procure o seguinte texto. O texto confirma que o controlador de domínio está a funcionar corretamente.

    * "test transmitido conectividade"
    * "transmitido teste publicidade"
    * "test transmitido MachineAccount"

Se forem satisfeitas as condições anteriores, é provável que o controlador de domínio está a funcionar corretamente. Se não for, conclua os seguintes passos:

1. Efetue um restauro autoritativa do controlador de domínio. Mantenha as seguintes informações em mente:
    * Apesar de não recomendamos [replicação FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), se utilizar replicação FRS, siga os passos para um restauro autoritativa. O processo é descrito em [utilizando a chave de registo BurFlags para reinicializar um serviço de replicação de ficheiros](https://support.microsoft.com/kb/290762).

        Para obter mais informações sobre BurFlags, consulte a mensagem de blogue [D2 e D4: o que são para?](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Se utilizar a replicação de DFSR, conclua os passos para um restauro autoritativa. O processo é descrito em [forçar uma sincronização autoritativa e não autoritativo para SYSVOL replicado por DFSR (como "D4/D2" para FRS)](https://support.microsoft.com/kb/2218556).

        Também pode utilizar as funções do PowerShell. Para obter mais informações, consulte [funções de PowerShell autoritativo/restauro não autoritativo de DFSR SYSVOL](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/).

2. Contornar o requisito de sincronização inicial, definindo a seguinte chave de registo como **0** no controlador de domínio no local. Se o DWORD não existir, pode criá-la sob o **parâmetros** nós.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations`

    Para obter mais informações, consulte [resolver 4013 de ID de evento de DNS: O servidor DNS não foi possível carregar AD integrado zonas DNS](https://support.microsoft.com/kb/2001093).

3. Desative o requisito de que o servidor de catálogo global estar disponíveis para validar o início de sessão do utilizador. Para tal, no controlador de domínio no local, defina a seguinte chave de registo **1**. Se o DWORD não existir, pode criá-la sob o **Lsa** nós.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures`

    Para obter mais informações, consulte [desativar o requisito de que o servidor de catálogo global estar disponíveis para validar os inícios de sessão do utilizador](http://support.microsoft.com/kb/241789).

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS e controlador de domínio em computadores diferentes
Se o DNS não estiver na mesma máquina virtual como controlador de domínio, tem de criar uma máquina virtual DNS para a ativação pós-falha de teste. Se o DNS e o controlador de domínio não estão na mesma máquina virtual, pode ignorar esta secção.

Pode utilizar um servidor DNS de raiz e criar todas as zonas necessárias. Por exemplo, se o domínio do Active Directory for contoso.com, pode criar uma zona DNS com o nome contoso.com. As entradas que correspondem ao Active Directory têm de ser atualizadas no DNS da seguinte forma:

1. Certifique-se de que estas definições são cumpridos antes de começar a qualquer outra máquina virtual no plano de recuperação:
   * A zona deve ter o nome depois do nome de raiz de floresta.
   * A zona tem de ser de segurança de ficheiros.
   * A zona tem de estar ativada para proteger e nonsecure atualizações.
   * A resolução da máquina virtual que aloja o controlador de domínio deve apontar para o endereço IP da máquina virtual DNS.

2. Execute o seguinte comando na VM que aloja o controlador de domínio:

    `nltest /dsregdns`

3. Execute os seguintes comandos para adicionar uma zona no servidor DNS, permitir atualizações nonsecure e adicione uma entrada para a zona no DNS:

    `dnscmd /zoneadd contoso.com  /Primary`

    `dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1`

    `dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>`

    `dnscmd /config contoso.com /allowupdate 1`

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre [proteger cargas de trabalho empresariais com o Azure Site Recovery](site-recovery-workload.md).
