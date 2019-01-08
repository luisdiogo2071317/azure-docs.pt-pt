---
title: Implementar a sua oferta no Azure Marketplace | Documentos da Microsoft
description: Saiba mais sobre e percorrer as instruções para implementar a sua oferta, a imagem de máquina virtual, o serviço de desenvolvedor, o serviço de dados, etc. – no Azure Marketplace.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 8f79b891-84e2-4f41-ba0d-66420e2c6b2e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: 058f50853795453617593a6a07e2951f15f28174
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54076116"
---
# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Implementar a sua oferta no Azure Marketplace
Quando estiver satisfeito com a oferta (ou seja, testou cenários de clientes, de marketing conteúdo, etc.) e está pronto para iniciar, solicitar **emitir para produção** sobre o **publicar** separador.  

1. Os quatro passos sob o passo a passo página na publicação portal deve ser concluído e verde. Para ofertas de Máquina Virtual, certifique-se de que as seguintes diretrizes foram aplicadas.
   
    ![desenho][img-pubportal-walkthru-checked]
2. Selecione o **publicar** separador da lista à esquerda.
   
    ![desenho][img-pubportal-menu-publish]
3. Clique no botão **solicitar aprovação para emitir para produção**. Assim que a solicitação é feita, a equipe de aprovação executa uma revisão final e, em seguida, a oferta estará disponível no Azure Marketplace.
   
    ![desenho][img-pubportal-publish-pushproduction]

> [!IMPORTANT]
> Em caso de máquinas virtuais, ao clicar no botão de aprovação de pedidos para emitir para produção, os seguintes passos são executados por trás da cena. Poderá ver o progresso de cada passo no separador Publicar na publicação portal. Tem de verificar esta página em intervalos regulares (até que o estado mostra "Apresentados") para qualquer informação de falha que precisam de correção da sua parte.
> 
> * Em primeiro lugar, o seu pedido de produção vai para a equipe de certificação que validam o vhd. No entanto, se estiver a atualizar a oferta já listada e o pedido tem apenas alterações de marketing, em seguida, o passo de certificação é ignorado.
> * O próximo passo, o pedido são fornecidos para a equipa de validação de conteúdos que verifique se o conteúdo de marketing da oferta.
> * Se os passos acima forem concluída com êxito, a oferta é aprovada na produção. Neste momento, o estado se tornar "listado" no portal de publicação. No entanto, este estado de "Apresentados" não implica que o processo foi concluído. Os passos seguintes têm de ser concluída antes da oferta está disponível no Azure Marketplace.
> * Assim que a oferta for aprovada em produção no passo acima, a oferta de início da replicação em todos os datacenters do Azure. Ele geralmente usa 24-48hours para a conclusão da replicação, mas poderá demorar até uma semana, dependendo do tamanho do vhd. No entanto, se estiver a atualizar a oferta já listada e ele tem apenas alterações de marketing, em seguida, a replicação é mais rápida.
> * Quando a replicação estiver concluída, em seguida, a oferta estará disponível no Azure Marketplace.
> 
> Pode sempre eliminar a oferta enquanto estiverem num **rascunho** Estado (ou seja, nunca **enviar para o teste** ou **emitir para produção**). Na **histórico** separador, clique no **descartar rascunho** na parte inferior da página para eliminar um rascunho.
> 
> 

## <a name="production-checklist-for-all-virtual-machine-offers"></a>Lista de verificação de produção para todas as ofertas de Máquina Virtual
* Certifique-se de que é um parceiro Microsoft Azure Certified
* Na guia SKUs, a opção "Ocultar este SKU do Marketplace porque sempre deve ser adquirido através de um modelo de solução" deve ser marcado como Sim, apenas se o SKU é uma parte de um modelo de solução. Em todos os outros casos, esta opção sempre deverá ser marcada como não.
* Lembre-se: Não deve alterar a definição de visibilidade SKU assim que o SKU está listado. Não suportamos essa funcionalidade.
* Certifique-se de que os logótipos seguir as diretrizes de logótipo do Azure Marketplace indicadas abaixo.
* Descrição da oferta e o SKU não deve ser o mesma.
* Do SKU título e oferecer longo resumo não devem ser o mesmo.
* Título de SKU e o resumo de oferta não devem ser o mesmo.
* Títulos de SKU não deve ser idênticos para uma oferta com várias SKUs.

**Diretrizes de logótipo do Azure Marketplace**

* O design do Azure tem uma paleta de cores simples. Mantenha o número de principal e secundário cores seu logótipo baixa.
* As cores de tema do portal do Azure são brancas e preto. Evite, por conseguinte, utilizar essas cores como a cor de fundo de suas logótipos. Utilize uma cor que tornaria os logótipos proeminente no portal do Azure. Recomendamos cores primárias simples. Se estiver a utilizar o fundo transparente, em seguida, certifique-se de que o texto/logótipo não é preto ou branco.
* Não utilize um plano de fundo de gradação o logótipo.
* Evite colocar texto, até mesmo a sua empresa, ou sua marca nome, o logótipo.
* O aspeto e funcionalidade do seu logótipo deve ser "simples" e deve evitar gradientes.
* O logótipo não deverá ser ampliado.

**Diretrizes adicionais para o logótipo de Hero:**

* O logótipo de Hero é opcional. O publicador pode optar por não carregue um logótipo de Hero. **No entanto uma vez carregado o ícone de hero não é possível eliminar a publicação do portal. Nessa altura, o parceiro tem de seguir as diretrizes do Azure Marketplace para os ícones de Hero outras que a oferta não será aprovada para produção.**
* O nome de exibição do publicador, título SKU e o resumo extenso da oferta são apresentados na cor da fonte branco. Por isso, deve evitar manter qualquer cor leve no plano de fundo do ícone Hero. Plano de fundo preto, branco e transparente não é permitido para os ícones de Hero.
* O publicador apresentar o nome, a título SKU, a oferta resumo há muito tempo e no botão Criar são inseridas por meio de programação no logótipo de Hero assim que a oferta vai listada. Portanto, não deve digitar qualquer texto durante a conceção o logótipo de Hero. Apenas deixe um espaço vazio à direita, uma vez que o texto (ou seja, nome a apresentar do publicador, título SKU, a oferta longa resumo) será incluído por meio de programação por nós por aí. O espaço vazio para o texto deve ser 415 x 100 à direita (e ele é contrabalançado por 370 px da esquerda).

## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Lista de verificação de produção adicionais para a máquina de Virtual já listadas oferece
* Verifique se já existe uma oferta com o mesmo nome de oferta da sua empresa. Se Sim, em seguida, deve adicionar uma nova versão do SKU da oferta existente em vez de criar uma nova oferta duplicada.
* Disco de dados não deve alterar entre duas versões do mesmo SKU.
* O Azure Marketplace não suporta a alteração de preços dos SKUS listados como tem impacto sobre a faturação dos clientes existentes. Certifique-se de que não altere os preços dos SKUs listados nas regiões onde o SKU está disponível. No entanto, pode adicionar novos SKUs ou adicionar novas regiões para um SKU existente.

## <a name="next-steps"></a>Passos Seguintes
Assim que a oferta for executada, teste os cenários de cliente para validar que todos os contratos e a funcionalidade funcionarem corretamente no ambiente de produção como testou e validou no ambiente de teste.

## <a name="see-also"></a>Consulte também
* [Introdução: Como publicar uma oferta no Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
