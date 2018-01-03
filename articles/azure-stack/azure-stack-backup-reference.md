---
title: "Referência de serviço de cópia de segurança de infraestrutura de pilha do Azure | Microsoft Docs"
description: "Este artigo contém material de referência para o serviço de cópia de segurança de infraestrutura de pilha do Azure."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 4e6e0a52b2c55239e38757223f54e5e94dc98c42
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="infrastructure-backup-service-reference"></a>Referência do serviço de cópia de segurança de infraestrutura

## <a name="azure-backup-infrastructure"></a>Infraestrutura de cópia de segurança do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pilha do Azure é composta por vários serviços que compõem o portal do Azure Resource Manager, e a experiência de gestão de infraestrutura. A experiência de gestão de dispositivo semelhantes da pilha do Azure está centrado nas reduzir a complexidade exposta ao operador da solução.

Cópia de segurança da infraestrutura foi concebida para internalize a complexidade da cópia de segurança e restaurar dados para os serviços de infraestrutura, garantir que os operadores pode concentrar-se em Gerir a solução e manter um SLA aos utilizadores.

Exportar os dados de cópia de segurança para uma partilha externa é necessário para evitar a armazenar cópias de segurança no mesmo sistema. Exigir uma partilha externa dá-o administrador a flexibilidade para determinar onde pretende armazenar os dados com base nas políticas de BC/DR de empresa existentes. 

### <a name="infrastructure-backup-components"></a>Componentes de infraestrutura de cópia de segurança

Cópia de segurança da infraestrutura inclui os seguintes componentes:

 - **Controlador de cópia de segurança de infraestrutura**  
 O controlador de cópia de segurança da infraestrutura de ser instanciado com e residem em cada nuvem de pilha do Azure.
 - **Fornecedor de cópia de segurança de recursos**  
 O fornecedor de cópia de segurança de recursos (cópia de segurança RP) é composto pelo utilizador interface e aplicação programa interfaces (API) s exposição funcionalidades básicas de cópia de segurança para a infraestrutura de pilha do Azure.

#### <a name="infrastructure-backup-controller"></a>Controlador de cópia de segurança de infraestrutura

O controlador de cópia de segurança da infraestrutura é um recurso de infraestrutura de serviço obtém criar instâncias do serviço para uma nuvem de pilha do Azure. Recursos de cópia de segurança são criados um regional nível e captura serviço específico de região de dados do AD, AC, do Azure Resource Manager, CRP, SRP, NRP, KeyVault, RBAC. 

### <a name="backup-resource-provider"></a>Fornecedor de cópia de segurança de recursos

O fornecedor de recursos de cópia de segurança apresenta a interface de utilizador no portal do Azure pilha para a configuração básica e listagem dos recursos de cópia de segurança. O operador pode efetuar as seguintes operações na interface de utilizador:

 - Ativar a cópia de segurança pela primeira vez, fornecendo a localização de armazenamento externo, credenciais e chave de encriptação
 - Criar a vista de concluir a cópia de segurança recursos e recursos de estado na criação
 - Modifique a localização de armazenamento onde o controlador de cópia de segurança coloca os dados de cópia de segurança
 - Modificar as credenciais que utiliza o controlador de cópia de segurança para aceder à localização de armazenamento externo
 - Modificar a chave de encriptação que utiliza o controlador de cópia de segurança para encriptar as cópias de segurança 


## <a name="backup-controller-requirements"></a>Requisitos de controlador de cópia de segurança

Esta secção descreve os requisitos importantes para a infraestrutura de cópia de segurança. Recomendamos que reveja as informações cuidadosamente antes de ativar a cópia de segurança para a instância de pilha do Azure e, em seguida, se referem novamente à mesma conforme necessário durante a implementação e operação subsequente.

Os requisitos incluem:

  - **Requisitos de software** – descreve as localizações de armazenamento suportadas e orientação de dimensionamento. 
  - **Requisitos de rede** – descreve os requisitos de rede para localizações de armazenamento diferente.  

### <a name="software-requirements"></a>Requisitos de software

#### <a name="supported-storage-locations"></a>Localizações de armazenamento suportadas

| Localização do armazenamento                                                                 | Detalhes                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Partilha de ficheiros SMB alojada num dispositivo de armazenamento no ambiente de rede fidedigna | Partilha SMB no mesmo centro de dados em que é implementada a pilha do Azure ou num centro de dados diferentes. Várias instâncias de pilha do Azure podem utilizar a mesma partilha de ficheiros. |
| Partilha de ficheiros SMB no Azure                                                          | Não é atualmente suportado.                                                                                                                                 |
| Armazenamento de BLOBs no Azure                                                            | Não é atualmente suportado.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>Versões suportadas do SMB

| SMB | Versão |
|-----|---------|
| SMB | 3     |

#### <a name="storage-location-sizing"></a>Dimensionamento de localização de armazenamento 

Controlador de cópia de segurança de infraestrutura será cópias de segurança a pedido. A recomendação é fazer cópias de segurança pelo últimas duas vezes um dia e a manter no máximo de sete dias de cópias de segurança. 

| Dimensionamento do ambiente | Tamanho previsto da cópia de segurança | Quantidade total de espaço necessário |
|-------------------|--------------------------|--------------------------------|
| 12 de 4 nós        | 10 GB                     | 140 GB                          |

### <a name="network-requirements"></a>Requisitos da rede
| Localização do armazenamento                                                                 | Detalhes                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Partilha de ficheiros SMB alojada num dispositivo de armazenamento no ambiente de rede fidedigna | É necessária a porta 445 se a instância de pilha do Azure reside num ambiente com firewall. Controlador de cópia de segurança da infraestrutura irá iniciar uma ligação ao servidor de ficheiros SMB através da porta 445. |
| Para utilizar o FQDN do servidor de ficheiros, o nome tem de ser resolvível do PEP             |                                                                                                                                                                                         |

> [!Note]  
> Nenhuma porta de entrada tem de ser aberta.


## <a name="infrastructure-backup-limits"></a>Limites de cópia de segurança de infraestrutura

Considere estes limites como planear, implementar e operar as instâncias de pilha do Microsoft Azure. A tabela seguinte descreve estes limites.

### <a name="infrastructure-backup-limits"></a>Limites de cópia de segurança de infraestrutura
| Identificador de limite                                                 | Limite        | Comentários                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Tipo de cópia de segurança                                                      | Apenas completo    | Controlador de cópia de segurança da infraestrutura só suporta cópias de segurança completas. Cópias de segurança incrementais não são suportadas.                                          |
| Cópias de segurança agendadas                                                | Apenas manual  | Controlador de cópia de segurança atualmente suporta apenas cópias de segurança a pedido                                                                                 |
| Tarefas de cópia de segurança simultâneas máximas                                   | 1            | Apenas uma tarefa de cópia de segurança Active Directory é suportada por instância de controlador de cópia de segurança.                                                                  |
| Configuração de comutador de rede                                     | Não está no âmbito | Administrador tem de copiar a configuração de comutador de rede utilizando ferramentas do OEM. Consulte a documentação para a pilha do Azure fornecidas pelo fornecedor cada OEM. |
| Anfitrião de ciclo de vida de hardware                                          | Não está no âmbito | Administrador tem de criar cópias de segurança anfitrião de ciclo de vida de Hardware utilizando ferramentas do OEM. Consulte a documentação para a pilha do Azure fornecidas pelo fornecedor cada OEM.      |
| Número máximo de partilhas de ficheiros                                    | 1            | Partilha de ficheiros apenas pode ser utilizada para armazenar dados de cópia de segurança                                                                                        |
| Serviços aplicacionais, a função, SQL Server, mysql dados do fornecedor de recursos de cópia de segurança | Não está no âmbito | Consulte a documentação de orientação publicada para implementação e gestão de valor acrescentado RPs criados pela Microsoft.                                                  |
| Fornecedores de cópia de segurança de terceiros de recursos                              | Não está no âmbito | Consulte a documentação de orientação publicada para implementação e gestão de valor acrescentado RPs criados pelos fornecedores de terceiros.                                          |

## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre o serviço de cópia de segurança da infraestrutura, consulte [dados de cópia de segurança e recuperação para a pilha do Azure com o serviço de cópia de segurança da infraestrutura](azure-stack-backup-infrastructure-backup.md).