---
title: "Remediar configurações de segurança no Centro de segurança do Azure | Microsoft Docs"
description: "Este documento mostra como implementar a recomendação de centro de segurança do Azure, os \"Corrigir configurações de segurança\"."
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
ms.openlocfilehash: 477973298d8cc9d99da78e36274933e0bb737c4f
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="remediate-security-configurations-in-azure-security-center"></a>Remediar configurações de segurança no Centro de segurança do Azure
Centro de segurança do Azure diariamente analisa o sistema operativo (SO) das suas máquinas virtuais (VMs) e para uma configuração que possam tornar as VMs e os computadores mais vulnerável a ataques. Centro de segurança recomenda que resolver vulnerabilidades quando a configuração do SO não corresponde às regras de configuração de segurança recomendados e recomenda alterações de configuração para resolver estas vulnerabilidades.

Para obter mais informações sobre as configurações específicas que estão a ser monitorizadas, consulte o [lista de regras de configuração recomendada](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Para saber como personalizar as avaliações de configuração de segurança, consulte [configurações de segurança de SO Personalizar no Centro de segurança do Azure (pré-visualização)](security-center-customize-os-security-config.md).

## <a name="implement-the-recommendation"></a>Implementar a recomendação
"Remediar configurações de segurança" é apresentada como uma recomendação no Centro de segurança. A recomendação é apresentada em **recomendações** > **computação**.

Este exemplo aborda a recomendação "Remediar configurações de segurança" em **computação**.
1. No Centro de segurança, no painel esquerdo, selecione **computação**.  
  O **computação** é aberta a janela.

   ![Remediar configurações de segurança][1]

2. Selecione **remediar configurações de segurança**.  
  O **configurações de segurança** é aberta a janela.

   ![A janela "Configurações de segurança"][2]

  A secção superior do dashboard apresenta:

  - **Falha de regras por gravidade**: O número total de regras que a configuração do SO falhou entre as VMs e computadores, descriminados por gravidade.
  - **As regras de tipo de falha**: O número total de regras que a configuração do SO falhou entre as VMs e computadores, descriminados por tipo.
  - **Falha de regras do Windows**: O número total de regras falhou devido as configurações de sistema operativo Windows.
  - **Falha de regras de Linux**: O número total de regras falhou devido as configurações de SO Linux.

  A secção inferior do dashboard apresenta uma lista de todas as regras com falhas para as VMs e computadores e a gravidade da atualização em falta. A lista contém os seguintes elementos:

  - **CCEID**: Identificador exclusivo de CCE o para a regra. Centro de segurança utiliza comuns configuração enumeração CCE () para atribuir os identificadores exclusivos para as regras de configuração.
  - **Nome**: O nome da regra falhou.
  - **Tipo de regra**: O *chave de registo*, *política de segurança*, ou *a política de auditoria* tipo de regra.
  - **Não. VMs & computadores**: O número total de VMs e os computadores que se aplica a regra de falha.
  - **Regra de gravidade**: valor de CCE o *crítico*, *importante*, ou *aviso*.
  - **Estado**: O estado atual da Recomendação:

    - **Aberto**: a recomendação ainda não foi tratada.
    - **Em curso**: A recomendação está atualmente a ser aplicada aos recursos e é necessária nenhuma ação por si.
    - **Resolvido**: foi aplicada a recomendação. Quando o problema foi resolvido, a entrada está desativada.

3. Para ver os detalhes de uma falha na regra, selecione-o na lista.

   ![Vista detalhada de uma regra de configuração falhados][3]

   A vista detalhada apresenta as seguintes informações:

   - **Nome**: O nome da regra.
   - **CCIED**: Identificador exclusivo de CCE o para a regra.
   - **Versão do SO**: versão de SO a da VM ou computador.
   - **Regra de gravidade**: valor de CCE o *crítico*, *importante*, ou *aviso*.
   - **Total de descrição**: A descrição da regra.
   - **Vulnerabilidade**: A explicação de vulnerabilidade ou arrisca-se a regra não é aplicada.
   - **Impacto potencial**: O impacto comercial quando a regra é aplicada.
   - **Contramedida**: os passos de remediação.
   - **Valor esperado**: O valor que é esperado ao centro de segurança analisa a configuração do SO da VM contra a regra.
   - **Valor real**: O valor que é devolvido após uma análise da sua configuração de SO de VM contra a regra.
   - **Regra de operação**: A operação de regra é utilizada pelo centro de segurança durante a análise da sua configuração de SO de VM contra a regra.

4. Na parte superior da janela Vista detalhada, selecione **pesquisa**.  
  Pesquisa abre-se uma lista de áreas de trabalho que tenham VMs e os computadores com o erro de correspondência de configurações de segurança selecionados. A seleção de área de trabalho é apresentada apenas se a regra selecionada aplica-se a várias VMs que estejam ligadas a diferentes áreas de trabalho.

   ![Áreas de trabalho listadas][4]

5. Selecione uma área de trabalho.  
  Uma consulta de pesquisa de análise de registos abre filtrado à área de trabalho com o erro de correspondência de configurações de segurança.

   ![Área de trabalho com vulnerabilidade do SO][5]

6. Selecione um computador na lista.  
  Um resultado de pesquisa novo abre-se com informações filtradas apenas para esse computador.

   ![Informações detalhadas sobre o computador selecionado][6]

## <a name="next-steps"></a>Passos Seguintes
Este artigo mostrou como implementar a recomendação de centro de segurança "Remedeie configurações de segurança". Para saber como personalizar as avaliações de configuração de segurança, consulte [configurações de segurança de SO Personalizar no Centro de segurança do Azure (pré-visualização)](security-center-customize-os-security-config.md).

Para rever as configurações específicas que estão a ser monitorizadas, consulte [lista de regras de configuração recomendada](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Centro de segurança utiliza comuns configuração enumeração CCE () para atribuir os identificadores exclusivos para as regras de configuração. Para obter mais informações, visite o [CCE](https://nvd.nist.gov/cce/index.cfm) site.

Para saber mais acerca do Centro de segurança, consulte os seguintes recursos:

* Para obter uma lista de VMs com Linux e Windows suportados, consulte [plataformas suportadas no Centro de segurança do Azure](security-center-os-coverage.md). 
* Para saber como configurar políticas de segurança para as suas subscrições do Azure e os grupos de recursos, consulte o artigo [definir políticas de segurança no Centro de segurança do Azure](security-center-policies.md). 
* Para saber como recomendações ajudam a proteger os seus recursos do Azure, consulte [gerir recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md). 
* Para saber como monitorizar o estado de funcionamento dos seus recursos do Azure, consulte [monitorização de estado de funcionamento de segurança no Centro de segurança do Azure](security-center-monitoring.md). 
* Para saber como gerir e responder a alertas de segurança, consulte [gerir e responder a alertas de segurança no Centro de segurança do Azure](security-center-managing-and-responding-alerts.md).
* Para saber como monitorizar o estado de funcionamento das suas soluções de parceiros, consulte [monitorizar soluções de parceiros com o Centro de segurança do Azure](security-center-partner-solutions.md).
* Para obter respostas a perguntas mais frequentes sobre como utilizar o serviço de mensagens em fila, consulte [FAQ do Centro de segurança do Azure](security-center-faq.md).
* Para obter mensagens do Blogue acerca da segurança do Azure e conformidade, consulte [blogue de segurança do Azure](http://blogs.msdn.com/b/azuresecurity/).

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/compute-blade.png
[2]:./media/security-center-remediate-os-vulnerabilities/os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
[4]: ./media/security-center-remediate-os-vulnerabilities/search.png
[5]: ./media/security-center-remediate-os-vulnerabilities/log-search.png
[6]: ./media/security-center-remediate-os-vulnerabilities/search-results.png
