---
title: Melhorar o desempenho através da compressão de ficheiros na CDN do Azure | Microsoft Docs
description: Saiba como melhorar a velocidade de transferência de ficheiro e melhorar o desempenho de carregamento de páginas por a compressão os ficheiros na CDN do Azure.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2018
ms.author: mazha
ms.openlocfilehash: 41e40c7e740e06654e7660c208db52fc2617d4b5
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Melhorar o desempenho através da compressão de ficheiros na CDN do Azure
A compressão do ficheiro é um método simple e eficaz para melhorar a velocidade de transferência de ficheiro e melhorar o desempenho de carregamento da página ao reduzir o tamanho do ficheiro antes de ser enviada do servidor. Compressão de ficheiros pode reduzir os custos de largura de banda e fornecer uma experiência mais reativa para os seus utilizadores.

Existem duas formas de ativar a compressão do ficheiro:

- Ative a compressão no seu servidor de origem. Neste caso, a CDN passa ao longo de ficheiros comprimidos e fornece-los aos clientes que solicitam-los.
- Ative a compressão diretamente nos servidores POP do CDN ("compressão no momento"). Neste caso, a CDN comprime os ficheiros e serve-las para os utilizadores finais, mesmo não foram comprimidos pelo servidor de origem.

> [!IMPORTANT]
> As alterações de configuração da CDN podem demorar algum tempo para propagar através da rede: 
- Para **CDN do Azure Standard da Microsoft** perfis, propagação normalmente conclusão na dez minutos. 
- Para **CDN do Azure Standard da Akamai** perfis, propagação normalmente concluída num minuto. 
- Para **CDN do Azure Standard da Verizon** e **CDN do Azure Premium da Verizon** perfis, propagação normalmente for concluída dentro de 90 minutos. 
>
> Se estiver a configurar a compressão pela primeira vez para o ponto final CDN, considere a aguardar 1-2 horas antes de resolver para garantir que as definições de compressão terem sido propagados para os POPs.
> 
> 

## <a name="enabling-compression"></a>Ativar a compressão
As camadas standard e premium da CDN fornecem a mesma funcionalidade de compressão, mas a interface de utilizador diferente. Para obter mais informações sobre as diferenças entre camadas CDN standard e premium, consulte [descrição geral da CDN do Azure](cdn-overview.md).

### <a name="standard-cdn-profiles"></a>Perfis da CDN padrão 
> [!NOTE]
> Esta secção aplica-se a **CDN do Azure Standard da Microsoft**, **CDN do Azure Standard da Verizon**, e **CDN do Azure Standard da Akamai** perfis.
> 
> 

1. Na página de perfil de CDN, selecione o ponto final da CDN que pretende gerir.
   
    ![Pontos finais de perfil CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    Abre a página de ponto final da CDN.
2. Selecione **compressão**.

    ![Seleção de compressão de CDN](./media/cdn-file-compression/cdn-compress-select-std.png)
   
    Abre a página de compressão.
3. Selecione **no** para ativar a compressão.
   
    ![Opções de compressão do ficheiro de CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Utilize os tipos de MIME predefinido ou modifique a lista ao adicionar ou remover tipos de MIME.
   
   > [!TIP]
   > Embora seja possível, não se recomenda a aplicar a compressão para formatos comprimidos. Por exemplo, ZIP, MP3, MP4 ou JPG.
   > 
 
5. Depois de efetuar as alterações, selecione **guardar**.

### <a name="premium-cdn-profiles"></a>Perfis da CDN Premium
> [!NOTE]
> Esta secção aplica-se apenas ao **CDN do Azure Premium da Verizon** perfis.
> 

1. Na página de perfil de CDN, selecione **gerir**.
   
    ![CDN gerir seleção](./media/cdn-file-compression/cdn-manage-btn.png)
   
    É aberto o portal de gestão do CDN.
2. Coloque o cursor sobre o **HTTP grande** separador, em seguida, coloque o cursor sobre o **definições da Cache** flyout. Selecione **compressão**.

    ![Seleção de compressão de CDN](./media/cdn-file-compression/cdn-compress-select.png)
   
    São apresentadas as opções de compressão.
   
    ![Opções de compressão do ficheiro de CDN](./media/cdn-file-compression/cdn-compress-files.png)
3. Ativar a compressão selecionando **ativar a compressão**. Introduza os tipos de MIME que pretende comprimir como uma lista delimitada por vírgulas (sem espaços) a **tipos de ficheiro** caixa.
   
   > [!TIP]
   > Embora seja possível, não se recomenda a aplicar a compressão para formatos comprimidos. Por exemplo, ZIP, MP3, MP4 ou JPG.
   > 
    
4. Depois de efetuar as alterações, selecione **atualização**.

## <a name="compression-rules"></a>Regras de compressão

### <a name="azure-cdn-standard-from-microsoft-profiles"></a>Azure CDN padrão de perfis da Microsoft

Para **CDN do Azure Standard da Microsoft** perfis, todos os ficheiros são elegíveis para compressão. No entanto, tem de ser um ficheiro de um tipo MIME que foi [configurado para compressão](#enabling-compression).

Estes perfis suportam as seguintes codificações de compressão:
- gzip (GNU zip)
- brotli 
 
Se o pedido de suportar mais do que um tipo de compressão, os tipos de compressão têm precedência sobre brotli compressão.

Quando um pedido para um recurso Especifica os resultados de pedido e da compressão de gzip uma falha de acerto na cache, o CDN do Azure efetua gzip compressão do elemento diretamente no servidor POP. Posteriormente, o ficheiro comprimido é servido a partir da cache.

### <a name="azure-cdn-from-verizon-profiles"></a>CDN do Azure da Verizon perfis

Para **CDN do Azure Standard da Verizon** e **CDN do Azure Premium da Verizon** são comprimidos perfis, apenas ficheiros elegíveis. Para ser elegível para compressão, um ficheiro tem de:
- Ser maior do que 128 bytes
- Pode ser inferior a 1 MB
 
Estes perfis suportam as seguintes codificações de compressão:
- gzip (GNU zip)
- DEFLATE
- bzip2
- brotli 
 
Se o pedido de suportar mais do que um tipo de compressão, os tipos de compressão têm precedência sobre brotli compressão.

Quando um pedido para um recurso Especifica brotli compressão (cabeçalho HTTP é `Accept-Encoding: br`) e os resultados de pedido numa cache falha de acerto na, CDN do Azure efetua brotli compressão do elemento diretamente no servidor POP. Posteriormente, o ficheiro comprimido é servido a partir da cache.

### <a name="azure-cdn-standard-from-akamai-profiles"></a>Azure CDN Standard da Akamai perfis

Para **CDN do Azure Standard da Akamai** perfis, todos os ficheiros são elegíveis para compressão. No entanto, tem de ser um ficheiro de um tipo MIME que foi [configurado para compressão](#enabling-compression).

Estes perfis suportam gzip compressão codificação apenas. Quando um ponto final do perfil pede um ficheiro com codificação gzip, sempre que for solicitada da origem, independentemente do pedido do cliente. 

## <a name="compression-behavior-tables"></a>Tabelas de comportamento da compressão
As tabelas seguintes descrevem o comportamento de compressão da CDN do Azure para cada cenário:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>A compressão está desativada ou ficheiro não é elegível para compressão
| Formato de pedido de cliente (através do cabeçalho de codificação de aceitar) | Formato de ficheiro em cache | A resposta da CDN para o cliente | Notas&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
| --- | --- | --- | --- |
| Comprimidos |Comprimidos |Comprimidos | |
| Comprimidos |Não comprimida |Não comprimida | |
| Comprimidos |Não colocar em cache |Comprimidos ou descomprimidos |A resposta de origem determina se o CDN efetua uma compressão. |
| Não comprimida |Comprimidos |Não comprimida | |
| Não comprimida |Não comprimida |Não comprimida | |
| Não comprimida |Não colocar em cache |Não comprimida | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>A compressão está ativada e o ficheiro é elegível para compressão
| Formato de pedido de cliente (através do cabeçalho de codificação de aceitar) | Formato de ficheiro em cache | Resposta da CDN para o cliente | Notas |
| --- | --- | --- | --- |
| Comprimidos |Comprimidos |Comprimidos |CDN transcodes entre formatos suportados. |
| Comprimidos |Não comprimida |Comprimidos |CDN efetua uma compressão. |
| Comprimidos |Não colocar em cache |Comprimidos |CDN executa uma compressão se a origem devolve um ficheiro descomprimido. <br/>**CDN do Azure da Verizon** passa o um ficheiro no primeiro pedido e, em seguida, comprimir e coloca em cache o ficheiro para pedidos subsequentes. <br/>Ficheiros com o `Cache-Control: no-cache` cabeçalho nunca são comprimidos. |
| Não comprimida |Comprimidos |Não comprimida |CDN efetua uma falha de descompressão. |
| Não comprimida |Não comprimida |Não comprimida | |
| Não comprimida |Não colocar em cache |Não comprimida | |

## <a name="media-services-cdn-compression"></a>Serviços de multimédia compressão de CDN
Para pontos finais ativados para a CDN de serviços de multimédia de transmissão em fluxo, a compressão está ativada por predefinição para os seguintes tipos MIME: 
- application/vnd.ms-sstr+xml 
- aplicação/dash + xml
- application/vnd.apple.mpegurl
- aplicação/f4m + xml 

## <a name="see-also"></a>Consulte também
* [Troubleshooting CDN file compression](cdn-troubleshoot-compression.md) (Resolver problemas de compressão de ficheiros da CDN)    

