---
title: Gerir recomendações de segurança no Centro de segurança do Azure | Documentos da Microsoft
description: Este documento explica-lhe como recomendações no Centro de segurança do Azure ajudar a proteger os seus recursos do Azure e mantenha-se em conformidade com as políticas de segurança.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2018
ms.author: rkarlin
ms.openlocfilehash: 3e8333b521832579a942d3fffb06103ad0431acc
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316622"
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Gerir recomendações de segurança no Centro de segurança do Azure
Este documento explica como utilizar as recomendações no Centro de segurança do Azure para o ajudar a proteger os seus recursos do Azure.

> [!NOTE]
> Este documento apresenta o serviço ao utilizar um exemplo de implementação.  Este documento não é um guia passo a passo.
>
>

## <a name="what-are-security-recommendations"></a>Quais são as recomendações de segurança?
O Centro de Segurança analisa periodicamente o estado de segurança dos seus recursos do Azure. Quando o Centro de Segurança identifica potenciais vulnerabilidades de segurança, cria recomendações. As recomendações orientam-no no processo de configuração de controlos necessários.

## <a name="implementing-security-recommendations"></a>Implementar recomendações de segurança
### <a name="set-recommendations"></a>Recomendações de conjunto
Na [definir políticas de segurança no Centro de segurança do Azure](security-center-azure-policy.md), vai aprender a:

* Configure políticas de segurança.
* Ative a recolha de dados.
* Escolha quais as recomendações para ver como parte da sua política de segurança.

Centro de recomendações de política atual em torno de atualizações do sistema, regras de linha de base, programas de antimalware [grupos de segurança de rede](../virtual-network/security-overview.md) em sub-redes e interfaces de rede, a auditoria de base de dados SQL, encriptação de dados transparente de banco de dados SQL, e firewalls de aplicações web.  [Definir políticas de segurança](security-center-azure-policy.md) fornece uma descrição de cada opção de recomendação.

### <a name="monitor-recommendations"></a>Recomendações de monitor
Após definir uma política de segurança, o Centro de Segurança analisa o estado de segurança dos seus recursos para identificar potenciais vulnerabilidades. O **recomendações** mosaico sob **descrição geral** permite-lhe determinar o número total de recomendações identificado pelo centro de segurança.

![Mosaico de recomendações][1]

Para ver os detalhes de cada recomendação, selecione o **mosaico de recomendações** sob **descrição geral**. **Recomendações** abre.

![Recomendações de filtro][2]

Pode filtrar recomendações. Para filtrar as recomendações, selecione **filtro** sobre o **recomendações** painel. O **filtro** é aberto o painel e selecionar os valores de gravidade e o estado que pretende ver.

As recomendações são apresentadas num formato de tabela em que cada linha representa uma recomendação específica. As colunas desta tabela são:

* **Descrição**: explica a recomendação e o que precisa ser feito para resolver o problema.
* **RECURSO**: lista os recursos aos quais se aplica esta recomendação.
* **ESTADO**: descreve o estado atual da Recomendação:
  * **Abra**: A recomendação ainda não foi tratada.
  * **Em curso**: A recomendação está atualmente a ser aplicada aos recursos e é necessária nenhuma ação por si.
  * **Resolvido**: A recomendação já foi concluída (neste caso, a linha está a cinzento).
* **GRAVIDADE**: descreve a gravidade dessa recomendação específica:
  * **Alta**: uma vulnerabilidade existe com um recurso significativo (por exemplo, um aplicativo, uma VM ou um grupo de segurança de rede) e necessita de atenção.
  * **Médio**: uma vulnerabilidade existe e passos não críticos ou adicionais são necessários para eliminá-la ou para concluir um processo.
  * **Baixa**: uma vulnerabilidade existe que deve ser resolvido, mas não necessita de atenção imediata. (Por predefinição, as recomendações baixas não são apresentadas, mas pode filtrar por recomendações baixas se pretender vê-los.)

Utilize a tabela abaixo como referência para ajudar a compreender as recomendações de disponibilidade e o que cada um deles faz se as aplicar.

> [!NOTE]
> Vai querer compreender a [clássico e modelos de implementação do Resource Manager](../azure-classic-rm.md) para recursos do Azure.
>
>

| Recomendação | Descrição |
| --- | --- |
| [Ativar a recolha de dados para subscrições](security-center-enable-data-collection.md) |Recomenda-se que ative a recolha de dados na política de segurança para cada uma das suas subscrições e todas as máquinas virtuais do Azure (VMs) e computadores não Azure. |
| [Remediar configurações de segurança](security-center-remediate-os-vulnerabilities.md) |Recomenda que Alinhe as configurações do SO com as regras de configuração de segurança recomendadas, por exemplo, não permita que as palavras-passe sejam guardadas. |
| [Aplicar atualizações do sistema](security-center-apply-system-updates.md) |Recomenda-se de que implante atualizações críticas e de segurança do sistema em falta para o Windows e VMs do Linux e computadores. |
| [Aplicar um Just-In-Time controlo de acesso de rede](security-center-just-in-time.md) | Recomenda-se de que aplicar just-in-acesso à VM do tempo. A apenas no tempo a funcionalidade está disponível no escalão Standard do Centro de segurança. Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança. |
| [Reiniciar após atualizações do sistema](security-center-apply-system-updates.md#reboot-after-system-updates) |Recomenda-se que reinicie uma VM para concluir o processo de aplicar atualizações do sistema. |
| [Adicionar uma firewall de aplicação Web](security-center-add-web-application-firewall.md) |Recomenda-se de que implante uma firewall de aplicações web (WAF) para pontos finais de web. Uma recomendação de WAF é apresentada para qualquer IP destinado ao público (IP de nível de instância ou IP com balanceamento de carga) que tenha um grupo de segurança de rede associado com portas de web de entrada aberta (80,443). </br>Centro de segurança recomenda que Aprovisiona uma WAF para ajudar na defesa contra ataques que visam as suas aplicações web em máquinas virtuais e no ambiente de serviço de aplicações. Um ambiente de serviço de aplicações (ASE) é uma [Premium](https://azure.microsoft.com/pricing/details/app-service/) service opção do plano do serviço de aplicações do Azure que fornece um ambiente totalmente isolado e dedicado para uma execução segura de aplicações do App Service do Azure. Para saber mais sobre o ASE, veja a [documentação do ambiente de serviço de aplicações](../app-service/environment/intro.md).</br>Pode proteger várias aplicações web no Centro de segurança com a adição desses aplicativos para as implementações existentes do WAF. |
| [Finalizar a proteção das aplicações](security-center-add-web-application-firewall.md#finalize-application-protection) |Para concluir a configuração de uma WAF, tráfego deve ser reencaminhado para a aplicação do WAF. Seguir essa recomendação conclui as alterações de configuração necessários. |
| [Adicionar uma Firewall da Próxima Geração](security-center-add-next-generation-firewall.md) |Recomenda-se de que adiciona a Next Generation Firewall (NGFW) a partir de um parceiro da Microsoft para aumentar sua proteções de segurança. |
| [Encaminhar o tráfego apenas através da NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Recomenda-se de que configura regras de grupo (NSG) de segurança de rede que imponham o tráfego de entrada para a sua VM através da sua NGFW. |
| [Instalar o Endpoint Protection](security-center-install-endpoint-protection.md) |Recomenda-se que aprovisione programas de antimalware em VMs (apenas VMs do Windows). |
| [Ativar grupos de segurança de rede em sub-redes ou máquinas virtuais](security-center-enable-network-security-groups.md) |Recomenda-se de que ativa NSGs em sub-redes ou VMs. |
| [Restringir o acesso através da Internet destinada ao ponto final](security-center-restrict-access-through-internet-facing-endpoints.md) |Recomenda-se de que configura regras de tráfego de entrada para NSGs. |
| [Ativar a auditoria e a deteção de ameaças em servidores SQL](security-center-enable-auditing-on-sql-servers.md) |Recomenda-se que ative a deteção de ameaças e auditoria para servidores SQL do Azure. (Apenas para serviços do SQL do azure. Não inclui SQL em execução nas suas máquinas virtuais.) |
| [Ativar a auditoria e a deteção de ameaças nas bases de dados SQL](security-center-enable-auditing-on-sql-databases.md) |Recomenda-se que ative a deteção de ameaças e auditoria para bases de dados SQL do Azure. (Apenas para serviços do SQL do azure. Não inclui SQL em execução nas suas máquinas virtuais.) |
| [Ativar a encriptação de dados transparente nas bases de dados SQL](security-center-enable-transparent-data-encryption.md) |Recomenda-se de que ativar a encriptação para bases de dados SQL. (Apenas serviço SQL do azure.) |
| [Ativar o Agente de VM](security-center-enable-vm-agent.md) |Permite-lhe ver as VMs que necessitam do Agente de VM. O agente da VM deve ser instalado em VMs para aprovisionar análise de patch, a análise de linha de base e programas de antimalware. O Agente de VM é instalado por predefinição em VMs que são implementadas a partir do Azure Marketplace. O artigo [VM Agent and Extensions – Part 2 (Agente de VM e Extensões – Parte 2)](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) fornece informações sobre como instalar o Agente de VM. |
| [Aplicar encriptação de discos](security-center-apply-disk-encryption.md) |Recomenda-se que encripte os discos da VM com o Azure Disk Encryption (VMs Windows e Linux). A encriptação é recomendada para o SO e os volumes de dados na sua VM. |
| [Disponibilizar detalhes de contacto de segurança](security-center-provide-security-contact-details.md) |Recomenda que fornecer segurança de informações de contacto para cada uma das suas subscrições. Informações de contacto são um e-mail endereço e número de telefone. As informações são utilizadas para contactá-lo se a nossa equipa de segurança Descubra que os recursos estão comprometidos. |
| [Atualizar a versão do SO](security-center-update-os-version.md) |Recomenda-se de que Atualize a versão do sistema operativo (SO) do serviço em nuvem para a versão mais recente disponível para sua família de SO.  Para saber mais sobre os serviços Cloud, veja a [descrição geral dos serviços de Cloud](../cloud-services/cloud-services-choose-me.md). |
| [Avaliação de vulnerabilidades não instalada](security-center-vulnerability-assessment-recommendations.md) |Recomenda-se de que instala uma solução de avaliação de vulnerabilidades na sua VM. |
| [Remediar vulnerabilidades](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Permite-lhe ver as vulnerabilidades de sistema e de aplicações detetadas pela solução de avaliação de vulnerabilidade instalada na sua VM. |
| [Ativar a encriptação da conta de armazenamento do Azure](security-center-enable-encryption-for-storage-account.md) | Recomenda-se de que ativa a encriptação do serviço de armazenamento do Azure para dados inativos. Encriptação de serviço de armazenamento (SSE) funciona ao encriptar os dados quando ele é escrito para o armazenamento do Azure e desencripta antes da obtenção. O SSE está atualmente disponível apenas para o serviço de Blobs do Azure e pode ser utilizado para blobs de blocos, blobs de páginas e blobs de acréscimo. Para obter mais informações, consulte [encriptação do serviço de armazenamento para dados Inativos](../storage/common/storage-service-encryption.md).</br>O SSE só é suportado em contas de armazenamento do Resource Manager. |
| [Ativar os controlos de aplicações adaptáveis](security-center-adaptive-application.md) | Recomenda-se de que aplicar controlos de aplicação adaptável nas suas VMs do Windows. Esta funcionalidade está disponível no escalão Standard do Centro de segurança. Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança. |
| Serviço de aplicações só devem estar acessível através de HTTPS | Recomenda-se de que limite o acesso do serviço de aplicações através de HTTPS apenas. |
| Web Sockets devem ser desativados para a aplicação Web| Recomenda-se de que examinar cuidadosamente a utilização de Web Sockets nas aplicações web.  O protocolo de Web Sockets está vulnerável a diferentes tipos de ameaças de segurança. |
| Utilizar domínios personalizados para a sua aplicação Web | Recomenda o uso de domínios personalizados para proteger uma aplicação web de ataques comuns, como o phishing e outros ataques relacionados com DNS. |
| Configurar restrições de IP para a aplicação Web | Recomenda-se de que definir uma lista de endereços IP que têm permissão para aceder à sua aplicação.  Utilização de restrições de IP de protege as aplicações web contra ataques comuns. |
| Não permitir que todos os ('* ') recursos para aceder à sua aplicação | Recomenda que não definir o parâmetro WEBSITE_LOAD_CERTIFICATES '*'. Definição do parâmetro '*"significa que todos os certificados serão carregados para o arquivo de certificados pessoais de aplicações web.  Isto pode levar ao abuso do princípio de privilégio mínimo porque é improvável que o site necessita de acesso a todos os certificados no tempo de execução. |
| CORS não deve permitir que todos os recursos aceder à sua aplicação | Recomenda-se de que permite que apenas necessários domínios interagir com a sua aplicação web. Entre os recursos de origens (CORS) de partilha não deve permitir que todos os domínios acedam à sua aplicação web. |
| Utilize a versão mais recente do .NET Framework suportada para a aplicação Web | Recomenda-se de que use a versão mais recente do .NET Framework para as classes de segurança mais recente. Utilizar classes mais antigas e de tipos pode fazer com que seu aplicativo vulnerável. |
| Utilizar a versão de Java mais recente suportada para a aplicação Web | Recomenda-se de que use a versão mais recente do Java para as classes de segurança mais recente. Utilizar classes mais antigas e de tipos pode fazer com que seu aplicativo vulnerável. |
| Utilizar a versão PHP mais recente suportada para a aplicação Web | Recomenda-se de que use a versão mais recente do PHP para as classes de segurança mais recente. Utilizar classes mais antigas e de tipos pode fazer com que seu aplicativo vulnerável. |
| [Adicionar uma firewall de aplicação Web](security-center-add-web-application-firewall.md) |Recomenda-se de que implante uma firewall de aplicações web (WAF) para pontos finais de web. Uma recomendação de WAF é apresentada para qualquer IP destinado ao público (IP de nível de instância ou IP com balanceamento de carga) que tenha um grupo de segurança de rede associado com portas de web de entrada aberta (80,443).</br></br>Centro de segurança recomenda que Aprovisiona uma WAF para ajudar na defesa contra ataques que visam as suas aplicações web em máquinas virtuais e no ambiente de serviço de aplicações. Um ambiente de serviço de aplicações (ASE) é uma [Premium](https://azure.microsoft.com/pricing/details/app-service/) service opção do plano do serviço de aplicações do Azure que fornece um ambiente totalmente isolado e dedicado para uma execução segura de aplicações do App Service do Azure. Para saber mais sobre o ASE, veja a [documentação do ambiente de serviço de aplicações](../app-service/environment/intro.md).</br></br>Pode proteger várias aplicações web no Centro de segurança com a adição desses aplicativos para as implementações existentes do WAF. |
| [Finalizar a proteção das aplicações](security-center-add-web-application-firewall.md#finalize-application-protection) |Para concluir a configuração de uma WAF, tráfego deve ser reencaminhado para a aplicação do WAF. Seguir essa recomendação conclui as alterações de configuração necessários. |
| Utilizar a versão de node. js mais recente suportada para a aplicação Web | Recomenda-se de que use a versão mais recente do node. js para as classes de segurança mais recente. Utilizar classes mais antigas e de tipos pode fazer com que seu aplicativo vulnerável. |
| CORS não deve permitir que todos os recursos de aceder à sua aplicação de função | Recomenda-se de que permite que apenas necessários domínios interagir com a sua aplicação web. Entre os recursos de origens (CORS) de partilha não deve permitir que todos os domínios acedam à sua aplicação de função. |
| Utilizar domínios personalizados para a aplicação de funções | Recomenda o uso de domínios personalizados para proteger uma aplicação de funções de ataques comuns, como o phishing e outros ataques relacionados com DNS. |
| Configurar restrições de IP para a aplicação de função | Recomenda-se de que definir uma lista de endereços IP que têm permissão para aceder à sua aplicação. Utilização de restrições de IP protege uma aplicação de funções de ataques comuns. |
| Função de aplicação só deve estar acessível através de HTTPS | Recomenda-se de que limite o acesso de aplicações de funções através de HTTPS apenas. |
| Depuração remota deve ser desativada para a aplicação de funções | Recomenda-se de que desativa a depuração para a aplicação de função se já não precisar de utilizá-lo. Depuração remota necessita de portas de entrada estar abertas na aplicação de funções. |
| Web Sockets devem ser desativados para a aplicação de função | Recomenda-se de que examinar cuidadosamente a utilização de Sockets Web nas aplicações de função. O protocolo de Web Sockets está vulnerável a diferentes tipos de ameaças de segurança. |
| Designar mais do que um proprietário na sua subscrição | Recomenda-se de que designar mais do que um proprietário da subscrição para ter a redundância de acesso de administrador. |
| Designar até 3 proprietários na sua subscrição | Recomenda-se de que designar menos de 3 proprietários de subscrições para reduzir a possibilidade de violação por um proprietário comprometido. |
| Ativar a MFA para contas com permissões de proprietário na sua subscrição | Recomenda-se que ative o multi-factor Authentication (MFA) para todas as contas de subscrição com privilégios de administrador para evitar falhas de segurança de contas ou recursos. |
| Ativar a MFA para contas com permissões de escrita na sua subscrição | Recomenda-se que ative o multi-factor Authentication (MFA) para todas as contas de subscrição com privilégios de escrita para evitar uma violação de contas ou recursos. |
| Ativar a MFA para contas com permissões de leitura na sua subscrição | Recomenda-se que ative o multi-factor Authentication (MFA) para todas as contas de subscrição com privilégios de leitura para evitar uma violação de contas ou recursos. |
| Remover contas externas com permissões de leitura da sua subscrição | Recomenda-se de que remover contas externas com privilégios de leitura da sua subscrição para impedir o acesso não monitorizado. |
| Remover contas externas com permissões de escrita da sua subscrição | Recomenda-se de que remover contas externas com privilégios de escrita da sua subscrição para impedir o acesso não monitorizado. |
| Remover contas externas com permissões de proprietário da sua subscrição | Recomenda-se de que remover contas externas com permissões de proprietário da sua subscrição para impedir o acesso não monitorizado. |
| Remover contas preteridas da subscrição | Recomenda que remova preterido contas das suas subscrições. |
| Remover contas preteridas com permissões de proprietário da subscrição | Recomenda que remova preterido contas com permissões de proprietário das suas subscrições. |

### <a name="apply-recommendations"></a>Aplicar recomendações
Depois de rever todas as recomendações, decida que um deve aplicar primeiro. Recomendamos que utilize a classificação de gravidade, como o parâmetro principal para avaliar quais recomendações deve ser aplicado primeiro.

Na tabela de recomendações acima, selecione uma recomendação e examiná-lo como um exemplo de como aplicar uma recomendação.

## <a name="next-steps"></a>Passos Seguintes
Neste documento, foram introduzidas para recomendações de segurança no Centro de segurança. Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Definir políticas de segurança no Centro de segurança do Azure](security-center-azure-policy.md) – Saiba como configurar políticas de segurança para as suas subscrições do Azure e grupos de recursos.
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md) – Saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de segurança do Azure](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Monitorizar soluções de parceiros com o Centro de Segurança do Azure](security-center-partner-solutions.md) – Saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de Segurança do Azure](https://blogs.msdn.com/b/azuresecurity/) – Encontre mensagens do blogue acerca da segurança e conformidade do Azure.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
