---
title: Proteger as máquinas e aplicações no Centro de segurança do Azure | Documentos da Microsoft
description: Este documento aborda recomendações no Centro de segurança que o ajudam a proteger as suas máquinas virtuais e computadores e suas aplicações web e ambientes de serviço de aplicações.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 53a6ae828009db24b56a11b1fdcbea19842191f4
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106622"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Proteger as máquinas e aplicações no Centro de segurança do Azure
Centro de segurança do Azure analisa o estado de segurança dos seus recursos do Azure. Quando o Centro de segurança identifica potenciais vulnerabilidades de segurança, cria recomendações que descreve o processo de configuração de controlos necessários. Recomendações se aplicam a tipos de recursos do Azure: máquinas virtuais (VMs) e computadores, aplicações, redes, SQL e identidade e acesso.

Este artigo aborda recomendações que se aplicam a máquinas e aplicações.

## <a name="monitoring-security-health"></a>Monitorizar o estado de funcionamento de segurança
Pode monitorizar o estado de segurança dos seus recursos na **Centro de segurança – descrição geral** dashboard. O **recursos** seção fornece o número de problemas identificados e o estado de segurança para cada tipo de recurso.

Pode ver uma lista de todos os problemas, selecionando **recomendações**. Para obter mais informações sobre como aplicar recomendações, veja [implementar recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md).

Para recomendações de serviços de uma lista completa de computação e de aplicação, consulte [recomendações](security-center-virtual-machine-recommendations.md).

Para continuar, selecione **computação e aplicações** sob **recursos** ou o menu principal do Centro de segurança.
![Dashboard do Centro de segurança][1]

## <a name="monitor-compute-and-app-services"></a>Serviços de computação de monitor e aplicação
Em **Computação**, existem quatro separadores:

- **Descrição geral**: monitorização e recomendações identificadas pelo Centro de Segurança.
- **VMs e computadores**: lista das suas VMS, computadores, máquinas virtuais e os atuais estados de segurança de cada um.
- **Serviços Cloud**: lista das suas funções de trabalho e na Web monitorizadas pelo Centro de Segurança.
- **Serviços de aplicações (pré-visualização)**: lista de seus ambientes do serviço de aplicações e o estado de segurança atual de cada.
Para continuar, selecione **computação e aplicações** sob **recursos** ou o menu principal do Centro de segurança.

![Computação][2]

Existem várias secções em cada separador e pode selecionar, em cada uma delas, uma opção individual para ver mais detalhes sobre os passos recomendados para resolver esse problema em concreto.

### <a name="monitoring-recommendations"></a>Recomendações de monitorização
Esta secção mostra o número total de VMs e computadores que foram inicializados para aprovisionamento automático e os respetivos Estados atuais. Este exemplo inclui uma recomendação, **Monitorizar problemas de estado de funcionamento do agente**. Selecione esta recomendação.

![Monitorizar problemas de estado de funcionamento do agente][3]

**Monitorizar problemas de estado de funcionamento do agente** é aberto. São listados os computadores e VMs que o Centro de Segurança não consegue monitorizar. Selecione um computador ou VM para informações detalhadas. O **ESTADO DE MONITORIZAÇÃO** fornece um motivo pelo qual o Centro de Segurança não consegue monitorizar. Consulte o [guia de resolução de problemas do Centro de Segurança](security-center-troubleshooting-guide.md) para obter uma lista de valores, descrições e passos de resolução do **ESTADO DE MONITORIZAÇÃO**.

### Computadores e VMs não monitorizadas <a name="unmonitored-vms-and-computers"></a>
Uma VM ou o computador não é monitorizado pelo centro de segurança se a máquina não está em execução a extensão do Microsoft Monitoring Agent. Um computador pode ter um agente local já instalado, por exemplo o OMS direcionar o agente ou o agente do SCOM. As máquinas com estes agentes são identificadas como não monitorizado porque estes agentes não são totalmente suportados no Centro de segurança. Para beneficiar totalmente de todas as capacidades do Centro de Segurança, é necessária a extensão MMA.

Pode instalar a extensão no computador, além do agente local já instalado ou VM não monitorizada. Configure ambos os agentes da mesma forma, ligando-os à mesma área de trabalho. Isto permite ao Centro de Segurança interagir com a extensão MMA e recolher dados. Consulte [Ativar a extensão VM](../azure-monitor/learn/quick-collect-azurevm.md) para obter instruções sobre como instalar a extensão MMA.

Consulte [problemas de estado do agente de monitorização](security-center-troubleshooting-guide.md#mon-agent) para saber mais sobre os motivos pelos quais o Centro de Segurança não consegue monitorizar com êxito VMs e computadores inicializados para aprovisionamento automático.

### <a name="recommendations"></a>Recomendações
Esta secção tem um conjunto de recomendações para cada VM e o computador, a funções web e de trabalho, a aplicações de Web do serviço de aplicações do Azure e o ambiente de serviço de aplicações do Azure que monitoriza o Centro de segurança. A primeira coluna indica a recomendação. A segunda coluna mostra o número total de recursos que são afetados por essa recomendação. A terceira coluna mostra a gravidade do problema, conforme ilustrado na seguinte captura de ecrã:

![Recomendações][4]

Cada recomendação tem um conjunto de ações que pode realizar depois de a selecionar. Por exemplo, se selecionar **atualizações de sistema em falta**, o número de VMs e computadores que têm patches em falta e a gravidade da atualização em falta for apresentada, conforme mostrado na captura de ecrã seguinte:

![Aplicar atualizações do Sistema][5]

**Aplicar atualizações do sistema** tem um resumo das atualizações críticas num formato de gráfico, um para Windows e outro para Linux. A segunda parte tem uma tabela com as seguintes informações:

- **NOME**: o nome da atualização em falta.
- **N.º DE VMs e COMPUTADORES**: o número total de VMs e computadores nos quais esta atualização está em falta.
- **GRAVIDADE da ATUALIZAÇÃO**: descreve a gravidade dessa recomendação específica:

    - **Crítico**: uma vulnerabilidade existe com um recurso significativo (aplicação, máquina virtual ou grupo de segurança de rede) e necessita de atenção.
    - **Importante**: passos não críticos ou adicionais são necessários para concluir um processo ou eliminar uma vulnerabilidade.
    - **Moderado**: uma vulnerabilidade deve ser resolvida, mas não necessita de atenção imediata. (Por predefinição, as recomendações baixas não são apresentadas, mas pode filtrar por recomendações baixas se pretender visualizá-las).


- **ESTADO**: o estado atual da recomendação:

    - **Aberto**: a recomendação ainda não foi tratada.
    - **Em curso**: a recomendação está a ser aplicada a esses recursos e não tem de tomar qualquer medida.
    - **Resolvido**: a recomendação já foi concluída. (Quando o problema for resolvido, a entrada fica a cinzento).

Para ver os detalhes das recomendações, clique no nome da atualização em falta na lista.

![Detalhes da recomendação][6]

> [!NOTE]
> As recomendações de segurança aqui são as mesmas que as sob o **recomendações** mosaico. Ver [implementar recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md) para obter mais informações sobre como resolver recomendações.
>
>

### <a name="vms-and-computers"></a>VMs e computadores
A secção de VMs e computadores dá-lhe uma descrição geral de todas as recomendações de VM e o computador. Cada coluna representa um conjunto de recomendações como é mostrado na seguinte captura de ecrã:

![Recomendações da VM e do computador][7]

Existem quatro tipos de ícones representados nesta lista:

![Computador não pertencente ao Azure][8] Computador não Azure.

![VM do Azure Resource Manager][9] VM do Azure Resource Manager.

![VM clássica do Azure][10] VM clássica do Azure.

![VMs identificadas a partir da área de trabalho][11] VMs que só são identificadas a partir da área de trabalho que faz parte da subscrição vista. Inclui VMs de outras subscrições que reportam à área de trabalho desta subscrição e VMs que foram instaladas com o agente direto do SCOM e não têm ID de recurso.

O ícone apresentado em cada recomendação ajuda-o a identificar rapidamente a VM e o computador que precisa de atenção e o tipo de recomendação. Também pode utilizar a opção de filtro para selecionar que opções verá neste ecrã.

![Filtro][12]

No exemplo anterior, uma VM tem uma recomendação crítica relativa à proteção de ponto final. Selecione a VM para obter mais informações sobre ele:

![Recomendação crítica][13]

Aqui verá os detalhes de segurança para o computador ou VM. Na parte inferior, pode ver a ação recomendada e a gravidade de cada problema.

### <a name="cloud-services"></a>Serviços em nuvem
Relativamente aos serviços cloud, é criada uma recomendação quando a versão do sistema operativo está desatualizada, conforme mostrado na captura de ecrã seguinte:

![Serviços em nuvem][14]

Num cenário em que tem uma recomendação (que não é o caso no exemplo anterior), tem de seguir os passos na recomendação para atualizar a versão do sistema operativo. Quando está disponível uma atualização, verá um alerta (vermelho ou cor de laranja, dependendo da gravidade do problema). Quando seleciona este alerta no WebRole1 (executa o Windows Server com a sua aplicação web automaticamente implementada no IIS) ou WorkerRole1 (executa o Windows Server com a sua aplicação web automaticamente implementada no IIS) linhas, verá mais detalhes sobre esta recomendação, conforme mostra a captura de ecrã seguinte:

![WorkerRole1][15]

Para ver uma explicação mais prescritiva sobre esta recomendação, clique em **Atualizar versão do SO** na coluna **DESCRIÇÃO**.

![Atualizar a versão do SO][16]

### <a name="app-services-preview"></a>Serviços de aplicação (Pré-visualização)

> [!NOTE]
> Monitorização do serviço de aplicações está em pré-visualização e disponível apenas no escalão Standard do Centro de segurança. Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança.
>
>

Sob **dos serviços de aplicações**, encontrar uma lista dos seus ambientes de serviço de aplicações e o estado de funcionamento resumo com base na avaliação do Centro de segurança efetuada.

![Serviços aplicacionais][17]

Existem três tipos de ícones representados nesta lista:

![Ambiente de serviços de aplicações][18] Ambiente de serviços de aplicações.

![Aplicação Web][19] Aplicação Web.

![Aplicação de função][24] Aplicação de função.

1. Selecione uma aplicação web. É aberta uma vista de resumida com três separadores:

  - **Recomendações**: com base nas avaliações realizadas pelo centro de segurança que falharam.
  - **Passados avaliações**: lista de avaliações realizadas pelo centro de segurança passado.
  - **Avaliações indisponíveis**: lista de avaliações de que não foi possível executar devido a um erro ou a recomendação não é relevante para o serviço de aplicações específico

  Sob **recomendações** é uma lista das recomendações para a aplicação web selecionada e a gravidade de cada recomendação.

  ![Vista de resumo][20]

2. Selecione uma recomendação para obter uma descrição da recomendação e uma lista de recursos de mau estado de funcionamento, de recursos de bom estado de funcionamento e de recursos não verificados.

  ![Descrição da recomendação][21]

  Sob **transmitido avaliações** é uma lista de avaliações aprovadas.  Gravidade dessas avaliações é sempre verde.

  ![Avaliações aprovadas][22]

3. Selecione uma avaliação com êxito da lista para obter uma descrição da avaliação, uma lista de recursos de mau e bom estado de funcionamento e uma lista de recursos não verificados. Há uma guia para os recursos de mau estado de funcionamento, mas essa lista está sempre vazia, uma vez que a avaliação passado.

    ![Recursos de estado de funcionamento][23]

## <a name="compute-and-app-recommendations"></a>Recomendações de computação e de aplicação
|Tipo de recurso|Classificação de segurança|Recomendação|Descrição|
|----|----|----|----|
|Máquina|50|Instalar o agente de monitorização nos seus computadores|Instale o agente de monitorização para ativar a recolha de dados, as atualizações de verificação, verificação de linha de base e o endpoint protection em cada máquina.|
|Máquina|50|Ativar o aprovisionamento automático e a recolha de dados para as suas subscrições |Ative aprovisionamento automático e a recolha de dados para máquinas nas suas subscrições para ativar a recolha de dados, as atualizações de verificação, verificação de linha de base e o endpoint protection em cada máquina adicionada às suas subscrições.|
|Máquina|40|Resolver problemas de estado de funcionamento de agente de monitorização nos seus computadores|Para proteção de centro de segurança completa, resolver problemas de agente de monitorização em suas máquinas, seguindo as instruções no guia de resolução de problemas| 
|Máquina|40|Resolver problemas de estado de funcionamento do Endpoint Protection nos seus computadores|Para proteção de centro de segurança completa, resolva problemas de agente de monitorização em suas máquinas, seguindo as instruções no guia de resolução de problemas.|
|Máquina|40|Resolver problemas relacionados com dados de análise em falta os seus computadores|Resolver problemas relacionados com dados de análise em falta nas máquinas virtuais e computadores. Dados de análise em falta nos seus resultados de máquinas na ausência de avaliações de segurança, tais como atualizar a análise de linha de base de análise e falta de verificação da solução de proteção de ponto final.|
|Máquina|40|Instalar atualizações de sistema nos seus computadores|Instalar a segurança do sistema em falta e atualizações críticas para proteger as máquinas virtuais Windows e Linux e computadores
|Máquina|40|Atualizar a versão do SO para as funções de serviço na cloud|Atualize a versão do sistema operativo (SO) para as funções de serviço em cloud para a versão mais recente disponível para a sua família de SO.|
|Máquina|35|Solucionar vulnerabilidades na configuração de segurança nos seus computadores|Remediar vulnerabilidades na configuração de segurança nos seus computadores para protegê-los contra ataques. |
|Máquina|35|Remediar vulnerabilidades na configuração de segurança em seus contentores|Remediar vulnerabilidades na configuração de segurança em máquinas com o Docker para protegê-las de ataques.|
|Máquina|25|Ativar Controlos de Aplicação Adaptáveis|Ative a aplicação controle para controle que aplicações podem ser executadas nas suas VMs localizadas no Azure. Isto ajudará a proteger as VMs contra software maligno. Centro de segurança utiliza machine learning para analisar as aplicações em execução em cada VM e ajuda a que aplicar permite regras com base neste informações. Esta capacidade simplifica o processo de configuração e manutenção de aplicação de regras de permissão.|
|Máquina|20|Instalar a solução do Endpoint Protection nos seus computadores|Instale uma solução de proteção de ponto final em suas máquinas virtuais, para protegê-los de ameaças e vulnerabilidades.|
|Máquina|20|Reiniciar os seus computadores para aplicar as atualizações do sistema|Reinicie os seus computadores para aplicar as atualizações de sistema e proteger o computador contra vulnerabilidades.|
|Serviço de aplicações|20|Aplicação Web só deve estar acessível através de HTTPS|Limitar apenas o acesso de aplicações Web através de HTTPS.|
|Serviço de aplicações|20|Função de aplicação só deve estar acessível através de HTTPS|Limitar apenas o acesso de aplicações de funções através de HTTPS.|
|Máquina|15|Aplicar encriptação de disco nas suas máquinas virtuais|Encripte discos da máquina virtual com o Azure Disk Encryption ambos os para máquinas virtuais Windows e Linux. Encriptação de disco do Azure (ADE) aproveita a funcionalidade de BitLocker padrão da indústria do Windows e a funcionalidade de DM-Crypt do Linux para fornecer encriptação de disco de SO e dados para ajudar a proteger e salvaguardar os seus dados e ajudam a cumprir a conformidade e segurança organizacionais compromissos no cliente do Azure key vault. Quando seu requisito de conformidade e segurança exige que encripte os dados de ponta a ponta com as suas chaves de encriptação, incluindo a encriptação do disco (temporária ligado localmente) efémeras, utilize a encriptação de discos do Azure. Em alternativa, por predefinição, o Managed Disks são encriptados em descanso por padrão, usando a encriptação do serviço de armazenamento do Azure onde as chaves de encriptação são chaves gerida pela Microsoft no Azure. Se esta implementação satisfaz os requisitos de segurança e conformidade, pode aproveitar a encriptação de disco gerido padrão para satisfazer os seus requisitos.|
|(Recursos de infraestrutura do serviço) de recursos de computação|10|Utilizar o Azure Active Directory para autenticação de cliente no Service Fabric|Efetue a autenticação de cliente apenas através do Azure Active Directory no Service Fabric.|
|(Conta de automatização) de recursos de computação|5| Ativar a encriptação da conta de automatização|Ative a encriptação de recursos de variável de conta de automatização ao armazenar dados confidenciais.|
|Serviço de aplicações|5|Ativar registos de diagnóstico no serviço de aplicações|Ativar os registos e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida. |
|(Balanceador de carga) de recursos de computação|5|Ativar registos de diagnóstico no balanceador de carga|Ativar os registos e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida. |
|(Pesquisa) de recursos de computação|5|Ativar registos de diagnóstico no serviço de pesquisa|Ativar os registos e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida. |
|(Barramento de serviço) de recursos de computação|5|Ativar registos de diagnóstico no Service Bus|Ativar os registos e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida. |
|(O stream analytics) de recursos de computação|5|Ativar registos de diagnóstico no Azure Stream Analytics|Ativar os registos e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida. |
|(Recursos de infraestrutura do serviço) de recursos de computação|5|Ativar registos de diagnóstico no Service Fabric|Ativar os registos e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida. |
|(Batch) de recursos de computação|5|Ativar registos de diagnóstico em contas do Batch|Ativar os registos e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida. |
|(Hub de eventos) de recursos de computação|5|Ativar registos de diagnóstico no Hub de eventos|Ativar os registos e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida. |
|(Aplicações lógicas) de recursos de computação|5|Ativar registos de diagnóstico no Logic Apps|Ativar os registos e retenha-los até um ano. Isto permite-lhe recriar os registos de atividade para efeitos de investigação quando ocorrer um incidente de segurança ou a rede estiver comprometida. |
|Máquina|30|Instale uma solução de avaliação de vulnerabilidades nas suas máquinas virtuais|Instale uma solução de avaliação de vulnerabilidades nas suas máquinas virtuais|
|Máquina|15|Adicionar uma firewall de aplicação Web| Implemente uma solução de firewall (WAF) de aplicações web para proteger seus aplicativos web. |
|Máquina|30|Remediar vulnerabilidades - por uma solução de avaliação de vulnerabilidade|As máquinas virtuais para o qual é implementar uma solução de terceiros de 3 de avaliação do vulnerabilidade estão a ser continuamente avaliadas em relação a aplicação e vulnerabilidades do SO. Sempre que encontram-se estas vulnerabilidades, elas estão disponíveis para obter mais informações, como parte da recomendação.|
|(Recursos de infraestrutura do serviço) de recursos de computação|15|Defina a propriedade ClusterProtectionLevel EncryptAndSign no Service Fabric|O Service Fabric fornece três níveis de proteção (nenhum, início de sessão e EncryptAndSign) para a comunicação de nó para nó a utilizar um certificado de cluster principal.  Defina o nível de proteção para garantir que todas as mensagens de nó para nó são criptografadas e assinadas digitalmente. |
|Serviço de aplicações|10|Depuração remota deve ser desativada para a aplicação Web|Desative a depuração para aplicativos Web, se já não precisar de utilizá-lo. Depuração remota necessita de portas de entrada estar abertas na aplicação de funções.|
|Serviço de aplicações|10|Depuração remota deve ser desativada para a aplicação de função|Desative a depuração para a aplicação de função se já não precisar de utilizá-lo. Depuração remota necessita de portas de entrada estar abertas na aplicação de funções.|
|Serviço de aplicações|10|Configurar restrições de IP para a aplicação Web|Defina uma lista de endereços IP que têm permissão para aceder à sua aplicação. Utilização de restrições de IP de protege as aplicações web contra ataques comuns.|
|Serviço de aplicações|10|Configurar restrições de IP para a aplicação de função| Defina uma lista de endereços IP que têm permissão para aceder à sua aplicação. Utilização de restrições de IP protege uma aplicação de funções de ataques comuns.|
|Serviço de aplicações|10|Não permitir que todos os ('* ') recursos para aceder à sua aplicação| Não permitir o conjunto de parâmetro WEBSITE_LOAD_CERTIFICATES "". Definição do parâmetro ' significa que todos os certificados são carregados para o arquivo de certificados pessoais de aplicações web. Isto pode levar ao abuso do princípio de privilégio mínimo porque é improvável que o site necessita de acesso a todos os certificados no tempo de execução.|
|Serviço de aplicações|5|Web Sockets devem ser desativados para a aplicação Web|Reveja a utilização de Web Sockets nas aplicações web. O protocolo de Web Sockets está vulnerável a diferentes tipos de ameaças de segurança.|
|Serviço de aplicações|5|Web Sockets devem ser desativados para a aplicação de função|Reveja a utilização de Sockets Web nas aplicações de função. O protocolo de Web Sockets está vulnerável a diferentes tipos de ameaças de segurança.|
|Serviço de aplicações|5|Utilizar domínios personalizados para a sua aplicação Web|Utilize domínios personalizados para proteger uma aplicação web de ataques comuns, como o phishing e outros ataques relacionados com DNS.|
|Serviço de aplicações|5|Utilizar domínios personalizados para a aplicação de funções|Utilize domínios personalizados para proteger uma aplicação de funções de ataques comuns, como o phishing e outros ataques relacionados com DNS.|
|(Batch) de recursos de computação|1|Configurar regras de alerta de métrica na conta de Batch|Configurar regras de alerta de métrica na conta do Batch e ativar as métricas de conjunto eliminar eventos completa e eventos de eliminar conjunto|
|(Barramento de serviço) de recursos de computação|1|Remover todas as regras de autorização, exceto RootManageSharedAccessKey do espaço de nomes do Service Bus |Os clientes do Service Bus não devem utilizar uma política de acesso de nível de espaço de nomes que fornece acesso a todas as filas e tópicos num espaço de nomes. Para alinhar com a segurança de privilégio mínimo modelo, deve criar políticas de acesso no nível de entidade para filas e tópicos para fornecer acesso a apenas a entidade específica.|
|(Hub de eventos) de recursos de computação|1|Remover todas as regras de autorização, exceto RootManageSharedAccessKey do espaço de nomes do Hub de eventos |Os clientes do Hub de eventos não devem utilizar uma política de acesso de nível de espaço de nomes que fornece acesso a todas as filas e tópicos num espaço de nomes. Para alinhar com a segurança de privilégio mínimo modelo, deve criar políticas de acesso no nível de entidade para filas e tópicos para fornecer acesso a apenas a entidade específica.|
|(Hub de eventos) de recursos de computação|5|Definir regras de autorização na entidade Hub de eventos|Regras de autorização na entidade para conceder acesso menos privilegiado Hub de eventos de auditoria.|
|Máquina|30|Instale uma solução de avaliação de vulnerabilidades nas suas máquinas virtuais|Instale uma solução de avaliação de vulnerabilidades nas suas máquinas virtuais|
|Serviço de aplicações|20|CORS não deve permitir que todos os recursos aceder às suas aplicações Web|Permitir que apenas necessários domínios interagir com a sua aplicação web. Entre os recursos de origens (CORS) de partilha não deve permitir que todos os domínios acedam à sua aplicação web.|
|Serviço de aplicações|20|CORS não deve permitir que todos os recursos de aceder à sua aplicação de função| Permitir que apenas necessários domínios interagir com a sua aplicação de função. Entre os recursos de origens (CORS) de partilha não deve permitir que todos os domínios acedam à sua aplicação de função.|
|Máquina|15|Adicionar uma firewall de aplicação Web| Implemente uma solução de firewall (WAF) de aplicações web para proteger seus aplicativos web. |
|Serviço de aplicações|10|Utilize a versão mais recente do .NET Framework suportada para a aplicação Web|Utilize a versão mais recente do .NET Framework para as classes de segurança mais recente. Utilizar classes mais antigas e de tipos pode fazer com que seu aplicativo vulnerável.|
|Serviço de aplicações|10|Utilizar a versão de Java mais recente suportada para a aplicação Web|Utilize a versão mais recente do Java para as classes de segurança mais recente. Utilizar classes mais antigas e de tipos pode fazer com que seu aplicativo vulnerável.|
|Serviço de aplicações|10|Utilizar a versão PHP mais recente suportada para a aplicação Web|Utilize a versão mais recente do PHP para as classes de segurança mais recente. Utilizar classes mais antigas e de tipos pode fazer com que seu aplicativo vulnerável.|
|Serviço de aplicações|10|Utilizar a versão de node. js mais recente suportada para a aplicação Web|Utilize a versão mais recente do node. js para as classes de segurança mais recente. Utilizar classes mais antigas e de tipos pode fazer com que seu aplicativo vulnerável.|
|Serviço de aplicações|10|Utilizar a versão de Python mais recente suportada para a aplicação Web|Utilize a versão mais recente do Python para as classes de segurança mais recente. Utilizar classes mais antigas e de tipos pode fazer com que seu aplicativo vulnerável.|
|VMs e computadores|1|Migrar máquinas virtuais para novos recursos de AzureRM|Utilizar o novo Azure Resource Manager v2 para as suas máquinas virtuais para fornecer aprimoramentos de segurança, tais como: controlo de acesso mais forte (RBAC), a auditoria melhor, a implantação com base em ARM e a governação de acesso para identidades geridas, acesso ao Cofre de chaves para segredos, Azure Autenticação baseada no AD e o suporte para etiquetas e grupos de recursos para facilitar a gestão segurança. |
|Máquina|30|Remediar vulnerabilidades - por uma solução de avaliação de vulnerabilidade|As máquinas virtuais para o qual é implementar uma solução de terceiros de 3 de avaliação do vulnerabilidade estão a ser continuamente avaliadas em relação a aplicação e vulnerabilidades do SO. Sempre que encontram-se estas vulnerabilidades, elas estão disponíveis para obter mais informações, como parte da recomendação.|




## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre as recomendações que se aplicam a outros tipos de recursos do Azure, consulte o seguinte:


* [Recomendações do Centro de segurança do Azure de compreensão para máquinas virtuais](security-center-virtual-machine-recommendations.md)
* [Monitorizar a identidade e acesso no Centro de segurança do Azure](security-center-identity-access.md)
* [Proteger a sua rede no Centro de Segurança do Azure](security-center-network-recommendations.md)
* [Proteger o seu serviço do SQL do Azure no Centro de segurança do Azure](security-center-sql-service-recommendations.md)

Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Definir políticas de segurança no Centro de Segurança do Azure](security-center-azure-policy.md) – Saiba como configurar políticas de segurança para as suas subscrições e grupos de recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png
