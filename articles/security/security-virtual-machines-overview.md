---
title: Funcionalidades de segurança do Azure utilizadas com máquinas virtuais do Azure | Microsoft Docs
description: Este artigo fornece uma descrição geral das principais funcionalidades de segurança do Azure que podem ser utilizadas com máquinas virtuais do Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 7d31a434d4ead6dd8f8a13a08d368389904b5b4a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34364238"
---
# <a name="azure-virtual-machines-security-overview"></a>Descrição geral de segurança de máquinas virtuais do Azure
Pode utilizar máquinas virtuais do Azure para implementar uma vasta gama de soluções de informáticas de uma forma seja ágil. O serviço suporta o Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP e BizTalk Services do Azure. Por isso, pode implementar qualquer linguagem em praticamente qualquer sistema operativo e qualquer carga de trabalho.

Uma máquina virtual do Azure fornece-lhe a flexibilidade de virtualização sem ter de comprar e manter o hardware físico que executa a máquina virtual. Pode criar e implementar as suas aplicações com a garantia que os seus dados estão protegidos e seguro nos centros de dados altamente seguros.

Com o Azure, pode criar soluções de segurança avançada, compatíveis que:

* Protege as máquinas virtuais de vírus e software maligno.
* Encripta dados confidenciais.
* Proteger o tráfego de rede.
* Identifique e detetar ameaças.
* Cumpre os requisitos de conformidade.

O objetivo deste artigo é fornecer uma descrição geral das principais funcionalidades de segurança do Azure que podem ser utilizadas com máquinas virtuais. Ligações para artigos dê detalhes de cada funcionalidade, pelo que pode saber mais.  

## <a name="antimalware"></a>Antimalware
Com o Azure, pode utilizar o software antimalware dos fornecedores de segurança, tais como a Microsoft, da Symantec, Micro de tendência e Kaspersky. Este software ajuda a proteger as máquinas virtuais a partir de ficheiros maliciosos, adware e outras ameaças.

Antimalware da Microsoft para máquinas virtuais e serviços Cloud do Azure é uma funcionalidade de proteção em tempo real que ajuda a identificar e remover vírus, spyware e outro software malicioso.  Antimalware da Microsoft para o Azure fornece configuráveis alertas ao conhecido software malicioso ou indesejável, tentar instalar-se ou executar nos seus sistemas do Azure.

Antimalware da Microsoft para o Azure é uma solução de agente único para aplicações e ambientes de inquilino. Foi concebido para ser executada em segundo plano, sem qualquer intervenção humana. Pode implementar a proteção com base nas necessidades das cargas de trabalho aplicações, com o básico secure-por-predefinição ou avançadas de configuração personalizada, incluindo a monitorização de antimalware.

Quando implementar e ativar o Antimalware da Microsoft para o Azure, as seguintes funcionalidades de núcleos estão disponíveis:

* **Proteção em tempo real**: monitoriza as atividades nos serviços em nuvem e em máquinas virtuais para detetar e bloquear a execução de software maligno.
* **Agendar análise**: periodicamente efetua a análise de destino para detetar software maligno, incluindo ativamente em execução programas.
* **Remediação de software maligno**: executa automaticamente ação no software maligno detetado, tais como eliminar ou quarentena ficheiros maliciosos e limpeza de entradas do registo malicioso.
* **Atualizações de assinatura**: instala automaticamente as assinaturas de proteção mais recentes (definições de vírus) para se certificar de que a proteção está atualizada numa frequência previamente determinada.
* **Atualizações de motores antimalware**: atualiza automaticamente o Microsoft Antimalware para o motor do Azure.
* **Atualizações da plataforma de antimalware**: atualiza automaticamente o Microsoft Antimalware para a plataforma do Azure.
* **A proteção ativa**: metadados de telemetria de relatórios para o Azure sobre ameaças detetadas e os recursos suspeitos para se certificar de resposta rápida. Permite a entrega de assinatura síncrono em tempo real através do Microsoft Active Protection System (MAPS).
* **Amostras de relatórios**: fornece e relatórios de exemplos para o Microsoft Antimalware para o serviço do Azure para o ajudar a otimizar o serviço e ative a resolução de problemas.
* **Exclusões**: permite que a aplicação e administradores de serviço configurar a determinados ficheiros, processos e as unidades para os excluir da proteção e ao analisar o desempenho e a outros motivos.
* **Recolha de eventos de antimalware**: regista o estado de funcionamento do serviço antimalware, atividades suspeitas e as ações de remediação executadas no registo de eventos de sistema operativo e recolhe-los na sua conta do storage do Azure.

Saiba mais sobre o software antimalware para ajudar a proteger as máquinas virtuais:

* [Antimalware da Microsoft para máquinas virtuais e serviços em nuvem do Azure](azure-security-antimalware.md)
* [Implementar Soluções Antimalware em Máquinas Virtuais do Azure](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Como instalar e configurar a segurança avançada do tendência Micro como um serviço numa VM do Windows](../virtual-machines/windows/classic/install-trend.md)
* [Como instalar e configurar o Symantec Endpoint Protection numa VM do Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Soluções de segurança no Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Módulo de hardware de segurança
Melhorar a segurança de chave pode melhorar as proteções de encriptação e autenticação. Pode simplificar a gestão e segurança dos seus segredos críticos e chaves armazenando-os no Cofre de chaves do Azure. 

O Key Vault oferece a opção de armazenar as suas chaves em módulos de hardware de segurança (HSMs) com certificação pela norma FIPS 140-2 de Nível 2. Chaves de encriptação do SQL Server para cópia de segurança ou [encriptação transparente de dados](https://msdn.microsoft.com/library/bb934049.aspx) podem todos ser armazenados no Cofre de chaves com quaisquer chaves ou segredos das suas aplicações. As permissões e acesso a estes itens protegidos são geridos através de [do Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Saiba mais:

* [O que é o Cofre de chaves do Azure?](../key-vault/key-vault-whatis.md)
* [Introdução ao Cofre de chaves do Azure](../key-vault/key-vault-get-started.md)
* [Blogue do Cofre de chaves do Azure](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Encriptação de disco de máquina virtual
Encriptação de disco do Azure é uma nova capacidade para encriptar os discos da máquina virtual do Windows e Linux. Encriptação de disco do Azure utiliza o padrão da indústria [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funcionalidade do Windows e o [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) funcionalidade do Linux para fornecer a encriptação de volume para o SO e os discos de dados.

A solução é integrada com o Cofre de chaves do Azure para o ajudar a controlar e gerir as chaves de encriptação de disco e segredos na sua subscrição do Cofre de chaves. Garante que todos os dados em discos de máquinas virtuais sejam encriptados Inativos no armazenamento do Azure.

Saiba mais:

* [Encriptação de disco do Azure para o Windows e as VMs de Linux IaaS](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Encriptação de disco do Azure para Linux e máquinas virtuais do Windows](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Encriptar uma máquina virtual](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Cópia de segurança da máquina virtual
Cópia de segurança do Azure é uma solução dimensionável, que ajuda a proteger os dados da aplicação com o investimento de capital e custos operacionais mínimos. Os erros das aplicações podem danificar os dados e os erros humanos podem introduzir erros nas suas aplicações. Com a cópia de segurança do Azure, as máquinas virtuais que executam o Windows e Linux estão protegidas.

Saiba mais:

* [O que é o Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Percurso de aprendizagem de cópia de segurança do Azure](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [FAQ de serviço de cópia de segurança do Azure](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Uma parte importante da estratégia BCDR da sua organização é perceber como manter as cargas de trabalho empresarias e aplicações em execução quando planeada e falhas não planeadas ocorrerem. O Azure Site Recovery ajuda orquestrar a replicação, ativação pós-falha e recuperação de cargas de trabalho e aplicações para que estes estão disponíveis a partir de uma localização secundária se a sua localização principal ficar inativo.

Recuperação de sites:

* **Simplifica a sua estratégia BCDR**: recuperação de sites facilita a replicação, ativação pós-falha e recuperação de várias cargas de trabalho de empresas e aplicações a partir de uma única localização. Recuperação de site orquestra a replicação e ativação pós-falha, mas não interceta os dados da aplicação ou tem quaisquer informação sobre o assunto.
* **Proporciona replicação flexível**: utilizando a recuperação de Site, pode replicar as cargas de trabalho em execução em máquinas virtuais Hyper-V, máquinas virtuais VMware e servidores físicos Windows/Linux.
* **Suporta a ativação pós-falha e recuperação**: recuperação de sites fornece ativações pós-falha de teste manobras recuperação após desastre sem afetar os ambientes de produção. Também pode executar as ativações pós-falha planeadas sem nenhuma perda de dados para as falhas esperadas ou as ativações pós-falha não planeadas com perda mínima de dados (dependendo da frequência de replicação) perante desastres inesperados. Após a ativação pós-falha, pode efetuar para os sites primários. A Recuperação de Sites fornece planos de recuperação que podem incluir scripts e livros de automatização do Azure, para que possa personalizar a ativação pós-falha e a recuperação de aplicações de várias camadas.
* **Elimina centros de dados secundários**: pode replicar para um site secundário no local ou para o Azure. Utilizar o Azure como um destino de recuperação após desastre, elimina o custo e a complexidade de manter um site secundário. Dados replicados são guardados no armazenamento do Azure.
* **Integra-se com tecnologias BCDR existentes**: parceiros da recuperação de Site com funcionalidades de outras aplicações de BCDR. Por exemplo, pode utilizar a recuperação de Site para ajudar a proteger o SQL Server de back-end de cargas de trabalho empresariais. Isto inclui suporte nativo para SQL Server Always On gerir a ativação pós-falha de grupos de disponibilidade.

Saiba mais:

* [O que é o Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Como funciona o Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Que cargas de trabalho estão protegidas pelo Azure Site Recovery?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Redes virtuais
Máquinas virtuais precisa de conectividade de rede. Para suportar este requisito, o Azure requer que as máquinas virtuais ligadas a uma rede virtual do Azure. 

Uma rede virtual do Azure é uma construção lógica desenvolvida com os recursos de infraestrutura de rede do Azure física. Cada rede virtual do Azure lógica está isolada da todas as outras redes virtuais do Azure. Este isolamento ajuda a assegurar que o tráfego de rede das implementações não está acessível para outros clientes do Microsoft Azure.

Saiba mais:

* [Descrição geral de segurança de rede do Azure](security-network-overview.md)
* [Descrição Geral da Rede Virtual](../virtual-network/virtual-networks-overview.md)
* [Funcionalidades de redes e partnerships para cenários de empresa](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Gestão de políticas de segurança e de relatórios
Centro de segurança do Azure ajuda-o a evitar, detetar e responder a ameaças. Oferece do Centro de segurança maior visibilidade e controlo sobre a segurança dos seus recursos Azure. Fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições do Azure. Ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de soluções de segurança.

Centro de segurança ajudam a otimizar e monitorizar a segurança das máquinas virtuais por:

* Fornecer [recomendações de segurança](../security-center/security-center-recommendations.md) para as máquinas virtuais. Recomendações de exemplo incluem: aplicar atualizações do sistema, configure os pontos finais ACLs, ativar antimalware, ativar grupos de segurança de rede e aplicar encriptação de disco.
* Monitorizar o estado das suas máquinas virtuais.

Saiba mais:

* [Introdução ao Centro de Segurança do Azure](../security-center/security-center-intro.md)
* [Perguntas mais frequentes sobre o Centro de segurança do Azure](../security-center/security-center-faq.md)
* [Operações e planeamento do Centro de segurança do Azure](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Conformidade
Máquinas virtuais do Azure está certificado para FISMA, FedRAMP, HIPAA, PCI DSS nível 1 e outros programas de conformidade de chave. Este certificação torna mais fácil para as suas próprias aplicações do Azure satisfazer os requisitos de conformidade e para a sua empresa resolver uma vasta gama de locais e internacionais requisitos regulamentares.

Saiba mais:

* [Centro de fidedignidade da Microsoft: conformidade](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Nuvem fidedigna: Segurança do Microsoft Azure, privacidade e conformidade](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
