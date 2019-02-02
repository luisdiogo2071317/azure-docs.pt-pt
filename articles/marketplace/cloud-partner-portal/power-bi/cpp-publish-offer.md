---
title: Publicar oferta de aplicação do Power BI - Azure Marketplace | Documentos da Microsoft
description: Publica uma oferta de aplicação do Power BI no Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
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
ms.date: 01/31/2019
ms.author: pbutlerm
ms.openlocfilehash: 2b3783060cf5502076ce3bc98cf07f005366a9e2
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666717"
---
# <a name="publish-power-bi-app-offer"></a>Publicar oferta de aplicação do Power BI

A última etapa, depois de ter definido a oferta no portal e criado os ativos técnicos associados, é submeter a oferta para publicação.  Para iniciar este processo, clique nas **Publish** botão no menu vertical no **nova oferta** janela.  Para obter mais informações, consulte [ofertas de publicar o Azure Marketplace e AppSource](../manage-offers/cpp-publish-offer.md).


## <a name="publishing-steps"></a>Passos de publicação

O diagrama seguinte descreve os passos principais no processo de publicação para a "ativação online".

![Etapas de processo de publicação para a aplicação do Power BI](./media/publishing-process-steps.png)

A tabela seguinte descreve estes passos e fornece uma estimativa de tempo máximo de sua conclusão:

|   Etapa de publicação            |   Hora     |   Descrição                                                                  |
| --------------------         |------------| ----------------                                                               |
| Validar a pré-requisitos       | 15 min     | Oferecer informações e oferecer definições são validadas.                            |
| Certificação                | 1 a 7 dias   | A equipa de certificação do Power BI analisa a sua oferta. Podemos executar a aplicação do Power BI através de um teste de verificação manual ao instalar a aplicação através de URL de instalação fornecido. Principais validações forem executadas como parte do processo de certificação da aplicação; Veja a seguir.         |
| Empacotamento                    | \< 1 hora  | Ativos de técnicos da oferta são empacotados para uso do cliente.                        |
| Levar o registo de geração | \< 1 hora  | Levar sistemas são configurados e implementados.                                      |
| Aprovação do publicador            | \-         | Revisão do publicador final e a confirmação antes da oferta entra no ar. Também terá uma ligação para a sua oferta de pré-visualização. Assim que estiver satisfeito com o aspeto de pré-visualização, clique nas **Go Live** botão no **estado** separador. Esta ação envia um pedido para a equipa de integração para listar a sua aplicação no AppSource.    |
| Em direto                         | \< 3 horas | A oferta é agora listada publicamente ("em direto") no AppSource e os clientes poderão ver e implementar a sua aplicação nas suas subscrições do Power BI. Também receberá um email de confirmação. Em qualquer momento, pode clicar no **todas as ofertas** separador e veja o estado para todas as suas ofertas listado na coluna da direita. Pode clicar no **estado** separador para ver o estado de fluxo de publicação em detalhes para a sua oferta. |
|   |   |

Permitir que até oito dias para este processo ser concluído. Depois de seguir estes passos de publicação, a oferta de aplicação do Power BI será listada na [AppSource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20) secção de aplicações do Power BI.


### <a name="app-certification-process"></a>Processo de certificação da aplicação

A equipa de integração da Microsoft usa o seguinte processo para validar a sua submissão de oferta do Power BI:

1. Ligações de ajuda e documentos legais são revistas.
2. Informações de contacto de suporte é validada.
3. URL de instalador é utilizado para verificar a instalação adequada. 
4. Aplicação é procurada malware e outros conteúdos maliciosos. 
5. É efetuada a verificação de que o conteúdo apresentado corresponde a descrição da aplicação.
6. Operações relacionadas com a aplicação funcionam conforme esperado no Power BI: Abra relatórios e dashboards com dados de exemplo, ligar a origens de dados personalizado, atualização, etc.

A equipe de certificação fornece comentários se encontrarem estes problemas.  Para obter mais informações sobre os requisitos de aplicação do Power BI, consulte a [documentação de aplicação do Power BI](https://go.microsoft.com/fwlink/?linkid=2028636).


## <a name="next-steps"></a>Passos Seguintes

Recomendamos que monitorize regularmente a sua aplicação no [Marketplace do AppSource](https://appsource.microsoft.com).  Além disso, deve utilizar o [Insights vendedor](../../cloud-partner-portal-orig/si-getting-started.md) recurso do [Cloud Partner Portal](https://cloudpartner.azure.com/#insights) para fornecer informações sobre os clientes do marketplace e o uso.  Também pode executar certas [atualizações para a sua oferta](./cpp-update-existing-offer.md).
