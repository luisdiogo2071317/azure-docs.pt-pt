---
title: "Centro de segurança do Azure guia de introdução - carregar a subscrição do Azure Security Center Standard | Microsoft Docs"
description: "Este guia de introdução mostra-lhe como atualizar para os escalão de preço Standard do Centro de segurança para segurança adicional."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2018
ms.author: terrylan
ms.openlocfilehash: ac4e3b36b08223f7e3b54850ed53a8185e85f0d2
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Início rápido: Carregar a subscrição do Azure Security Center Standard
Centro de segurança do Azure fornece proteção de gestão e ameaças de segurança unificada nas suas cargas de trabalho de nuvem híbrida. Enquanto o escalão gratuito oferece uma segurança limitada para apenas os recursos do Azure, o escalão Standard expande estas capacidades no local e de outras nuvens. Padrão de centro de segurança ajuda a localizar e corrigir vulnerabilidades de segurança, aplicar controlos de acesso e a aplicação para bloquear atividades maliciosas, detetar ameaças utilizar a análise e intelligence e responder rapidamente quando sob ataque. Pode tentar padrão de centro de segurança sem custos para os primeira 60 dias.

Neste artigo, atualizar para o escalão padrão para segurança adicional e instalar o Microsoft Monitoring Agent em máquinas virtuais para monitorizar vulnerabilidades de segurança e ameaças.

## <a name="prerequisites"></a>Pré-requisitos
Para começar a utilizar o Centro de Segurança, tem de possuir uma subscrição do Microsoft Azure. Se não tiver uma subscrição, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).

Para atualizar uma subscrição para o escalão Standard, tem de ser atribuída a função de proprietário da subscrição, contribuinte de subscrição ou administrador de segurança.

## <a name="enable-your-azure-subscription"></a>Ativar a sua subscrição do Azure

1. Inicie sessão no [Portal do Azure](https://azure.microsoft.com/features/azure-portal/).
2. No menu **Microsoft Azure**, selecione **Centro de Segurança**. **Centro de segurança - descrição geral** abre.

 ![Descrição geral do Centro de segurança][2]

**Centro de segurança – descrição geral** fornece uma vista unificada para a postura de segurança das híbridos na nuvem cargas de trabalho, permitindo-lhe para detetar e avaliar a segurança das cargas de trabalho e identificar e mitigar o risco. Centro de segurança ativa automaticamente qualquer uma das suas subscrições do Azure não foram anteriormente integrado pelo utilizador, ou outro utilizador de subscrição para o escalão gratuito.

Pode ver e filtrar a lista de subscrições, clicando no **subscrições** item de menu. Centro de segurança agora irá começar a avaliar a segurança destas subscrições para identificar vulnerabilidades de segurança. Para personalizar os tipos de avaliações, pode modificar a política de segurança. As políticas de segurança definem a configuração pretendida para as suas cargas de trabalho e ajudam a garantir a conformidade com os requisitos da empresa ou regulamentares.

Minutos de iniciar o Centro de segurança pela primeira vez, poderá ver:

- **Recomendações** para formas melhorar a segurança das suas subscrições do Azure. Ao clicar no **recomendações** mosaico inicia uma lista prioritária.
- Um inventário dos **computação**, **redes**, **armazenamento & dados**, e **aplicações** recursos que estão agora a ser avaliado em matéria por Centro de segurança juntamente com a postura de segurança de cada.

Para tirar partido do Centro de segurança, tem de concluir os passos abaixo para atualizar para o escalão Standard e instalar o Microsoft Monitoring Agent.

## <a name="upgrade-to-the-standard-tier"></a>Atualizar para o escalão Standard
Para efeitos de inícios rápidos do Centro de segurança e tutoriais tem de atualizar para o escalão padrão. Os primeiro 60 dias são gratuitos e pode regressar ao escalão gratuito qualquer altura.

1. No menu principal do Centro de segurança, selecione **integração de segurança avançada**.

2. Em **integração de segurança avançada**, o Centro de segurança apresenta uma lista de subscrições e áreas de trabalho elegíveis para a integração. Selecione uma subscrição a partir da lista.

  ![Selecione uma subscrição][4]

3. **Política de segurança** fornece informações sobre os grupos de recursos contidos na subscrição. **Preços** é também aberta.
4. Em **preços**, selecione **padrão** para atualizar do gratuito para Standard e clique em **guardar**.

  ![Selecione a opção padrão][5]

Agora que tiver atualizado para o escalão Standard, tem acesso a funcionalidades adicionais do Centro de segurança, incluindo **controlos de aplicação adaptável**, **apenas no acesso VM de tempo**, **segurança alertas**, **ameaça intelligence**, **playbooks automatização**e muito mais. Tenha em atenção que os alertas de segurança só serão apresentado quando o Centro de segurança Deteta atividades maliciosas.

  ![Alertas de segurança][7]

## <a name="automate-data-collection"></a>Automatizar a recolha de dados
Centro de segurança recolhe dados de VMs do Azure e computadores não do Azure para monitorizar vulnerabilidades de segurança e ameaças. Os dados são recolhidos através do Microsoft Monitoring Agent, que lê várias configurações relacionadas com segurança e registos de eventos a partir da máquina e copia os dados para a sua área de trabalho para análise. Por predefinição, o Centro de segurança criará uma nova área de trabalho por si.

Quando o aprovisionamento automático está ativado, o Centro de segurança instala o Microsoft Monitoring Agent no suportadas todas as VMs do Azure e de quaisquer novos que são criados. Aprovisionamento Automático é vivamente recomendado.

Para ativar o aprovisionamento automático do Microsoft Monitoring Agent:

1. No menu principal do Centro de segurança, selecione **política de segurança**.
2. Selecione uma subscrição.
3. Em **política de segurança**, selecione **recolha de dados**.
4. Em **recolha de dados**, selecione **no** para ativar o aprovisionamento automático.
5. Selecione **Guardar**.

  ![Ativar aprovisionamento automático][6]

Com esta nova aprofundadas sobre as suas VMs do Azure, o Centro de segurança podem fornecer recomendações adicionais relacionados com o sistema de estado, as configurações de segurança do SO, a proteção de ponto final de atualização, bem como geram alertas de segurança adicionais.

  ![Recomendações][8]

## <a name="clean-up-resources"></a>Limpar recursos
Outras inícios rápidos e tutoriais nesta coleção tirar partido deste guia de introdução. Se pretender continuar com a trabalhar com inícios rápidos subsequentes e tutoriais, continuar a executar o escalão Standard e manter o aprovisionamento automático ativada. Se não pretender continuar ou pretende voltar para o escalão gratuito:

1. Regressar ao menu principal do Centro de segurança e selecionar **política de segurança**.
2. Selecione a subscrição ou a política que pretende voltar a gratuito. **Política de segurança** abre.
3. Em **componentes da política**, selecione **escalão de preço**.
4. Selecione **livres** para alterar a subscrição do padrão camada para o escalão gratuito.
5. Selecione **Guardar**.

Se pretender desativar aprovisionamento automático:

1. Regressar ao menu principal do Centro de segurança e selecionar **política de segurança**.
2. Selecione a subscrição que pretende desativar aprovisionamento automático.
3. Em **política de segurança – recolha de dados**, selecione **desativar** em **integração** para desativar aprovisionamento automático.
4. Selecione **Guardar**.

>[!NOTE]
> Desativar aprovisionamento automático não remover o Microsoft Monitoring Agent VMs do Azure onde o agente tiver sido aprovisionado. Desativar automática aprovisionamento limites monitorização de segurança para os seus recursos.
>

## <a name="next-steps"></a>Passos Seguintes
Este guia de introdução atualizados para o escalão Standard e aprovisionamento do Microsoft Monitoring Agent para gestão unificada de segurança e proteção contra ameaças entre as cargas de trabalho de nuvem híbrida. Para obter mais informações sobre como utilizar o Centro de segurança, avance para o guia de introdução para computadores com Windows integração estão no local e de outras nuvens.

> [!div class="nextstepaction"]
> [Início rápido: Computadores com Windows carregar ao centro de segurança do Azure](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/onboarding.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
