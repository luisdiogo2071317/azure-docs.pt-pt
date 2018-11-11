---
title: Notas de versão do Kit de desenvolvimento do Microsoft Azure Stack | Documentos da Microsoft
description: Melhorias, correções e problemas conhecidos ao Kit de desenvolvimento do Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2018
ms.author: sethm
ms.reviewer: misainat
ms.openlocfilehash: 8e8518cdf95e1b97bd4b641322c1b2a3fdc3bf9e
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282463"
---
# <a name="asdk-release-notes"></a>Notas de versão ASDK  
Este artigo fornece informações sobre melhorias, correções e problemas conhecidos no Azure Stack Development Kit (ASDK). Se não tiver a certeza qual é a versão que está a executar, pode [utilizar o portal para verificar](.\.\azure-stack-updates.md#determine-the-current-version).

> Mantenha-se atualizado com o que há de novo no ASDK inscrevendo-se para o [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11809090"></a>Compilação 1.1809.0.90

### <a name="new-features"></a>Novos recursos
Esta compilação inclui as seguintes melhorias e correções para o Azure Stack.  

<!--  2712869   | IS  ASDK -->  
- **Cliente de syslog do Azure Stack (disponibilidade geral)** este cliente permite que o reencaminhamento de auditorias, alertas e registos de segurança relacionados com a infraestrutura do Azure Stack para um syslog segurança ou de servidor de informações e eventos (SIEM) software de gestão externamente em relação ao Azure Stack. Agora, o cliente de syslog suporta especificando a porta em que o servidor syslog está a escutar.

Com esta versão, o cliente de syslog está disponível em geral e pode ser usado em ambientes de produção.

Para obter mais informações, consulte [reencaminhamento do Azure Stack syslog](../azure-stack-integrate-security.md).

### <a name="fixed-issues"></a>Problemas corrigidos

<!-- TBD - IS ASDK -->
- No portal, o gráfico de memória livre/utilizado capacidade de geração de relatórios agora é exato. Pode agora mais fiável prever o número de VMs que é capazes de criar.

<!-- TBD - IS ASDK --> 
- Foi corrigido um problema em que criou as máquinas virtuais no portal de utilizador do Azure Stack e o portal apresentado um número incorreto de discos de dados que pode anexar a uma VM da série DS. Séries de DS VMs capaz de acomodar os discos de dados como a configuração do Azure.

- Os seguintes problemas de disco gerido foram corrigidos na 1809 e também foram corrigidos no 1808 [do Azure Stack correção 1.1808.5.110](https://support.microsoft.com/help/4468920/): 

   <!--  2966665 – IS, ASDK --> 
   - Foi corrigido o problema em que anexar discos de dados SSD para máquinas de virtuais de disco gerido (DS, DSv2, Fs, Fs_V2) falhou com um erro de tamanho de premium: *Falha ao atualizar discos para a máquina virtual "vmname" erro: pedida não é possível efetuar a operação porque tipo de conta de armazenamento "Premium_LRS" não é suportada para o tamanho da VM "Standard_DS/Ds_V2/FS/Fs_v2)*. 
   
   - Criação de um disco gerido de VM usando **createOption**: **Attach** falhar com o seguinte erro: *operação de longa execução falhou com o estado "Falhou". Informações adicionais: "Ocorreu um erro de execução interna."*
   Código de erro: InternalExecutionError ErrorMessage: Ocorreu um erro de execução interna.
   
   Este problema agora foi corrigido.

- <!-- 2702741 -  IS, ASDK --> Foi corrigido o problema no qual os IPs públicos que foram implementadas utilizando a alocação de dinâmica método não eram a garantia de ser preservadas quando uma paragem da Desalocação é emitido. Agora são preservadas.

- <!-- 3078022 - IS, ASDK --> Se uma VM foi parada-desalocada antes de 1808 não pode ser reatribuída após a atualização 1808.  Este problema é resolvido de 1809. Instâncias que foram neste estado e não foi possível iniciar podem ser iniciadas na 1809 com esta correção. A correção também evita que este problema ocorrer.

- **Várias correções** de desempenho, estabilidade, segurança e o sistema operativo que é utilizado pelo Azure Stack


### <a name="changes"></a>Alterações

### <a name="known-issues"></a>Problemas conhecidos

#### <a name="portal"></a>Portal  

<!-- 2515955   | IS ,ASDK--> 
- *Todos os serviços* substitui *mais serviços* nos portais do administrador e utilizador do Azure Stack. Agora, pode utilizar *todos os serviços* como uma alternativa para navegar nos portais do Azure Stack, da mesma forma que nos portais do Azure.

<!--  TBD – IS, ASDK --> 
- *Básico A* tamanhos de máquinas virtuais foram extinguidos para [criar conjuntos de dimensionamento de máquinas virtuais](../azure-stack-compute-add-scalesets.md) (VMSS) através do portal. Para criar um VMSS com este tamanho, utilize o PowerShell ou um modelo.

#### <a name="health-and-monitoring"></a>Estado de funcionamento e monitorização

<!-- 1264761 - IS ASDK -->  
- Poderá ver alertas para o *controlador de estado de funcionamento* componente que tem os seguintes detalhes:  

   #1 do alerta:
   - NOME: Função de infraestrutura mau estado de funcionamento
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O Scanner de Heartbeat do controlador de estado de funcionamento não está disponível. Isto pode afetar as métricas e relatórios de estado de funcionamento.  

  Alerta #2:
   - NOME: Função de infraestrutura mau estado de funcionamento
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O Scanner de falhas de controlador do Estado de funcionamento não está disponível. Isto pode afetar as métricas e relatórios de estado de funcionamento.

  Ambos os alertas podem ser ignoradas com segurança e fechará automaticamente ao longo do tempo.  

<!-- 2368581 - IS. ASDK --> 
- Um operador do Azure Stack, se receber um alerta de pouca memória e máquinas de virtuais de inquilino não for possível implementar com um *erro de criação de recursos de infraestrutura de VM*, é possível que o carimbo de data / Azure Stack está disponível memória esgotada. Utilize o [Planeador de capacidade do Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para melhor compreender a capacidade disponível para as cargas de trabalho.


#### <a name="compute"></a>Computação 

<!-- TBD – IS, ASDK -->
- Reattaching um disco desligado para a mesma máquina virtual (VM) com o mesmo nome e o LUN falha com um erro **não é possível anexar o disco de dados 'datadisk"a VM"vm1"**. O erro ocorre porque o disco está atualmente a ser desligado ou o último desanexar falha na operação. Aguarde até que o disco é completamente desanexado e, em seguida, tente novamente ou elimine/desanexe o disco explicitamente novamente. A solução alternativa é voltar a ligá-lo com um nome diferente ou num LUN diferente. 

<!-- 3235634 – IS, ASDK -->
- Para implementar VMs com tamanhos que contém um **v2** sufixo; por exemplo, **Standard_A2_v2**, especifique o sufixo como **Standard_A2_v2** (v em minúsculas). Não utilize **Standard_A2_V2** (V maiúsculo). Isso funciona no global Azure e é uma inconsistência no Azure Stack.

<!-- 3099544 – IS, ASDK --> 
- Quando cria uma nova máquina de virtual (VM) com o portal do Azure Stack, e selecionar o tamanho da VM, a coluna USD/mês é apresentada com um **indisponível** mensagem. Esta coluna não deve aparecer; exibindo a VM coluna preço não é suportada no Azure Stack.

<!-- 3090289 – IS, ASDK --> 
- Depois de aplicar o 1808 de atualização, pode encontrar os seguintes problemas quando implementar VMs com discos geridos:

   1. Se a subscrição tiver sido criada antes da atualização de 1808, implantação de VM com Managed Disks poderá falhar com uma mensagem de erro interno. Para resolver o problema, siga estes passos para cada subscrição:
      1. No portal do inquilino, aceda a **subscrições** e encontrar a subscrição. Clique em **fornecedores de recursos**, em seguida, clique em **Microsoft. Compute**e, em seguida, clique em **voltar a registar**.
      2. Na mesma subscrição, aceda a **controlo de acesso (IAM)** e certifique-se de que **disco gerido do Azure Stack –** está listado.
   2. Se tiver configurado o ambiente multi-inquilino, a implementação de VMs numa assinatura associada um diretório de convidado poderá falhar com uma mensagem de erro interno. Para resolver o problema, siga estes passos:
      1. Aplicar a [correção de pilha do 1808 Azure](https://support.microsoft.com/help/4468920).
      2. Siga os passos em [este artigo](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) reconfigurar cada um dos seus diretórios de convidado.

<!-- 2869209 – IS, ASDK --> 
- Ao utilizar o [ **Add-AzsPlatformImage** cmdlet](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), tem de utilizar o **- OsUri** parâmetro como a conta de armazenamento URI onde o disco é carregado. Se utilizar o caminho local do disco, o cmdlet falhar com o seguinte erro: *operação de longa execução falhou com o estado 'Com falhas'*. 

<!--  2966665 – IS, ASDK --> 
- Anexar discos de dados SSD para tamanho premium geridos disco máquinas virtuais (DS, DSv2, Fs, Fs_V2) falha com um erro: *Falha ao atualizar discos para a máquina virtual "vmname" erro: pedida não é possível efetuar a operação porque o tipo de conta de armazenamento " Premium_LRS não é suportada para o tamanho da VM "Standard_DS/Ds_V2/FS/Fs_v2)*

   Para contornar este problema, utilize *Standard_LRS* discos de dados em vez de *Premium_LRS discos*. Usar *Standard_LRS* discos de dados não é alterado IOPs ou o custo de faturação.  

<!--  2795678 – IS, ASDK --> 
- Ao utilizar o portal para criar máquinas virtuais (VM) num tamanho VM premium (DS, Ds_v2, FS, FSv2), a VM é criada numa conta de armazenamento standard. Criação de uma conta de armazenamento standard não afeta funcionalmente, IOPs, ou de faturação. 

   Pode ignorar o aviso que diz: *que escolheu utilizar um disco padrão num tamanho que suporte discos premium. Isso pode afetar o desempenho do sistema operativo e não é recomendado. Considere utilizar o armazenamento premium (SSD) em vez disso.*

<!-- 2967447 - IS, ASDK --> 
- Experiência de criar o conjunto de dimensionamento de máquinas virtuais (VMSS) fornece baseada em CentOS 7.2 como uma opção para implementação. Porque essa imagem não está disponível no Azure Stack, selecione outro sistema operacional para a sua implementação ou utilizar um modelo Azure Resource Manager, especificando outra imagem do CentOS que tenha sido transferida antes da implantação do marketplace pela operadora de rede.

<!-- TBD -  IS ASDK --> 
- Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.

<!-- TBD -  IS ASDK --> 
- Quando não for possível criar uma imagem de VM, pode ser adicionado um item com falha que não é possível eliminar para o painel de computação de imagens VM.

  Como solução, criar uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (New-VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item falhado. Em seguida, 15 minutos depois de criar a imagem fictícia, pode com êxito eliminá-la.

  Em seguida, pode repetir a transferência da imagem VM que falhou anteriormente.

<!-- TBD -  IS ASDK --> 
- Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

<!-- 1662991 - IS ASDK --> 
- Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico.

<!-- 2724961- IS ASDK --> 
- Quando registra o **Microsoft.Insight** fornecedor de recursos nas definições de subscrição e criar uma VM do Windows com o convidado SO diagnóstico ativado, o gráfico de percentagem de CPU na página de descrição geral de VM não será capaz de mostrar dados métricos.
 
  Para localizar o gráfico de percentagem de CPU para a VM, vá para o **métricas** métricas de convidado de painel e mostrar todas as VMS suportadas do Windows.

 

#### <a name="networking"></a>Redes
<!-- 1766332 - IS, ASDK --> 
- Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

<!-- 1902460 -  IS ASDK --> 
- O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

<!-- 16309153 -  IS ASDK --> 
- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.

<!-- 2702741 -  IS ASDK --> 
- IPs públicos, que são implementadas utilizando o método alocação dinâmica não é garantido ser preservadas quando uma paragem da Desalocação é emitido.

<!-- 2529607 - IS ASDK --> 
- Durante o Azure Stack *segredo rotação*, existe um período em que os endereços IP públicos estão inacessíveis para dois a cinco minutos.

<!-- 2664148 - IS ASDK --> 
- Em cenários em que o inquilino está a aceder a suas máquinas virtuais utilizando um túnel S2S VPN, poderão surgir um cenário em que as tentativas de ligação falharem se a sub-rede no local foi adicionada ao Gateway de rede Local já foi criado o gateway. 


<!--  #### SQL and MySQL  -->


#### <a name="app-service"></a>Serviço de Aplicações
<!-- 2352906 - IS ASDK --> 
- Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.

<!-- TBD - IS ASDK --> 
- Para ampliar a infraestrutura (funções de trabalho, gerenciamento, funções de front-end), tem de utilizar o PowerShell, tal como descrito nas notas de versão para computação.  


#### <a name="usage"></a>Utilização  
<!-- TBD -  IS ASDK --> 
- Dados de medidor de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.

<!-- #### Identity -->

## <a name="build-11808097"></a>Compilação 1.1808.0.97

### <a name="new-features"></a>Novos recursos
Esta compilação inclui as seguintes melhorias e correções para o Azure Stack.  

<!-- 1658937 | ASDK, IS --> 
- **Iniciar as cópias de segurança através de um agendamento predefinido** -como uma aplicação do Azure Stack agora podem disparar automaticamente cópias de segurança de infra-estrutura periodicamente para eliminar a intervenção humana. O Azure Stack irá limpar também automaticamente a partilha externa para cópias de segurança que sejam mais antigas do que o período de retenção definida. Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](.\.\azure-stack-backup-enable-backup-powershell.md).

<!-- 2496385 | ASDK, IS -->  
- **Tempo para o tempo total de cópia de segurança de transferência de dados foi adicionados.** Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](.\.\azure-stack-backup-enable-backup-powershell.md).

<!-- 1702130 | ASDK, IS --> 
- **Cópia de segurança capacidade externa mostra agora a capacidade correta da partilha externa.** (Anteriormente era isso codificar a 10 GB.) Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](.\.\azure-stack-backup-enable-backup-powershell.md).
 
<!-- 2753130 |  IS, ASDK   -->  
- **Os modelos do Resource Manager do Azure suportam agora o elemento de condição** -agora, pode implementar um recurso num modelo do Azure Resource Manager com uma condição. É possível criar o modelo para implementar um recurso com base numa condição, como avaliar se um valor de parâmetro está presente. Para obter informações sobre como utilizar um modelo como uma condição, consulte [implementar condicionalmente um recurso](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) e [secção de variáveis de modelos Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) na documentação do Azure. 

   Também pode utilizar modelos para [implementar recursos para mais de uma subscrição ou grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

<!--2753073 | IS, ASDK -->  
- **O suporte de versão do recurso de API de Network foi atualizado** para incluir suporte para a API versão 2017-10-01 de 2015-06-15 para recursos de rede do Azure Stack. Suporte para versões de recursos entre 2017-10-01 e 2015-06-15 não está incluído nesta versão. Consulte a [considerações sobre o funcionamento em rede do Azure Stack](.\.\user\azure-stack-network-differences.md) para diferenças de funcionalidade.

<!-- 2272116 | IS, ASDK   -->  
- **O Azure Stack adicionou suporte para pesquisas de DNS inversos para direcionados para pontos finais de infraestrutura do Azure Stack** (o que é para o portal, adminportal, gestão e adminmanagement). Isso permite que os nomes de ponto final externo do Azure Stack ser resolvido a partir de um endereço IP.

<!-- 2780899 |  IS, ASDK   --> 
- **O Azure Stack agora suporta a adição de interfaces de rede adicionais a uma VM existente.**  Esta funcionalidade está disponível com o portal, PowerShell e CLI. Para obter mais informações, consulte [adicionar ou remover interfaces de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) na documentação do Azure. 

<!-- 2222444 | IS, ASDK   -->  
- **Foram feitos aprimoramentos na precisão e a resiliência para os medidores de utilização de rede**. Medidores de utilização de rede estão agora mais precisos e levam em subscrições de conta suspensa, períodos de indisponibilidade e condições de corrida.

<!-- 2297790 | IS, ASDK -->  
- **Melhorias para o cliente de syslog do Azure Stack (funcionalidade de pré-visualização)**. Este cliente permite que o reencaminhamento de auditoria e registos relacionados com a infraestrutura do Azure Stack para um syslog servidor ou segurança informações e eventos (SIEM) software de gestão externo para o Azure Stack. O cliente de syslog agora suporta o protocolo TCP com texto sem formatação ou encriptação de TLS 1.2, sendo que a última opção é a configuração predefinida. Pode configurar a ligação de TLS com a autenticação seja somente de servidor ou mútua.

  Para configurar a forma como o cliente de syslog comunica (por exemplo, o protocolo, encriptação e autenticação) com o servidor syslog, utilize o cmdlet Set-SyslogServer. Este cmdlet está disponível a partir do ponto de extremidade com privilégios (PEP). 

- <!-- ASDK --> **Itens de galeria para conjuntos de dimensionamento de máquinas virtuais estão agora incorporados**.  O conjunto de dimensionamento de máquinas virtuais itens da Galeria são agora disponibilizados nos portais de utilizador e administrador sem ter de transferi-los. 

- <!-- IS, ASDK --> **O conjunto de dimensionamento de máquinas virtuais dimensionamento**.  Pode utilizar o portal para [dimensionar um conjunto de dimensionamento de Máquina Virtual](/azure/azure-stack/azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (VMSS).   

- <!-- 2489570 | IS ASDK--> **Suporte para configurações de política de IPSec/IKE personalizadas** para [gateways de VPN no Azure Stack](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).

- <!-- | IS ASDK--> **Item do marketplace Kubernetes**. Agora, pode implementar clusters Kubernetes com o [item do mercado de Kubernetes](/azure/azure-stack/azure-stack-solution-template-kubernetes-cluster-add). Os utilizadores podem selecionar o item de Kubernetes e preencher alguns parâmetros para implementar um cluster de Kubernetes no Azure Stack. O objetivo dos modelos é simplificar para os utilizadores para implementações de Kubernetes de programador/teste de configuração em poucos passos.

<!-- ####### | IS, ASDK -->  
- **O Azure Resource Manager inclui o nome da região.** Com esta versão, objetos obtidos a partir do Azure Resource Manager agora irão incluir o atributo de nome de região. Se um script de PowerShell existente passar diretamente o objeto em outro cmdlet, o script pode produzir um erro e falhar. Este é o comportamento de conformidade do Azure Resource Manager e requer o cliente da chamada subtrair o atributo de região. Para obter mais informações sobre o Azure Resource Manager, veja [documentação do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/).

<!-- TBD | IS, ASDK -->  
- **Mova subscrições entre fornecedores de delegado.** Agora pode mover subscrições entre subscrições de fornecedor delegado novas ou existentes que pertencem ao mesmo inquilino do diretório. As subscrições que pertencem à subscrição do fornecedor predefinida também podem ser movidas para as subscrições do fornecedor delegado no mesmo diretório-inquilino. Para obter mais informações, consulte [delegar ofertas no Azure Stack](.\.\azure-stack-delegated-provider.md).
 
<!-- 2536808 IS ASDK --> 
- **Melhorada a hora de criação de VM** para as VMs que são criadas com imagens, transfira a partir do Azure marketplace.

### <a name="fixed-issues"></a>Problemas corrigidos
- <!-- IS ASDK--> Foi corrigido o problema para a criação de um conjunto de disponibilidade no portal do que resultou no conjunto de um domínio de falha e o domínio de atualização de 1.

<!-- TBD | ASDK, IS --> 
- Vários aprimoramentos foram feitos para o processo de atualização para que seja mais confiável. Além disso, foram efetuadas correções para a infraestrutura subjacente, o que melhora a drenagem do nó, minimizando, assim, potenciais períodos de inatividade para cargas de trabalho durante a atualização.

<!--2292271 | ASDK, IS --> 
- Foi corrigido um problema em que um limite de Quota de modificação não foi aplicada a subscrições existentes.  Agora, quando gerar um limite de Quota para um recurso de rede que faz parte de uma oferta e plano associados a uma subscrição de utilizador, o novo limite aplica-se a subscrições existentes, bem como as novas subscrições.

<!-- 2448955 | IS ASDK --> 
- Agora pode consultar os registos de atividades para sistemas que são implementados num fuso horário UTC + N com êxito.    

<!-- 2319627 |  ASDK, IS --> 
- Pré-verificação de parâmetros de configuração de cópia de segurança (chave de caminho/nome de utilizador/palavra-passe/encriptação) já não define definições incorretas para a configuração de cópia de segurança. (Anteriormente, definições incorretas tiverem sido definidas para a cópia de segurança e cópia de segurança, em seguida, iria falhar quando acionado.)

<!-- 2215948 |  ASDK, IS --> 
- Agora atualiza a lista de cópia de segurança ao eliminar manualmente a cópia de segurança da partilha externa.

<!-- 2360715 |  ASDK, IS -->  
- Quando configurar a integração no datacenter, já não aceder o ficheiro de metadados do AD FS de um compartilhamento. Para obter mais informações, consulte [configurar a integração do AD FS ao fornecer o ficheiro de metadados de Federação](.\.\azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

<!-- 2388980 | ASDK, IS --> 
- Foi corrigido um problema que impediu que os utilizadores atribuído um endereço IP público existente que tinha sido anteriormente atribuído a uma Interface de rede ou um balanceador de carga para uma nova Interface de rede ou Balanceador de carga.  

<!-- 2551834 - IS, ASDK --> 
- Quando seleciona *descrição geral* para uma conta de armazenamento em portais do administrador ou utilizador, o *Essentials* painel apresenta agora todas as informações esperadas corretamente. 

<!-- 2551834 - IS, ASDK --> 
- Quando seleciona *etiquetas* para uma conta de armazenamento em portais do administrador ou utilizador, as informações agora apresentado corretamente.

<!-- TBD - IS ASDK --> 
- Esta versão do Azure Stack corrige o problema que impediu a aplicação das atualizações de controladores de pacotes de extensão do OEM.

<!-- 2055809- IS ASDK --> 
- Foi corrigido um problema que impediu que a eliminar VMs a partir do painel de computação quando a VM não conseguiu ser criado.  

<!--  2643962 IS ASDK -->  
- O alerta para *baixa capacidade de memória* deixa de aparecer incorretamente.

<!--  TBD ASDK --> 
- A máquina virtual que aloja o ponto final de privilégio (PEP) foi aumentada para 4GB. ASDK, esta máquina virtual tem o nome AzS-ERCS01.

- <!--  TBD – IS, ASDK --> *Básico A* tamanhos de máquinas virtuais foram extinguidos para [criar conjuntos de dimensionamento de máquinas virtuais](.\.\azure-stack-compute-add-scalesets.md) (VMSS) através do portal. Para criar um VMSS com este tamanho, utilize o PowerShell ou um modelo. 

### <a name="known-issues"></a>Problemas conhecidos

#### <a name="portal"></a>Portal  
- <!-- 2967387 – IS, ASDK --> A conta que utiliza para iniciar sessão no portal de utilizador ou administrador do Azure Stack indicará **utilizador não identificado**. Isto ocorre quando a conta não tem qualquer um *primeira* ou *última* nome especificado. Para contornar este problema, edite a conta de utilizador para fornecer o primeiro ou último nome. Deve, em seguida, termine sessão e, em seguida, inicie sessão novamente para o portal. 

-  <!--  2873083 - IS ASDK --> Quando utiliza o portal para criar um dimensionamento de máquina virtual definido (VMSS), o *tamanho da instância* pendente não carregados corretamente ao utilizar o Internet Explorer. Para contornar este problema, use outro navegador ao utilizar o portal para criar um VMSS.  

#### <a name="portal"></a>Portal  
<!-- 2931230 – IS  ASDK --> 
- Não não possível eliminar os planos que são adicionados a uma subscrição de utilizador como um plano de suplemento, mesmo quando remover o plano de subscrição de utilizador. O plano irá permanecer até que as subscrições que referenciam o plano de suplemento também são eliminadas. 

<!--2760466 – IS  ASDK --> 
- Quando instala um novo ambiente do Azure Stack que executar esta versão, o alerta que indica *ativação necessária* poderão não ser apresentados. [Ativação](.\.\azure-stack-registration.md) é necessária antes de poder utilizar a distribuição de mercado. 

<!-- TBD - IS ASDK --> 
- Os dois tipos de subscrição administrativas que foram introduzidos com versão 1804 não devem ser utilizados. Os tipos de subscrição são **medição subscrição**, e **subscrição de consumo**. Esses tipos de subscrição são **medição subscrição**, e **subscrição de consumo**. Esses tipos de subscrição são visíveis no novos ambientes do Azure Stack a partir da versão 1804, mas ainda não estão prontos a utilizar. Deve continuar a utilizar o **subscrição do fornecedor predefinido** tipo.

<!-- 2403291 - IS ASDK --> 
- Poderá não ter o uso da barra de deslocamento horizontal na parte inferior de portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a estrutura de ligações para navegar até um painel anterior no portal ao selecionar o nome do painel pretende visualizar na lista de trilha encontrado na parte superior esquerda do portal.
  ![Navegação estrutural](media/asdk-release-notes/breadcrumb.png)

<!-- TBD -  IS ASDK --> 
- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

<!-- TBD -  IS ASDK --> 
- Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.

<!--  TBD | ASDK -->  
- O fuso horário predefinido para a sua implementação do Azure Stack irá agora é definido para UTC. Pode selecionar um fuso horário ao instalar o Azure Stack, no entanto, ele automaticamente reverterá para UTC como predefinição durante a instalação.

#### <a name="health-and-monitoring"></a>Estado de funcionamento e monitorização
<!-- 1264761 - IS ASDK -->  
- Poderá ver alertas para o *controlador de estado de funcionamento* componente que tem os seguintes detalhes:  

   #1 do alerta:
   - NOME: Função de infraestrutura mau estado de funcionamento
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O Scanner de Heartbeat do controlador de estado de funcionamento não está disponível. Isto pode afetar as métricas e relatórios de estado de funcionamento.  

  Alerta #2:
   - NOME: Função de infraestrutura mau estado de funcionamento
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O Scanner de falhas de controlador do Estado de funcionamento não está disponível. Isto pode afetar as métricas e relatórios de estado de funcionamento.

  Ambos os alertas podem ser ignoradas com segurança e fechará automaticamente ao longo do tempo.  

<!-- 2368581 - IS. ASDK --> 
- Um operador do Azure Stack, se receber um alerta de pouca memória e máquinas de virtuais de inquilino não for possível implementar com um *erro de criação de recursos de infraestrutura de VM*, é possível que o carimbo de data / Azure Stack está disponível memória esgotada. Utilize o [Planeador de capacidade do Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para melhor compreender a capacidade disponível para as cargas de trabalho.

<!-- TBD - IS. ASDK --> 
- Ao executar o **teste AzureStack** cmdlet no ponto final com privilégios (PEP), o **desempenho de instâncias de função do Azure Stack infraestrutura** teste irá gerar uma mensagem de aviso para a VM ERCS. Pode ignorar a mensagem de aviso e continuar a utilizar o ASDK com segurança.

#### <a name="compute"></a>Computação
<!-- 2494144 - IS, ASDK --> 
- Ao selecionar um tamanho de máquina virtual para uma implementação da máquina virtual, alguns tamanhos de VM da série F não estão visíveis como parte do Seletor de tamanho, ao criar uma VM. Os seguintes tamanhos VM não aparecem no Seletor de: *F8s_v2*, *F16s_v2*, *F32s_v2*, e *F64s_v2*.  
  Como solução, utilize um dos seguintes métodos para implementar uma VM. Cada método, tem de especificar o tamanho da VM que pretende utilizar.

- <!-- 3099544 – IS, ASDK --> Quando cria uma nova máquina de virtual (VM) com o portal do Azure Stack, e selecionar o tamanho da VM, a coluna USD/mês é apresentada com um **indisponível** mensagem. Esta coluna não deve aparecer; exibindo a VM coluna preço não é suportada no Azure Stack.

- <!-- 2869209 – IS, ASDK --> Ao utilizar o [ **Add-AzsPlatformImage** cmdlet](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), tem de utilizar o **- OsUri** parâmetro como a conta de armazenamento URI onde o disco é carregado. Se utilizar o caminho local do disco, o cmdlet falhar com o seguinte erro: *operação de longa execução falhou com o estado 'Com falhas'*. 

- <!--  2966665 – IS, ASDK --> Anexar discos de dados SSD para tamanho premium geridos disco máquinas virtuais (DS, DSv2, Fs, Fs_V2) falha com um erro: *Falha ao atualizar discos para a máquina virtual "vmname" erro: pedida não é possível efetuar a operação porque o tipo de conta de armazenamento " Premium_LRS não é suportada para o tamanho da VM "Standard_DS/Ds_V2/FS/Fs_v2)*

   Para contornar este problema, utilize *Standard_LRS* discos de dados em vez de *Premium_LRS discos*. Usar *Standard_LRS* discos de dados não é alterado IOPs ou o custo de faturação.  

- <!--  2795678 – IS, ASDK --> Ao utilizar o portal para criar máquinas virtuais (VM) num tamanho VM premium (DS, Ds_v2, FS, FSv2), a VM é criada numa conta de armazenamento standard. Criação de uma conta de armazenamento standard não afeta funcionalmente, IOPs, ou de faturação. 

   Pode ignorar o aviso que diz: *que escolheu utilizar um disco padrão num tamanho que suporte discos premium. Isso pode afetar o desempenho do sistema operativo e não é recomendado. Considere utilizar o armazenamento premium (SSD) em vez disso.*

- <!-- 2967447 - IS, ASDK --> Experiência de criar o conjunto de dimensionamento de máquinas virtuais (VMSS) fornece baseada em CentOS 7.2 como uma opção para implementação. Porque essa imagem não está disponível no Azure Stack, selecione outro sistema operacional para a sua implementação ou utilizar um modelo ARM especificando outra imagem do CentOS que foi transferida pela operadora de rede antes da implantação do marketplace.

<!-- TBD -  IS ASDK --> 
- Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.

<!-- TBD -  IS ASDK --> 
- Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar uma VM da série D. Série de D suportada todas as VMs pode acomodar os discos de dados como a configuração do Azure.

<!-- TBD -  IS ASDK --> 
- Quando não for possível criar uma imagem de VM, pode ser adicionado um item com falha que não é possível eliminar para o painel de computação de imagens VM.

  Como solução, criar uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (New-VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item falhado. Em seguida, 15 minutos depois de criar a imagem fictícia, pode com êxito eliminá-la.

  Em seguida, pode repetir a transferência da imagem VM que falhou anteriormente.

<!-- TBD -  IS ASDK --> 
- Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

<!-- 1662991 - IS ASDK --> 
- Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico.

<!-- 2724961- IS ASDK --> 
- Quando registra o **Microsoft.Insight** fornecedor de recursos nas definições de subscrição e criar uma VM do Windows com o convidado SO diagnóstico ativado, a página de descrição geral VM não mostra os dados das métricas. 

 

#### <a name="networking"></a>Redes
<!-- 1766332 - IS, ASDK --> 
- Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

<!-- 1902460 -  IS ASDK --> 
- O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

<!-- 16309153 -  IS ASDK --> 
- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.

<!-- 2702741 -  IS ASDK --> 
- IPs públicos, que são implementadas utilizando o método alocação dinâmica não é garantido ser preservadas quando uma paragem da Desalocação é emitido.

<!-- 2529607 - IS ASDK --> 
- Durante o Azure Stack *segredo rotação*, existe um período em que os endereços IP públicos estão inacessíveis para dois a cinco minutos.

<!-- 2664148 - IS ASDK --> 
- Em cenários em que o inquilino está a aceder a suas máquinas virtuais utilizando um túnel S2S VPN, poderão surgir um cenário em que as tentativas de ligação falharem se a sub-rede no local foi adicionada ao Gateway de rede Local já foi criado o gateway. 


#### <a name="sql-and-mysql"></a>SQL e o MySQL
<!-- TBD - ASDK --> 
- Servidores a alojar a base de dados tem de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo serviços de aplicações.

<!-- IS, ASDK --> 
- Caracteres especiais, incluindo espaços e períodos, não são suportadas no **família** dê um nome ao criar um SKU para os fornecedores de recursos do SQL e o MySQL.

#### <a name="app-service"></a>Serviço de Aplicações
<!-- 2352906 - IS ASDK --> 
- Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.

<!-- TBD - IS ASDK --> 
- Para ampliar a infraestrutura (funções de trabalho, gerenciamento, funções de front-end), tem de utilizar o PowerShell, tal como descrito nas notas de versão para computação.  

<!-- TBD - IS ASDK --> 
- Serviço de aplicações só pode ser implementado para o *subscrição do fornecedor predefinido* neste momento.

#### <a name="usage"></a>Utilização  
<!-- TBD -  IS ASDK --> 
- Dados de medidor de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.

<!-- #### Identity -->


## <a name="build-11807076"></a>Compilação 1.1807.0.76

### <a name="new-features"></a>Novos recursos
Esta compilação inclui as seguintes melhorias e correções para o Azure Stack.  

- <!-- 1658937 | ASDK, IS --> **Iniciar as cópias de segurança através de um agendamento predefinido** -como uma aplicação do Azure Stack agora podem disparar automaticamente cópias de segurança de infra-estrutura periodicamente para eliminar a intervenção humana. O Azure Stack irá limpar também automaticamente a partilha externa para cópias de segurança que sejam mais antigas do que o período de retenção definida. Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](.\.\azure-stack-backup-enable-backup-powershell.md).

- <!-- 2496385 | ASDK, IS -->  **Tempo para o tempo total de cópia de segurança de transferência de dados foi adicionados.** Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](.\.\azure-stack-backup-enable-backup-powershell.md).

-   <!-- 1702130 | ASDK, IS -->  **Cópia de segurança capacidade externa mostra agora a capacidade correta da partilha externa.** (Anteriormente era isso codificar a 10 GB.) Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](.\.\azure-stack-backup-enable-backup-powershell.md).
 
- <!-- 2753130 |  IS, ASDK   -->  **Os modelos do Resource Manager do Azure suportam agora o elemento de condição** -agora, pode implementar um recurso num modelo do Azure Resource Manager com uma condição. É possível criar o modelo para implementar um recurso com base numa condição, como avaliar se um valor de parâmetro está presente. Para obter informações sobre como utilizar um modelo como uma condição, consulte [implementar condicionalmente um recurso](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) e [secção de variáveis de modelos Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) na documentação do Azure. 

   Também pode utilizar modelos para [implementar recursos para mais de uma subscrição ou grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

- <!--2753073 | IS, ASDK -->  **O suporte de versão do recurso de API de Network foi atualizado** para incluir suporte para a API versão 2017-10-01 de 2015-06-15 para recursos de rede do Azure Stack.  Suporte para versões de recursos entre 2017-10-01 e 2015-06-15 não está incluído nesta versão, mas será incluído numa versão futura.  Consulte a [considerações sobre o funcionamento em rede do Azure Stack](.\.\user\azure-stack-network-differences.md) para diferenças de funcionalidade.

- <!-- 2272116 | IS, ASDK   -->  **O Azure Stack adicionou suporte para pesquisas de DNS inversos para direcionados para pontos finais de infraestrutura do Azure Stack** (o que é para o portal, adminportal, gestão e adminmanagement). Isso permite que os nomes de ponto final externo do Azure Stack ser resolvido a partir de um endereço IP.

- <!-- 2780899 |  IS, ASDK   --> **O Azure Stack agora suporta a adição de interfaces de rede adicionais a uma VM existente.**  Esta funcionalidade está disponível com o portal, PowerShell e CLI. Para obter mais informações, consulte [adicionar ou remover interfaces de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) na documentação do Azure. 

- <!-- 2222444 | IS, ASDK   -->  **Foram feitos aprimoramentos na precisão e a resiliência para os medidores de utilização de rede**. Medidores de utilização de rede estão agora mais precisos e levam em subscrições de conta suspensa, períodos de indisponibilidade e condições de corrida.

- <!-- 2297790 | IS, ASDK -->  **Melhorias para o cliente de syslog do Azure Stack (funcionalidade de pré-visualização)**. Este cliente permite que o reencaminhamento de auditoria e registos relacionados com a infraestrutura do Azure Stack para um syslog servidor ou segurança informações e eventos (SIEM) software de gestão externo para o Azure Stack. O cliente de syslog agora suporta o protocolo TCP com texto sem formatação ou encriptação de TLS 1.2, sendo que a última opção é a configuração predefinida. Pode configurar a ligação de TLS com a autenticação seja somente de servidor ou mútua.

<!-- 2297790 - IS, ASDK --> 
- **O Azure Stack inclui agora uma *Syslog* cliente** como uma *funcionalidade de pré-visualização*. Este cliente permite que o reencaminhamento de registos de auditoria e segurança relacionados com a infraestrutura do Azure Stack para um Syslog servidor ou segurança informações e eventos (SIEM) software de gestão que é externo ao Azure Stack. Atualmente, o cliente de Syslog só suporta ligações não autenticadas de UDP através da porta de predefinição 514. O payload de cada mensagem do Syslog é formatado em comum o formato de evento (CEF).

  Com esta pré-visualização, pode ver um número muito maior de auditorias e alertas. 

  Uma vez que esta funcionalidade ainda está em pré-visualização, não confiar no mesmo em ambientes de produção.

  Para obter mais informações, consulte [reencaminhamento do Azure Stack syslog](.\.\azure-stack-integrate-security.md).

- <!-- ####### | IS, ASDK -->  **O Azure Resource Manager inclui o nome da região.** Com esta versão, objetos obtidos a partir do Azure Resource Manager agora irão incluir o atributo de nome de região. Se um script de PowerShell existente passar diretamente o objeto em outro cmdlet, o script pode produzir um erro e falhar. Este é o comportamento de conformidade do Azure Resource Manager e requer o cliente da chamada subtrair o atributo de região. Para obter mais informações sobre o Azure Resource Manager, veja [documentação do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/).

- <!-- TBD | IS, ASDK -->  **Mova subscrições entre fornecedores de delegado.** Agora pode mover subscrições entre subscrições de fornecedor delegado novas ou existentes que pertencem ao mesmo inquilino do diretório. As subscrições que pertencem à subscrição do fornecedor predefinida também podem ser movidas para as subscrições do fornecedor delegado no mesmo diretório-inquilino. Para obter mais informações, consulte [delegar ofertas no Azure Stack](.\.\azure-stack-delegated-provider.md).
 
- <!-- 2536808 IS ASDK --> **Melhorada a hora de criação de VM** para as VMs que são criadas com imagens, transfira a partir do Azure marketplace.

### <a name="fixed-issues"></a>Problemas corrigidos

- <!-- TBD | ASDK, IS --> Vários aprimoramentos foram feitos para o processo de atualização para que seja mais confiável. Além disso, foram efetuadas correções para a infraestrutura subjacente, o que melhora a drenagem do nó, minimizando, assim, potenciais períodos de inatividade para cargas de trabalho durante a atualização.

-   <!--2292271 | ASDK, IS --> Foi corrigido um problema em que um limite de Quota de modificação não foi aplicada a subscrições existentes.  Agora, quando gerar um limite de Quota para um recurso de rede que faz parte de uma oferta e plano associados a uma subscrição de utilizador, o novo limite aplica-se a subscrições existentes, bem como as novas subscrições.

- <!-- 2448955 | IS ASDK --> Agora pode consultar os registos de atividades para sistemas que são implementados num fuso horário UTC + N com êxito.    

- <!-- 2319627 |  ASDK, IS --> Pré-verificação de parâmetros de configuração de cópia de segurança (chave de caminho/nome de utilizador/palavra-passe/encriptação) já não define definições incorretas para a configuração de cópia de segurança. (Anteriormente, definições incorretas tiverem sido definidas para a cópia de segurança e cópia de segurança, em seguida, iria falhar quando acionado.)

- <!-- 2215948 |  ASDK, IS --> Agora atualiza a lista de cópia de segurança ao eliminar manualmente a cópia de segurança da partilha externa.

- <!-- 2360715 |  ASDK, IS -->  Quando configurar a integração no datacenter, já não aceder o ficheiro de metadados do AD FS de um compartilhamento. Para obter mais informações, consulte [configurar a integração do AD FS ao fornecer o ficheiro de metadados de Federação](.\.\azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> Foi corrigido um problema que impediu que os utilizadores atribuído um endereço IP público existente que tinha sido anteriormente atribuído a uma Interface de rede ou um balanceador de carga para uma nova Interface de rede ou Balanceador de carga.  

- <!-- 2551834 - IS, ASDK --> Quando seleciona *descrição geral* para uma conta de armazenamento em portais do administrador ou utilizador, o *Essentials* painel apresenta agora todas as informações esperadas corretamente. 

- <!-- 2551834 - IS, ASDK --> Quando seleciona *etiquetas* para uma conta de armazenamento em portais do administrador ou utilizador, as informações agora apresentado corretamente.

- <!-- TBD - IS ASDK --> Esta versão do Azure Stack corrige o problema que impediu a aplicação das atualizações de controladores de pacotes de extensão do OEM.

-   <!-- 2055809- IS ASDK --> Foi corrigido um problema que impediu que a eliminar VMs a partir do painel de computação quando a VM não conseguiu ser criado.  

- <!--  2643962 IS ASDK -->  O alerta para *baixa capacidade de memória* deixa de aparecer incorretamente.

<!--  TBD ASDK --> 
- A máquina virtual que aloja o ponto final de privilégio (PEP) foi aumentada para 4GB. ASDK, esta máquina virtual tem o nome AzS-ERCS01.

- **Várias correções** de desempenho, estabilidade, segurança e o sistema operativo que é utilizado pelo Azure Stack


<!-- ### Changes  -->
<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Problemas conhecidos

#### <a name="portal"></a>Portal
<!-- 2931230 – IS  ASDK --> 
- Não não possível eliminar os planos que são adicionados a uma subscrição de utilizador como um plano de suplemento, mesmo quando remover o plano de subscrição de utilizador. O plano irá permanecer até que as subscrições que referenciam o plano de suplemento também são eliminadas. 

<!-- 2551834 - IS, ASDK --> 
- Quando seleciona **descrição geral** para uma conta de armazenamento em portais do administrador ou utilizador, as informações a partir do *Essentials* não apresentar o painel.  Painel Essentials apresenta informações sobre a conta, como o respetivo *grupo de recursos*, *localização*, e *ID de subscrição*.  Outras opções de descrição geral são acessíveis, como *serviços* e *monitorização*, bem como opções para *abrir no Explorador de* ou *eliminar conta de armazenamento* .  

  Para ver as informações disponíveis, utilize o [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) cmdlet do PowerShell.

<!-- 2551834 - IS, ASDK --> 
- Quando seleciona **etiquetas** para uma conta de armazenamento em portais do administrador ou utilizador, as informações de falha ao carregar e não apresenta.  

  Para ver as informações disponíveis, utilize o [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) cmdlet do PowerShell.

<!-- TBD - IS ASDK --> 
- Não utilize os novos tipos de subscrição administrativos do *medição subscrição*, e *subscrição de consumo*. Esses novos tipos de subscrição foram introduzidos com versão 1804, mas ainda não estão prontos a utilizar. Deve continuar a utilizar o *fornecedor predefinido* tipo de subscrição.
  
<!-- TBD - IS ASDK --> 
- Não é possível aplicar atualizações de controladores mediante a utilização de um pacote de extensão de OEM com esta versão do Azure Stack.  Não é uma solução para este problema.
 
<!-- TBD - IS ASDK --> 
- A capacidade [para abrir um novo pedido de suporte na lista pendente](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para o Azure Stack Development Kit, utilize https://aka.ms/azurestackforum.    

<!-- 2403291 - IS ASDK --> 
- Poderá não ter o uso da barra de deslocamento horizontal na parte inferior de portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a estrutura de ligações para navegar até um painel anterior no portal ao selecionar o nome do painel pretende visualizar na lista de trilha encontrado na parte superior esquerda do portal.
  ![Navegação estrutural](media/asdk-release-notes/breadcrumb.png)

<!-- TBD -  IS ASDK --> 
- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

<!-- TBD -  IS ASDK --> 
- Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.

- <!--  TBD | ASDK -->  O fuso horário predefinido para a sua implementação do Azure Stack irá agora é definido para UTC. Pode selecionar um fuso horário ao instalar o Azure Stack, no entanto, ele automaticamente reverterá para UTC como predefinição durante a instalação.

#### <a name="health-and-monitoring"></a>Estado de funcionamento e monitorização
<!-- 1264761 - IS ASDK -->  
- Poderá ver alertas para o *controlador de estado de funcionamento* componente que tem os seguintes detalhes:  

   #1 do alerta:
   - NOME: Função de infraestrutura mau estado de funcionamento
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O Scanner de Heartbeat do controlador de estado de funcionamento não está disponível. Isto pode afetar as métricas e relatórios de estado de funcionamento.  

  Alerta #2:
   - NOME: Função de infraestrutura mau estado de funcionamento
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O Scanner de falhas de controlador do Estado de funcionamento não está disponível. Isto pode afetar as métricas e relatórios de estado de funcionamento.

  Ambos os alertas podem ser ignoradas com segurança e fechará automaticamente ao longo do tempo.  

<!-- 2368581 - IS. ASDK --> 
- Um operador do Azure Stack, se receber um alerta de pouca memória e máquinas de virtuais de inquilino não for possível implementar com um *erro de criação de recursos de infraestrutura de VM*, é possível que o carimbo de data / Azure Stack está disponível memória esgotada. Utilize o [Planeador de capacidade do Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para melhor compreender a capacidade disponível para as cargas de trabalho.

<!-- TBD - IS. ASDK --> 
- Ao executar o cmdlet de teste AzureStack no ponto final de privilégio (PEP), irá gerar uma mensagem de aviso para a VM ERCS. Pode continuar a utilizar o ASDK. 


#### <a name="compute"></a>Computação
<!-- TBD - IS, ASDK --> 
- Ao selecionar um tamanho de máquina virtual para uma implementação da máquina virtual, alguns tamanhos de VM da série F não estão visíveis como parte do Seletor de tamanho, ao criar uma VM. Os seguintes tamanhos VM não aparecem no Seletor de: *F8s_v2*, *F16s_v2*, *F32s_v2*, e *F64s_v2*.  
  Como solução, utilize um dos seguintes métodos para implementar uma VM. Cada método, tem de especificar o tamanho da VM que pretende utilizar.

  - **Modelo de Gestor de recursos do Azure:** quando utiliza um modelo, defina o *vmSize* no modelo para igual o tamanho da VM que pretende utilizar. Por exemplo, a entrada seguinte é utilizada para implementar uma VM que utiliza a *F32s_v2* tamanho:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **CLI do Azure:** pode utilizar o [az vm crie](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) comando e especifique o tamanho da VM como um parâmetro, semelhante a `--size "Standard_F32s_v2"`.

  - **PowerShell:** com o PowerShell, pode usar [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) com o parâmetro que especifica o tamanho da VM, semelhante às `-VMSize "Standard_F32s_v2"`.


<!-- TBD -  IS ASDK --> 
- Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.

<!-- TBD -  IS ASDK --> 
- Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar uma VM da série D. Série de D suportada todas as VMs pode acomodar os discos de dados como a configuração do Azure.

<!-- TBD -  IS ASDK --> 
- Quando não for possível criar uma imagem de VM, pode ser adicionado um item com falha que não é possível eliminar para o painel de computação de imagens VM.

  Como solução, criar uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (New-VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item falhado. Em seguida, 15 minutos depois de criar a imagem fictícia, pode com êxito eliminá-la.

  Em seguida, pode repetir a transferência da imagem VM que falhou anteriormente.

<!-- TBD -  IS ASDK --> 
- Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

<!-- 1662991 - IS ASDK --> 
- Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico.

#### <a name="networking"></a>Redes
<!-- TBD - IS ASDK --> 
- Não é possível criar rotas definidas pelo utilizador no portal do administrador ou utilizador. Como solução, utilize [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

<!-- 1766332 - IS, ASDK --> 
- Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

<!-- 2388980 -  IS ASDK --> 
- Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar VM desse endereço IP. Desassociação parece funcionar, mas o endereço IP público atribuído anteriormente que permanece associado com a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que criar.

  Este comportamento ocorre mesmo que a reatribuir o endereço IP a uma VM nova (geralmente conhecida como uma *alternância de VIP*). Todos os futuro tenta se conectar por meio deste resultado de endereço IP numa ligação para a VM original e não para a nova.

<!-- 2292271 - IS ASDK --> 
- Se aumentar um limite de Quota para um recurso de rede que faz parte de uma oferta e o plano que está associado uma subscrição de inquilino, o novo limite não se aplica a essa subscrição. No entanto, o novo limite é aplicável às novas assinaturas que são criadas após a quota for aumentada.

  Para contornar este problema, utilize um plano de suplemento para aumentar uma Quota de rede quando o plano já está associado uma subscrição. Para obter mais informações, consulte como [disponibilizar um plano de suplemento](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

<!-- 2304134 IS ASDK --> 
- Não é possível eliminar uma subscrição que tem recursos de zona DNS ou os recursos de tabela de rotas associados ao mesmo. Para eliminar com êxito a subscrição, tem primeiro de eliminar os recursos de zona DNS e a tabela de rotas da subscrição do inquilino.

<!-- 1902460 -  IS ASDK --> 
- O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

<!-- 16309153 -  IS ASDK --> 
- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.

<!-- TBD -  IS ASDK --> 
- O Azure Stack não suporta a adição de interfaces de rede adicionais para uma instância VM depois de implementada a VM. Se a VM exigir mais de uma interface de rede, tem de ser definidos no momento da implementação.


#### <a name="sql-and-mysql"></a>SQL e o MySQL
<!-- TBD - ASDK --> 
- Servidores a alojar a base de dados tem de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo serviços de aplicações.

<!-- IS, ASDK --> 
- Caracteres especiais, incluindo espaços e períodos, não são suportadas no **família** dê um nome ao criar um SKU para os fornecedores de recursos do SQL e o MySQL.

#### <a name="app-service"></a>Serviço de Aplicações
<!-- 2352906 - IS ASDK --> 
- Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.

<!-- TBD - IS ASDK --> 
- Para ampliar a infraestrutura (funções de trabalho, gerenciamento, funções de front-end), tem de utilizar o PowerShell, tal como descrito nas notas de versão para computação.  

<!-- TBD - IS ASDK --> 
- Serviço de aplicações só pode ser implementado para o *subscrição do fornecedor predefinido* neste momento. <!-- In a future update, App Service will deploy into the new *Metering subscription* that was introduced in Azure Stack 1804. When Metering is supported for use, all existing deployments will be migrated to this new subscription type. -->

#### <a name="usage"></a>Utilização  
<!-- TBD -  IS ASDK --> 
- Dados de medidor de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.

<!-- #### Identity -->
