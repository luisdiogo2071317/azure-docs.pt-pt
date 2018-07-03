---
title: O Azure File Sync no local as definições de firewall e proxy | Documentos da Microsoft
description: O Azure File Sync no local a configuração de rede
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
ms.openlocfilehash: 7d86082abb6412072af44a6b2d794bcf536fa18d
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342731"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>As definições do proxy e da firewall do Azure File Sync
O Azure File Sync liga-se os servidores no local para os ficheiros do Azure, ativar a sincronização de múltiplos site e recursos de camada de cloud. Como tal, um servidor no local tem de estar ligado à internet. Administrador de TI tem de decidir o melhor caminho para o servidor chegar a serviços cloud do Azure.

Este artigo fornece informações sobre os requisitos específicos e as opções disponíveis para a ligação com êxito e segura de seu servidor para o Azure File Sync.

> [!Important]
> O Azure File Sync ainda não suporta firewalls e redes virtuais para uma conta de armazenamento. 

## <a name="overview"></a>Descrição geral
O Azure File Sync atua como um serviço de orquestração entre o Windows Server, a partilha de ficheiros e vários outros serviços do Azure para sincronizar os dados, tal como descrito no seu grupo de sincronização. Para o Azure File Sync funcione corretamente, terá de configurar os servidores para comunicar com os serviços do Azure seguintes:

- Storage do Azure
- Azure File Sync
- Azure Resource Manager
- Serviços de autenticação

> [!Note]  
> O agente de sincronização de ficheiros do Azure no Windows Server inicia todos os pedidos para a cloud services que resulta em apenas ter a ter em consideração o tráfego de saída de uma perspectiva de firewall. <br /> Nenhum serviço do Azure inicia uma ligação ao agente do Azure File Sync.


## <a name="ports"></a>Portas
O Azure File Sync move dados de ficheiros e metadados exclusivamente através de HTTPS e requer a porta 443 para ser abrir saída.
Como resultado, todo o tráfego é encriptado.

## <a name="networks-and-special-connections-to-azure"></a>Redes e ligações especiais para o Azure
O agente de sincronização de ficheiros do Azure não tem requisitos sobre canais especiais, como [ExpressRoute](../../expressroute/expressroute-introduction.md), etc. para o Azure.

O Azure File Sync irá funcionar através de meios que disponíveis que permitem o alcance para o Azure, automaticamente adaptação de várias características de rede, como a largura de banda, latência, bem como oferta de controlo de administração para ajuste fino. Nem todas as funcionalidades estão disponíveis neste momento. Se quiser configurar o comportamento específico, fale conosco [UserVoice de ficheiros do Azure](https://feedback.azure.com/forums/217298-storage?category_id=180670).

## <a name="proxy"></a>Proxy
O Azure File Sync suporta as definições de proxy de aplicações específicas e todo o computador.

Definições de proxy de todo o computador são transparentes para o agente de sincronização de ficheiros do Azure como o tráfego inteiro do servidor é encaminhado através do proxy.

Definições de proxy de aplicações específicas permitem a configuração de um proxy especificamente para o tráfego do Azure File Sync. Definições de proxy de aplicações específicas são suportados na versão do agente 3.0.12.0 ou posterior e podem ser configuradas durante a instalação do agente ou utilizando o cmdlet do PowerShell Set-StorageSyncProxyConfiguration.

Comandos do PowerShell para configurar definições de proxy de aplicações específicas:
```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```

## <a name="firewall"></a>Firewall
Conforme mencionado na secção anterior, 443 às necessidades de porta a ser abra saída. Com base em políticas no seu datacenter, o ramo ou a região, restringir ainda mais o tráfego através desta porta a domínios específicos pode ser pretendido ou necessário.

A tabela seguinte descreve os domínios necessários para a comunicação:

| Serviço | Domínio | Utilização |
|---------|----------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | Qualquer chamada de usuário (como o PowerShell) passa para/por este URL, incluindo a chamada de registo do servidor inicial. |
| **Azure Active Directory** | https://login.windows.net | Chamadas de Gestor de recursos do Azure têm de ser feitas por um usuário autenticado. Para ter êxito, este URL é utilizado para autenticação de utilizador. |
| **Azure Active Directory** | https://graph.windows.net/ | Como parte da implementação do Azure File Sync, será criado um principal de serviço no Azure Active Directory da subscrição. Este URL é usado para isso. Este principal é utilizado para delegar a um conjunto mínimo de direitos para o serviço de sincronização de ficheiros do Azure. O utilizador que executa a configuração inicial do Azure File Sync tem de ser um utilizador autenticado com privilégios de proprietário da subscrição. |
| **Armazenamento do Azure** | &ast;.core.windows.net | Quando o servidor transfere um ficheiro, em seguida, o servidor executa esse movimento de dados com mais eficiência quando se fala diretamente para a partilha de ficheiros do Azure na conta de armazenamento. O servidor tem uma chave SAS que só permite o acesso de partilha de ficheiros de destino. |
| **Sincronização de ficheiros do Azure** | &ast;.one.microsoft.com | Após o registo do servidor inicial, o servidor recebe um URL regional para a instância do serviço Azure File Sync nessa região. O servidor pode utilizar o URL para se comunicar diretamente e eficiente com a instância de lidar com a sincronizar. |

> [!Important]
> Quando a permitir o tráfego para &ast;. one.microsoft.com, o tráfego para mais do que apenas o serviço de sincronização é possível a partir do servidor. Existem muitos serviços da Microsoft mais disponíveis em subdomínios.

Se &ast;. one.microsoft.com é demasiado abrangente, pode limitar a comunicação do servidor ao permitir que apenas explícitas instâncias regionais do serviço Azure File Sync. Qual instância (s) para escolher depende da região do serviço de sincronização de armazenamento para o qual ter implementado e o servidor registado. Essa é a região que é necessário permitir que para o servidor. Em breve haverá mais URLs para permitir que novas funcionalidades de continuidade de negócio. 

| Região | URL de ponto de final regionais do Azure File Sync |
|--------|---------------------------------------|
| Leste da Austrália | https://kailani-aue.one.microsoft.com |
| Canadá Central | https://kailani-cac.one.microsoft.com |
| EUA Leste | https://kailani1.one.microsoft.com |
| Sudeste Asiático | https://kailani10.one.microsoft.com |
| Reino Unido Sul | https://kailani-uks.one.microsoft.com |
| Europa Ocidental | https://kailani6.one.microsoft.com |
| EUA Oeste | https://kailani.one.microsoft.com |

> [!Important]
> Se definir essas regras de firewall detalhadas, verifique este documento, muitas vezes e atualizar as regras de firewall para evitar interrupções do serviço devido a listagens de URL Desatualizadas ou incompletas nas definições do firewall.

## <a name="summary-and-risk-limitation"></a>Resumo e o risco de limitação
As listas anteriormente este documento contém os URLs do Azure File Sync atualmente se comunica com. Firewalls tem de ser capazes de permitir o tráfego de saída para esses domínios, bem como as respostas dos mesmos. A Microsoft está empenhada manter esta lista atualizada.

Configuração de domínio, restringindo as regras de firewall pode ser uma medida para melhorar a segurança. Se forem utilizadas estas configurações de firewall, é necessário ter em mente que URLs serão adicionados e alterados ao longo do tempo. Por isso é uma medida prudente para verificar as tabelas neste documento como parte de um processo de gestão de alteração de uma versão de agente de sincronização de ficheiros do Azure para outra numa implementação de teste do agente mais recente. Desta forma, pode garantir que a firewall está configurada para permitir o tráfego para domínios o agente mais recente requer.

## <a name="next-steps"></a>Passos Seguintes
- [Planear uma implementação do Azure File Sync](storage-sync-files-planning.md)
- [Implementar o Azure File Sync (pré-visualização)](storage-sync-files-deployment-guide.md)
