---
title: "Remediar configurações de segurança no Centro de segurança do Azure | Microsoft Docs"
description: "Este documento mostra como implementar a recomendação de centro de segurança do Azure **remediar configurações de segurança**."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: terrylan
ms.openlocfilehash: 412234b1486fa15cbc399bcf43be8ce90aac252a
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="remediate-security-configurations-in-azure-security-center"></a>Remediar configurações de segurança no Centro de segurança do Azure
Centro de segurança do Azure diariamente analisa o sistema operativo (SO) das suas máquinas virtuais (VMs) e para uma configuração que possam tornar as VMs e os computadores mais vulnerável a ataques. Centro de segurança recomenda que resolver vulnerabilidades quando a configuração do SO não coincide com as regras de configuração de segurança recomendados e recomenda alterações na configuração para resolver estas vulnerabilidades.

Para obter mais informações sobre as configurações específicas a serem monitorizados, consulte o [lista de regras de configuração recomendada](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Consulte [personalizar as configurações do SO segurança](security-center-customize-os-security-config.md) para saber como personalizar as avaliações de configuração de segurança no Centro de segurança.

## <a name="implement-the-recommendation"></a>Implementar a recomendação
Remediar a segurança, erro de correspondência de configurações é apresentado como uma recomendação no Centro de segurança. Esta recomendação será apresentada em **recomendações** e, em **computação**.

Neste exemplo, iremos abordar o **remediar configurações de segurança** recomendação em **computação**.
1. Selecione **computação** sob o menu principal do Centro de segurança.

   ![Remediar configurações de segurança][1]

2. Em **computação**, selecione **remediar configurações de segurança**. **Configurações de segurança** abre.

   ![Configurações de segurança][2]

  Fornece a parte superior do dashboard:

  - O número total de regras por gravidade que a configuração do SO falhou da VM e do computador.
  - O número total de regras por tipo de que a configuração do SO falhou da VM e do computador.
  - O número total de regras falhou pelas configurações do sistema operativo Windows e as configurações de SO Linux.

  Na parte inferior do dashboard apresenta uma lista de todas as regras com falhas nas suas VMs e computadores e a gravidade da atualização em falta. A lista inclui:

  - **CCEID**: Identificador exclusivo de CCE para a regra. Centro de segurança utiliza comuns configuração enumeração CCE () para atribuir os identificadores exclusivos para as regras de configuração.
  - **NOME**: nome da regra falha
  - **TIPO de regra**: a chave de registo, a política de segurança ou a política de auditoria
  - **N.º VMs &AMP; computadores**: número Total de VMs e os computadores que a ativação ruled aplica-se a
  - **GRAVIDADE de regra**: valor de gravidade CCE de crítico, importante ou de aviso
  - **ESTADO**: o estado atual da recomendação:

    - **Aberto**: a recomendação ainda não foi tratada
    - **Em curso**: A recomendação está atualmente a ser aplicada a esses recursos, e não é necessária nenhuma ação
    - **Resolvido**: a recomendação já foi concluída. (Quando o problema foi resolvido, a entrada é desativada)

3. Selecione uma regra com falhas na lista para ver os detalhes.

   ![Regras de configuração falharam][3]

  As informações seguintes são fornecidas neste painel:

  - NOME – Nome da regra
  - CCIED - Identificador exclusivo de CCE para a regra
  - Versão do SO – versão do SO da VM ou do computador
  - GRAVIDADE de regra – CCE gravidade valor crítico, importante ou de aviso
  - DESCRIÇÃO completa – Descrição da regra
  - VULNERABILIDADE – Explicação de vulnerabilidade ou se não for aplicada a regra de risco
  - IMPACTO potencial – Impacto comercial quando a regra é aplicada
  - CONTRAMEDIDA – passos de remediação
  - VALOR ESPERADO – Valor esperado ao centro de segurança analisa a configuração do SO da VM contra a regra
  - VALOR real – O valor devolvido após a análise da sua configuração de SO de VM contra a regra
  - – REGRA regra operação utilizada pelo centro de segurança durante a análise da sua configuração de SO de VM contra a regra

4. Selecione o **pesquisa** ícone na fita superior. Pesquisa abre listagem de áreas de trabalho que tenham VMs e os computadores com o erro de correspondência de configurações de segurança selecionados. Este painel de seleção de área de trabalho só é apresentada se a regra selecionada aplica-se a várias VMs que estejam ligadas a diferentes áreas de trabalho.

  ![Áreas de trabalho listadas][4]

5. Selecione uma área de trabalho. Uma consulta de pesquisa de análise de registos abre filtrado à área de trabalho com o erro de correspondência de configurações de segurança.

  ![Área de trabalho com vulnerabilidade do SO][5]

6. Selecione um computador a partir da lista para obter mais informações. Resultado da pesquisa outro abre-se com informações filtradas apenas para esse computador.

  ![Filtrada para esse computador][6]

## <a name="next-steps"></a>Passos Seguintes
Este artigo mostrou como implementar a recomendação de centro de segurança "Remedeie configurações de segurança". Consulte [personalizar as configurações do SO segurança](security-center-customize-os-security-config.md) para saber como personalizar as avaliações de configuração de segurança no Centro de segurança.

Pode rever o conjunto de regras de configuração [aqui](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Centro de segurança utiliza CCE (Common Configuration enumeração) para atribuir os identificadores exclusivos para as regras de configuração. Visite o [CCE](https://nvd.nist.gov/cce/index.cfm) site para obter mais informações.

Para saber mais acerca do Centro de segurança, consulte os seguintes recursos:

* [Plataformas suportadas no Centro de segurança do Azure](security-center-os-coverage.md) -fornece uma lista de VMs com Linux e Windows suportados.
* [Definir políticas de segurança no Centro de segurança do Azure](security-center-policies.md) -Saiba como configurar políticas de segurança para as subscrições do Azure e os grupos de recursos.
* [Gerir recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md) -Saiba como recomendações ajudam a proteger os seus recursos do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de segurança do Azure](security-center-monitoring.md) -Saiba como monitorizar o estado de funcionamento dos seus recursos Azure.
* [Gerir e responder a alertas de segurança no Centro de segurança do Azure](security-center-managing-and-responding-alerts.md) -aprender a gerir e responder a alertas de segurança.
* [Monitorizar soluções de parceiros com o Centro de segurança do Azure](security-center-partner-solutions.md) -Saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [FAQ do Centro de segurança do Azure](security-center-faq.md) -encontre as perguntas mais frequentes sobre como utilizar o serviço.
* [Blogue de segurança do Azure](http://blogs.msdn.com/b/azuresecurity/) -encontre mensagens do Blogue acerca da segurança do Azure e conformidade.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
