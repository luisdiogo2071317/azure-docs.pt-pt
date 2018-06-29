---
title: Proteção de dados de cliente no Azure
description: Este artigo aborda como o Azure protege os dados de cliente.
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
ms.openlocfilehash: 9a3b00e39f78f65b05b7d730447440d481979539
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102528"
---
# <a name="protection-of-customer-data-in-azure"></a>Proteção de dados de cliente no Azure   
O acesso a dados do cliente por parte de funcionários operacionais e de suporte da Microsoft é negado por defeito. Quando é concedido acesso aos dados de cliente, é necessária a aprovação de liderança e, em seguida, o acesso cuidadosamente é gerido e registado. Os requisitos de controlo de acesso são estabelecidos pela política de segurança do Microsoft Azure seguintes:

- Sem acesso a dados de clientes por predefinição
- Não existem contas de utilizador ou administrador no cliente VMs
- Conceder o privilégio necessário para concluir tarefas; auditoria e registo de pedidos de acesso

Técnico de suporte do Microsoft Azure é atribuídos a contas exclusivas do empresa AD pela Microsoft. Microsoft Azure baseia-se no Microsoft Active Directory empresarial, geridos pelo MSIT, para controlar o acesso a sistemas de informação de chave. É necessária a autenticação multifator e apenas é concedido o acesso de consolas seguras.

Todas as tentativas de acesso são monitorizadas e podem ser apresentadas através de um conjunto básico de relatórios.

## <a name="data-protection"></a>Proteção de dados
Azure fornece aos clientes com a segurança dos dados forte – por predefinição e as opções do cliente.

**A segregação de dados** -Azure é um serviço de multi-inquilino, que significa que os clientes várias implementações e máquinas virtuais são armazenadas no mesmo hardware físico. Azure utiliza o isolamento lógico segregue os dados de outros dados cada cliente se. Segregação fornece a escala e económico vantagens dos serviços de multi-inquilino ao rigorosamente a impedir que os clientes acedem a dados entre si.

**Proteção de dados em rest** -os clientes são responsáveis por garantir que os dados armazenados no Azure são encriptados de acordo com as normas. O Azure disponibiliza uma vasta gama de capacidades de encriptação, concedendo aos clientes a flexibilidade para escolher a solução que melhor se adeque às suas necessidades. O Cofre de chaves do Azure ajuda os clientes facilmente manter o controlo das chaves utilizado por serviços e aplicações em nuvem para encriptar dados. Encriptação de disco do Azure permite aos clientes encriptar as máquinas virtuais. Encriptação do serviço de armazenamento do Azure torna possível encriptar todos os dados colocados a conta de armazenamento de um cliente.

**Proteção de dados em trânsito** -os clientes podem ativar a encriptação para o tráfego entre as suas próprias VMs e os utilizadores finais. Azure protege os dados em trânsito de ou para componentes externos e dados em trânsito internamente, tal impedindo duas redes virtuais. Azure utiliza o padrão da indústria segurança TLS (Transport Layer) 1.2 ou superior protocolo com chaves de encriptação RSA/SHA256 2048 bits, como recomendado pela CESG/NCSC, para encriptar as comunicações entre:

- o cliente e a nuvem
- internamente entre centros de dados e de sistemas do Azure

**Encriptação** -encriptação de dados no armazenamento e em trânsito pode ser implementado por clientes, como melhor prática para garantir a confidencialidade e integridade dos dados. É simples para os clientes configurar os seus serviços em nuvem do Azure para utilizar SSL para proteger as comunicações da Internet e ainda entre o seu Azure alojadas as VMs.

**Redundância de dados** -Microsoft garante que os dados estão protegidos se existir um cyberattack ou danos físicos para um datacenter. Os clientes podem optar por:

- armazenamento em país para considerações sobre a conformidade ou de latência
- fora do país armazenamento para fins de recuperação após desastre de segurança ou

Os dados podem ser replicados dentro de uma área geográfica selecionado para a redundância, mas não são transmitidos fora-lo. Os clientes têm várias opções para replicar dados, incluindo o número de cópias e número e a localização dos centros de dados de replicação.

Quando criar a sua conta de armazenamento, tem de selecionar uma das seguintes opções de replicação:

- Armazenamento localmente redundante (LRS). O Armazenamento localmente redundante mantém três cópias dos seus dados. LRS é replicado três vezes num único local e numa única região. O LRS protege os dados de falhas normais de hardware, mas não da falha de um único local.
- Armazenamento com redundância de zona (ZRS). O Armazenamento com redundância de zona mantém três cópias dos seus dados. O ZRS é replicado três vezes em dois ou três locais para fornecer uma durabilidade superior ao LRS. A replicação ocorre numa única região ou em duas regiões. O ZRS garante que os seus dados são duráveis numa única região.
- Armazenamento georredundante (GRS). O Armazenamento georredundante está ativado para a sua conta do Storage por predefinição aquando da sua criação. O GRS mantém seis cópias dos seus dados. Com a GRS, os dados são replicados três vezes numa região principal. Os dados também são replicados três vezes numa região secundária a centenas de quilómetros região primária, fornecendo o nível mais elevado de durabilidade. Se ocorrer uma falha na região primária, o Armazenamento do Azure fará uma ativação pós-falha para a região secundária. O GRS garante que os seus dados são duráveis em duas regiões separadas.

**Destruição dados** - quando os clientes eliminar dados ou deixe o Azure, Microsoft segue strict normas para substituir os recursos de armazenamento antes de reutilização, bem como destruição física do hardware desativado. Microsoft executa uma eliminação completa de dados no pedido do cliente e no cessação de contrato.

## <a name="customer-data-ownership"></a>Propriedade de dados de cliente
Microsoft não inspecionar, aprovar ou monitorizar as aplicações que os clientes implementar no Azure. Além disso, a Microsoft não souber que tipos de clientes de dados optar por armazenar no Azure. Microsoft não alega a propriedade de dados sobre as informações de cliente introduzidas no Azure.

## <a name="records-management"></a>Gestão de registos
Azure estabelecida requisitos de retenção de registos interno para dados de back-end. Os clientes são responsáveis por identificar os seus próprios requisitos de retenção do registo. Para os registos armazenados no Azure, o cliente é responsável por extrair os seus dados e a reter o conteúdo fora do Azure durante um período de retenção especificado de cliente.

O Azure oferece a capacidade para exportar os dados e relatórios do produto de auditoria de cliente. As exportações são guardadas localmente para manter as informações para uma retenção definidos pelo cliente. o período de tempo.

## <a name="electronic-discovery-e-discovery"></a>Deteção eletrónica (descoberta)
Clientes do Azure são responsáveis por complying com requisitos de deteção eletrónica na sua utilização dos serviços do Azure. Se um cliente do Azure tem de manter os seus dados de cliente, pode exportar e onde guardar os dados localmente. Além disso, os clientes podem pedir exportações os seus dados do departamento de suporte de cliente do Azure. Para além de permitir que os clientes exportar os seus dados, o Azure efetua um vasto conjunto de registo e monitorização internamente.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Funcionalidades de segurança da base de dados de SQL do Microsoft Azure](azure-infrastructure-sql.md)
- [Gestão e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização da infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Integridade da infraestrutura do Azure](azure-infrastructure-integrity.md)
