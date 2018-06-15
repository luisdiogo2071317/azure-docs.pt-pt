---
title: Adicionar uma firewall de aplicação web no Centro de segurança do Azure | Microsoft Docs
description: Este documento mostra como implementar as recomendações do Centro de segurança do Azure **adicionar uma firewall de aplicação web** e **finalizar proteção aplicação**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2018
ms.author: terrylan
ms.openlocfilehash: e28a1f6b865dae3abe2cb9dfac2921c6a2034491
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34203527"
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Adicionar uma firewall de aplicação web no Centro de segurança do Azure
Centro de segurança do Azure poderá recomendar que adicionar uma firewall de aplicação web (WAF) de um parceiro da Microsoft para proteger as suas aplicações web. Este documento explica-lhe como um exemplo de como aplicar esta recomendação.

É apresentada uma recomendação WAF para qualquer destinado ao IP público (IP de nível de instância ou IP com balanceamento de carga) que tenha um grupo de segurança de rede associada com portas web de entrada aberta (80,443).

Centro de segurança recomenda que Aprovisiona uma WAF para ajudar a proteger contra ataques direcionada para as aplicações web em máquinas virtuais e no externo aplicação serviço ambientes (ASE) implementados em-o [Isolated](https://azure.microsoft.com/pricing/details/app-service/windows/) plano de serviço. O plano Isolado aloja as aplicações num ambiente privado e dedicado do Azure e é ideal para aplicações que exigem ligações seguras à rede no local ou para desempenho e dimensionamento adicionais. Para além da sua aplicação está a ser um ambiente isolado, a aplicação tem de ter um endereço IP externo, o Balanceador de carga. Para saber mais sobre ASE, consulte o [a documentação de ambiente de serviço de aplicação](../app-service/environment/intro.md).

> [!NOTE]
> Este documento apresenta o serviço ao utilizar um exemplo de implementação.  Este documento não é um guia passo a passo.
>
>

## <a name="implement-the-recommendation"></a>Implementar a recomendação
1. Em **recomendações**, selecione **proteger a aplicação web através de firewall de aplicações web**.
   ![Proteger web de aplicação][1]
2. Em **proteger as suas aplicações web através de firewall de aplicações web**, selecione uma aplicação web. **Adicionar uma Firewall de aplicação Web** abre.
   ![Adicionar uma firewall de aplicação Web][2]
3. Pode optar por utilizar uma firewall de aplicação web existente se estiver disponível ou pode criar um novo. Neste exemplo, não existem nenhum WAFs existentes disponível pelo que iremos criar uma WAF.
4. Para criar uma WAF, selecione uma solução da lista de parceiros integrados. Neste exemplo, selecione **Firewall de aplicações Web Barracuda**.
5. **Firewall de aplicações Web barracuda** abre-se de fornecer informações sobre a solução de parceiros. Selecione **Criar**.

   ![Painel de informações de firewall][3]

6. **Nova Firewall de aplicação Web** se abre, onde pode efetuar **configuração de VM** passos e fornecer **WAF informações**. Selecione **configuração de VM**.
7. Em **configuração de VM**, introduza as informações necessárias para rotação a máquina virtual que executa o WAF.
   ![Configuração de VM][4]
8. Volte ao **nova Firewall de aplicação Web** e selecione **WAF informações**. Em **WAF informações**, configurar WAF próprio. Passo 7 permite-lhe configurar a máquina virtual no qual o WAF é executado e passo 8 permite-lhe aprovisionar WAF próprio.

## <a name="finalize-application-protection"></a>Finalizar a proteção das aplicações
1. Volte ao **recomendações**. Uma nova entrada foi gerada depois de criado o WAF chamado **finalizar proteção aplicação**. Esta entrada permite-lhe saber que precisa para concluir o processo de preparar realmente WAF dentro da rede Virtual do Azure para que possam proteger a aplicação.

   ![Finalizar a proteção das aplicações][5]

2. Selecione **finalizar proteção aplicação**. É aberto um novo painel. Pode ver o que há uma aplicação web que tem de ter o respetivo tráfego reencaminhado.
3. Selecione a aplicação web. É aberto um painel que dá-lhe os passos para finalizar a configuração de firewall de aplicação web. Conclua os passos e, em seguida, selecione **restringir o tráfego**. Centro de segurança, em seguida, faz a cópia de segurança wiring por si.

   ![Restringir tráfego][6]

> [!NOTE]
> Pode proteger várias aplicações web no Centro de segurança ao adicionar estas aplicações para as implementações de WAF existentes.
>
>

Os registos do que WAF agora estão totalmente integrados. Centro de segurança pode começar a recolher e analisar os registos para que este pode superfície alertas de segurança importantes para si automaticamente.

## <a name="next-steps"></a>Passos Seguintes
Este documento mostrou como implementar a recomendação de centro de segurança "Adicionar uma aplicação web". Para obter mais informações sobre como configurar uma firewall de aplicação web, consulte o seguinte:

* [Configurar uma Firewall de aplicação Web (WAF) para o ambiente de serviço de aplicações](../app-service/environment/app-service-app-service-environment-web-application-firewall.md)

Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Definir políticas de segurança no Centro de Segurança do Azure](security-center-policies.md) – Saiba como configurar políticas de segurança para as suas subscrições e grupos de recursos do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de segurança do Azure](security-center-monitoring.md) – Saiba como monitorizar o estado de funcionamento dos seus recursos Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Gerir recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md) – Saiba como recomendações ajudam a proteger os seus recursos do Azure.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de segurança do Azure](http://blogs.msdn.com/b/azuresecurity/) – encontre mensagens do Blogue acerca da segurança do Azure e conformidade.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
