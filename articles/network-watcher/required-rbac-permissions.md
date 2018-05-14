---
title: Permissões necessárias para utilizar as capacidades de observador de rede do Azure | Microsoft Docs
description: Saiba que permissões de controlo de acesso baseado em funções do Azure são necessários para funcionar com capacidades de observador de rede.
services: network-watcher
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jdial
ms.openlocfilehash: 09f3a1e1d9c6796cb55ae8f0ab18bf8e1b3fa198
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Permissões de controlo de acesso baseado em funções necessárias para utilizar as capacidades de observador de rede

Controlo de acesso do Azure baseada em funções (RBAC) permite-lhe atribuir apenas as ações específicas para os membros da sua organização que necessitam para concluir as responsabilidades atribuídas. Para utilizar as capacidades de observador de rede, a conta, inicie sessão no Azure, tem de ser atribuída para a [proprietário](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner), [contribuinte](/role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor), ou [contribuinte de rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor) funções incorporadas, ou atribuído a um [função personalizada](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) atribuída as ações listadas para cada capacidade de observador de rede nas secções que se seguem. Para saber mais sobre as funcionalidades do observador de rede, consulte [que é o observador de rede?](network-watcher-monitoring-overview.md).

## <a name="network-watcher"></a>Observador de Rede

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Obter um observador de rede                                          |
| Microsoft.Network/networkWatchers/write                             | Criar ou atualizar um observador de rede                             |
| Microsoft.Network/networkWatchers/delete                            | Eliminar um observador de rede                                       |

## <a name="nsg-flow-logs"></a>Registos de fluxo do NSG

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Configurar um fluxo de registo                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Consultar o estado para um registo de fluxo                                    |

## <a name="connection-troubleshoot"></a>Resolver problemas de ligação

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | Resolver problemas relacionados com os resultados da consulta de uma ligação de teste                |
| Microsoft.Network/networkWatchers/troubleshoot/action               | Executar uma ligação de teste de resolução de problemas                             |

## <a name="connection-monitor"></a>Monitor de ligação

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Iniciar o monitor de ligação                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Parar um monitor de ligação                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Um monitor de ligação de consulta                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Obter um monitor de ligação                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Criar um monitor de ligação                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Eliminar um monitor de ligação                                    |

## <a name="packet-capture"></a>Captura de pacotes

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Consultar o estado de uma captura de pacotes                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Parar uma captura de pacotes                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Obtenha uma captura de pacotes                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | criar uma captura de pacotes                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Eliminar uma captura de pacotes                                        |

## <a name="ip-flow-verify"></a>Certifique-se de fluxo IP

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | Certifique-se de um fluxo IP                                              |

## <a name="next-hop"></a>Próximo salto

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | Obter o salto seguinte a partir de uma VM                                     |

## <a name="network-security-group-view"></a>Vista de grupo de segurança de rede

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Grupos de segurança de vista                                           |

## <a name="topology"></a>Topologia

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Obter a topologia                                                   |

## <a name="reachability-report"></a>Relatório de acessibilidade

| Ação                                                              | Nome                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Obter um relatório de acessibilidade do Azure                               |

## <a name="additional-actions"></a>Ações adicionais

As capacidades de observador de rede também requerem as seguintes ações:

- Microsoft.Storage/Read
- Microsoft.Authorization/Read
- Microsoft.Resources/subscriptions/resourceGroups/Read
- Microsoft.Storage/storageAccounts/listServiceSas/Action
- Microsoft.Storage/storageAccounts/listAccountSas/Action
- Microsoft.Storage/storageAccounts/listKeys/Action
- Microsoft.Compute/virtualMachines/Read
- Microsoft.Compute/virtualMachines/Write
- Microsoft.Compute/virtualMachineScaleSets/Read
- Microsoft.Compute/virtualMachineScaleSets/Write
- Microsoft.Insights/alertRules/*
- Microsoft.Support/*