---
title: O Azure Stack 1809 atualização | Documentos da Microsoft
description: Saiba mais sobre o que há de novo na atualização 1809 para os sistemas integrados do Azure Stack, incluindo os problemas conhecidos e onde pode transferir a atualização.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: 4b83d3743f65a3ad1b757c2e6dee8416701b24f3
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48243113"
---
# <a name="azure-stack-1809-update"></a>Atualização de 1809 de pilha do Azure

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve o conteúdo do pacote de atualização de 1809. O pacote de atualização inclui melhorias, correções e problemas conhecidos para esta versão do Azure Stack. Este artigo também inclui uma ligação para que possa transferir a atualização. Problemas conhecidos são divididos em problemas diretamente relacionados com o processo de atualização e problemas com a compilação (após a instalação).

> [!IMPORTANT]  
> Este pacote de atualização é apenas para sistemas integrados do Azure Stack. Não é aplicável este pacote de atualização para o Azure Stack Development Kit.

## <a name="build-reference"></a>Criar a referência

É o número de compilação de atualização do Azure Stack 1809 **1.1809.x.xx**.  

### <a name="new-features"></a>Novos recursos

Esta atualização inclui os seguintes aprimoramentos para o Azure Stack:

- <!--  2712869   | IS  ASDK -->  **Cliente de syslog do Azure Stack (disponibilidade geral)** este cliente permite que o reencaminhamento de auditorias, alertas e registos de segurança relacionados com a infraestrutura do Azure Stack para um syslog segurança ou de servidor de informações e eventos (SIEM) software de gestão externamente em relação ao Azure Stack. Agora, o cliente de syslog suporta especificando a porta em que o servidor syslog está a escutar.

Com esta versão, o cliente de syslog está disponível em geral e pode ser usado em ambientes de produção.

Para obter mais informações, consulte [reencaminhamento do Azure Stack syslog](azure-stack-integrate-security.md).

### <a name="fixed-issues"></a>Problemas corrigidos

- <!-- 2702741 -  IS ASDK --> Foi corrigido o problema no qual os IPs públicos que foram implementadas utilizando a alocação de dinâmica método não eram a garantia de ser preservadas quando uma paragem da Desalocação é emitido. Agora são preservadas.

- <!-- 3078022 - IS ASDK --> Se uma VM foi parada-desalocada antes de 1808 não pode ser reatribuída após a atualização 1808.  Este problema é resolvido de 1809. Instâncias que foram neste estado e não foi possível iniciar podem ser iniciadas na 1809 com esta correção. A correção também evita que este problema ocorrer.

### <a name="changes"></a>Alterações

- <!-- 1697698  | IS, ASDK --> *Tutoriais de início rápido* na ligação de agora de dashboard do portal de utilizador para os artigos relevantes na documentação do Azure Stack online.

- <!-- 2515955   | IS ,ASDK--> *Todos os serviços* substitui *mais serviços* nos portais do administrador e utilizador do Azure Stack. Agora, pode utilizar *todos os serviços* como uma alternativa para navegar nos portais do Azure Stack, da mesma forma que nos portais do Azure.

- <!--  TBD – IS, ASDK --> *Básico A* tamanhos de máquinas virtuais foram extinguidos para [criar conjuntos de dimensionamento de máquinas virtuais](azure-stack-compute-add-scalesets.md) (VMSS) através do portal. Para criar um VMSS com este tamanho, utilize o PowerShell ou um modelo.  

### <a name="common-vulnerabilities-and-exposures"></a>Vulnerabilidades e exposições comuns

Esta atualização instala as seguintes atualizações de segurança:  

- [ADV180022](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180022)
- [CVE-2018-0965](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-0965)
- [CVE-2018-8271](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8271)
- [CVE-2018-8332](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8332)
- [CVE-2018-8335](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8335)
- [CVE-2018-8392](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8392)
- [CVE-2018-8393](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8393)
- [CVE-2018-8410](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8410)
- [CVE-2018-8419](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8419)
- [CVE-2018-8420](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8420)
- [CVE-2018-8424](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8424)
- [CVE-2018-8433](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8433)
- [CVE-2018-8434](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8434)
- [CVE-2018-8435](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8435)
- [CVE-2018-8438](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8438)
- [CVE-2018-8439](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8439)
- [CVE-2018-8440](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8440)
- [CVE-2018-8442](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8442)
- [CVE-2018-8443](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8443)
- [CVE-2018-8446](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8446)
- [CVE-2018-8449](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8449)
- [CVE-2018-8455](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8455)
- [CVE-2018-8462](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8462)
- [CVE-2018-8468](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8468)
- [CVE-2018-8475](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8475)

Para obter mais informações sobre essas vulnerabilidades, clique nos links anteriores, ou consulte o artigo da Base de dados de conhecimento da Microsoft [4457131](https://support.microsoft.com/help/4457131).

### <a name="prerequisites"></a>Pré-requisitos

- Instalar o Azure Stack [1808 atualizar](azure-stack-update-1808.md) antes de aplicar a atualização do Azure Stack 1809. 
- Instalar o mais recente disponível [atualização ou correção para a versão 1808](azure-stack-update-1808.md#post-update-steps).

  > [!TIP]  
  > Subscrever o seguinte procedimento *RRS* ou *Atom* feeds para se manter atualizado com correções do Azure Stack:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss ... 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom ...


- Antes de iniciar a instalação desta atualização, execute [AzureStack teste](azure-stack-diagnostic-test.md) com os parâmetros seguintes para validar o status do seu Azure Stack e resolver quaisquer problemas operacionais detectados, incluindo todos os avisos e falhas. Também rever alertas ativos e resolver qualquer um que requerem uma ação.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos com o processo de atualização

- <!-- 2468613 - IS --> Durante a instalação desta atualização, poderá ver alertas com o título *erro – o modelo para typu FaultType UserAccounts.New está em falta.*  Pode ignorar com segurança estes alertas. Estes alertas serão fechado automaticamente depois de concluída a instalação desta atualização.

- <!-- 2489559 - IS --> Não tente criar máquinas virtuais durante a instalação desta atualização. Para obter mais informações sobre a gestão de atualizações, consulte [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md#plan-for-updates).

- <!-- 2830461 - IS --> Em determinadas circunstâncias quando uma atualização necessita de atenção, o alerta correspondente pode não ser gerado. O estado preciso ainda será refletido no portal e não é afetado.

### <a name="post-update-steps"></a>Passos de pós-atualização

*Não há nenhuma etapa posterior à atualização para atualização 1809.*

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - [Link to KB]()  
 -->

## <a name="known-issues-post-installation"></a>Problemas conhecidos (após a instalação)

Seguem-se após a instalação problemas conhecidos para esta versão de compilação.

### <a name="portal"></a>Portal

- A documentação técnica do Azure Stack enfoca a versão mais recente. Devido a alterações de portais entre versões, o que vê quando utilizar os portais do Azure Stack pode variar do que vê na documentação.

- <!-- 2930718 - IS ASDK --> No portal do administrador, ao acessar os detalhes de qualquer subscrição do utilizador, depois de fechar o painel e clique em **recente**, não é apresentado o nome de subscrição de utilizador.

- <!-- 3060156 - IS ASDK --> O administrador e o utilizador portais, clicando em definições do portal e selecionando **eliminar todas as definições e dashboards privados** não funcionar conforme esperado. É apresentada uma notificação de erro. 

- <!-- 2930799 - IS ASDK --> O administrador e o utilizador portais, em **todos os serviços**, o elemento **planos de proteção DDoS** incorretamente está listado. Não é, na verdade, disponível no Azure Stack. Se tentar criá-lo, é apresentado um erro a indicar que o portal não foi possível criar o item do marketplace. 

- <!-- 2930820 - IS ASDK --> Nos portais do administrador e o utilizador, se pesquisar por "Docker", o item incorretamente é devolvido. Não é, na verdade, disponível no Azure Stack. Se tentar criá-lo, é apresentado um painel com uma indicação de erro. 

- <!-- 2967387 – IS, ASDK --> A conta que utiliza para iniciar sessão no portal de utilizador ou administrador do Azure Stack indicará **utilizador não identificado**. Isto ocorre quando a conta não tem qualquer um *primeira* ou *última* nome especificado. Para contornar este problema, edite a conta de utilizador para fornecer o primeiro ou último nome. Deve, em seguida, termine sessão e, em seguida, inicie sessão novamente para o portal.  

-  <!--  2873083 - IS ASDK --> Quando utiliza o portal para criar um dimensionamento de máquina virtual definido (VMSS), o *tamanho da instância* pendente não carregados corretamente ao utilizar o Internet Explorer. Para contornar este problema, use outro navegador ao utilizar o portal para criar um VMSS.  

- <!-- TBD  ASDK --> O fuso horário predefinido para todas as implementações do Azure Stack agora são definidas para Hora Universal Coordenada (UTC). Pode selecionar um fuso horário ao instalar o Azure Stack, no entanto, ele é automaticamente revertida para UTC como predefinição durante a instalação.

- <!-- 2931230 – IS  ASDK --> Não não possível eliminar os planos que são adicionados a uma subscrição de utilizador como um plano de suplemento, mesmo quando remover o plano de subscrição de utilizador. O plano irá permanecer até que as subscrições que referenciam o plano de suplemento também são eliminadas. 

- <!--2760466 – IS  ASDK --> Quando instala um novo ambiente do Azure Stack que executar esta versão, o alerta que indica *ativação necessária* poderão não ser apresentados. [Ativação](azure-stack-registration.md) é necessária antes de poder utilizar a distribuição de mercado.  

- <!-- TBD - IS ASDK --> Os dois tipos de subscrição administrativas que estavam [introduzido com a versão 1804](azure-stack-update-1804.md#new-features) não deve ser utilizado. Os tipos de subscrição são **medição subscrição**, e **subscrição de consumo**. Esses tipos de subscrição são visíveis no novos ambientes do Azure Stack a partir da versão 1804, mas ainda não estão prontos a utilizar. Deve continuar a utilizar o **fornecedor predefinido** tipo de subscrição.

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

- <!-- 2869209 – IS, ASDK --> Ao utilizar o [ **Add-AzsPlatformImage** cmdlet](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), tem de utilizar o **- OsUri** parâmetro como a conta de armazenamento URI onde o disco é carregado. Se utilizar o caminho local do disco, o cmdlet falhar com o seguinte erro: *operação de longa execução falhou com o estado 'Com falhas'*. 

- <!--  2966665 – IS, ASDK --> Anexar discos de dados SSD para tamanho premium geridos disco máquinas virtuais (DS, DSv2, Fs, Fs_V2) falha com um erro: *Falha ao atualizar discos para a máquina virtual "vmname" erro: pedida não é possível efetuar a operação porque o tipo de conta de armazenamento " Premium_LRS não é suportada para o tamanho da VM "Standard_DS/Ds_V2/FS/Fs_v2)*

   Para contornar este problema, utilize *Standard_LRS* discos de dados em vez de *Premium_LRS discos*. Usar *Standard_LRS* discos de dados não é alterado IOPs ou o custo de faturação. 

- <!--  2795678 – IS, ASDK --> Ao utilizar o portal para criar máquinas virtuais (VM) num tamanho VM premium (DS, Ds_v2, FS, FSv2), a VM é criada numa conta de armazenamento standard. Criação de uma conta de armazenamento standard não afeta funcionalmente, IOPs, ou de faturação. 

   Pode ignorar o aviso que diz: *que escolheu utilizar um disco padrão num tamanho que suporte discos premium. Isso pode afetar o desempenho do sistema operativo e não é recomendado. Considere utilizar o armazenamento premium (SSD) em vez disso.*

- <!-- 2967447 - IS, ASDK --> A experiência de criação de conjunto (VMSS) de dimensionamento de máquina virtual fornece 7.2 baseada em CentOS, como uma opção para implementação. Porque essa imagem não está disponível no Azure Stack, selecione outro sistema operacional para a sua implementação ou utilizar um modelo do Azure Resource Manager, especificando outra imagem do CentOS que foi transferida pela operadora de rede antes da implantação do marketplace.  

- <!-- 2724873 - IS --> Ao utilizar os cmdlets do PowerShell **Start-AzsScaleUnitNode** ou **Stop-AzsScaleunitNode** para gerir unidades de escala, a primeira tentativa para iniciar ou parar a unidade de escala poderá falhar. Se o cmdlet falhar na primeira execução, execute o cmdlet uma segunda vez. A segunda execução deve ser bem-sucedida para concluir a operação. 

- <!-- TBD - IS ASDK --> Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar a uma VM da série DS. Séries de DS VMs capaz de acomodar os discos de dados como a configuração do Azure.

- <!-- TBD - IS ASDK --> Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

- <!-- 1662991 IS ASDK --> Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico.  

- <!-- 2724961- IS ASDK --> Quando registra o **Microsoft.Insight** fornecedor de recursos nas definições de subscrição e criar uma VM do Windows com o convidado SO diagnóstico ativado, o gráfico de percentagem de CPU na página de descrição geral de VM não mostra os dados das métricas.

   Para localizar dados de métricas, como o gráfico de percentagem de CPU para a VM, vá para a janela de métricas e mostrar todas as as VM de Windows convidado métricas suportadas.



### <a name="networking"></a>Redes  

- <!-- 1766332 - IS ASDK --> Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

- <!-- 1902460 - IS ASDK --> O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

- <!-- 16309153 - IS ASDK --> Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.

- <!-- 2529607 - IS ASDK --> Durante o Azure Stack *segredo rotação*, existe um período em que os endereços IP públicos estão inacessíveis para dois a cinco minutos.

-   <!-- 2664148 - IS ASDK --> Em cenários em que o inquilino está a aceder a suas máquinas virtuais utilizando um túnel S2S VPN, poderão surgir um cenário em que as tentativas de ligação falharem se a sub-rede no local foi adicionada ao Gateway de rede Local já foi criado o gateway. 


<!-- ### SQL and MySQL-->


### <a name="app-service"></a>Serviço de Aplicações

- <!-- 2352906 - IS ASDK --> Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.


### <a name="usage"></a>Utilização  

- <!-- TBD - IS ASDK --> Os públicos dados de medidor de utilização do endereço IP mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Transferir a atualização

Pode transferir o pacote de atualização do Azure Stack 1809 partir [aqui](https://aka.ms/azurestackupdatedownload).
  

## <a name="next-steps"></a>Passos Seguintes

- Para rever a política de manutenção para sistemas integrados do Azure Stack e o que deve fazer para manter seu sistema num estado suportado, consulte [política de manutenção do Azure Stack](azure-stack-servicing-policy.md).  
- Para utilizar o ponto final com privilégios (PEP) para monitorizar e retomar as atualizações, consulte [monitorizar atualizações no Azure Stack, utilizando o ponto final com privilégios](azure-stack-monitor-update.md).  
- Para uma descrição geral da gestão de atualizações no Azure Stack, veja [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md).  
- Para obter mais informações sobre como aplicar as atualizações com o Azure Stack, veja [aplicar atualizações no Azure Stack](azure-stack-apply-updates.md).  
