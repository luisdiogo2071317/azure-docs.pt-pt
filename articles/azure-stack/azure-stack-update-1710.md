---
title: O Azure Stack 1710 atualização (compilação 20171020.1) | Documentos da Microsoft
description: Saiba mais sobre as novidades na atualização 1710 para o Azure Stack integrada sistemas, os problemas conhecidos e onde pode transferir a atualização.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: cf870551a3dbd9b5ea0ef6f886dc6451e43b2c25
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377195"
---
# <a name="azure-stack-1710-update-build-201710201"></a>O Azure Stack 1710 atualização (compilação 20171020.1)

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve as melhorias e correções nesse pacote de atualização, problemas conhecidos para esta versão e onde pode transferir a atualização. Problemas são divididos em problemas conhecidos de conhecidos diretamente relacionados com o processo de atualização e problemas conhecidos com a compilação (após a instalação).

> [!IMPORTANT]
> Este pacote de atualização é apenas para sistemas integrados do Azure Stack. Não é aplicável este pacote de atualização ao Kit de desenvolvimento do Azure Stack.

## <a name="improvements-and-fixes"></a>Melhorias e correções

Esta atualização inclui as seguintes melhorias de qualidade e correções para o Azure Stack.
 
### <a name="windows-server-2016-improvements-and-fixes"></a>Melhorias do Windows Server 2016 e correções

- Atualizações do Windows Server 2016: 10 de Outubro de 2017 — KB4041691 (compilação do SO 14393.1770. Ver [ https://support.microsoft.com/help/4041691 ](https://support.microsoft.com/help/4041691) para obter mais informações.

### <a name="additional-quality-improvements-and-fixes"></a>Melhorias de qualidade adicionais e correções

- Adicionados cmdlets do PowerShell com privilégios de ponto de extremidade para ajudar a resolver problemas e atualizar o servidor de protocolo NTP (Network Time).
- Foi adicionado suporte para atualizar os módulos de ponto final do ponto final com privilégios Just Enough Administration (JEA) e a lista de permissões do cmdlet. 
- Erros de idioma local foi corrigido o ponto final com privilégios.
- Adicionada a capacidade de rodar credenciais de gateway.
- Remover a conta de administrador local CBLocalAdmin. 
- Foi corrigido o conteúdo do modelo de alerta de heartbeat tornar-se de pulsação alerta trabalho corretamente após uma atualização.
- Foi corrigido o fornecedor de recursos de infraestrutura para lidar com tempos limite durante as operações de FRU. 
- Foi adicionada a capacidade para programadores da cloud utilizar perfis de API do Azure Resource Manager no Azure Stack.
- Desativar o serviço de atualização do Windows na máquina de virtual de implementação (DVM). 
- Removido o poder de nó ações liga/desliga a interface do usuário.
- Várias outras desempenho e estabilidade correções. 
 
## <a name="known-issues-with-the-update-process"></a>Problemas conhecidos com o processo de atualização

Esta secção contém as questões importantes que poderá encontrar durante a instalação da atualização 1710.

> [!IMPORTANT]
> Se a atualização falhar, quando tente posteriormente retomar a atualização, tem de utilizar o `Resume-AzureStackUpdate` cmdlet a partir do ponto final com privilégios. Não é retomada a atualização com o portal de administrador. (Este é um problema conhecido nesta versão.) Para obter mais informações, consulte [monitorizar atualizações no Azure Stack, utilizando o ponto final com privilégios](azure-stack-monitor-update.md).

| Sintoma  | Causa  | Resolução |
|---------|---------|---------|
|Quando efetuar uma atualização, um erro semelhante ao seguinte<br>podem ocorrer durante o passo de "Armazenamento anfitriões reiniciar o nó de armazenamento"<br> do plano de ação de atualização.<br><br>**{"name": "Reiniciar armazenamento Hosts", "Descrição": "reinicie<br> anfitriões de armazenamento. errorMessage","": "escreva"Restart"da função<br> "Bare-metal"gerada uma exceção: \n\n o computador<br> 05 de nome de anfitrião é ignorada. Falha ao obter o respetivo LastBootUpTime<br> através do serviço WMI com a seguinte mensagem de erro:<br> o servidor de RPC não está disponível.<br> (Exceção de HRESULT: 0x800706BA). \nat reinício-Host** | Este problema é causado por um driver inadequadas potencial presente em algumas configurações. | 1. Inicie sessão para a interface de web do controlador (BMC) de gestão da placa base e reinicie o anfitrião que esteja identificado na mensagem de erro.<br><br>2. Retome a atualização utilizando o ponto final com privilégios. |
| Ao efetuar uma atualização, o processo de atualização é apresentado a cair<br> e não faça o curso após o passo "passo: executar passo 2.4 - instalação<br> Atualizar"do plano de ação de atualização.<br><br>Este passo é depois seguido por uma série de processos de cópia de. nupkg<br> partilhas de ficheiros de ficheiros para a infraestrutura interna. Por exemplo:<br><br>**Copiar ficheiros de 1 do content\PerfCollector\VirtualMachines para <br> \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\ <br>PerfCounterConfiguration**<br><br>Em alternativa, verá a mensagem:<br><br>**WarningMessage:Task: Invocação da interface 'LiveUpdate'<br> da função 'Cloud\Fabric\VirtualMachines"falhou:<br> escreva 'LiveUpdate' da função 'VirtualMachines' gerado um<br> exceção: não existe espaço suficiente no disco .**  | O problema é causado por ficheiros de registo a ser preenchida os discos numa máquina virtual de infraestrutura e de um problema no servidor de escalamento horizontal ficheiro (SOFS do Windows Server) que irá ser entregue numa atualização subsequente. | Contacte o serviço de cliente da Microsoft e suporte (CSS) para obter assistência. | 
| Quando efetuar uma atualização, um erro semelhante ao seguinte<br> podem ocorrer durante o passo "passo: executar passo 2.13.2 - atualização<br> *VM_Name*"do plano de ação de atualização. (A máquina virtual<br> nome pode variar.)<br><br>**ActionPlanInstanceWarning ece/MachineName:<br> WarningMessage:Task: invocação de interface 'LiveUpdate' de<br> função 'Cloud\Fabric\WAS' falhou: escreva 'LiveUpdate' da função<br> 'WAS' provocou uma exceção: erro durante a armazenamento<br> inicialização: Ocorreu um erro ao tentar tornar uma API<br> chamada para o serviço Microsoft Storage: {"Message": "um tempo limite<br> ocorreu durante a comunicação com o Service Fabric.<br> Tipo de exceção: TimeoutException.<br> Mensagem de exceção: operação excedeu o limite de tempo. "}**  | O problema é causado por um tempo limite de e/s no Windows Server que será corrigido numa atualização subsequente. | Contacte a Microsoft CSS para obter assistência.
| Quando efetuar uma atualização, um erro semelhante ao seguinte<br> podem ocorrer durante o passo "Step 21 VMs do reinício do SQL server".<br><br>**Escreva 'LiveUpdateRestart' da função 'VirtualMachines' gerado uma<br> exceção: VerboseMessage: [VirtualMachines:LiveUpdateRestart]<br> consultar MachineName do VM-Sql01. - 10/13/2017 5:23:50 horas VerboseMessage: [VirtualMachines: LiveUpdateRestart]<br> VM está marcada como HighlyAvailable. - 10/13/2017 5:23:50 horas<br> VerboseMessage: [VirtualMachines:LiveUpdateRestart] em<br>MS. Internal.ServerClusters.ExceptionHelp.Build em<br>MS. Internal.ServerClusters.ClusterResource.BeginTakeOffline<br>(força booleana) em failoverclusters.<br> StopClusterResourceCommand.BeginTimedOperation() na <br>Microsoft.FailoverClusters.PowerShell.TimedCmdlet.Wrapped<br>ProcessRecord() em failoverclusters.<br> FCCmdlet.ProcessRecord() - 10/13/2017 5:23:50 horas aviso<br>mensagem: tarefa: invocação de interface de 'LiveUpdateRestart' do<br> função 'Cloud\Fabric\VirtualMachines"falhou:** | Este problema pode ocorrer se não foi possível reiniciar a máquina virtual. | Contacte a Microsoft CSS para obter assistência.
| Ao efetuar uma atualização, pode ocorrer um erro semelhante ao seguinte:<br><br>**2017-10-22T01:37:37.5369944Z 'Shutdown' de tipo de função 'SQL'<br> provocou uma exceção: Ocorreu um erro ao nó a colocar em pausa<br> 's45r1004 Sql01'.at Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2 <br>\Fabric\work\ Applications \ EnterpriseCloud <br>EngineApplicationType&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\ <br> CloudDeployment\Roles\SQL\SQL.psm1:line 542 no<br> Encerramento, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\ <br>\EnterpriseCloudEngineApplicationType aplicações&#95;App1\ <br>EnterpriseCloudEngineServicePkg.Code.1.0.597.18\Cloud<br> Deployment\Classes\SQL\SQL.psm1: linha 50 em < ScriptBlock&#62;,<br> <No file>: linha 18 em < ScriptBlock&#62;, < nenhum ficheiro&#62;: linha 16** | Este problema pode ocorrer se a máquina virtual não pode ser colocada num estado suspenso para drenar as funções. | Contacte a Microsoft CSS para obter assistência.
| Ao efetuar uma atualização, pode ocorrer qualquer um dos seguintes erros:<br><br>**O tipo 'Validar' da função "ADFS" provocou uma exceção: validação<br> para a função de AD FS/Graph falhou com o erro: erro ao verificar o ADFS<br> ponto final de sonda *endpoint_URI*: chamadas de exceção<br> " GetResponse"com"0"argumentos:" o servidor remoto<br> devolveu um erro: (503) servidor indisponível. "em Invoke -<br>ADFSGraphValidation**<br><br>**O tipo 'Validar' da função "ADFS" provocou uma exceção: validação<br> para a função de AD FS/Graph falhou com o erro: erro ao obter<br> propriedades do AD FS: não foi possível ligar ao <br>net.tcp://localhost: 1500/política. A tentativa de ligação há<br> para um intervalo de tempo de 00:00:02.0498923. Código de erro do TCP<br> 10061: não foi possível ligar porque o destino<br> máquina ativamente recusou-127.0.0.1:1500.<br> em Invoke ADFSGraphValidation** | O plano de ação de atualização não é possível validar o estado de funcionamento de serviços de Federação do Active Directory (AD FS). | Contacte a Microsoft CSS para obter assistência.

## <a name="known-issues-post-installation"></a>Problemas conhecidos (após a instalação)

Esta secção contém os problemas conhecidos com criar 20171020.1 após a instalação.

### <a name="portal"></a>Portal

- Não pode ser possível ver os recursos de computação ou o armazenamento no portal do administrador. Isto indica que ocorreu um erro durante a instalação da atualização e que a atualização foi incorretamente considerada conclusão com êxito. Se este problema ocorrer, contacte a Microsoft CSS para obter assistência.
- Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de engrenagem no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.
- Ao visualizar as propriedades de um grupo de recursos, o **mover** botão está desativado. Este comportamento é esperado. Mover grupos de recursos entre subscrições não é atualmente suportada.
- Para qualquer fluxo de trabalho em que selecionar uma subscrição, o grupo de recursos ou a localização numa lista pendente, poderá ter uma ou mais dos seguintes problemas:

   - Poderá ver uma linha em branco na parte superior da lista. Deve ainda ser possível selecionar um item, conforme o esperado.
   - Se a lista de itens na lista pendente for curta, poderá não conseguir ver qualquer um dos nomes de item.
   - Se tiver várias subscrições de utilizador, a lista de lista pendente do grupo de recursos pode estar vazia. 

   Para solucionar os dois últimos problemas, pode digitar o nome da subscrição ou do grupo de recursos (se souber) ou pode utilizar o PowerShell em vez disso.
- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.
- Não é possível ver permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, pode verificar as permissões com o PowerShell.
- O **estado de funcionamento do serviço** painel não consegue carregar. Ao abrir o painel de estado de funcionamento do serviço no portal do administrador ou utilizador, Azure Stack exibirá uma mensagem de erro e não carrega informações. Este comportamento está previsto. Embora pode selecionar e abrir o estado de funcionamento do serviço, esta funcionalidade ainda não está disponível, mas será implementada numa versão futura do Azure Stack.
 

### <a name="backup"></a>Cópia de segurança

- Não ative a cópia de segurança de infra-estrutura sobre o **cópia de segurança da infraestrutura** painel.

### <a name="health-and-monitoring"></a>Estado de funcionamento e monitorização

- Se reiniciar uma instância de função de infraestrutura, poderá receber uma mensagem a indicar que o reinício falhou. No entanto, o reinício, na verdade, foi concluída com êxito.

### <a name="marketplace"></a>Marketplace
- Quando tentar adicionar itens para o mercado do Azure Stack, utilizando o **adicionar a partir do Azure** opção, nem todos os itens podem ser visíveis para download.
- Os utilizadores podem procurar o marketplace completo sem uma subscrição e podem ver itens administrativos, como os planos e ofertas. Esses itens estão não funcional para os utilizadores.

### <a name="compute"></a>Computação
- Os utilizadores recebem a opção para criar uma máquina virtual com o armazenamento georredundante. Esta configuração faz com que a criação da máquina virtual efetuar a ativação.
- Pode configurar um conjunto apenas com um domínio de falha de um e um domínio de atualização de um de disponibilidade de máquina virtual.
- Não existe nenhuma experiência do marketplace para criar conjuntos de dimensionamento de máquina virtual. Pode criar um conjunto utilizando um modelo de dimensionamento.
- Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.
 
### <a name="networking"></a>Redes
- Não é possível criar um balanceador de carga com um endereço IP público com o portal. Como solução, pode utilizar o PowerShell para criar o Balanceador de carga.
- Tem de criar uma regra de tradução (NAT) de endereço de rede ao criar um balanceador de carga de rede. Se não o fizer, receberá um erro quando tentar adicionar uma regra NAT depois de criar o Balanceador de carga.
- Não é possível desassociar um endereço IP público de uma máquina virtual (VM) depois que a VM é criada e associada esse endereço IP. Desassociação parecerá funcionar, mas o endereço IP público atribuído anteriormente que permanece associado com a VM original. Este comportamento ocorre mesmo que a reatribuir o endereço IP a uma VM nova (geralmente conhecida como uma *alternância de VIP*). Todos os futuro tenta se conectar por meio deste resultado de endereço IP numa ligação para a VM associada originalmente e não para a nova. Atualmente, tem de utilizar novos endereços IP públicos apenas para a criação de VM nova.
 
### <a name="sqlmysql"></a>SQL/MySQL
- Pode demorar até uma hora até os inquilinos podem criar bases de dados num novo SQL ou MySQL SKU. 
- Criação de itens diretamente no SQL e o MySQL que aloja os servidores que não são executadas pelo fornecedor de recursos não é suportada e pode resultar num estado sem correspondência.
 
### <a name="app-service"></a>Serviço de Aplicações
- Um utilizador tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.
 
### <a name="field-replaceable-unit-fru-procedures"></a>Procedimentos de (FRU) de unidade substituível em campo

- Durante a execução da atualização, as imagens offline não sejam corrigidas. Se precisar de substituir um nó de unidade de escala, trabalhe com o fornecedor do hardware de OEM para se certificar de que o nó substituído tem o nível de patch mais recente.

## <a name="download-the-update"></a>Transferir a atualização

Pode transferir o pacote de atualização 1710 [aqui](https://aka.ms/azurestackupdatedownload).

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral do gerenciamento de atualizações no Azure Stack, veja [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md).
- Para obter informações sobre como aplicar as atualizações, consulte [aplicar atualizações no Azure Stack](azure-stack-apply-updates.md).
