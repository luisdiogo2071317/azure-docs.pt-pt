---
title: Componentes de sistema de informações do Azure e limites
description: Este artigo fornece uma descrição geral da arquitetura do Microsoft Azure e gestão.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 8db1dce5fcc56c229d1fdd746bafbd2fae2c9bad
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102543"
---
# <a name="azure-information-system-components-and-boundaries"></a>Componentes de sistema de informações do Azure e limites
Este artigo fornece uma descrição geral da arquitetura do Microsoft Azure e gestão. O ambiente de sistema do Azure é constituído pelas redes seguintes:

- Rede de produção do Microsoft Azure (rede do Azure)
- Microsoft Corporate rede (rede de rede empresarial)

Separado as equipas de TI são responsáveis por operações e manutenção da rede do Azure e redes de rede empresarial.

## <a name="azure-architecture"></a>Arquitetura do Azure
Microsoft Azure é uma informática em nuvem plataforma e infraestrutura para criar, implementar, gerir aplicações e serviços através de uma rede de centros de dados gerida pela Microsoft. Com base no número de recursos especificados por clientes, o Azure cria VMs com base na necessidade de recursos. Estas VMs com o Microsoft Azure hipervisor, que foi concebido para utilizar na nuvem e não está acessíveis para o público.

Em cada nó de servidor físico do Azure, não há um hipervisor que é executado diretamente através do hardware. Hipervisor divide um nó num número de variável de máquinas virtuais do convidado (VMs). Cada nó também tem um especial "raiz" VM, que é executado o sistema operativo anfitrião. Firewall do Windows está ativada em cada VM. As portas apenas abrir e endereçáveis, internamente ou externamente, são portas explicitamente definidas no ficheiro de definição de serviço configurado pelo cliente. Todos os tráfego e acesso para o disco e rede é mediated pela hipervisor e SO de raiz.

Na camada de anfitrião, as VMs do Azure a executar uma versão personalizada e protegida de servidor do Windows mais recente. Microsoft Azure utiliza uma versão stripped-down do servidor do Windows que inclua apenas esses componentes são necessários para as VMs do anfitrião. Isto é feito para melhorar o desempenho e reduzir a superfície de ataque. Limites de máquina são impostos pelo hipervisor, que não dependam de segurança do sistema operativo.

**Gestão do Azure por controladores de recursos de infraestrutura (FCs)**: no Azure, as VMs em execução nos servidores físicos (painéis/nós) são agrupados em "clusters" de cerca de 1000. As VMs independentemente são geridas por um componente de software de plataforma ampliada e redundante chamado de FC.

Cada FC gere o ciclo de vida das aplicações em execução no seu cluster e aprovisiona e monitoriza o estado de funcionamento do hardware sob o controlo dele. Executa a ambas as operações autonomic, como reincarnating instâncias de VM em servidores de bom estado de funcionamento quando determina que um servidor falhou. O FC também efetua operações de gestão de aplicações como implementar, atualizar e ampliar aplicações.

O Centro de dados está dividido em clusters. Clusters isolam falhas de nível de FC e impedem que determinadas classes de erros que afetam servidores para além de cluster no qual ocorrem. FCs que servem um determinado cluster do Azure são agrupadas para um Cluster de FC.

**Inventário de Hardware**: um inventário de dispositivos de hardware e de rede do Azure está preparado durante o processo de configuração de arranque de configuração e documentado no ficheiro de configuração de datacenter.xml. Qualquer novo hardware e componentes de rede, introduzir o ambiente de produção do Azure tem de seguir o processo de configuração de arranque de configuração. O FC é responsável por gerir o inventário completo listado no ficheiro de configuração de datacenter.xml.

**SO gerido de FC**: equipa o SO fornece imagens de SO sob a forma de Virtual discos de rígido (VHD) que são implementadas em todos os anfitriões e VMs de convidado no ambiente de produção do Azure. A equipa de SO constrói estes "Imagens de Base" através de um processo de compilação offline automatizada. A imagem de Base é uma versão do sistema operativo no qual o kernel e outros componentes do núcleo foram modificadas e otimizados para suportar o ambiente do Azure.

Existem três tipos de imagens de SO gerido de recursos de infraestrutura:

- SO de anfitrião – o sistema operativo do anfitrião é um sistema operativo personalizado que é executada em VMs do anfitrião
- SO nativo – nativo sistema operativo que é executado no inquilinos (por exemplo, armazenamento do Azure) que não tem qualquer hipervisor
- SO convidado – SO convidado que é executada em VMs de convidado

O anfitrião e sistemas de operativos geridos de FC nativo foram concebidos para utilização na nuvem e não se encontra acessíveis publicamente.

**Anfitriões e SO nativo**: SO nativo e de sistema operativo anfitrião são imagens protegidas do SO que alojam os agentes de recursos de infraestrutura (FA) e ser executado no nó de computação (é executada como primeira VM no nó) e nós de armazenamento. As vantagens da utilização otimizada Base imagens do anfitrião e nativo SO é o que reduz a área de superfície exposta pelo APIs ou os componentes que apresentam riscos de segurança altos e aumentam os requisitos de espaço do SO. Estes sistemas de operativos requisitos de espaço reduzidos incluir apenas os componentes necessários para o Azure. Isto melhora o desempenho e reduz a superfície de ataque.

**SO convidado**: componentes internos do Azure em execução em VMs de SO convidado não tem nenhuma oportunidade de executar o protocolo RDP (Remote Desktop Protocol) ao contrário dos clientes externos. As alterações efetuadas nas definições de configuração de linha de base seriam necessários para percorrer a alteração e processo de gestão de versões.

## <a name="azure-datacenters"></a>Datacenters do Azure
A equipa de infraestrutura de nuvem da Microsoft e operações (MCIO) gere físicas infraestrutura instalações e de centro de dados para todos os serviços online da Microsoft judicial. MCIO é principalmente responsável por gerir os controlos físicos e ambientais dentro dos centros de dados, bem como gerir e dispositivos de rede de perímetro externa (Routers de limite e Routers de centro de dados). MCIO é também responsável por configurar o hardware de servidor mínima bare em bastidores no Centro de dados. Os clientes têm sem interação direta com o Azure.

## <a name="service-management--service-teams"></a>Gestão do serviço & equipas de serviço
Suporte do serviço do Azure é gerido por um número de grupos de engenharia conhecido como 'Equipas de serviço'. Cada uma das equipas de serviço é responsável por uma área de suporte do Azure. Equipa de cada serviço tem de se um engenheiro 24x7 disponíveis para investigar e resolver falhas no serviço. As equipas de serviço não, por predefinição, dispõe de acesso físico ao hardware a funcionar no Azure.

Se a equipas de serviço:

- Plataforma de Aplicações
- Azure Active Directory
- Computação do Azure
- Net do Azure
- Serviços de engenharia do cloud
- ISSD: segurança
- Autenticação Multifator
- SQL Database
- Armazenamento

## <a name="types-of-users"></a>Tipos de utilizadores
Todos os utilizadores internos do Azure tem o estado de empregado categorizado com um nível de sensibilidade que define o respetivo acesso aos dados de cliente (acesso ou sem acesso). Os funcionários (ou subcontratados) da Microsoft são consideradas a utilizadores internos. Todos os outros utilizadores são considerados os utilizadores externos. Privilégios de utilizador para o Azure (permissões de autorização após a autenticação ocorre) são descritos na tabela que se segue:

| Função | Internos ou externos | Nível de sensibilidade | Privilégios autorizados e funções efetuadas | Tipo de acesso
| --- | --- | --- | --- | --- |
| Engenheiro de centro de dados do Azure | Interno | Sem acesso a dados de cliente | Gerir a segurança física da local; Realize patrols dentro ou fora do Centro de dados e monitorizar todos os pontos de entrada; Executar serviços escort dentro e fora do datacenter para determinados não limpa empregados que fornecem serviços gerais (jantar, limpeza) ou de trabalho IT no Centro de dados; Realize a rotina de monitorização e manutenção de hardware de rede; Efetue a gestão de incidentes e de trabalho de break-fix através de uma variedade de ferramentas; Realize a rotina de monitorização e manutenção de hardware físico nos centros de dados; Acesso ao ambiente a pedido dos proprietários da propriedade. Com capacidade efetuar as investigações forenses, relatório de incidente, registo e exigir formação de segurança obrigatória & requisitos da política; Propriedade operacional e da manutenção das ferramentas de segurança críticas, tais como scanners e recolha de registos. | Acesso persistente para o ambiente |
| Triagem de incidentes do Microsoft Azure (engenheiros de resposta rápida) | Interno | Acesso a dados de cliente | Gerir as comunicações entre as equipas de operações de infraestrutura, de suporte e Azure engenharia; Triagem plataforma incidentes, problemas de implementação e pedidos de serviço. | Apenas no acesso de tempo para o ambiente - com acesso persistente limitado aos sistemas não cliente |
| Engenheiros de implementação do Microsoft Azure | Interno | Acesso a dados de cliente | Efetue atualizações/implementação de componentes de plataforma, software e as alterações de configuração agendada como suporte do Microsoft Azure. | Apenas no acesso de tempo para o ambiente - com acesso persistente limitado aos sistemas não cliente |
| Suporte de indisponibilidade do cliente do Microsoft Azure (inquilino) | Interno | Acesso a dados de cliente | Depurar e diagnosticar falhas de plataforma e falhas para contas do Microsoft Azure; e de computação individuais inquilinos Analise a falhas e unidade correções críticas à plataforma/cliente, melhoramentos técnica de unidade através de suporte. | Apenas no acesso de tempo para o ambiente - com acesso persistente limitado aos sistemas não cliente |
| Engenheiros de sistemas do Site em tempo real do Microsoft Azure (monitorização engenheiros) & incidente | Interno | Acesso a dados de cliente | Responsável por diagnosticar e mitigar o estado de funcionamento de plataforma utilizando as ferramentas de diagnóstico; Unidade correções para controladores de volume, repare itens resultantes de falhas e ajudar as ações de restauro de indisponibilidade. | Apenas no acesso de tempo para o ambiente - com acesso persistente limitado aos sistemas não cliente |
|Clientes do Microsoft Azure | Externo | N/A | N/D | N/A |

Azure utiliza identificadores exclusivos para autenticar utilizadores organizacionais e clientes (ou processos agir em nome de utilizadores organizacionais) a todos os recursos/dispositivos que fazem parte do ambiente do Azure.

**Autenticação interna do Microsoft Azure**: as comunicações entre componentes internos do Azure estão protegidas com encriptação TLS. Na maioria dos casos, os certificados x. 509 são autoassinados. Exceções são efetuadas para certificados com ligações que foi acedidos a partir da fora da rede do Azure e para o FCs. FCs ter certificados emitidos por uma Microsoft Certificate de autoridade (AC) que é copiado por uma AC de raiz fidedigna. Isto permite que as chaves públicas de FC ser feito facilmente. Além disso, as chaves públicas de FC são utilizadas por ferramentas de programador do Microsoft para que quando os programadores submeter novas imagens de aplicação, são encriptadas com uma chave pública de FC para poder proteger quaisquer segredos incorporados.

**Autenticação de dispositivos de Hardware do Microsoft Azure**: FC o mantém um conjunto de credenciais (chaves de e/ou as palavras-passe) utilizado para autenticar para vários dispositivos de hardware sob o controlo dele. O sistema utilizado para de transporte, persistência e a utilização destas credenciais foi concebido para impedir que os programadores do Azure, os administradores e acesso de serviços de cópia de segurança/técnico a informações confidenciais, confidenciais ou privadas.

Encriptação baseada na identidade de mestre de FC chave pública é utilizada no programa de configuração FC e reconfiguração de FC exceder o tempo para transferir as credenciais utilizadas para aceder a dispositivos de hardware de rede. As credenciais são obtidas e desencriptadas de FC quando necessita de-los.

**Dispositivos de rede**: contas de serviço de rede estão configuradas pela equipa de redes do Azure para ativar um cliente do Microsoft Azure autenticar dispositivos de rede (routers, comutadores e Balanceadores de carga).

## <a name="secure-service-administration"></a>Proteger a administração de serviço
Técnico de operações do Microsoft Azure é necessários para utilizar estações de trabalho administração seguro (SAWs; os clientes podem implementar controlos semelhantes utilizando estações de trabalho de acesso privilegiado ou PAWs). A abordagem VIMOS é uma extensão com a prática recomendada bem estabelecida para utilizar admin separada e as contas de utilizador para técnico administrativos. Esta prática utiliza uma conta de administrador individualmente atribuída está separada da conta de utilizador padrão do utilizador. VIMOS baseia-se que prática de separação de conta ao fornecer uma estação de trabalho fidedigna para essas contas confidenciais.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Funcionalidades de segurança da base de dados de SQL do Microsoft Azure](azure-infrastructure-sql.md)
- [Gestão e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização da infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Integridade da infraestrutura do Azure](azure-infrastructure-integrity.md)
- [Proteção de dados de cliente no Azure](azure-protection-of-customer-data.md)
