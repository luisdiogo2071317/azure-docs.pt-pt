---
title: Resolução de problemas de RBAC no Azure | Documentos da Microsoft
description: Resolva problemas com o controlo de acesso baseado em função do Azure (RBAC).
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 186bcf26639f5cff2dcbf1e805913ac7edab7df4
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437371"
---
# <a name="troubleshooting-rbac-in-azure"></a>Resolução de problemas de RBAC no Azure

Este artigo responde a perguntas comuns sobre o controlo de acesso baseado em funções (RBAC), para que saiba o que esperar ao utilizar as funções no portal do Azure e pode resolver problemas de acesso. Estas três funções abrangem todos os tipos de recursos:

* Proprietário  
* Contribuinte  
* Leitor  

Os proprietários e contribuintes têm acesso total para a experiência de gestão, mas um Contribuidor não é possível conceder acesso a outros utilizadores ou grupos. As coisas ficam um pouco mais interessantes com a função de leitor, pelo que é onde vamos gastar algum tempo. Para obter informações sobre como conceder acesso, seee [gerir o acesso com RBAC e o portal do Azure](role-assignments-portal.md).

## <a name="app-service"></a>Serviço de Aplicações
### <a name="write-access-capabilities"></a>Capacidades de acesso de escrita
Se conceder um acesso de só de leitura do utilizador para uma aplicação web individual, algumas funcionalidades estão desativadas que não pode estar esperando. As seguintes capacidades de gestão requerem **escrever** aceder a uma aplicação web (Contribuidor ou proprietário) e não estão disponíveis em qualquer cenário de só de leitura.

* Comandos (como iniciar, parar, etc.)
* Como a alteração das definições de configuração geral, as definições de dimensionamento, as definições de cópia de segurança e as definições de monitorização
* Aceder a credenciais de publicação e outros segredos, como as definições da aplicação e as cadeias de ligação
* Registos de transmissão em fluxo
* Configuração de registos de diagnóstico
* Console (linha de comandos)
* Implementações de Active Directory e recentes (para implementação contínua do local git)
* Estimativa de gastos
* Testes Web
* Rede virtual (visível apenas para um leitor se anteriormente tiver sido configurada uma rede virtual por um utilizador com acesso de escrita).

Se não é possível aceder a qualquer um destes mosaicos, terá de solicitar o administrador de acesso de contribuinte para a aplicação web.

### <a name="dealing-with-related-resources"></a>Lidar com os recursos relacionados
Aplicações Web são complicadas pela presença de alguns recursos diferentes que interação. Este é um grupo de recursos típicos com os Web sites alguns:

![Grupo de recursos de aplicação Web](./media/troubleshooting/website-resource-model.png)

Como resultado, se conceder alguém o acesso apenas na aplicação web, grande parte da funcionalidade no painel do Web site no portal do Azure está desativado.

Esses itens requerem **escrever** aceder para o **plano do App Service** que corresponde ao seu Web site:  

* Ver a aplicação web do escalão de preço (gratuita ou Standard)  
* Configuração de dimensionamento (número de instâncias, tamanho da máquina virtual, definições de dimensionamento automático)  
* Quotas (armazenamento, CPU, largura de banda)  

Esses itens requerem **escrever** acesso a todo **grupo de recursos** que contém o seu Web site:  

* Certificados SSL e enlaces (certificados SSL podem ser partilhados entre sites no mesmo grupo de recursos e localização geográfica)  
* Regras de alerta  
* Definições de dimensionamento automático  
* Componentes de informações de aplicação  
* Testes Web  

## <a name="azure-functions"></a>Funções do Azure
Algumas funcionalidades do [as funções do Azure](../azure-functions/functions-overview.md) necessitam de acesso de escrita. Por exemplo, se um utilizador tem atribuído a função de leitor, não poderão ver as funções dentro de uma aplicação de funções. O portal apresentará **(sem acesso)**.

![Aplicações de funções sem acesso](./media/troubleshooting/functionapps-noaccess.png)

Um leitor pode clicar a **funcionalidades de plataforma** separador e, em seguida, clique em **todas as definições** ver algumas definições relacionadas com uma aplicação de funções (semelhante a uma aplicação web), mas não podem modificar qualquer uma destas definições.

## <a name="virtual-machine"></a>Máquina virtual
Bem como com as aplicações web, algumas funcionalidades no painel da máquina virtual exigem acesso de escrita para a máquina virtual ou para outros recursos no grupo de recursos.

Máquinas virtuais estão relacionadas com nomes de domínio, redes virtuais, contas de armazenamento e regras de alerta.

Esses itens requerem **escrever** aceder para o **Máquina Virtual**:

* Pontos Finais  
* Endereços IP  
* Discos  
* Extensões  

Eles exigem **escrever** acesso a ambos os **Máquina Virtual**e o **grupo de recursos** (juntamente com o nome de domínio) que estejam na:  

* Conjunto de disponibilidade  
* Conjunto com balanceamento de carga  
* Regras de alerta  

Se não é possível aceder a qualquer um destes mosaicos, peça ao administrador de acesso de Contribuidor ao grupo de recursos.

## <a name="next-steps"></a>Passos Seguintes
* [Gerir o acesso com RBAC e o portal do Azure](role-assignments-portal.md)
* [Ver registos de atividade para alterações RBAC](change-history-report.md)

