---
title: Resolver problemas de migração do Azure | Microsoft Docs
description: Fornece uma descrição geral dos problemas conhecidos no serviço Azure migrar, resolução de problemas e sugestões para erros comuns.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 896e918f6031f3bc6b925a2ecdfa2a5c82f00e0b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36228259"
---
# <a name="troubleshoot-azure-migrate"></a>Resolver problemas do Azure Migrate

## <a name="troubleshoot-common-errors"></a>Resolver erros comuns

[Migrar do Azure](migrate-overview.md) avalia cargas de trabalho no local para a migração para o Azure. Utilize este artigo para resolver problemas quando implementar e utilizar a migração do Azure.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Falha na criação do projeto de migração com erro *pedidos tem de conter os cabeçalhos de identidade do utilizador*

Este problema pode ocorrer para os utilizadores que não têm acesso ao inquilino do Azure Active Directory (Azure AD) da organização. Quando um utilizador é adicionado a um inquilino do Azure AD pela primeira vez, he/she recebe um convite de correio eletrónico para associar o inquilino. Os utilizadores precisam de aceder ao e-mail e aceitar o convite para obter adicionada com êxito para o inquilino. Se não for possível ver a mensagem de e-mail, aceder a um utilizador que já tem acesso ao inquilino e peça-lhes para reenviar o convite para si através dos passos especificados [aqui](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Assim que recebeu o e-mail de convite, terá de abrir mensagem de correio eletrónico e clique na hiperligação no e-mail para aceitar o convite. Quando isto é feito, tem de terminar sessão no portal do Azure e início de sessão novamente, atualizar o browser não funcionará. Em seguida, pode tentar criar o projeto de migração.

### <a name="performance-data-for-disks-and-networks-adapters-shows-as-zeros"></a>Dados de desempenho para os adaptadores de discos e redes mostram como zeros

Isto pode ocorrer se o nível de definição de estatísticas no vCenter server estiver definido como inferior a três. Em três nível ou superior, o vCenter armazena histórico do desempenho da VM de computação, armazenamento e rede. Para três de nível inferior, vCenter não armazena o armazenamento e dados de rede, mas apenas para dados da CPU e memória. Neste cenário, dados de desempenho mostra como zero no Azure migrar e migrar do Azure fornece a recomendação de tamanho para discos e redes com base nos metadados recolhidos das máquinas no local.

Para ativar a recolha de dados de desempenho de disco e da rede, altere o nível de definições de estatísticas a três. Em seguida, aguarde pelo menos um dia para detetar o seu ambiente e avaliá-lo.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Posso agentes instalados e utilizados a visualização de dependência para criar grupos. Agora publique ativação pós-falha, as máquinas mostram ação "Instalar o agente", em vez de "Dependências de vista"
* Post planeada ou ativação pós-falha não planeada, no local as máquinas estão desativadas e do Azure são aceleradas máquinas equivalentes. Estas máquinas adquiram um endereço MAC diferente. Estes podem adquirir um endereço IP diferente, com base em se o utilizador optou por manter o endereço IP no local ou não. Se forem diferentes endereços IP e MAC, Azure migrar associa as máquinas no local quaisquer dados de dependência de mapa de serviço e pede-lhe utilizador para instalar agentes em vez de visualização de dependências.
* Após a ativação pós-falha de teste, as máquinas no local se mantêm ativadas conforme esperado. Máquinas equivalentes aceleradas no Azure adquiram outro endereço de MAC e poderão adquirir o endereço IP diferente. A menos que o utilizador bloqueia o tráfego de saída de análise de registos dessas máquinas, Azure migrar associa as máquinas no local quaisquer dados de dependência de mapa de serviço e pede-lhe utilizador para instalar agentes em vez de visualização de dependências.

## <a name="collector-errors"></a>Erros de recoletor

### <a name="deployment-of-collector-ova-failed"></a>Implementação do recoletor OVA falhou

Isto pode acontecer se o OVA parcialmente é transferido ou devido ao browser se estiver a utilizar o cliente de web vSphere para implementar o OVA. Certifique-se de que a transferência estiver concluída e tente implementar OVA com um browser diferente.

### <a name="collector-is-not-able-to-connect-to-the-internet"></a>Recoletor não é possível estabelecer ligação à internet

Isto pode acontecer quando a máquina que está a utilizar se encontrar atrás de um proxy. Certifique-se de que fornecer as credenciais de autorização se o proxy precise.
Se estiver a utilizar qualquer proxy firewall baseada no URL para controlar a conectividade de saída, certifique-se a lista branca que estes necessários URLs:

**URL** | **Objetivo**  
--- | ---
*.portal.azure.com | Necessário para verificar a conectividade com o serviço do Azure e validar a sincronização de hora problemas.
*.oneget.org | Necessário para transferir o powershell com base vCenter PowerCLI módulo.

**O recoletor não é possível ligar ao projeto com o ID de projeto e chave posso copiada a partir do portal.**

Certifique-se ter copiados e colados as informações corretas. Para resolver problemas, instale o Microsoft Monitoring Agent (MMA) e verifique se o MMA pode ligar para o projeto da seguinte forma:

1. No recoletor VM, transfira o [MMA](https://go.microsoft.com/fwlink/?LinkId=828603).
2. Para iniciar a instalação, faça duplo clique o ficheiro transferido.
3. Na configuração, no **boas-vindas** página, clique em **seguinte**. Na página **Termos de Licenciamento**, clique em **Concordo** para aceitar a licença.
4. No **pasta de destino**, manter ou modificar a pasta de instalação predefinida > **seguinte**.
5. No **opções de configuração do agente**, selecione **Log Analytics do Azure** > **seguinte**.
6. Clique em **adicionar** para adicionar uma nova área de trabalho de análise de registos. Cole no ID de projeto e a chave que copiou. Clique depois em **Seguinte**.
7. Certifique-se de que o agente pode ligar ao projeto. Se não pode ser, verifique as definições. Se o agente pode ligar, mas o recoletor não é possível, contacte o suporte.


### <a name="error-802-date-and-time-synchronization-error"></a>Erro 802: Data e hora erro de sincronização

O relógio do servidor pode ser fora de sincronização com a hora atual em mais de cinco minutos. Altere a hora do relógio de recoletor de VM para corresponder à hora atual, da seguinte forma:

1. Abra uma linha de comandos de administrador na VM.
2. Para verificar o fuso horário, execute w32tm /tz.
3. Sincronizar o tempo, execute w32tm /resync.

### <a name="vmware-powercli-installation-failed"></a>Falha na instalação do VMware PowerCLI

Azure migrar recoletor PowerCLI de transfere e instala-o no dispositivo de. Falha na instalação PowerCLI dever-se inacessível pontos finais para o repositório de PowerCLI. Para resolver problemas, tente instalar manualmente PowerCLI no recoletor VM utilizando o passo seguinte:

1. Abra o Windows PowerShell no modo de administrador
2. Vá para o diretório C:\ProgramFiles\ProfilerService\VMWare\Scripts\
3. Execute o script InstallPowerCLI.ps1

### <a name="error-unhandledexception-internal-error-occured-systemiofilenotfoundexception"></a>Ocorreu um erro interno de UnhandledException erro: System.IO.FileNotFoundException

Este é um problema visto no Recoletor com versões anteriores à 1.0.9.5. Se tiver uma versão de Recoletor 1.0.9.2 ou versões de pré-GA como 1.0.8.59, irá deparar-se com este problema. Siga a ligação [aqui apresentada para os fóruns para obter uma resposta detalhada](https://social.msdn.microsoft.com/Forums/azure/en-US/c1f59456-7ba1-45e7-9d96-bae18112fb52/azure-migrate-connect-to-vcenter-server-error?forum=AzureMigrate).

[Atualize o Recoletor para corrigir o problema](https://aka.ms/migrate/col/checkforupdates).

### <a name="error-unabletoconnecttoserver"></a>Error UnableToConnectToServer

Não é possível ligar ao vCenter Server "Servername.com:9443" devido ao erro: não ocorreu nenhum ponto final à escuta em https://Servername.com:9443/sdk que pudesse aceitar a mensagem.

Verifique se que estejam a executar a versão mais recente do dispositivo de recoletor, caso contrário, atualizar a aplicação para o [versão mais recente](https://docs.microsoft.com/azure/migrate/concepts-collector#how-to-upgrade-collector).

Se o problema ocorre ainda a versão mais recente, isto pode dever-se ao porque a máquina de recoletor está não é possível resolver o nome do servidor vCenter especificado ou a porta especificada está incorreta. Por predefinição, se a porta não for especificada, recoletor irá tentar estabelecer ligação com o número da porta 443.

1. Tente enviar um ping Servername.com da máquina do recoletor.
2. Se o passo 1 falhar, tente ligar ao servidor do vCenter através do endereço IP.
3. Identifique o número de porta correto para ligar ao vCenter.
4. Por fim, verifique se o servidor do vCenter está em execução.

## <a name="troubleshoot-readiness-issues"></a>Resolver problemas de preparação

**Problema** | **Corrigir**
--- | ---
Tipo de arranque não suportado | Azure não suporta VMs com o tipo de arranque EFI. Recomenda-se para converter o tipo de arranque BIOS antes de executar uma migração. <br/><br/>Pode utilizar [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) para fazer a migração dessas VMs irá converter o tipo de arranque da VM BIOS durante a migração.
SO de Windows condicionalmente suportados | O SO passou o respetivo fim de suporte de data e tem um personalizada suporta contrato (CSA) para [suportar no Azure](https://aka.ms/WSosstatement), considere atualizar o sistema operativo antes de migrar para o Azure.
SO Windows não suportado | Azure só suporta [versões de SO Windows selecionadas](https://aka.ms/WSosstatement), considere atualizar o sistema operativo da máquina antes de migrar para o Azure.
SO Linux condicionalmente endossado | Azure patrocina apenas [versões de SO Linux selecionadas](../virtual-machines/linux/endorsed-distros.md), considere atualizar o sistema operativo da máquina antes de migrar para o Azure.
SO Linux não endossado | O computador pode efetuar o arranque no Azure, mas sem suporte de SO é fornecido pelo Azure, considere atualizar o sistema operativo para um [aprovadas versão do Linux](../virtual-machines/linux/endorsed-distros.md) antes de migrar para o Azure
Sistema de operativo desconhecido | O sistema operativo da VM foi especificado como 'Outro' no vCenter Server, devido a que migrar do Azure não consegue identificar a preparação da VM do Azure. Certifique-se de que o SO em execução dentro da máquina é [suportado](https://aka.ms/azureoslist) pelo Azure antes de migrar a máquina.
Número de bits do SO não suportados | As VMs com um SO de 32 bits podem efetuar o arranque no Azure, mas recomenda-se a atualização do SO da VM de 32 bits para 64 bits antes de migrar para o Azure.
Requer subscrição do Visual Studio. | As máquinas tem um cliente Windows SO em execução dentro do mesmo que só é suportado em subscrição do Visual Studio.
VM não encontrada para o desempenho de armazenamento necessária. | O desempenho de armazenamento (IOPS/débito) necessário para a máquina excede o suporte de VM do Azure. Reduza os requisitos de armazenamento para a máquina antes da migração.
VM não encontrada para o desempenho de rede necessários. | O desempenho de rede (na/saída) necessário para a máquina excede o suporte de VM do Azure. Reduza os requisitos de rede da máquina.
VM não foi encontrado no escalão de preço especificado. | Se o escalão de preço está definido como Standard, considere downsizing da VM antes de migrar para o Azure. Se a camada de dimensionamento é básica, considere alterar o escalão de preço de avaliação Standard.
VM não foi encontrado na localização especificada. | Utilize outra localização de destino antes da migração.
Um ou mais discos unsuitable. | Um ou mais discos ligados à VM não cumprem os requisitos do Azure. Para cada disco ligado à VM, certifique-se de que o tamanho do disco < 4 TB, caso contrário, reduzir o tamanho de disco antes de migrar para o Azure. Certifique-se de que o desempenho (IOPS/débito) necessário para cada disco é suportado pelo Azure [geridos discos da máquina virtual](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits).   
Um ou mais adaptadores de rede unsuitable. | Remova os adaptadores de rede não utilizadas da máquina antes da migração.
A contagem do disco excede o limite | Remova os discos da máquina antes da migração.
O tamanho do disco excede o limite | Azure suporta discos com até tamanho 4 TB. Reduzir discos para menos de 4 TB antes da migração.
Disco indisponível na localização especificada | Certifique-se o disco na localização de destino antes de migrar.
Disco indisponível para a redundância especificada | O disco deve utilizar o tipo de armazenamento de redundância definido nas definições de avaliação (LRS por predefinição).
Não foi possível determinar a adequação do disco devido a um erro interno | Tente criar uma nova avaliação para o grupo.
Não foi encontrada uma VM com os núcleos e memória necessários | Não foi possível ao Azure ajustar um tipo VM adequado. Reduza a memória e o número de núcleos da máquina no local antes de migrar.
Não foi possível determinar a adequabilidade da VM devido a um erro interno. | Tente criar uma nova avaliação para o grupo.
Não foi possível determinar o adequação a um ou mais discos devido a um erro interno. | Tente criar uma nova avaliação para o grupo.
Não foi possível determinar o adequação a um ou mais adaptadores de rede devido a um erro interno. | Tente criar uma nova avaliação para o grupo.


## <a name="collect-logs"></a>Recolher registos

**Como posso recolher registos no recoletor VM?**

Registo está ativado por predefinição. Os registos estão localizados da seguinte forma:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Para recolher o rastreio de eventos para o Windows, efetue o seguinte:

1. No recoletor VM, abra uma janela de comando do PowerShell.
2. Executar **Get-registo de eventos - LogName aplicação | export-csv eventlog.csv**.

**Como posso recolher registos de tráfego de rede portal?**

1. Abra o browser e navegue até e inicie sessão no [para o portal](https://portal.azure.com).
2. Premir a tecla F12 para iniciar as ferramentas de programador. Se necessário, desmarque a definição **limpar as entradas navegação**.
3. Clique em de **rede** separador e começar a capturar o tráfego de rede:
 - No Chrome, selecione **Preserve registo**. A gravação deve iniciar automaticamente. Um círculo vermelho indica que o tráfego está a ser captura. Se não for apresentada, clique no círculo preto para iniciar
 - No Edge/IE, gravação deve iniciar automaticamente. Se não, clique no botão Reproduzir verde.
4. Tente recriar o erro.
5. Depois de ter encontrou o erro durante a gravação, parar a gravação e guarde uma cópia da atividade registada:
 - No Chrome, clique com botão direito e clique em **guardar como HAR com conteúdo**. Isto zips e exporta os registos como um ficheiro de .har.
 - No Edge/IE, clique em de **exportação capturado o tráfego** ícone. Isto zips e exporta o registo.
6. Navegue para o **consola** separador para verificar a existência de avisos ou erros. Para guardar o registo da consola:
 - No Chrome, clique com botão direito em qualquer local no registo de consola. Selecione **guardar como**, para exportar e zip o registo.
 - No Edge/IE, clique com botão direito sobre os erros e selecione **copie todos os**.
7. Feche as ferramentas de programador.

## <a name="collector-error-codes-and-recommended-actions"></a>Códigos de erro do recoletor e ações recomendadas

|           |                                |                                                                               |                                                                                                       |                                                                                                                                            |
|-----------|--------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Código de Erro | Nome de erro                      | Mensagem                                                                       | Causas possíveis                                                                                        | Ação recomendada                                                                                                                          |
| 601       | CollectorExpired               | O recoletor expirou.                                                        | Recoletor Expirado.                                                                                    | Transfira uma nova versão do recoletor e tente novamente.                                                                                      |
| 751       | UnableToConnectToServer        | Não foi possível ligar ao vCenter Server "%Name;" devido ao erro: %ErrorMessage;     | Consulte a mensagem de erro para obter detalhes.                                                             | Resolva o problema e tente novamente.                                                                                                           |
| 752       | InvalidvCenterEndpoint         | O servidor "%Name;" não é um vCenter Server.                                  | Forneça os detalhes do vCenter Server.                                                                       | Tente novamente a operação com os detalhes do vCenter Server corretos.                                                                                   |
| 753       | InvalidLoginCredentials        | Não é possível ligar ao vCenter Server "%Name;" devido ao erro: %ErrorMessage; | Falha ao ligar ao vCenter Server devido a credenciais de início de sessão inválidas.                             | Certifique-se de que as credenciais de início de sessão fornecidas estão corretas.                                                                                    |
| 754       | NoPerfDataAvaialable           | Dados de desempenho não disponíveis.                                               | Verifique o nível de estatísticas no vCenter Server. Deve ser definido para 3 para dados de desempenho estar disponível. | Alterar o Nível de Estatísticas para 3 (para duração de 5 minutos, 30 minutos e 2 horas) e experimente aguardar pelo menos um dia.                   |
| 756       | NullInstanceUUID               | Foi encontrada uma máquina com InstanceUUID nulo                                  | O vCenter Server pode ter um objeto inapropriado.                                                      | Resolva o problema e tente novamente.                                                                                                           |
| 757       | VMNotFound                     | A máquina virtual não foi encontrada                                                  | A máquina virtual pode ser eliminada: %VMID;                                                                | Certifique-se de que as máquinas virtuais selecionadas enquanto efetua o controlo do âmbito do inventário do vCenter existem durante a deteção                                      |
| 758       | GetPerfDataTimeout             | VCenter pedido excedeu o tempo limite. Mensagem % Message;                                  | As credenciais do vCenter Server estão incorretas                                                              | Verifique as credenciais do servidor vCenter e certifique-se de que servidor vCenter está acessível. Repita a operação. Se o problema persistir, contacte o suporte. |
| 759       | VmwareDllNotFound              | DLL VMWare.Vim não encontrado.                                                     | O PowerCLI não se encontra corretamente instalado.                                                                   | Verifique se PowerCLI se encontra corretamente instalado. Repita a operação. Se o problema persistir, contacte o suporte.                               |
| 800       | ServiceError                   | O serviço do Recoletor do Azure Migrate não está em execução.                               | O serviço do Recoletor do Azure Migrate não está em execução.                                                       | Utilize o services.msc para iniciar o serviço e tentar novamente a operação.                                                                             |
| 801       | PowerCLIError                  | A instalação do VMware PowerCLI falhou.                                          | A instalação do VMware PowerCLI falhou.                                                                  | Repita a operação. Se o problema persistir, instale-a manualmente e repita a operação.                                                   |
| 802       | TimeSyncError                  | A hora não está sincronizada com o servidor de horas da internet.                            | A hora não está sincronizada com o servidor de horas da internet.                                                    | Certifique-se de que a hora no computador está bem definida para o fuso horário do computador e repita a operação.                                 |
| 702       | OMSInvalidProjectKey           | Chave de projeto inválida especificada.                                                | Chave de projeto inválida especificada.                                                                        | Repita a operação com a chave de projeto correta.                                                                                              |
| 703       | OMSHttpRequestException        | Erro ao enviar o pedido. Mensagem % Message;                                | Verifique o ID e a chave do projeto e certifique-se de que ponto final está alcançável.                                       | Repita a operação. Se o problema persistir, contacte o Suporte da Microsoft.                                                                     |
| 704       | OMSHttpRequestTimeoutException | Pedido HTTP excedeu o tempo limite. Mensagem % Message;                                     | Verifique o id e a chave do projeto e certifique-se de que ponto final está alcançável.                                        | Repita a operação. Se o problema persistir, contacte o Suporte da Microsoft.                                                                     |
