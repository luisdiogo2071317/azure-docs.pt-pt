---
title: Atualizar uma oferta existente para o Azure Marketplace | Documentos da Microsoft
description: Explica como atualizar uma oferta existente do Azure Marketplace, com o Portal de parceiro de Cloud.
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
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: bfe2b0c70c8b912f6489ed461f5bcf6f233ed60d
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810364"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Atualizar uma oferta existente para o Azure Marketplace
==============================================

Existem vários tipos de atualizações que pode aplicar a sua oferta depois é estejam ativos.   O [Cloud Partner Portal](https://cloudpartner.azure.com/) possui vários recursos para ajudá-lo a editar uma oferta para garantir que as alterações estão corretas, incluindo:

-  Adicionar a nova versão de imagem de máquina virtual (VM) para um SKU existente
-  Alterar um SKU está disponível de regiões
-  Adicionar novos SKUs
-  A atualizar os metadados de mercado para os SKUs ou ofertas 
-  A atualizar preços pay as you go oferece
-  Comparando recursos
-  Histórico de recursos

Depois de modificar um SKU ou uma oferta, devem ser republicada antes das alterações, aceda ao vivo.
Este artigo explica os diferentes aspectos da atualizar a sua oferta VM.


<a name="unpermitted-changes-to-vm-offersku"></a>Unpermitted alterações à oferta/SKU de VM
-----------------------------------

Existem alguns atributos de uma oferta de VM ou SKU que não pode ser modificado depois da oferta está em direto no Azure Marketplace.

-  ID de oferta e o ID de publicador da oferta.
-  ID de SKU de SKUs existentes.
-  Contagem dos SKUs existentes do disco de dados.
-  Modelo de faturação/licença é alterado para SKUs existentes.


<a name="updating-the-vm-image-version-for-a-sku"></a>A atualizar a versão da imagem VM para uma SKU
---------------------------------------

A imagem de VM para uma SKU da sua oferta pode ter sido atualizada com funcionalidades adicionais, patches de segurança, etc. Em tais cenários, pode querer atualizar a imagem VM que referencia o SKU. Utilize os seguintes passos para atualizar uma imagem de VM. 

1.  Inicie sessão para o [Portal de parceiros da Cloud](https://cloudpartner.azure.com/).
2.  Sob **todas as ofertas**, localize a oferta que pretende atualizar.
3.  Sob o **SKUs** formulário, clique no SKU de imagem, cujo VM pretende atualizar.
4.  Sob **versão de disco**, clique em **+ nova versão de disco** para adicionar uma nova imagem VM.
5.  Fornecer as novas imagens de VM **versão de disco**. A versão de disco tem de cumprir os [versão semântica](http://semver.org/) formato. Versões devem ter o formato x.y. z, em que X, Y e Z são números inteiros. Certifique-se de que a nova versão que fornece é maior do que as versões anteriores, caso contrário o novo número de versão não irão aparecer no portal do Azure ou do Azure marketplace após voltar a publicar.
6.  Para **URL de VHD do SO**, introduza a assinatura de acesso partilhado (SAS) URI criada para o sistema de operativo VHD. Tenha em atenção que o número de discos de dados não é possível alterar entre diferentes versões do SKU. Se as versões anteriores tinham discos de dados configurados, esta nova versão também tem de ter configurados os discos de dados.
7.  Clique em **publicar** para iniciar o fluxo de trabalho de publicação para obter a nova versão VM para entrar em direto para o Azure marketplace e o portal do Azure.


<a name="changing-regions-a-sku-is-available-in"></a>Regiões de alterar um SKU está disponível em
--------------------------------------

Ao longo do tempo, talvez queira tornar a sua oferta/SKU disponível em mais regiões.
Em alternativa, pode querer parar o/SKU de oferta de suporte numa determinada região.
Para implementar essas alterações, siga os passos abaixo.

1.  Inicie sessão para o [Portal de parceiros da Cloud](https://cloudpartner.azure.com/).
2.  Sob **todas as ofertas** encontrar a oferta que pretende atualizar.
3.  Sob o **SKUs** de formulário, clique no SKU cuja disponibilidade de região que pretende atualizar.
4.  Clique no **selecionar países** botão sob os **disponibilidade de país/região** campo.
5.  A disponibilidade de região pop-up, adicionar/remover regiões do que pretende para este SKU.
6.  Clique em **publicar** para iniciar o fluxo de trabalho de publicar para atualizar a disponibilidade de região de SKUs.

Se um SKU está a ser disponibilizado numa nova região, terá a capacidade de especificar os preços para essa região específica através da **exportar dados de preços** funcionalidade. Se estiver a adicionar novamente uma região que era uma vez disponível antes, não será possível atualizar o seu preço, uma vez que não são permitidas alterações aos preços.


<a name="adding-a-new-sku"></a>Adicionar um novo SKU
----------------

Para disponibilizar um SKU de novo para a sua oferta existente, siga os passos abaixo.

1.  Inicie sessão para o [Portal de parceiros da Cloud](https://cloudpartner.azure.com/).
2.  Sob **todas as ofertas** encontrar a oferta que pretende atualizar.
3.  Sob o **SKUs** formulário, clik na **adicionar novo SKU** e fornecer uma **ID de SKU** no pop-up.
4.  Siga o resto dos passos detalhados [publique uma máquina virtual no Azure Marketplace](./cloud-partner-portal-publish-virtual-machine.md).
5.  Clique em **publicar** para iniciar o fluxo de trabalho de publicar para que o seu novo SKU ativadas.


<a name="updating-offer-marketplace-metadata"></a>A atualizar os metadados do marketplace de oferta.
------------------------------------

Pode haver situações em que precisa atualizar os metadados de marketplace associados a sua oferta, como atualizar a logótipos da empresa, entre outras. Utilize os seguintes passos para atualizar os metadados da sua oferta.

1.  Inicie sessão para o [Portal de parceiros da Cloud](https://cloudpartner.azure.com/).
2.  Sob **todas as ofertas** encontrar a oferta que pretende atualizar.
3.  Goto a **Marketplace** formam e siga as instruções para efetuar quaisquer alterações.
4.  Clique em **publicar** para iniciar o fluxo de trabalho de publicar para que as suas alterações, aceda ao vivo.


<a name="updating-pricing-on-published-offers"></a>A atualização dos preços das ofertas publicadas
------------------------------------

Assim que a oferta pay as you go é publicada, não é possível aumentar o preço de um SKU existente, mas pode criar um novo SKU sob a mesma oferta, elimine o SKU antigo e, em seguida, voltar a publicar a sua oferta. Também permitimos que diminuir o preço nas ofertas de já publicados. Para diminuir o preço da oferta:

1.  Clique no SKU para o qual deseja diminuir a preços.
2.  Se tiver definido os preços a 1 x 1 GUI, pode alterar o preço diretamente na interface do Usuário. Se definir preços por meio da folha de cálculo de importação/exportação, só pode reduzir os preços por meio da funcionalidade de importação/exportação.
3.  Clique em **Guardar**.
4.  Voltar a publicar a oferta e aceda ao vivo.

Os preços vão ser visíveis para os novos clientes depois de ser completamente publicada no Web site e todos os clientes novos pagará o novo preço reduzido.

Para clientes existentes, a diminuição do preço será refletida retroativamente para o início do ciclo de faturação durante o qual a diminuição do preço entrou em vigor.
Se já ter sido faturadas pelo ciclo durante o qual uma diminuição do preço ocorreu, receberá um reembolso durante o próximo ciclo de faturação para cobrir o preço reduzido.


<a name="simplified-currency-pricing"></a>Preços de moeda simplificada
---------------------------

A partir de Setembro de 2018, chamado de uma nova seção **preços de moeda simplificado** ficavam visíveis para si. Microsoft está simplificando o negócio do Azure Marketplace, permitindo que mais previsível em termos de preços e coleções a partir de seus clientes em todo o mundo. Este simplificando irá ser alcançado ao reduzir o número de moedas em que podemos criar uma fatura seus clientes.

A nova secção, irá demorar preços nestas novas moedas. Depois de todos os clientes terem sido migrados para nestas novas moedas de liquidação, vai ser descontinuada a secção de preços original e permanecerá apenas a secção "Simplificado preços moeda".

Terá até 1 de Novembro de 2018 para definir um novo preço para as regiões em que a moeda de liquidação está mudando. Não será capaz de aumentar o preço para regiões em que a moeda de liquidação não está a mudar. Seguem-se as regiões em que está mudando a moeda:

| País                  | Código de ISO2 | Moeda de faturação simplificada |
|--------------------------|-----------|-----------------------------|
| **Argélia**              | DZ        | USD                         |
| **Argentina**            | AR        | USD                         |
| **Barém**              | BH        | USD                         |
| **Bielorrússia**              | POR        | USD                         |
| **Brasil**               | BR        | BRL                         |
| **Bulgária**             | BG        | EUR                         |
| **Chile**                | CL        | USD                         |
| **Colômbia**             | CO        | USD                         |
| **Costa Rica**           | CR        | USD                         |
| **Croácia**              | RH        | EUR                         |
| **República Checa**       | CZ        | EUR                         |
| **Egipto**                | POR EXEMPLO        | USD                         |
| **Guatemala**            | GT        | USD                         |
| **RAE de Hong Kong**            | HK        | USD                         |
| **Hungria**              | HU        | EUR                         |
| **Islândia**              | É        | EUR                         |
| **Indonésia**            | ID        | USD                         |
| **Israel**               | IL        | USD                         |
| **Jordânia**               | JO        | USD                         |
| **Cazaquistão**           | KZ        | USD                         |
| **Quénia**                | KE        | USD                         |
| **Kuwait**               | KW        | USD                         |
| **Listenstaine**        | LI        | EUR                         |
| **Macedónia (FYRO)**     | MK        | USD                         |
| **Malásia**             | MEU        | USD                         |
| **México**               | MX        | USD                         |
| **Montenegro**           | -ME        | USD                         |
| **Marrocos**              | MA        | USD                         |
| **Nigéria**              | NG        | USD                         |
| **Omã**                 | OM        | USD                         |
| **Paquistão**             | PK        | USD                         |
| **Paraguai**             | PY        | USD                         |
| **Peru**                 | PE        | USD                         |
| **Filipinas**          | PH        | USD                         |
| **Polónia\***             | PL        | EUR                         |
| **Catar**                | CONTROLE DE QUALIDADE        | USD                         |
| **Roménia**              | RO        | EUR                         |
| **Arábia Saudita**         | SA        | USD                         |
| **Sérvia**               | RS        | USD                         |
| **Singapura**            | SG        | USD                         |
| **África do Sul**         | ZA        | USD                         |
| **Tailândia**             | TH        | USD                         |
| **Trindade e Tobago**  | TT        | USD                         |
| **Tunísia**              | TN        | USD                         |
| **Turquia**               | TR        | USD                         |
| **Ucrânia**              | UA        | USD                         |
| **Emirados Árabes Unidos** | AE        | USD                         |
| **Uruguai**              | UY        | USD                         |
|  |  |  |

> [!NOTE]
> Se usar APIs para publicar a sua oferta, pode ver uma nova secção dentro da oferta com o nome **JSON**. Isso poderia ser anotado como `virtualMachinePricingV2` ou `monthlyPricingV2`, dependendo do tipo de oferta.

Se tiver quaisquer perguntas sobre esta alteração, contacte o suporte do Azure Marketplace.


<a name="compare-feature"></a>Compare recursos
---------------

Quando fizer alterações numa oferta publicada já, pode utilizar o *comparar* funcionalidade para auditar as alterações que foram feitas. Para utilizar Compare:

1.  Em qualquer momento no processo de edição, pode clicar a **comparar** botão para a sua oferta.

2.  Ver versões lado a lado dos ativos de marketing e metadados.


<a name="history-of-publishing-actions"></a>Histórico de ações de publicação
-----------------------------

Para ver qualquer atividade de publicação histórica, clique nas **histórico** separador na navegação esquerda do Portal de parceiro de Cloud. Aqui, poderá ver timestamped ações que foram executadas durante o tempo de vida das suas ofertas do Azure Marketplace.
