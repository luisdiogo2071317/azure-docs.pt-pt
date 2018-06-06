---
title: Sincronização de ficheiros do Azure no local definições da firewall e proxy | Microsoft Docs
description: Sincronização de ficheiros do Azure no local a configuração de rede
services: storage
documentationcenter: ''
author: fauhse
manager: aungoo
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: fauhse
ms.openlocfilehash: 5014c8204b6b6da539a41aaa3308d8787fb517a7
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738535"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>As definições do proxy e da firewall do Azure File Sync
Sincronização de ficheiros do Azure liga os servidores no local ao Azure Files, ativar a sincronização de vários site e as funcionalidades de criação de camadas de nuvem. Como tal, um servidor no local tem de estar ligado à internet. Administrador de TI tem de decidir o melhor caminho para o servidor para aceder a serviços em nuvem do Azure.

Este artigo irá fornecer informações sobre os requisitos específicos e as opções disponíveis para ligação segura e com êxito o servidor para a sincronização de ficheiros do Azure.

> [!Important]
> Sincronização de ficheiros do Azure ainda não suporta as firewalls e redes virtuais para uma conta de armazenamento. 

## <a name="overview"></a>Descrição geral
Sincronização de ficheiros do Azure funciona como um serviço de orquestração entre o servidor do Windows, a partilha de ficheiros do Azure e vários outros serviços do Azure para sincronizar os dados conforme descrito no seu grupo de sincronização. Para a sincronização de ficheiros de Azure funcione corretamente, terá de configurar os servidores para comunicar com os seguintes serviços do Azure:

- Storage do Azure
- Azure File Sync
- Azure Resource Manager
- Serviços de autenticação

> [!Note]  
> O agente de sincronização de ficheiros do Azure no Windows Server inicia todos os pedidos para serviços que resulta em apenas necessário considerar o tráfego de saída de uma perspetiva de firewall em nuvem. <br /> Nenhum serviço Azure inicia uma ligação ao agente de sincronização de ficheiros do Azure.


## <a name="ports"></a>Portas
Sincronização de ficheiros do Azure move os dados de ficheiros e metadados exclusivamente através de HTTPS e é necessária a porta 443 para ser abrir saída.
Como resultado todo o tráfego é encriptado.

## <a name="networks-and-special-connections-to-azure"></a>Redes e ligações especiais para o Azure
O agente de sincronização de ficheiros do Azure não tem requisitos sobre canais especiais, como [ExpressRoute](../../expressroute/expressroute-introduction.md), etc. para o Azure.

Sincronização de ficheiros do Azure irá funcionar através de meios que disponíveis que permitem alcance no Azure, automaticamente adaptação várias características de rede, como a largura de banda, latência, bem como o controlo de administração para fine-tuning da oferta. Nem todas as funcionalidades estão disponíveis neste momento. Se gostaria de configurar um comportamento específico, indique através de [Azure ficheiros UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670).

## <a name="proxy"></a>Proxy
Sincronização de ficheiros do Azure, atualmente, suporta as definições de proxy de toda a máquina. Esta definição de proxy é transparente para o agente de sincronização de ficheiros do Azure como o tráfego completo do servidor é encaminhado através deste proxy.

As definições de proxy de aplicação específicos estão atualmente em desenvolvimento e serão suportadas numa futura versão do agente de sincronização de ficheiros do Azure. Isto irá permitir a configuração de um proxy especificamente para tráfego de sincronização de ficheiros do Azure.

## <a name="firewall"></a>Firewall
Tal como mencionado na secção anterior, a porta 443 precisa de ser abra saída. Baseado em políticas no seu centro de dados, o ramo ou a região, restringir ainda mais o tráfego através desta porta para domínios específicos pode ser pretendido ou necessário.

A tabela seguinte descreve os domínios necessários para a comunicação:

| Serviço | Domínio | Utilização |
|---------|----------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | Qualquer chamada de utilizador (por exemplo, o PowerShell) realiza para/este URL, incluindo a chamada de registo do servidor inicial. |
| **Azure Active Directory** | https://login.windows.net | Chamadas de Gestor de recursos do Azure têm de ser efetuadas por um utilizador autenticado. Tenha êxito, este URL é utilizado para autenticação de utilizador. |
| **Azure Active Directory** | https://graph.windows.net/ | Como parte da implementação de sincronização de ficheiros do Azure, será criado um principal de serviço do Azure Active Directory da subscrição. Este URL é utilizado para esse. Este principal é utilizada para delegar um conjunto mínimo de direitos para o serviço de sincronização de ficheiros do Azure. O utilizador que efetua a configuração inicial de sincronização de ficheiros do Azure tem de ser um utilizador autenticado com privilégios de proprietário da subscrição. |
| **Armazenamento do Azure** | &ast;.core.windows.net | Quando o servidor transfere um ficheiro, em seguida, o servidor realiza esse movimento de dados de forma mais eficiente quando se fala diretamente para a partilha de ficheiros do Azure na conta de armazenamento. O servidor tem uma chave SAS que permita que apenas para acesso de partilha de ficheiros de destino. |
| **Sincronização de ficheiros do Azure** | &ast;.one.microsoft.com | Após o registo de servidor inicial, o servidor recebe um URL regional para a instância do serviço de sincronização de ficheiros do Azure nessa região. O servidor pode utilizar o URL para comunicar diretamente e de forma eficiente com a instância de lidar com a sincronização. |

> [!Important]
> Quando a permitir o tráfego para &ast;. one.microsoft.com, o tráfego para mais do que apenas o serviço de sincronização é possível do servidor. Muitos serviços Microsoft mais estão disponíveis em subdomínios.

Se &ast;. one.microsoft.com é demasiado abrangentes, pode limitar a comunicação do servidor, permitindo apenas explícitas regionais as instâncias do serviço de sincronização de ficheiros do Azure. As instâncias para escolher dependem da região do serviço de sincronização de armazenamento para o qual implementou e registar o servidor. Que é a região que tem de permitir para o servidor. Logo que será mais URLs para ativar novas funcionalidades de continuidade do negócio. 

| Região | URL de ponto final regional de sincronização de ficheiros do Azure |
|--------|---------------------------------------|
| Leste da Austrália | https://kailani-aue.one.microsoft.com |
| Canadá Central | https://kailani-cac.one.microsoft.com |
| EUA Leste | https://kailani1.one.microsoft.com |
| Sudeste Asiático | https://kailani10.one.microsoft.com |
| Reino Unido Sul | https://kailani-uks.one.microsoft.com |
| Europa Ocidental | https://kailani6.one.microsoft.com |
| EUA Oeste | https://kailani.one.microsoft.com |

> [!Important]
> Se definir estas regras de firewall de detalhado, verifique frequentemente este documento e atualizar as regras de firewall para evitar interrupções do serviço devido a desatualizada ou incompletas listagens de URL nas definições da firewall.

## <a name="summary-and-risk-limitation"></a>Resumo e o risco de limitação
As listas anteriores neste documento contêm os URLs de sincronização de ficheiros do Azure atualmente comunica com. As firewalls tem de ser capazes de permitir o tráfego de saída para estes domínios, bem como as respostas dos mesmos. Microsoft strives manter esta lista atualizada.

Configurar o domínio restringir as regras de firewall pode ser uma medida para melhorar a segurança. Se estas configurações de firewall são utilizadas, um tem tenha em atenção que os URLs serão adicionados e alterados ao longo do tempo. Desta forma é uma medida recomendada uma investigação para verificar as tabelas neste documento como parte de um processo de gestão de alteração de uma versão de agente de sincronização de ficheiros do Azure para outra numa implementação de teste do agente mais recente. Desta forma, pode garantir que a firewall está configurada para permitir tráfego para domínios do agente mais recente requer.

## <a name="next-steps"></a>Passos Seguintes
- [Planear uma implementação de sincronização de ficheiros do Azure](storage-sync-files-planning.md)
- [Implementar a sincronização de ficheiros do Azure (pré-visualização)](storage-sync-files-deployment-guide.md)
