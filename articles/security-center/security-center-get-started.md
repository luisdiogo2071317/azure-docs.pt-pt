---
title: "Início Rápido do Centro de Segurança do Azure - Carregar uma subscrição do Azure para o Centro de Segurança Standard | Microsoft Docs"
description: "Este início rápido mostra como fazer a atualização para o escalão de preço Standard do Centro de Segurança para obter segurança adicional."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: terrylan
ms.openlocfilehash: 0004db332ec13e23ed49f2c19e3454a516ca6a40
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Início Rápido: Carregar uma subscrição do Azure para o Centro de Segurança Standard
O Centro de Segurança do Azure oferece gestão de segurança unificada e proteção contra ameaças entre cargas de trabalho na cloud híbrida. Enquanto o escalão Gratuito oferece segurança limitada apenas para os recursos do Azure, o escalão Standard expande estas capacidades para o local e para outras clouds. O Centro de Segurança Standard ajuda a localizar e corrigir vulnerabilidades de segurança, aplicar controlos de acesso e aplicação para bloquear atividades maliciosas, detetar ameaças com análise e inteligência, e a responder rapidamente quando sob ataque. Pode experimentar o Centro de Segurança Standard sem custos durante os primeiros 60 dias.

Neste artigo, vai fazer a atualização para o escalão Standard para obter segurança adicional e instalar o Microsoft Monitoring Agent nas suas máquinas virtuais para monitorizar ameaças e vulnerabilidades de segurança.

## <a name="prerequisites"></a>Pré-requisitos
Para começar a utilizar o Centro de Segurança, tem de possuir uma subscrição do Microsoft Azure. Se não tiver uma subscrição, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).

Para atualizar uma subscrição para o escalão Standard, tem de lhe ser atribuída a função Proprietário da Subscrição, Contribuidor da Subscrição ou Administrador de Segurança.

## <a name="enable-your-azure-subscription"></a>Ativar a subscrição do Azure

1. Inicie sessão no [Portal do Azure](https://azure.microsoft.com/features/azure-portal/).
2. No menu **Microsoft Azure**, selecione **Centro de Segurança**. **Centro de Segurança - Descrição Geral** é aberto.

 ![Descrição geral do Centro de Segurança][2]

**Centro de Segurança – Descrição Geral** oferece uma vista unificada da postura de segurança das suas cargas de trabalho da cloud híbrida, permitindo-lhe detetar e avaliar a segurança das cargas de trabalho e identificar e mitigar o risco. O Centro de Segurança ativa automaticamente qualquer uma das suas subscrições do Azure que não foram anteriormente carregadas por si ou por outro utilizador da subscrição para o escalão Gratuito.

Pode ver e filtrar a lista de subscrições, clicando no item de menu **Subscrições**. O Centro de Segurança irá agora começar a avaliar a segurança destas subscrições para identificar vulnerabilidades de segurança. Para personalizar os tipos de avaliações, pode modificar a política de segurança. As políticas de segurança definem a configuração pretendida para as suas cargas de trabalho e ajudam a garantir a conformidade com os requisitos da empresa ou regulamentares.

Poucos minutos depois de iniciar o Centro de Segurança pela primeira vez, poderá ver:

- **Recomendações** de formas de melhorar a segurança das suas subscrições do Azure. Clique no mosaico **Recomendações** para iniciar uma lista prioritária.
- Um inventário de recursos de **Computação**, **Redes**, **Armazenamento e dados** e **Aplicações**, que estão agora a ser avaliados pelo Centro de Segurança, juntamente com a postura de segurança de cada um deles.

Para tirar total partido do Centro de Segurança, tem de executar os passos abaixo para fazer a atualização para o escalão Standard e instalar o Microsoft Monitoring Agent.

## <a name="upgrade-to-the-standard-tier"></a>Atualizar para o escalão Standard
Para efeitos dos inícios rápidos e tutoriais do Centro de Segurança, tem de atualizar para o escalão Standard. Os primeiro 60 dias são gratuitos e pode regressar ao escalão Gratuito em qualquer altura.

1. No menu principal do Centro de Segurança, selecione **Inclusão de segurança avançada**.

2. Em **Inclusão de segurança avançada**, o Centro de Segurança apresenta uma lista de subscrições e áreas de trabalho elegíveis para inclusão. Selecione uma subscrição da lista.

  ![Selecionar uma subscrição][4]

3. **Política de segurança** fornece informações sobre os grupos de recursos contidos na subscrição. A área **Preços** também abre.
4. Em **Preços**, selecione **Standard** para atualizar do escalão Gratuito para Standard e clique em **Guardar**.

  ![Selecionar Standard][5]

Agora que atualizou para o escalão Standard, tem acesso a funcionalidades adicionais do Centro de Segurança, incluindo **controlos de aplicação adaptativos**, **acesso à VM just-in-time**, **alertas de segurança**, **informações sobre ameaças**, **playbooks de automatização** e muito mais. Tenha em atenção que os alertas de segurança só serão apresentado quando o Centro de Segurança detetar atividade maliciosa.

  ![Alertas de segurança][7]

## <a name="automate-data-collection"></a>Automatizar a recolha de dados
O Centro de Segurança recolhe dados de VMs do Azure e de computadores que não pertencem ao Azure para monitorizar ameaças e vulnerabilidades de segurança. Os dados são recolhidos com o Microsoft Monitoring Agent, que lê várias configurações relacionadas com segurança e registos de eventos a partir da máquina e copia os dados para a sua área de trabalho para análise. Por predefinição, o Centro de Segurança irá criar uma nova área de trabalho para si.

Quando o aprovisionamento automático está ativado, o Centro de Segurança instala o Microsoft Monitoring Agent em todas as VMs do Azure suportadas e nas VMs novas que forem criadas. O aprovisionamento automático é vivamente recomendado.

Para ativar o aprovisionamento automático do Microsoft Monitoring Agent:

1. No menu principal do Centro de Segurança, selecione **Política de Segurança**.
2. Selecione uma subscrição.
3. Em **Política de segurança**, selecione **Recolha de Dados**.
4. Em **Recolha de Dados**, selecione **Ativado** para ativar o aprovisionamento automático.
5. Selecione **Guardar**.

  ![Ativar o aprovisionamento automático][6]

Com estas novas informações sobre as VMs do Azure, o Centro de Segurança pode fornecer Recomendações adicionais relacionadas com o estado de atualização do sistema de estado, configurações de segurança do SO, proteção de ponto final, bem como gerar Alertas de segurança adicionais.

  ![Recomendações][8]

## <a name="clean-up-resources"></a>Limpar recursos
Outros inícios rápidos e tutoriais desta coleção têm por base este início rápido. Se pretender continuar a trabalhar com inícios rápidos e tutoriais posteriores, continue a executar o escalão Standard e mantenha o aprovisionamento automático ativado. Se não pretender continuar ou quiser voltar para o Escalão gratuito:

1. Regresse ao menu principal do Centro de Segurança e selecione **Política de Segurança**.
2. Selecione a subscrição ou a política para a qual pretende voltar como Gratuita. **Política de segurança** abre-se.
3. Em **COMPONENTES DA POLÍTICA**, selecione **Escalão de preço**.
4. Selecione **Gratuito** para alterar a subscrição, do Escalão standard para o Escalão gratuito.
5. Selecione **Guardar**.

Se pretender desativar aprovisionamento automático:

1. Regresse ao menu principal do Centro de Segurança e selecione **Política de segurança**.
2. Selecione a subscrição para a qual pretende desativar o aprovisionamento automático.
3. Em **Política de segurança – Recolha de Dados**, selecione **Desativar** em **Inclusão** para desativar o aprovisionamento automático.
4. Selecione **Guardar**.

>[!NOTE]
> Desativar o aprovisionamento automático não remove o Microsoft Monitoring Agent das VMs do Azure onde o agente tiver sido aprovisionado. Desativar o aprovisionamento automático limita a monitorização da segurança dos seus recursos.
>

## <a name="next-steps"></a>Passos seguintes
Neste início rápido, fez a atualização para o escalão Standard e aprovisionou o Microsoft Monitoring Agent para gestão de segurança unificada e proteção contra ameaças entre as cargas de trabalho da cloud híbrida. Para obter mais informações sobre como utilizar o Centro de Segurança, avance para o início rápido para inclusão de computadores Windows que estão no local e noutras clouds.

> [!div class="nextstepaction"]
> [Início Rápido: integrar computadores Windows no Centro de Segurança do Azure](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/onboarding.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png
