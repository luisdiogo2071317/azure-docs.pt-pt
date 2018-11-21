---
title: Tornar a sua aplicação do Azure em direto oferta no Azure Marketplace | Documentos da Microsoft
description: .
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: pbutlerm
ms.openlocfilehash: 18a8e6ae8ab3bd4299c6a014f938e73a2a021492
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263597"
---
<a name="make-your-azure-application-offer-live-on-azure-marketplace"></a>Tornar a sua aplicação do Azure em direto oferta no Azure Marketplace 
===========================================================

Agora que\'ve preenchido todos os detalhes de oferta, ele\'tempo s para publicar a sua oferta e torná-lo em direto no Azure Marketplace. Existem alguns passos, certifique-se do conteúdo de marketing e sua aplicação a cumprir os requisitos de qualidade para ser Azure Certified, antes de avançar o Web site em direto.

![Publicar o fluxo](./media/cloud-partner-portal-publish-managed-app/publish_flow.png)

Permitir que\'s esse processo mais detalhadamente para compreender melhor o que acontece durante cada passo, e os itens de ação neste processo para si garantir que a sua oferta continua a fazer o curso.

<a name="publishing-process"></a>Processo de publicação 
------------------

Deverá clicar em \"Publish\" sob a **Editor** separador para iniciar o processo de publicação.

![Oferecer sequência em direto do Go 1 - publicar](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_publish.png)

Sob o **estado** guia, verá os passos de publicação e passo que a oferta é a parte.

![Oferecer sequência em direto do Go 2 - fluxo de trabalho](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_workflow.png)

Em qualquer momento no processo de publicação, também pode iniciar sessão e clique nas **oferece todos os** separador para ver o estado mais recente para qualquer uma das suas ofertas. Pode clicar diretamente no status da sua oferta e consulte os detalhes sobre onde a sua oferta está no processo de publicação.

> [!WARNING]
> Se a atribuição de utilização do cliente não estiver ativada, obterá a seguinte mensagem de erro durante o processo de publicação: "parceiros do Azure ao cliente utilização atribuição em falta num ou mais modelos do Azure Resource Manager. Para resolver, adicione um controlo de GUID para o modelo Azure Resource Manager em pacotes de maintemplate. JSON para o seguinte plano: servicenow. Para obter mais detalhes, consulte http://aka.ms/customerusageattribution. " 

Vamos percorrer cada um dos passos publicação e falar sobre o que acontece em cada passo e quanto deve calcular a que cada passo irá demorar.

### <a name="validate-prerequisites-1-day"></a>Validar a pré-requisitos (\<1 dia) 

Quando clica em \"Publish\", será realizada uma verificação automatizada para garantir que\'ve preenchido todos os campos obrigatórios na sua oferta. Se quaisquer campos não são preenchidos, será apresentado um aviso junto ao campo da e terá de preenchê-lo com precisão, em seguida, clique em \'publicar\' novamente.

Uma vez que\'ve concluído este passo corretamente, um pop-up será apresentada pedir um endereço de e-mail. Este é o e-mail para o qual receberá publicação notificações de estado para o restante do processo de publicação. Depois de submeter o seu endereço de e-mail, este passo é concluído.

![Oferecer sequência em direto do Go 1 – publicar 1](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_publish1.png)

### <a name="certification-5-days"></a>Certificação (\<5 dias) 

Este passo é onde podemos executar vários testes para garantir que o pacote de aplicações do Azure cumpre os requisitos para ser Azure Certified.

Uma vez que este passo pode demorar vários dias, pode iniciar sessão fora do Portal do Cloud Partner. Nós lhe enviaremos uma notificação por e-mail se existirem quaisquer erros. Se tudo o que passar com êxito, o processo será automaticamente avançar para o passo de aprovisionamento.

### <a name="packaging-and-lead-generation-registration-1-hour"></a>Empacotamento e levar o registo de geração (\<1 hora) 

Durante esta fase, podemos está a combinar o conteúdo técnico e marketing para o qual será a página de produto no Web site.

### <a name="offer-available-in-preview"></a>Oferta disponível em pré-visualização 

Receberá um e-mail de notificação que a oferta foi concluída com êxito os passos necessários para aceder à oferta em pré-visualização. Neste passo, deve pré-visualizar a oferta e certificar-se de que tudo parece como deveria ser. Verifique se a VM está corretamente implementada no ambiente de teste.

Apenas as subscrições de na lista de permissões podem fazer esta verificação.\*

### <a name="publisher-sign-out"></a>Fim de sessão de publicador 

Depois de verificar tudo parece correto e que funciona corretamente na pré-visualização, está pronto para entrar em direto. Clique em Go Live, no separador de estado e vamos começar os passos para tornar a sua oferta em direto na produção e no Web site. Normalmente, irá demorar várias horas desde o momento de clicar em Go Live e quando a oferta está publicada no Web site. Nós lhe enviaremos uma notificação por e-mail depois de oficialmente em direto no site da sua oferta.

![Oferecer sequência em direto aceda 6 - Go em direto](./media/cloud-partner-portal-publish-managed-app/newOffer_managedApp_goLive.png)

### <a name="microsoft-review"></a>Revisão da Microsoft 

Assim que estiver pronto para começar e clicado em \"Go Live\", um worksite será criado no Visual Studio online, seguido de uma revisão de código da Microsoft de que o conteúdo do pacote. A revisão de código abordará padrões/práticas recomendadas para utilizar durante a criação de modelos, dicas e truques para otimizar a criação de recursos. Se os comentários resulta no publicador fazer alterações aos ficheiros, o processo de publicação terão de ser iniciada novamente. A publicação atual será cancelada e terá de publicar novamente com os comentários de comentários resolvidos.

### <a name="live"></a>Em direto

A oferta está agora em direto no Azure Marketplace e o Portal do Azure. Os clientes poderão ver e implementar a sua aplicação gerida do azure nas suas subscrições do Azure. Pode clicar na guia todas as ofertas e ver o estado para todas as suas ofertas listadas na coluna da direita. Pode clicar no estado para ver o estado de fluxo de publicação em detalhes para a sua oferta.

### <a name="error-handling"></a>Processamento de erros 

Se ocorrer um erro, receberá um e-mail de notificação informando-o que ocorreu um erro com instruções sobre os passos seguintes. Também pode ver erros em qualquer altura durante este processo ao clicar no separador de estado. Verá a partir do qual o processo que ocorreu o erro, juntamente com uma mensagem de erro que descreva o que precisa de ser resolvido.

Se encontrar erros durante o processo de publicação, é necessário corrigir estes, em seguida, clique em \'publicar\' para reiniciar o processo. Tem de começar no início dos passos de publicação em Validar pré-requisitos quando voltar a publicar após qualquer correção de erro.

Se estiver a ter problemas de resolução de um erro, deve abrir um pedido de suporte para receber ajuda de nossos engenheiros de suporte.

### <a name="canceling-the-publishing-request"></a>A cancelar o pedido de publicação

Pode iniciar o processo de publicação e têm uma necessidade para cancelar o pedido. Apenas pode cancelar um pedido de publicação depois do pedido de publicação atinge a etapa de aprovação do publicador. Para cancelar, clique em \'Cancelar publicar\'. O estado de publicação será reposto para o passo 1, e para publicar novamente, deve clicar em publicar e siga os passos no estado.
