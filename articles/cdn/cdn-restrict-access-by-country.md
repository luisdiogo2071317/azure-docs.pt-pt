---
title: Restringir o conteúdo da CDN do Azure por país | Microsoft Docs
description: Saiba como restringir o acesso por país ao seu conteúdo da CDN do Azure utilizando a funcionalidade de filtragem de georreplicação.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: v-deasim
ms.openlocfilehash: 661356aeb2369bc1bbddd6caee57b256dd9e1212
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285020"
---
# <a name="restrict-azure-cdn-content-by-country"></a>Restringir o conteúdo da CDN do Azure por país

## <a name="overview"></a>Descrição geral
Quando um utilizador solicita o conteúdo, por predefinição, o conteúdo for servido independentemente da localização do utilizador que efetua o pedido. No entanto, em alguns casos, poderá querer restringir o acesso ao conteúdo por país. Com o *filtragem georreplicação* funcionalidade, pode criar regras sobre os caminhos específicos no ponto final de CDN para permitir ou bloquear o conteúdo selecionado países.

> [!IMPORTANT]
> **Azure CDN padrão da Microsoft** perfis não suportam a com base no caminho georreplicação filtragem.
> 

## <a name="standard-profiles"></a>Perfis de padrão
Os procedimentos nesta secção destinam **CDN do Azure Standard da Akamai** e **CDN do Azure Standard da Verizon** apenas os perfis. 

Para **CDN do Azure Premium da Verizon** perfis, tem de utilizar o **gerir** portal para ativar a filtragem de georreplicação. Para obter mais informações, consulte [CDN do Azure Premium da Verizon perfis](#azure-cdn-premium-from-verizon-profiles).

### <a name="define-the-directory-path"></a>Definir o caminho do diretório
Para aceder a funcionalidade de filtragem de georreplicação, selecione o ponto final de CDN no portal, em seguida, selecione **filtragem Georreplicação** em definições no menu da esquerda. 

![Filtragem de Georreplicação padrão](./media/cdn-filtering/cdn-geo-filtering-standard.png)

Do **caminho** caixa, especifique o caminho relativo para a localização a que os utilizadores serão permitidos ou negados o acesso. 

Pode aplicar a filtragem de georreplicação para todos os seus ficheiros com um reencaminhamento de barra (/) ou selecionar pastas específicas ao especificar os caminhos de diretório (por exemplo, */pictures/*). Também pode aplicar a filtragem de georreplicação para um único ficheiro (por exemplo */pictures/city.png*). São permitidas várias regras; Depois de introduzir uma regra, é apresentada uma linha em branco para introduzir a seguinte regra.

Por exemplo, todos os filtros de caminho do diretório que se seguem são válidos:   
*/*                                 
*/Photos/*     
*/Photos/Strasbourg /*     
*/Photos/Strasbourg/City.png*

### <a name="define-the-type-of-action"></a>Definir o tipo de ação

Do **ação** lista, selecione **permitir** ou **bloco**: 

- **Permitir**: apenas os utilizadores de países/regiões especificados são permitidos acesso aos recursos de pedido do caminho de recursiva.

- **Bloco**: os utilizadores de países/regiões especificados for negados o acesso a recursos de pedido do caminho de recursiva. Se não existem outras opções de filtragem de país tiverem sido configuradas para essa localização, em seguida, todos os outros utilizadores terão permissão de acesso.

Por exemplo, uma georreplicação regra de filtragem para bloquear o caminho */fotografias/Strasbourg/* filtra os seguintes ficheiros:     
*http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg*
*http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg*

### <a name="define-the-countries"></a>Definir países/regiões
Do **indicativos de país** lista, selecione de que pretende bloquear ou permitir que o caminho de países. 

Depois de terminar de selecionar países/regiões, selecione **guardar** para ativar a nova regra de filtragem de georreplicação. 

![Regras de filtragem de Georreplicação](./media/cdn-filtering/cdn-geo-filtering-rules.png)

### <a name="clean-up-resources"></a>Limpar recursos
Para eliminar uma regra, selecione-o da lista no **filtragem Georreplicação** página, em seguida, escolha **eliminar**.

## <a name="azure-cdn-premium-from-verizon-profiles"></a>O Azure CDN Premium da Verizon perfis
Para **para a CDN do Azure Premium da Verizon** perfis, a interface de utilizador para criar uma regra de filtragem de georreplicação é diferente:

1. No menu superior no seu perfil de CDN do Azure, selecione **gerir**.

2. No portal da Verizon, selecione **HTTP grande**, em seguida, selecione **filtragem de país**.

    ![Filtragem de Georreplicação padrão](./media/cdn-filtering/cdn-geo-filtering-premium.png)

3. Selecione **Adicionar filtro de país**.

    O **um passo:** é apresentada a página.

4. Introduza o caminho do diretório, selecione **bloco** ou **adicionar**, em seguida, selecione **seguinte**.

    O **passo dois:** é apresentada a página. 

5. Selecione um ou mais países da lista, em seguida, selecione **concluir** para ativar a regra. 
    
    A nova regra é apresentada na tabela no **filtragem de país** página.

    ![Regras de filtragem de Georreplicação](./media/cdn-filtering/cdn-geo-filtering-premium-rules.png)

### <a name="clean-up-resources"></a>Limpar recursos
Na tabela de regras filtragem de país, selecione o ícone Eliminar junto a uma regra eliminá-la ou no ícone de edição para modificá-lo.

## <a name="considerations"></a>Considerações
* Alterações à configuração da filtragem de georreplicação não entram em vigor imediatamente:
   * Para os perfis **CDN do Azure Standard da Microsoft**, a propagação normalmente fica concluída em 10 minutos. 
   * Para os perfis **CDN do Azure Standard da Akamai**, a propagação normalmente fica concluída num minuto. 
   * Para **CDN do Azure Standard da Verizon** e **CDN do Azure Premium da Verizon** perfis, propagação normalmente conclui em 10 minutos. 
 
* Esta funcionalidade não suporta carateres universais (por exemplo, *).

* A configuração de filtragem de georreplicação associada com o caminho relativo é aplicado em modo recursivo para esse caminho.

* Apenas uma regra pode ser aplicada para o mesmo caminho relativo. Ou seja, não é possível criar vários filtros de país que apontam para o mesmo caminho relativo. No entanto, dado que os filtros de país recursiva, uma pasta pode ter vários filtros de país. Por outras palavras, pode ser atribuída uma subpasta da pasta configurada anteriormente um filtro de noutro país.

* A funcionalidade de filtragem de georreplicação utiliza indicativos de país para definir países/regiões do que um pedido é permitido ou bloqueado para um diretório protegido. Embora perfis Akamai e da Verizon suportarem a maior parte dos mesmo indicativos de país, existem algumas diferenças. Para obter mais informações, consulte [indicativos de país CDN do Azure](https://msdn.microsoft.com/library/mt761717.aspx). 

