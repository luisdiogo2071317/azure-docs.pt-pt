---
title: Referência de serviço de cópia de segurança de infra-estrutura de pilha do Azure | Documentos da Microsoft
description: Este artigo contém o material de referência para o serviço de cópia de segurança de infraestrutura do Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: D6EC0224-97EA-446C-BC95-A3D32F668E2C
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: 5347fd5ce0f2d2951c34df89bcffafffed6db5c5
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054136"
---
# <a name="infrastructure-backup-service-reference"></a>Referência de serviço de cópia de segurança de infra-estrutura

## <a name="azure-backup-infrastructure"></a>Infraestrutura de cópia de segurança do Azure

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

O Azure Stack é composta por vários serviços que compõem o portal, o Azure Resource Manager, e a experiência de gestão da infraestrutura. A experiência de gestão de tipo de aplicação do Azure Stack se concentra na redução da complexidade exposta para o operador da solução.

Cópia de segurança de infra-estrutura foi concebida para internalizar a complexidade de criar cópias de segurança e restaurar os dados para serviços de infraestrutura, garantir que os operadores pode focar a solução de gerenciamento e manutenção de um SLA para os utilizadores.

Exportar os dados de cópia de segurança para uma partilha externa é necessário para evitar a armazenar cópias de segurança no mesmo sistema. Exigir uma partilha externa oferece ao administrador a flexibilidade para determinar onde pretende armazenar os dados com base nas políticas de BC/DR de empresa existentes. 

### <a name="infrastructure-backup-components"></a>Componentes de cópia de segurança da infraestrutura

Cópia de segurança da infraestrutura inclui os seguintes componentes:

 - **Controlador de cópia de segurança de infra-estrutura**  
 O controlador de cópia de segurança da infraestrutura é instanciado com e reside em cada Cloud do Azure Stack.
 - **Fornecedor de recursos de cópia de segurança**  
 O fornecedor de recursos de Backup (cópia de segurança RP) é composta pelo utilizador interface e a aplicação programa interfaces (API) s expor a funcionalidade básica de cópia de segurança para a infraestrutura do Azure Stack.

#### <a name="infrastructure-backup-controller"></a>Controlador de cópia de segurança de infra-estrutura

O controlador de cópia de segurança de infra-estrutura é um recurso de infraestrutura do serviço de serviço será instanciado para uma Cloud do Azure Stack. Recursos de cópia de segurança são criados num nível e captura de serviço específico da região dados regionais do AD, AC, Azure Resource Manager, CRP, o SRP, o NRP, o Cofre de chaves, o RBAC. 

### <a name="backup-resource-provider"></a>Fornecedor de recursos de cópia de segurança

O fornecedor de recursos de cópia de segurança apresenta uma interface de utilizador no portal do Azure Stack para a configuração básica e lista de recursos de cópia de segurança. Operador pode efetuar as seguintes operações na interface do usuário:

 - Ativar cópia de segurança pela primeira vez, fornecendo a localização de armazenamento externo, credenciais e chave de encriptação
 - Criar a vista de concluir a cópia de segurança recursos e recursos de estado a ser criado
 - Modifique a localização de armazenamento em que o controlador de cópia de segurança coloca os dados de cópia de segurança
 - Modificar as credenciais que utiliza o controlador de cópia de segurança para aceder à localização de armazenamento externo
 - Modificar a chave de encriptação que utiliza o controlador de cópia de segurança para encriptar as cópias de segurança 


## <a name="backup-controller-requirements"></a>Requisitos do controlador de cópia de segurança

Esta secção descreve os requisitos importantes para a infraestrutura de cópia de segurança. Recomendamos que reveja as informações cuidadosamente antes de ativar cópia de segurança para a sua instância do Azure Stack e, em seguida, regressar mais tarde a ela conforme necessário durante a implantação e operação subseqüente.

Os requisitos incluem:

  - **Requisitos de software** – descreve as localizações de armazenamento suportadas e a orientação de dimensionamento. 
  - **Requisitos de rede** – descreve os requisitos de rede para localizações de armazenamento diferentes.  

### <a name="software-requirements"></a>Requisitos de software

#### <a name="supported-storage-locations"></a>Localizações de armazenamento suportadas

| Localização do armazenamento                                                                 | Detalhes                                                                                                                                                  |
|----------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Partilha de ficheiros SMB alojada num dispositivo de armazenamento dentro do ambiente de rede fidedigna | Partilha SMB no mesmo datacenter em que o Azure Stack é implementado ou num centro de dados diferente. Várias instâncias do Azure Stack, podem utilizar a mesma partilha de ficheiros. |
| Partilha de ficheiros SMB no Azure                                                          | Não são atualmente suportados.                                                                                                                                 |
| Armazenamento de BLOBs no Azure                                                            | Não são atualmente suportados.                                                                                                                                 |

#### <a name="supported-smb-versions"></a>Versões suportadas do SMB

| SMB | Versão |
|-----|---------|
| SMB | 3.x     |

#### <a name="storage-location-sizing"></a>Dimensionamento de localização de armazenamento 

Controlador de cópia de segurança de infra-estrutura faz a cópia de segurança de dados a pedido. Recomenda-se fazer o backup finalmente duas vezes um dia e manter no máximo de sete dias de cópias de segurança. 

| Dimensionamento do ambiente | Tamanho previsto de cópia de segurança | Quantidade total de espaço necessário |
|-------------------|--------------------------|--------------------------------|
| 12 de 4 nós        | 10 GB                     | 140 GB                          |

### <a name="network-requirements"></a>Requisitos da rede
| Localização do armazenamento                                                                 | Detalhes                                                                                                                                                                                 |
|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Partilha de ficheiros SMB alojada num dispositivo de armazenamento dentro do ambiente de rede fidedigna | É necessária a porta 445 se a instância do Azure Stack reside num ambiente com firewall. Controlador de cópia de segurança da infraestrutura será feita uma ligação ao servidor de ficheiros SMB através da porta 445. |
| Para utilizar o FQDN do servidor de ficheiros, o nome tem de ser resolvido a partir do PEP             |                                                                                                                                                                                         |

> [!Note]  
> Não existem portas de entrada tem de ser aberto.


## <a name="infrastructure-backup-limits"></a>Limites de cópia de segurança de infra-estrutura

Considere estes limites, como planejar, implantar e operar as instâncias do Microsoft Azure Stack. A tabela seguinte descreve estes limites.

### <a name="infrastructure-backup-limits"></a>Limites de cópia de segurança da infraestrutura
| Identificador de limite                                                 | Limite        | Comentários                                                                                                                                    |
|------------------------------------------------------------------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Tipo de cópia de segurança                                                      | Apenas completo    | Controlador de cópia de segurança de infra-estrutura só suporta cópias de segurança completas. Cópias de segurança incrementais não são suportadas.                                          |
| Cópias de segurança agendadas                                                | Apenas manual  | Controlador de cópia de segurança atualmente só suporta cópias de segurança a pedido                                                                                 |
| Tarefas de cópia de segurança em simultâneo máximas                                   | 1            | Apenas uma tarefa de cópia de segurança Active Directory é suportada por instância do controlador de cópia de segurança.                                                                  |
| Configuração de comutador de rede                                     | Não está no âmbito | Administrador tem de criar cópias de segurança a configuração de comutador de rede com as ferramentas do OEM. Consulte a documentação para o Azure Stack, fornecido por cada fornecedor de OEM. |
| Anfitrião de ciclo de vida do hardware                                          | Não está no âmbito | Administrador tem de fazer backup de anfitrião de ciclo de vida do Hardware usando ferramentas de OEM. Consulte a documentação para o Azure Stack, fornecido por cada fornecedor de OEM.      |
| Número máximo de partilhas de ficheiros                                    | 1            | Apenas uma partilha de ficheiros pode ser utilizada para armazenar cópias de segurança                                                                                        |
| Cópia de segurança dos serviços de aplicações, função, SQL, dados de fornecedor de recursos do mysql | Não está no âmbito | Veja a orientação publicada para a implantação e gerenciamento de valor agregado RPs criado pela Microsoft.                                                  |
| Fornecedores de cópia de segurança de terceiros de recursos                              | Não está no âmbito | Veja a orientação publicada para a implantação e gerenciamento de valor agregado RPs criados por fornecedores de terceiros.                                          |

## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre o serviço de cópia de segurança da infraestrutura, consulte [cópia de segurança e recuperação de dados para o Azure Stack com o serviço de cópia de segurança da infraestrutura](azure-stack-backup-infrastructure-backup.md).