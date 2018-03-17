---
title: "Pré-requisitos da implementação (ASDK) do Kit de desenvolvimento de pilha do Azure | Microsoft Docs"
description: Reveja os requisitos de hardware e de ambiente para o Kit de desenvolvimento de pilha do Azure (ASDK).
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 24a766ada1cdd1cb854b6869d571acd73e376327
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="azure-stack-deployment-planning-considerations"></a>Implementação do Azure da pilha considerações de planeamento
Antes de implementar o Kit de desenvolvimento de pilha do Azure (ASDK), certifique-se de que o computador de anfitrião do kit de desenvolvimento cumpre os requisitos descritos neste artigo.


## <a name="hardware"></a>Hardware
| Componente | Mínimo | Recomendado |
| --- | --- | --- |
| Unidades de disco: Sistema Operativo |1 disco do SO com 200 GB disponíveis, no mínimo, para a partição do sistema (SSD ou HDD) |1 disco do SO com 200 GB disponíveis, no mínimo, para a partição do sistema (SSD ou HDD) |
| Unidades de disco: dados do kit de desenvolvimento geral<sup>*</sup>  |4 discos. Cada disco fornece um mínimo de 140 GB de capacidade (SSD ou HDD). Todos os discos disponíveis são utilizados. |4 discos. Cada disco fornece um mínimo de 250 GB de capacidade (SSD ou HDD). Todos os discos disponíveis são utilizados. |
| Computação: CPU |Dual-Socket: 12 Núcleos Físicos (total) |Dual-Socket: 16 Núcleos Físicos (total) |
| Computação: Memória |96 GB de RAM |128 GB de RAM (este é o mínimo para suportar fornecedores de recursos de PaaS).|
| Computação: BIOS |Hyper-V Ativado (com suporte SLAT) |Hyper-V Ativado (com suporte SLAT) |
| Rede: NIC |Certificação Windows Server 2012 R2 necessária para a NIC; nenhuma funcionalidade específica necessária |Certificação Windows Server 2012 R2 necessária para a NIC; nenhuma funcionalidade específica necessária |
| Certificação de logótipo do HW |[Certificado para o Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Certificado para o Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

<sup>*</sup> Precisa de mais do que esta capacidade opção recomendada se planear adicionar muitas do [itens do marketplace](asdk-marketplace-item.md) a partir do Azure.

**Configuração de unidade de disco de dados:** todas as unidades de dados tem de ser do mesmo tipo (todas as SAS, todos os SATA ou NVMe todos os) e capacidade. Se forem utilizadas unidades de disco SAS, as unidades de disco devem ser ligadas através de um caminho único (sem MPIO; é fornecido suporte a caminhos múltiplos).

**Opções de configuração de HBA**

* (Preferencial) HBA Simples
* HBA RAID – O adaptador deve ser configurado no modo “Pass Through”
* HBA RAID – Os discos devem ser configurados como Disco Único, RAID-0

**Combinações de tipo de barramento suportado e suportes de dados**

* HDD SATA
* HDD SAS
* HDD RAID
* RAID SSD (se o tipo de suporte de dados for especificado/desconhecido<sup>*</sup>)
* SSD SATA + HDD SATA
* SSD SAS + HDD SAS
* NVMe

<sup>*</sup> Controladores RAID sem capacidade de pass-through não consegue reconhecer o tipo de suporte. Esses controladores marcar HDD e SSD como não especificado. Nesse caso, o SSD é utilizado como armazenamento persistente em vez de dispositivos a colocação em cache. Por conseguinte, pode implementar o kit de desenvolvimento nesses SSDs.

**Exemplo de HBAs**: LSI 9207-8i, LSI-9300-8i ou LSI-9265-8i no modo Pass Through

Estão disponíveis configurações do OEM de exemplo.

## <a name="operating-system"></a>Sistema operativo
|  | **Requisitos** |
| --- | --- |
| **Versão do SO** |Windows Server 2012 R2 ou posterior. A versão do sistema operativo não é crítica antes do início da implementação, conforme irá arrancar o computador anfitrião para o VHD que está incluído na instalação de pilha do Azure. Sistema operativo e todos os patches necessários já estão integrados com a imagem. Não utilize a quaisquer chaves para ativar a quaisquer instâncias de Windows Server utilizadas no kit de desenvolvimento. |

> [!TIP]
> Depois de instalar o sistema operativo, pode utilizar o [Verificador de implementação para a pilha de Azure](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) para confirmar que o seu hardware cumpre todos os requisitos.

## <a name="account-requirements"></a>Requisitos de conta
Normalmente, implementa o kit de desenvolvimento com ligação à internet, onde pode ligar ao Microsoft Azure. Neste caso, tem de configurar uma conta do Azure Active Directory (Azure AD) para implementar o kit de desenvolvimento.

Se o ambiente não está ligado à internet ou se não pretender utilizar o Azure AD, pode implementar a pilha do Azure, utilizando os serviços de Federação do Active Directory (AD FS). O kit de desenvolvimento inclui as suas próprias instâncias do AD FS e serviços de domínio do Active Directory. Se implementar utilizando esta opção, não tem de configurar as contas de antecedência.

>[!NOTE]
Se implementar utilizando a opção do AD FS, tem de voltar a implementar o Azure pilha para mudar para o Azure AD.

### <a name="azure-active-directory-accounts"></a>Contas do Active Directory do Azure
Para implementar a pilha do Azure, utilizando uma conta do Azure AD, tem de preparar uma conta do Azure AD antes de executar a script do PowerShell de implementação. Esta conta torna-se o Administrador Global para o inquilino do Azure AD. É utilizado para aprovisionar e delegar principais de serviço para todos os serviços de pilha do Azure que interagem com o Azure Active Directory e do Graph API e aplicações. Também é utilizado como o proprietário da subscrição de fornecedor predefinido (que é possível alterar mais tarde). Pode iniciar sessão no portal do administrador do seu sistema de pilha do Azure utilizando esta conta.

1. Crie uma conta do Azure AD, que é o administrador do diretório do Azure AD, pelo menos, um. Se já tiver uma conta, pode utilizá-la. Caso contrário, pode criar uma gratuitamente em [ https://azure.microsoft.com/free/ ](http://azure.microsoft.com/pricing/free/) (na China, visite <http://go.microsoft.com/fwlink/?LinkID=717821> em vez disso,). Se pretender mais tarde [registar pilha do Azure com o Azure](asdk-register.md), também tem de ter uma subscrição deste conta recentemente criada.
   
    Guarde estas credenciais para ser utilizado como o administrador de serviço. Esta conta pode configurar e gerir nuvens de recursos, contas de utilizador, planos de inquilino, quotas e preços. No portal, pode criar nuvens de sites, nuvens privadas de máquinas virtuais, criar planos e gerir subscrições do utilizador.
1. Crie conta de utilizador, pelo menos, um teste no seu Azure AD para que pode iniciar sessão para o kit de desenvolvimento de inquilino.
   
   | **Conta do Active Directory do Azure** | **Suportado?** |
   | --- | --- |
   | Conta escolar ou profissional, com uma subscrição do Azure público válida |Sim |
   | Conta da Microsoft com a Subscrição Pública do Azure válida |Sim |
   | Conta escolar ou profissional, com uma subscrição válida do Azure China |Sim |
   | Conta profissional ou escolar com válido US Government subscrição do Azure |Sim |

## <a name="network"></a>Rede
### <a name="switch"></a>Comutador
Uma porta disponível num comutador para a máquina do kit de desenvolvimento.  

A máquina do kit de desenvolvimento suporta a ligação uma porta de comutador de acesso ou a porta de ramal. Não são necessárias quaisquer funcionalidades especializadas no comutador. Se estiver a utilizar uma porta de ramal ou se tiver de configurar um ID de VLAN, terá de fornecer o ID de VLAN como um parâmetro de implementação.

### <a name="subnet"></a>Subrede
Não ligar a máquina do kit de desenvolvimento para as sub-redes que se seguem:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Estas sub-redes estão reservados para as redes internas no ambiente do kit de desenvolvimento.

### <a name="ipv4ipv6"></a>IPv4/IPv6
É apenas suportado o IPv4. Não pode criar redes IPv6.

### <a name="dhcp"></a>DHCP
Verifique se existe um servidor DHCP disponível na rede à qual liga o NIC. Se o DHCP não estiver disponível, terá de preparar uma rede IPv4 estática adicional para além daquela utilizada pelo anfitrião. Tem de fornecer esse endereço IP e gateway como um parâmetro de implementação.

### <a name="internet-access"></a>Acesso à Internet
Pilha do Azure requer acesso à Internet, diretamente ou através de um proxy transparente. Pilha do Azure não suporta a configuração de um proxy web para ativar o acesso à Internet. O IP do anfitrião e as novas IP atribuída à MAS-BGPNAT01 (pelo DHCP ou o IP estático) tem de ser capazes de aceder à Internet. As portas 80 e 443 são utilizadas em domínios graph.windows.net e login.microsoftonline.com.


## <a name="next-steps"></a>Passos Seguintes
[Transferir o pacote de implementação ASDK](asdk-download.md)
