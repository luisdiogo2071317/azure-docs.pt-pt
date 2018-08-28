---
title: O Azure Stack 1807 atualização | Documentos da Microsoft
description: Saiba mais sobre o que há de novo na atualização 1807 para os sistemas integrados do Azure Stack, incluindo os problemas conhecidos e onde pode transferir a atualização.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/27/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: afbe3ff2e6be4e03f8de8ac2490922c3ec788733
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091385"
---
# <a name="azure-stack-1807-update"></a>Atualização de 1807 de pilha do Azure

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve o conteúdo do pacote de atualização de 1807. Esta atualização inclui melhorias, correções e problemas conhecidos para esta versão do Azure Stack e onde pode transferir a atualização. Problemas conhecidos são divididos em problemas diretamente relacionados com o processo de atualização e problemas com a compilação (após a instalação).

> [!IMPORTANT]  
> Este pacote de atualização é apenas para sistemas integrados do Azure Stack. Não é aplicável este pacote de atualização para o Azure Stack Development Kit.

## <a name="build-reference"></a>Criar a referência

É o número de compilação de atualização do Azure Stack 1807 **1.1807.0.76**.  

### <a name="new-features"></a>Novos recursos

Esta atualização inclui os seguintes aprimoramentos para o Azure Stack.

- <!-- 1658937 | ASDK, IS --> **Iniciar as cópias de segurança através de um agendamento predefinido** -como uma aplicação do Azure Stack agora podem disparar automaticamente cópias de segurança de infra-estrutura periodicamente para eliminar a intervenção humana. O Azure Stack irá limpar também automaticamente a partilha externa para cópias de segurança que sejam mais antigas do que o período de retenção definida. Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](azure-stack-backup-enable-backup-powershell.md).

- <!-- 2496385 | ASDK, IS -->  **Tempo para o tempo total de cópia de segurança de transferência de dados foi adicionados.** Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](azure-stack-backup-enable-backup-powershell.md).

-   <!-- 1702130 | ASDK, IS -->  **Cópia de segurança capacidade externa mostra agora a capacidade correta da partilha externa.** (Anteriormente era isso codificar a 10 GB.) Para obter mais informações, consulte [ativar cópia de segurança para o Azure Stack com o PowerShell](azure-stack-backup-enable-backup-powershell.md).

- <!-- 2508488 |  IS   -->  **Expandir a capacidade** pela [adicionar nós de unidade de escala adicionais](azure-stack-add-scale-node.md).

- <!-- 2753130 |  IS, ASDK   -->  **Os modelos do Resource Manager do Azure suportam agora o elemento de condição** -agora, pode implementar um recurso num modelo do Azure Resource Manager com uma condição. É possível criar o modelo para implementar um recurso com base numa condição, como avaliar se um valor de parâmetro está presente. Para obter informações sobre como utilizar um modelo como uma condição, consulte [implementar condicionalmente um recurso](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) e [secção de variáveis de modelos Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) na documentação do Azure. 

   Também pode utilizar modelos para [implementar recursos para mais de uma subscrição ou grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

- <!--2753073 | IS, ASDK -->  **O suporte de versão do recurso de API de Network foi atualizado** para incluir suporte para a API versão 2017-10-01 de 2015-06-15 para recursos de rede do Azure Stack.  Suporte para versões de recursos entre 2017-10-01 e 2015-06-15 não está incluído nesta versão, mas será incluído numa versão futura.  Consulte a [considerações sobre o funcionamento em rede do Azure Stack](user/azure-stack-network-differences.md) para diferenças de funcionalidade.

- <!-- 2272116 | IS, ASDK   -->  **O Azure Stack adicionou suporte para pesquisas de DNS inversos para direcionados para pontos finais de infraestrutura do Azure Stack** (o que é para o portal, adminportal, gestão e adminmanagement). Isso permite que os nomes de ponto final externo do Azure Stack ser resolvido a partir de um endereço IP.

- <!-- 2780899 |  IS, ASDK   --> **O Azure Stack agora suporta a adição de interfaces de rede adicionais a uma VM existente.**  Esta funcionalidade está disponível com o portal, PowerShell e CLI. Para obter mais informações, consulte [adicionar ou remover interfaces de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) na documentação do Azure. 

- <!-- 2222444 | IS, ASDK   -->  **Foram feitos aprimoramentos na precisão e a resiliência para os medidores de utilização de rede**.  Medidores de utilização de rede estão agora mais precisos e levam em subscrições de conta suspensa, períodos de indisponibilidade e condições de corrida.

- <!-- 2753080 | IS -->  **Atualize notificação de disponibilidade.** Ligado do Azure Stack implementações serão periodicamente verificar um ponto de extremidade seguro e determinar se uma atualização está disponível para a sua nuvem. Esta notificação é apresentada no mosaico de atualização, como aconteceria após a procurar e importar uma nova atualização manualmente. Leia mais sobre [gerir atualizações para o Azure Stack](azure-stack-updates.md).

- <!-- 2297790 | IS, ASDK -->  **Melhorias para o cliente de syslog do Azure Stack (funcionalidade de pré-visualização)**. Este cliente permite que o reencaminhamento de auditoria e registos relacionados com a infraestrutura do Azure Stack para um syslog servidor ou segurança informações e eventos (SIEM) software de gestão externo para o Azure Stack. O cliente de syslog agora suporta o protocolo TCP com texto sem formatação ou encriptação de TLS 1.2, sendo que a última opção é a configuração predefinida. Pode configurar a ligação de TLS com a autenticação seja somente de servidor ou mútua.

  Para configurar como o cliente de syslog comunica (por exemplo, o protocolo, encriptação e autenticação) com o servidor syslog, utilize o *Set-SyslogServer* cmdlet. Este cmdlet está disponível a partir do ponto de extremidade com privilégios (PEP).

  Para adicionar o certificado de cliente para a autenticação mútua de TLS 1.2 do cliente do syslog, utilize o cmdlet Set-SyslogClient no PEP.

  Com esta pré-visualização, pode ver um número muito maior de auditorias e alertas. 

  Uma vez que esta funcionalidade ainda está em pré-visualização, não se baseiam no mesmo em ambientes de produção.

  Para obter mais informações, consulte [reencaminhamento do Azure Stack syslog](azure-stack-integrate-security.md).

<!-- - | ####### | IS, ASDK |  **Azure Resource Manager includes the region name.** With this release, objects retrieved from the Azure Resource Manager will now include the region name attribute. If an existing PowerShell script directly passes the object to another cmdlet, the script may produce an error and fail. This is Azure Resource Manager compliant behavior, and requires the calling client to subtract the region attribute. For more information about the Azure Resource Manager see [Azure Resource Manager Documentation](https://docs.microsoft.com/azure/azure-resource-manager/). verify 8-10 mdb -->

- <!-- TBD | IS, ASDK -->  **Alterações à funcionalidade de fornecedores de delegado.** Começando com 1807 os fornecedores de delegado modelo é simplificado para melhor se alinham com o modelo do revendedor do Azure e fornecedores de delegado não poderá criar outros fornecedores de delegado, essencialmente, mesclar o modelo e tornando o fornecedor delegado funcionalidade disponível num único nível. Para ativar a transição para o novo modelo e a gestão das subscrições, as subscrições de utilizador agora podem ser movidas entre subscrições de fornecedor delegado novas ou existentes que pertencem ao mesmo inquilino do diretório. Subscrições de utilizador que pertence à subscrição do fornecedor predefinida também podem ser movidas para as subscrições do fornecedor delegado no mesmo diretório-inquilino.  Para obter mais informações, consulte [delegar ofertas no Azure Stack](azure-stack-delegated-provider.md).

- <!-- 2536808 IS ASDK --> **Melhorada a hora de criação de VM** para as VMs que são criadas com imagens, transfira a partir do Azure marketplace.

- <!-- TBD | IS, ASDK -->  **Melhorias na usabilidade do Planeador de capacidade de pilha do Azure**. O Azure Stack [Planeador de capacidade](http://aka.ms/azstackcapacityplanner) agora oferece uma experiência simplificada para inserir a S2D cache e a capacidade de S2D para definir a solução SKUs. Foi removido o limite VM de 1000.


### <a name="fixed-issues"></a>Problemas de fixos

- <!-- TBD | ASDK, IS --> Vários aprimoramentos foram feitos para o processo de atualização para que seja mais confiável. Além disso, correções feitas a infraestrutura subjacente, que minimizar o potencial período de indisponibilidade para cargas de trabalho durante a atualização.

- <!--2292271 | ASDK, IS --> Foi corrigido um problema em que um limite de Quota de modificação não foi aplicada a subscrições existentes. Agora, quando gerar um limite de Quota para um recurso de rede que faz parte de uma oferta e plano associados a uma subscrição de utilizador, o novo limite aplica-se a subscrições existentes, bem como as novas subscrições.

- <!-- 448955 | IS ASDK --> Agora pode consultar os registos de atividades para sistemas que são implementados num fuso horário UTC + N com êxito.    

- <!-- 2319627 |  ASDK, IS --> Pré-verificação de parâmetros de configuração de cópia de segurança (chave de caminho/nome de utilizador/palavra-passe/encriptação) já não define definições incorretas para a configuração de cópia de segurança. (Anteriormente, definições incorretas tiverem sido definidas para a cópia de segurança e cópia de segurança, em seguida, iria falhar quando acionado.)

- <!-- 2215948 |  ASDK, IS --> Agora atualiza a lista de cópia de segurança ao eliminar manualmente a cópia de segurança da partilha externa.

- <!-- 2332636 | IS -->  Atualização para esta versão já não repõe o proprietário de predefinição da subscrição do fornecedor de predefinição incorporada **CloudAdmin** utilizador quando implantado com o AD FS.

- <!-- 2360715 |  ASDK, IS -->  Quando configurar a integração no datacenter, já não aceder o ficheiro de metadados do AD FS a partir de uma partilha de ficheiros do Azure. Para obter mais informações, consulte [configurar a integração do AD FS ao fornecer o ficheiro de metadados de Federação](azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> Foi corrigido um problema que impediu que os utilizadores atribuído um endereço IP público existente que tinha sido anteriormente atribuído a uma Interface de rede ou um balanceador de carga para uma nova Interface de rede ou Balanceador de carga.  

- <!-- 2551834 - IS, ASDK --> Quando seleciona *descrição geral* para uma conta de armazenamento em portais do administrador ou utilizador, o *Essentials* painel apresenta agora todas as informações esperadas corretamente. 

- <!-- 2551834 - IS, ASDK --> Quando seleciona *etiquetas* para uma conta de armazenamento em portais do administrador ou utilizador, as informações agora apresentado corretamente.

- <!-- TBD - IS ASDK --> Esta versão do Azure Stack corrige o problema que impediu a aplicação das atualizações de controladores de pacotes de extensão do OEM.

-   <!-- 2055809- IS ASDK --> Foi corrigido um problema que impediu que a eliminar VMs a partir do painel de computação quando a VM não conseguiu ser criado.  

- <!--  2643962 IS ASDK -->  O alerta para *baixa capacidade de memória* deixa de aparecer incorretamente.

- **Várias correções** de desempenho, estabilidade, segurança e o sistema operativo que é utilizado pelo Azure Stack.


<!-- ### Additional releases timed with this update    -->

### <a name="common-vulnerabilities-and-exposures"></a>Vulnerabilidades e exposições comuns
O Azure Stack utiliza instalações Server Core do Windows Server 2016 para a infraestrutura de chaves de anfitrião. Esta versão instala as seguintes atualizações do Windows Server 2016 em servidores de infraestrutura para o Azure Stack: 
- [CVE-2018-8206](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8206)
- [CVE-2018-8222](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8222)
- [CVE-2018-8282](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8282)
- [CVE-2018-8304](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8304)
- [CVE-2018-8307](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8307)
- [CVE-2018-8308](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8308) 
- [CVE-2018-8309](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8309)
- [CVE-2018-8313](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-8313)  

Para obter mais informações sobre essas vulnerabilidades, clique nos links anteriores ou veja os artigos da Base de dados de conhecimento da Microsoft [4338814](https://support.microsoft.com/help/4338814) e [4345418](https://support.microsoft.com/help/4345418).



## <a name="before-you-begin"></a>Antes de começar

### <a name="prerequisites"></a>Pré-requisitos

- Instalar o Azure Stack [1805 atualizar](azure-stack-update-1805.md) antes de aplicar a atualização do Azure Stack 1807.  Não ocorreu nenhuma atualização 1806.  

- Instalar o mais recente disponível [atualização ou correção para a versão 1805](azure-stack-update-1805.md#post-update-steps).  
  > [!TIP]  
  > Subscrever o seguinte procedimento *RRS* ou *Atom* feeds para se manter atualizado com correções do Azure Stack:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss ... 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom ...


- Antes de iniciar a instalação da atualização 1807, execute [AzureStack teste](azure-stack-diagnostic-test.md) para validar o status do seu Azure Stack e resolver quaisquer problemas operacionais encontrados. Também rever alertas ativos e resolver qualquer um que requerem uma ação.

### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos com o processo de atualização

- <!-- 2468613 - IS --> Durante a instalação desta atualização, poderá ver alertas com o título *erro – o modelo para typu FaultType UserAccounts.New está em falta.*  Pode ignorar com segurança estes alertas. Estes alertas serão fechado automaticamente depois de concluída a instalação desta atualização.

- <!-- 2489559 - IS --> Não tente criar máquinas virtuais durante a instalação desta atualização. Para obter mais informações sobre a gestão de atualizações, consulte [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md#plan-for-updates).

- <!-- 2830461 - IS --> Em determinadas circunstâncias quando uma atualização necessita de atenção, o alerta correspondente pode não ser gerado. O estado preciso ainda será refletido no portal e não é afetado.

### <a name="post-update-steps"></a>Passos de pós-atualização

- <!-- 2933866 – IS --> **Estado melhorado para instalações de atualização falhada.** Esta versão apresenta duas novas categorias de estado para fornecer mais detalhes sobre instalações de atualização falhada de operadores. As duas categorias são *PreparationFailed*, e *InstallationFailed*. Depois de instalar esta versão, poderá ver informações de falhas de instalação de atualização anteriores revisto para refletir estas novas categorias. 

<!-- *There are no post-update steps for update 1807.* -->

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - Link to KB  
 -->

## <a name="known-issues-post-installation"></a>Problemas conhecidos (após a instalação)

Seguem-se após a instalação problemas conhecidos para esta versão de compilação.

### <a name="portal"></a>Portal
- <!-- 2931230 – IS  ASDK --> Não não possível eliminar os planos que são adicionados a uma subscrição de utilizador como um plano de suplemento, mesmo quando remover o plano de subscrição de utilizador. O plano irá permanecer até que as subscrições que referenciam o plano de suplemento também são eliminadas. 

- <!--2760466 – IS  ASDK --> Quando instala um novo ambiente do Azure Stack que executar esta versão, o alerta que indica *ativação necessária* poderão não ser apresentados. [Ativação](azure-stack-registration.md) é necessária antes de poder utilizar a distribuição de mercado.  

- <!-- TBD - IS ASDK --> Os dois tipos de subscrição administrativas que estavam [introduzido com a versão 1804](azure-stack-update-1804.md#new-features) não deve ser utilizado. Os tipos de subscrição são **medição subscrição**, e **subscrição de consumo**. Esses tipos de subscrição são visíveis no novos ambientes do Azure Stack a partir da versão 1804, mas ainda não estão prontos a utilizar. Deve continuar a utilizar o **fornecedor predefinido** tipo de subscrição.

- <!-- 2403291 - IS ASDK --> Poderá não ter o uso da barra de deslocamento horizontal na parte inferior de portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a estrutura de ligações para navegar até um painel anterior no portal ao selecionar o nome do painel pretende visualizar na lista de trilha encontrado na parte superior esquerda do portal.

  ![Navegação estrutural](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Pode não ser possível ver os recursos de computação ou o armazenamento no portal do administrador. A causa deste problema é um erro durante a instalação da atualização que faz com que a atualização para ser comunicadas incorretamente conclusão com êxito. Se este problema ocorrer, contacte o suporte técnico da Microsoft para obter assistência.

- <!-- TBD - IS --> Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de engrenagem no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.

- <!-- TBD - IS ASDK --> A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

- <!-- TBD - IS ASDK --> Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.


### <a name="health-and-monitoring"></a>Estado de funcionamento e monitorização
- <!-- 1264761 - IS ASDK -->  Poderá ver alertas para o **controlador de estado de funcionamento** componente que tem os seguintes detalhes:  

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

  Ambos os alertas podem ser ignoradas com segurança e vai ser fechado automaticamente ao longo do tempo.  


- <!-- 2812138 | IS --> Poderá ver um alerta para **armazenamento** componente que tem os seguintes detalhes:

   - NOME: Erro de comunicação interno do serviço de armazenamento  
   - GRAVIDADE: crítico  
   - COMPONENTE: armazenamento  
   - Descrição: Ocorreu um erro de comunicação interna do serviço de armazenamento quando enviar pedidos para os seguintes nós.  

    O alerta pode ser ignorado com segurança, mas terá de fechar o alerta manualmente.

- <!-- 2368581 - IS. ASDK --> Um operador do Azure Stack, se receber um alerta de pouca memória e máquinas de virtuais de inquilino não for possível implementar com um **erro de criação de recursos de infraestrutura de VM**, é possível que o carimbo de data / Azure Stack está disponível memória esgotada. Utilize o [Planeador de capacidade do Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para melhor compreender a capacidade disponível para as cargas de trabalho.


### <a name="compute"></a>Computação

- <!-- 2724873 - IS --> Ao utilizar os cmdlets do PowerShell **Start-AzsScaleUnitNode** ou **Stop-AzsScaleunitNode** para gerir unidades de escala, a primeira tentativa para iniciar ou parar a unidade de escala poderá falhar. Se o cmdlet falhar na primeira execução, execute o cmdlet uma segunda vez. A segunda execução deve ser bem-sucedida para concluir a operação. 

- <!-- 2494144 - IS, ASDK --> Ao selecionar um tamanho de máquina virtual para uma implementação da máquina virtual, alguns tamanhos de VM da série F não estão visíveis como parte do Seletor de tamanho, ao criar uma VM. Os seguintes tamanhos VM não aparecem no Seletor de: *F8s_v2*, *F16s_v2*, *F32s_v2*, e *F64s_v2*.  
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


- <!-- TBD - IS ASDK --> Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.

- <!-- TBD - IS --> Quando cria um conjunto de disponibilidade no portal, vai **New** > **computação** > **do conjunto de disponibilidade**, só pode criar um conjunto de disponibilidade com um domínio de falha e o domínio de atualização de 1. Como solução, ao criar uma nova máquina virtual, criar o conjunto com o PowerShell, CLI, ou a partir de disponibilidade do portal.

- <!-- TBD - IS ASDK --> Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar a uma VM da série DS. Séries de DS VMs capaz de acomodar os discos de dados como a configuração do Azure.

- <!-- TBD - IS ASDK --> Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

- <!-- 1662991 IS ASDK --> Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico.  

- <!-- 2724961- IS ASDK --> Quando registra o **Microsoft.Insight** fornecedor de recursos nas definições de subscrição e criar uma VM do Windows com o convidado SO diagnóstico ativado, a página de descrição geral VM não mostra os dados das métricas. 

   Para localizar dados de métricas, como o gráfico de percentagem de CPU para a VM, vá para o **métricas** métricas de convidado de painel e mostrar todas as VMS suportadas do Windows.

### <a name="networking"></a>Redes  

- <!-- 1766332 - IS ASDK --> Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

- <!-- 1902460 - IS ASDK --> O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

- <!-- 16309153 - IS ASDK --> Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.

- <!-- 2702741 -  IS ASDK --> IPs públicos, que são implementadas utilizando o método alocação dinâmica não é garantido ser preservadas quando uma paragem da Desalocação é emitido.

- <!-- 2529607 - IS ASDK --> Durante o Azure Stack *segredo rotação*, existe um período em que os endereços IP públicos estão inacessíveis para dois a cinco minutos.

-   <!-- 2664148 - IS ASDK --> Em cenários em que o inquilino está a aceder a suas máquinas virtuais utilizando um túnel S2S VPN, poderão surgir um cenário em que as tentativas de ligação falharem se a sub-rede no local foi adicionada ao Gateway de rede Local já foi criado o gateway. 


### <a name="sql-and-mysql"></a>SQL e o MySQL



- <!-- No Number - IS, ASDK -->  Caracteres especiais, incluindo espaços e períodos, não são suportadas no **família** dê um nome ao criar um SKU para os fornecedores de recursos do SQL e o MySQL. 

- <!-- TBD - IS --> O fornecedor de recursos é suportado para criar itens nos servidores desse host SQL ou MySQL. Itens criados num servidor de anfitrião que não são criados pelo fornecedor de recursos podem resultar num estado sem correspondência.  

> [!NOTE]  
> <!-- TBD - IS --> Depois de atualizar para esta versão do Azure Stack, pode continuar a utilizar os fornecedores de recursos do SQL e o MySQL que implementou anteriormente.  Recomendamos que Atualize para o SQL e o MySQL quando uma nova versão estiver disponível. Como o Azure Stack, aplica atualizações de fornecedores de recursos do SQL e o MySQL sequencialmente. Por exemplo, se utilizar a versão 1804, aplicar primeiro a versão 1805 e, em seguida, atualize para 1807.  
>  
> A instalação desta atualização não afeta a utilização atual de fornecedores de recursos SQL ou MySQL pelos seus utilizadores. 
> Independentemente da versão dos fornecedores de recursos que utilizar, os dados de usuários em seus bancos de dados não é tocados e permanecem acessíveis.  

### <a name="app-service"></a>Serviço de Aplicações

- <!-- 2352906 - IS ASDK --> Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.

- <!-- 2489178 - IS ASDK --> Para ampliar a infraestrutura (funções de trabalho, gerenciamento, funções de front-end), tem de utilizar o PowerShell, tal como descrito nas notas de versão para computação.

- <!-- TBD - IS ASDK --> Serviço de aplicações só pode ser implementado para o *subscrição do fornecedor predefinido* neste momento. Numa atualização futura, o serviço de aplicações irá implementar para a nova *medição subscrição* que foi introduzida em 1804 de pilha do Azure. Quando a medição é suportada para utilização, todas as implementações existentes serão migradas para este novo tipo de subscrição.


### <a name="usage"></a>Utilização  
- <!-- TBD - IS ASDK --> Dados de medidor de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Transferir a atualização
Pode transferir o pacote de atualização do Azure Stack 1807 partir [aqui](https://aka.ms/azurestackupdatedownload).


## <a name="next-steps"></a>Passos Seguintes
- Para rever a política de manutenção para sistemas integrados do Azure Stack e o que deve fazer para manter seu sistema num estado suportado, consulte [política de manutenção do Azure Stack](azure-stack-servicing-policy.md).  
- Para utilizar o ponto final com privilégios (PEP) para monitorizar e retomar as atualizações, consulte [monitorizar atualizações no Azure Stack, utilizando o ponto final com privilégios](azure-stack-monitor-update.md).  
- Para uma descrição geral da gestão de atualizações no Azure Stack, veja [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md).  
- Para obter mais informações sobre como aplicar as atualizações com o Azure Stack, veja [aplicar atualizações no Azure Stack](azure-stack-apply-updates.md).  
