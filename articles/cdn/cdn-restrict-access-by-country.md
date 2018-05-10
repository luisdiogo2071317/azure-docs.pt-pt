---
title: Restringir o conteúdo da CDN do Azure por país | Microsoft Docs
description: Saiba como restringir o acesso ao seu conteúdo da CDN do Azure utilizando a funcionalidade de filtragem de georreplicação.
services: cdn
documentationcenter: ''
author: lichard
manager: akucer
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: bb757ab115d03ab04dac4468d23f446696a971a9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="restrict-azure-cdn-content-by-country"></a>Restringir o conteúdo da CDN do Azure por país

## <a name="overview"></a>Descrição geral
Quando um utilizador solicita o conteúdo, por predefinição, o conteúdo for servido independentemente de onde o utilizador efetuou este pedido de. Em alguns casos, poderá querer restringir o acesso ao conteúdo por país. Este artigo explica como utilizar o *filtragem georreplicação* funcionalidade para configurar o serviço para permitir ou bloquear o acesso por país.

> [!IMPORTANT]
> Os produtos da CDN do Azure fornecem todas a mesma funcionalidade filtragem de georreplicação mas tem uma diferença pequena nos indicativos de país te suportam. Consulte o passo 3 para uma ligação para as diferenças.


Para obter informações sobre as considerações que se aplicam ao configurar este tipo de restrição, consulte [considerações](cdn-restrict-access-by-country.md#considerations).  

![Filtragem de país](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## <a name="step-1-define-the-directory-path"></a>Passo 1: Definir o caminho do diretório
> [!IMPORTANT]
> **Azure CDN padrão da Microsoft** perfis não suportam a com base no caminho georreplicação filtragem.
>


Selecione o ponto final no portal e localizar o separador Georreplicação filtragem no painel de navegação esquerdo para encontrar esta funcionalidade.

Quando configurar um filtro de país, tem de especificar o caminho relativo para a localização a que os utilizadores serão permitidos ou negados o acesso. Pode aplicar a filtragem de georreplicação para todos os seus ficheiros com uma barra (/) ou pastas selecionadas especificando os caminhos de diretório */pictures/*. Também pode aplicar a filtragem de georreplicação para um único ficheiro especificando o ficheiro e que sai de saída a barra no final */pictures/city.png*.

Filtro de caminho de diretório de exemplo:

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## <a name="step-2-define-the-action-block-or-allow"></a>Passo 2: Definir a ação: bloquear ou permitir
**Bloco:** de países/regiões especificados será negado a utilizadores acesso a recursos de pedidos a partir de que o caminho de recursiva. Se não existem outras opções de filtragem de país tiverem sido configuradas para essa localização, em seguida, todos os outros utilizadores terão permissão de acesso.

**Permitir:** apenas os utilizadores de países/regiões especificados terão permissão de acesso a recursos de pedidos a partir de que o caminho de recursiva.

## <a name="step-3-define-the-countries"></a>Passo 3: Definir países/regiões
Selecione países/regiões que pretende bloquear ou permitir para o caminho. 

Por exemplo, a regra do bloqueio /Photos/Strasbourg/irá filtrar ficheiros, incluindo:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### <a name="country-codes"></a>Códigos de país
A funcionalidade de filtragem de georreplicação utiliza indicativos de país para definir países/regiões do que um pedido será permitido ou bloqueado para um diretório protegido. Embora os produtos da CDN do Azure todas as fornecem a mesma funcionalidade filtragem de georreplicação, há uma pequena diferença no indicativos de país suportam. Para informações, consulte [indicativos de país do Azure CDN](https://msdn.microsoft.com/library/mt761717.aspx). 

## <a name="considerations"></a>Considerações
* As alterações à sua configuração de filtragem de país não entram em vigor imediatamente:
   * Para **CDN do Azure Standard da Microsoft** perfis, propagação normalmente conclusão na dez minutos. 
   * Para **CDN do Azure Standard da Akamai** perfis, propagação normalmente concluída num minuto. 
   * Para **CDN do Azure Standard da Verizon** e **CDN do Azure Premium da Verizon** perfis, propagação normalmente for concluída dentro de 90 minutos.  
* Esta funcionalidade não suporta carateres universais (por exemplo, ' *').
* A configuração de filtragem de georreplicação associada com o caminho relativo será aplicado em modo recursivo para esse caminho.
* Apenas uma regra pode ser aplicada para o mesmo caminho relativo (não é possível criar vários filtros de país que apontam para o mesmo caminho relativo. No entanto, uma pasta pode ter vários filtros de país. Isto acontece devido à natureza recursiva de filtros de país. Por outras palavras, pode ser atribuída uma subpasta da pasta configurada anteriormente um filtro de noutro país.

