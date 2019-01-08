---
title: Tornar a sua máquina Virtual em direto oferta no Azure Marketplace
description: Tornar a sua máquina Virtual em direto oferta no Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: ad22f1944d3fe9a088b66da4cf4df7136db497f7
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54075343"
---
<a name="make-your-virtual-machine-offer-live-on-azure-marketplace"></a>Tornar a sua máquina Virtual em direto oferta no Azure Marketplace
=========================================================

Depois de preencher todos os detalhes de oferta, está na altura de publicar a sua oferta e torná-lo em direto no Azure Marketplace. Existem alguns estágios que atravessa a oferta. Certifique-se do conteúdo de marketing e cumprir os requisitos de qualidade, para poder ser Azure Certified e aceda ao vivo no site da sua imagem de VM.

![Oferecer sequência em direto aceda 0](./media/cloud-partner-portal-offer-go-live-azure-marketplace/makeanofferlive.png)

Vamos percorrer esse processo mais detalhadamente para compreender melhor o que acontece durante cada passo. Nesse processo, precisará agir para garantir a que sua oferta continua a fazer o curso.

<a name="publishing-process"></a>Processo de publicação
------------------

No separador Editor para iniciar o processo de publicação, clique em "Publicar".

![Oferecer sequência em direto aceda 1 - publicar](./media/cloud-partner-portal-offer-go-live-azure-marketplace/publish.png)

No separador Estado, verá os passos de publicação e onde a sua oferta está no processo.

![Aceda ao vivo sequência 2 - estado da oferta](./media/cloud-partner-portal-offer-go-live-azure-marketplace/status.png)

Em qualquer momento no processo de publicação, também pode iniciar sessão e clique no separador de todas as ofertas para ver o estado mais recente para qualquer uma das suas ofertas. Pode clicar diretamente no status da sua oferta e consulte os detalhes sobre onde a sua oferta está no processo de publicação.

![Oferecer sequência em direto do Go 3 - oferece todos os Estado](./media/cloud-partner-portal-offer-go-live-azure-marketplace/alloffersstatus.png)

Vamos examinar cada um dos passos publicação, abordarei o que acontece em cada passo e quanto deve calcular cada passo irá demorar.

**Validar a pré-requisitos (\<1 dia)**

Quando clica em "Publicar", uma verificação automatizada terá lugar para se certificar de que já preencheu todos os campos obrigatórios na sua oferta. Se quaisquer campos não são preenchidos, será apresentado um aviso junto ao campo da e precisará para preenchê-lo com precisão, em seguida, clique em 'Publish' novamente.

Depois de concluir este passo corretamente, um pop-up irá solicitar um endereço de e-mail. Este é o e-mail para o qual receberá publicação notificações de estado para o restante do processo de publicação. Depois de submeter o seu endereço de e-mail, este passo é concluído.

![Oferta Go Live sequência 4 – publicar a sua oferta](./media/cloud-partner-portal-offer-go-live-azure-marketplace/publishyouroffer.png)

**Certificação (\<5 dias)**

Este passo é onde podemos executar vários testes para garantir que sua imagem VM cumpre os requisitos de certificação do Azure. Todas as diretrizes que precisará para se certificar de que passar os requisitos de certificação estão [aqui](../cloud-partner-portal/virtual-machine/cpp-prerequisites.md).

Uma vez que este passo pode demorar vários dias, pode iniciar sessão fora do Portal do Cloud Partner. Nós lhe enviaremos uma notificação por e-mail se existirem quaisquer erros que tem de cumprir. Se tudo o que passar com êxito, o processo será automaticamente avançar para o passo de aprovisionamento.

**Aprovisionamento (\<1 dia)**

Durante esta fase, iremos estiver a replicar as imagens para todos os centros de dados do Azure global e pode demorar até um dia para concluir.

**Empacotamento e registo de geração de levar (\<1 hora)**

Durante esta fase, podemos está a combinar o conteúdo técnico e marketing para o qual será a página de produto no Web site.

Além disso, se tiver configurado a funcionalidade de geração de levar, iremos validar que a integração de CRM está a funcionar, enviando um líder de teste para seu sistema de CRM. Verá um registo com dados falsos preencher no seu CRM ou tabelas do Azure após a conclusão deste passo. Toda a documentação para a geração de levar está localizada [aqui](./cloud-partner-portal-get-customer-leads.md).

**Oferta disponível em pré-visualização**

Receberá um e-mail de notificação que a oferta foi concluída com êxito os passos necessários para aceder à oferta em pré-visualização. Durante este passo, deve pré-visualizar a oferta e certificar-se de que tudo parece como deve ser e que a VM implementa corretamente no ambiente de teste.

*Apenas as subscrições de na lista de permissões podem fazer esta verificação.*

**Aprovação do publicador**

Depois de verificar tudo parece correto e que funciona corretamente na pré-visualização, está pronto para entrar em direto. Clique em **Go Live** sob a **estado** separador e começará a passos para tornar a sua oferta em direto na produção e no Web site. Normalmente, irá demorar várias horas desde o momento de clicar em Go Live e quando a oferta está publicada no Web site. Nós lhe enviaremos uma notificação por e-mail depois de oficialmente em direto no site da sua oferta.

![Oferecer sequência em direto do Go 5 - go em direto](./media/cloud-partner-portal-offer-go-live-azure-marketplace/golive.png)

**Em direto**

A oferta está agora em direto no Azure Marketplace e o portal do Azure e os clientes poderão ver e implementar a sua máquina virtual das subscrições do Azure. Em qualquer momento, pode clicar na guia todas as ofertas e ver o estado para todas as suas ofertas listadas na coluna da direita. Pode clicar no estado para ver o estado de fluxo de publicação em detalhes para a sua oferta.

<a name="error-handling"></a>Tratamento de erros
--------------

Durante o processo de publicação, pode ser encontrado um erro. Se for encontrado um erro, receberá um e-mail de notificação informando-o de que ocorreu um erro com instruções sobre os passos seguintes. Também pode ver erros em qualquer altura durante este processo ao clicar no separador de estado. Verá a partir do qual o processo que ocorreu o erro, juntamente com uma mensagem de erro que descreva o que precisa de ser resolvido.

![Aceda ao vivo sequência 6 - mensagem de erro da oferta](./media/cloud-partner-portal-offer-go-live-azure-marketplace/errormessage.png)

Se encontrar erros durante o processo de publicação, são necessárias para corrigir estes problemas, em seguida, clique em "Publicar" para reiniciar o processo. Tem de começar no início dos passos de publicação em Validar pré-requisitos quando apagarmos após qualquer correção de erro.

Se estiver a ter problemas de resolução de um erro, deve abrir um pedido de suporte para receber ajuda de nossos engenheiros de suporte.

![Oferecer sequência em direto do Go 7 – obter suporte](./media/cloud-partner-portal-offer-go-live-azure-marketplace/getsupport.png)

<a name="canceling-the-publishing-request"></a>A cancelar o pedido de publicação
--------------------------------

Pode iniciar o processo de publicação e têm uma necessidade para cancelar o pedido. Apenas pode cancelar um pedido de publicação depois do pedido de publicação atinge a etapa de aprovação do publicador. Para cancelar, clique em "Cancelar publicar". O estado de publicação será reposto para o passo 1, e para publicar novamente, deve clicar em publicar e siga os passos no estado.

![Aceda ao vivo sequência 8 - estado da oferta](./media/cloud-partner-portal-offer-go-live-azure-marketplace/status5.png)

