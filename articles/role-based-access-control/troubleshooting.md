---
title: Resolução de problemas RBAC no Azure | Microsoft Docs
description: Resolva problemas com o controlo de acesso do Azure baseada em funções (RBAC).
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 557d3330ef155181c050a18b14d31b65ba1f2dcf
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295405"
---
# <a name="troubleshooting-rbac-in-azure"></a>RBAC de resolução de problemas no Azure

Este artigo responde a questões recorrentes sobre o controlo de acesso baseado em funções (RBAC), para que saiba o que esperar quando utiliza as funções no portal do Azure e pode resolver problemas de acesso. Estas três funções abrangem todos os tipos de recursos:

* Proprietário  
* Contribuinte  
* Leitor  

Os proprietários e contribuintes têm acesso total à experiência de gestão, mas um contribuinte não é possível conceder acesso a outros utilizadores ou grupos. Coisas obter um pouco mais interessantes com a função de leitor, pelo que é onde irá gastamos algum tempo. Para obter informações sobre como conceder acesso, seee [gerir o acesso através do portal do Azure e o RBAC](role-assignments-portal.md).

## <a name="app-service"></a>Serviço de Aplicações
### <a name="write-access-capabilities"></a>Capacidades de acesso de escrita
Se conceder um acesso de só de leitura de utilizador para uma aplicação web única, algumas funcionalidades estão desativadas que não seria de esperar. As seguintes capacidades de gestão requerem **escrever** aceder a uma aplicação web (contribuinte ou proprietário) e não estão disponíveis em qualquer cenário de só de leitura.

* Comandos (como iniciar, parar, etc.)
* Alterar definições como a configuração geral, as definições de dimensionamento, as definições de cópia de segurança e as definições de monitorização
* Aceder a credenciais de publicação e outros segredos, como as definições de aplicação e as cadeias de ligação
* Registos de transmissão em fluxo
* Configuração de registos de diagnóstico
* Consola (linha de comandos)
* Implementações de Active Directory e recentes (para a implementação contínua de local git)
* Estimativa de gastos
* Testes Web
* Rede virtual (visível apenas para um leitor se anteriormente tiver sido configurada uma rede virtual por um utilizador com acesso de escrita).

Se não é possível aceder a qualquer um destes mosaicos, terá de peça ao seu administrador de acesso de Contribuidor na aplicação web.

### <a name="dealing-with-related-resources"></a>Lidar com recursos relacionados
As Web apps são complicou pela presença de alguns diferentes recursos interplay. Segue-se um grupo de recursos típico com alguns sites:

![Grupo de recursos de aplicação Web](./media/troubleshooting/website-resource-model.png)

Como resultado, se conceder alguém acesso apenas na aplicação web, muitas das funcionalidades no painel do Web site no portal do Azure está desativado.

Estes itens requerem **escrever** acesso a **plano do App Service** que corresponde ao seu Web site:  

* Visualizar a aplicação web do escalão de preço (gratuita ou Standard)  
* Configuração de dimensionamento (número de instâncias, tamanho da máquina virtual, definições de dimensionamento automático)  
* Quotas (storage, largura de banda, CPU)  

Estes itens requerem **escrever** acesso a totalidade **grupo de recursos** que contém o seu Web site:  

* Certificados SSL e os enlaces (certificados SSL podem ser partilhados entre sites no mesmo grupo de recursos e geolocalização)  
* Regras de alerta  
* Definições de dimensionamento automático  
* Componentes de informações de aplicação  
* Testes Web  

## <a name="azure-functions"></a>Funções do Azure
Algumas funcionalidades do [das funções do Azure](../azure-functions/functions-overview.md) necessitam de acesso de escrita. Por exemplo, se um utilizador tem atribuído a função de leitor, não estarão possam ver as funções dentro de uma aplicação de função. O portal irá apresentar **(sem acesso)**.

![Função sem acesso a aplicações](./media/troubleshooting/functionapps-noaccess.png)

Pode clicar um leitor a **funcionalidades da plataforma** separador e, em seguida, clique em **todas as definições** ver algumas definições relacionadas com uma aplicação de função (semelhante a uma aplicação web), mas não os podem modificar qualquer uma destas definições.

## <a name="virtual-machine"></a>Máquina virtual
Muito como web Apps, algumas funcionalidades no painel da máquina virtual requerem acesso de escrita para a máquina virtual ou a outros recursos no grupo de recursos.

Máquinas virtuais relacionadas com os nomes de domínio, redes virtuais, contas de armazenamento e regras de alertas.

Estes itens requerem **escrever** acesso a **Máquina Virtual**:

* Pontos Finais  
* Endereços IP  
* Discos  
* Extensões  

Esses requisitos requerem **escrever** acesso a ambos os **Máquina Virtual**e o **grupo de recursos** (juntamente com o nome de domínio) que está a ser:  

* Conjunto de disponibilidade  
* Conjunto com balanceamento de carga  
* Regras de alerta  

Se não é possível aceder a qualquer um destes mosaicos, peça ao seu administrador de acesso de contribuinte ao grupo de recursos.

## <a name="next-steps"></a>Passos Seguintes
* [Gerir o acesso através do portal do Azure e do RBAC](role-assignments-portal.md)
* [Ver registos de atividade para que as alterações do RBAC](change-history-report.md)

