---
title: Integridade de infraestrutura do Azure
description: Este artigo aborda a integridade da infraestrutura do Azure.
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
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 867bc66a68bec662153d8336e649cf46df02f101
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901321"
---
# <a name="azure-infrastructure-integrity"></a>Integridade de infraestrutura do Azure

## <a name="software-installation"></a>Instalação de software
Todos os componentes da pilha de software que estão instalados no ambiente do Azure são personalizadas incorporadas seguindo o processo de Security Development Lifecycle (SDL) da Microsoft. Todos os componentes de software (incluindo as imagens do sistema operacional e base de dados SQL) são implementados como parte do processo de alteração e gerenciamento de liberação. O sistema operacional que é executado em todos os nós é uma versão personalizada do Windows Server 2008 ou Windows Server 2012. A versão exata é escolhida por FC, de acordo com a função tenciona para o sistema operativo reproduzir. Além disso, o sistema operativo anfitrião não permite a instalação de quaisquer componentes de software não autorizado.

Alguns componentes do Microsoft Azure (por exemplo, RDFE, Portal do programador, etc.) são implementadas como os clientes do Azure na VM de convidado em execução no SO convidado.

## <a name="virus-scans-on-builds"></a>Verificações de vírus nas compilações
Compilações de componente (incluindo o sistema operacional) de software do Azure tem de passar por uma verificação de vírus com a ferramenta de software antivírus do Microsoft Endpoint Protection (MEP). Cada verificação de vírus, irá criar um registo no diretório build associado, com detalhes sobre o que foi analisado e os resultados da análise. A verificação de vírus faz parte do código-fonte compilação para cada componente no Azure. Código não será movido para produção sem ter um vírus limpo e com êxito de análise. Se existirem quaisquer problemas observados, a compilação é congelada e irá, em seguida, vá para as equipes de segurança na Microsoft Security para identificar em que o código "não autorizados" inserido a compilação.

## <a name="closedlocked-environment"></a>Ambiente bloqueado/fechado
Por predefinição, nós de infraestrutura do Azure e máquinas virtuais convidadas não tem as contas de utilizador criadas nos mesmos. Além disso, as contas de administrador do Windows padrão também estão desativadas. Os administradores do Microsoft Azure em direto suporte (WALS) podem – com uma autenticação adequada – iniciar sessão nestas máquinas e administrar a rede de produção do Azure para reparações de emergência.

## <a name="microsoft-azure-sql-database-authentication"></a>Autenticação de base de dados SQL do Microsoft Azure
Tal como acontece com qualquer implementação do SQL Server, gestão de contas de utilizador deve ser rigidamente controlada. Base de dados do Microsoft Azure SQL só suporta a autenticação do SQL Server. Utilizador de contas com palavras-passe fortes e configurado com específicas direitos devem ser usados também para complementar o modelo de segurança de dados do cliente.

## <a name="firewallacls-between-msft-corpnet-and-microsoft-azure-cluster"></a>Firewall/ACLs entre MSFT CorpNet e Cluster do Microsoft Azure
ACLs/Firewall entre a plataforma de serviço e de rede empresarial do MS proteger a base de dados do Microsoft Azure SQL contra acesso não autorizado insider. Além disso, apenas os utilizadores de intervalos de endereços IP da Microsoft CorpNet podem acessar o ponto de final de gestão de plataforma do WinFabric.

## <a name="firewallacls-between-nodes-in-an-azure-sql-db-cluster"></a>Firewall/ACLs entre nós num cluster de BD SQL do Azure
Como proteção adicional, como parte da defesa em profundidade-estratégia, ACLs/Firewall foram implementados entre nós num cluster do Microsoft Azure SQL DB. Todas as comunicações dentro do cluster de plataforma do WinFabric, bem como todo o código em execução é fidedigno.

## <a name="custom-mas-watchdogs"></a>MAs personalizadas (Watchdogs)
Base de dados do Microsoft Azure SQL emprega MAs personalizadas chamadas watchdogs para monitorizar o estado de funcionamento do cluster de BD SQL do Microsoft Azure.

## <a name="web-protocols"></a>Protocolos de Web

### <a name="role-instance-monitoring-and-restart"></a>Monitorização de instâncias de função e reinício
Azure garante que todas as em execução funções (web com acesso à Internet ou funções de trabalho de processamento de back-end) implementadas estão sujeitos a constante estado de funcionamento da monitorização para garantir que eles são efetivamente e com eficiência a fornecer os serviços em que já foi aprovisionados. No caso de uma função de fica danificada, por uma falha crítica do aplicativo a ser o problema de configuração subjacente ou hospedado dentro da instância de função em si, Microsoft Azure FC detectará o problema dentro da instância de função e iniciar um Estado de correção .

### <a name="compute-connectivity"></a>Conectividade de computação
Azure garante que a aplicação/serviço implementado está acessível através de protocolos padrão baseada na web. Instâncias virtuais de função da web com acesso à Internet terão conectividade externa de Internet e estarão acessíveis diretamente pelos utilizadores do web. Instâncias de virtual de função de trabalho de processamento de back-end tem conectividade de Internet externa, mas não podem ser acedidas diretamente por um usuário de web externo, para proteger a confidencialidade e a integridade das operações que funções de trabalho executam em nome do instâncias de virtual de função web acessível publicamente.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que a Microsoft faz para proteger a infraestrutura do Azure, veja:

- [Recursos do Azure, no local e a segurança física](azure-physical-security.md)
- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Recursos de segurança da base de dados do Microsoft Azure SQL](azure-infrastructure-sql.md)
- [Gerenciamento e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização de infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Proteção de dados do cliente no Azure](azure-protection-of-customer-data.md)
