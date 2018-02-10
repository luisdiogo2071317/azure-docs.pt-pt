---
title: "Melhorar o desempenho através da compressão de ficheiros na CDN do Azure | Microsoft Docs"
description: "Saiba como melhorar a velocidade de transferência de ficheiro e aumenta o desempenho de carregamento de página por a compressão os ficheiros na CDN do Azure."
services: cdn
documentationcenter: 
author: dksimpson
manager: akucer
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: mazha
ms.openlocfilehash: 77d889f5d56ed839665588cf359b73e0f9ad28b5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Melhorar o desempenho através da compressão de ficheiros na CDN do Azure
A compressão é um método simple e eficaz para melhorar a velocidade de transferência de ficheiro e aumentar o desempenho de carregamento da página ao reduzir o tamanho do ficheiro antes de ser enviada do servidor. Esta reduz os custos de largura de banda e fornece uma experiência mais reativa para os seus utilizadores.

Existem duas formas de ativar a compressão:

* Ative a compressão no seu servidor de origem. Neste caso, a CDN atravessa os ficheiros comprimidos e fornece-los aos clientes que solicitam-los.
* Ative a compressão diretamente nos servidores de limite de CDN. Neste caso, a CDN comprime os ficheiros e serve-las para os utilizadores finais, mesmo não são comprimidos pelo servidor de origem.

> [!IMPORTANT]
> As alterações de configuração da CDN podem demorar algum tempo para propagar através da rede. Para <b>CDN do Azure da Akamai</b> perfis, propagação normalmente conclusão num minuto.  Para <b>CDN do Azure da Verizon</b> perfis, propagação normalmente for concluída dentro de 90 minutos. Se estiver a configurar a compressão pela primeira vez para o ponto final CDN, considere a aguardar 1-2 horas antes de resolver para garantir que as definições de compressão terem sido propagados para os POPs.
> 
> 

## <a name="enabling-compression"></a>Ativar a compressão
As camadas Standard e Premium CDN fornecem a mesma funcionalidade de compressão, mas a interface de utilizador diferente. Para obter mais informações sobre as diferenças entre camadas Standard e Premium CDN, consulte [descrição geral da CDN do Azure](cdn-overview.md).

### <a name="standard-tier"></a>Escalão Standard
> [!NOTE]
> Esta secção aplica-se a **CDN do Azure Standard da Verizon** e **CDN do Azure Standard da Akamai** perfis.
> 
> 

1. Na página de perfil de CDN, selecione o ponto final da CDN que pretende gerir.
   
    ![Pontos finais de perfil CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    Abre a página de ponto final da CDN.
2. Selecione **compressão**.

    ![Pontos finais de perfil CDN](./media/cdn-file-compression/cdn-compress-select-std.png)
   
    Abre a página de compressão.
3. Selecione **no** para ativar a compressão.
   
    ![Opções de compressão de CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Utilize os tipos de formato predefinidos ou modifique a lista ao adicionar ou remover tipos de formato.
   
   > [!TIP]
   > Embora seja possível, não se recomenda a aplicar a compressão para formatos comprimidos. Por exemplo, ZIP, MP3, MP4 ou JPG.
   > 
 
5. Depois de efetuar as alterações, selecione **guardar**.

### <a name="premium-tier"></a>Escalão Premium
> [!NOTE]
> Esta secção aplica-se apenas ao **CDN do Azure Premium da Verizon** perfis.
> 
> 

1. Na página de perfil de CDN, selecione **gerir**.
   
    ![Botão de gerir do perfil de CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    É aberto o portal de gestão do CDN.
2. Coloque o cursor sobre o **HTTP grande** separador, em seguida, coloque o cursor sobre o **definições da Cache** flyout. Selecione **compressão**.

    ![Seleção de compressão do ficheiro](./media/cdn-file-compression/cdn-compress-select.png)
   
    Opções de compressão são apresentadas.
   
    ![Opções de compressão do ficheiro](./media/cdn-file-compression/cdn-compress-files.png)
3. Ativar a compressão selecionando **ativar a compressão**. Introduza os tipos de MIME que pretende comprimir como uma lista delimitada por vírgulas (sem espaços) a **tipos de ficheiro** caixa.
   
   > [!TIP]
   > Embora seja possível, não se recomenda a aplicar a compressão para formatos comprimidos. Por exemplo, ZIP, MP3, MP4 ou JPG.
   > 
    
4. Depois de efetuar as alterações, selecione **atualização**.

## <a name="compression-rules"></a>Regras de compressão

### <a name="azure-cdn-from-verizon-profiles-both-standard-and-premium"></a>CDN do Azure da Verizon perfis (standard e premium)

Para **CDN do Azure da Verizon** são comprimidos perfis, apenas ficheiros elegíveis. Para ser elegível para compressão, um ficheiro tem de:
- Ser maior do que 128 bytes.
- Pode ser inferior a 1 MB.
 
Estes perfis suportam **gzip** (GNU zip), **deflate**, **bzip2**, ou **br** (Brotli) codificação. Se o pedido de suportar mais do que um tipo de compressão, os tipos de compressão têm precedência sobre Brotli compressão.

Quando um pedido para um recurso Especifica a codificação de Brotli (inclui o cabeçalho HTTP `Accept-Encoding: br`) e os resultados de pedido numa cache falha de acerto na, CDN do Azure efetua Brotli compressão do elemento no servidor de origem. Posteriormente, o ficheiro comprimido é servido diretamente a partir da cache.

### <a name="azure-cdn-from-akamai-profiles"></a>CDN do Azure da Akamai perfis

Para **CDN do Azure da Akamai** perfis, todos os ficheiros são elegíveis para compressão. No entanto, tem de ser um ficheiro de um tipo MIME que foi [configurado para compressão](#enabling-compression).

Estes perfis suportam apenas **gzip** codificação. Quando solicita um ponto final do perfil **gzip** codificado ficheiros, que são sempre pediram da origem, independentemente do pedido do cliente. 

## <a name="compression-behavior-tables"></a>Tabelas de comportamento da compressão
As tabelas seguintes descrevem o comportamento de compressão da CDN do Azure para cada cenário:

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>A compressão está desativada ou ficheiro não é elegível para compressão
| Formato de pedido de cliente (através do cabeçalho de codificação de aceitar) | Formato de ficheiro em cache | Resposta da CDN para o cliente | Notas |
| --- | --- | --- | --- |
| Comprimidos |Comprimidos |Comprimidos | |
| Comprimidos |Não comprimida |Não comprimida | |
| Comprimidos |Não colocar em cache |Comprimidos ou descomprimidos |Depende da resposta de origem |
| Não comprimida |Comprimidos |Não comprimida | |
| Não comprimida |Não comprimida |Não comprimida | |
| Não comprimida |Não colocar em cache |Não comprimida | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>A compressão está ativada e o ficheiro é elegível para compressão
| Formato de pedido de cliente (através do cabeçalho de codificação de aceitar) | Formato de ficheiro em cache | Resposta da CDN para o cliente | Notas |
| --- | --- | --- | --- |
| Comprimidos |Comprimidos |Comprimidos |CDN transcodes entre formatos suportados |
| Comprimidos |Não comprimida |Comprimidos |CDN efetua a compressão |
| Comprimidos |Não colocar em cache |Comprimidos |CDN executa compressão se a origem devolve um ficheiro descomprimido. **CDN do Azure da Verizon** passa o um ficheiro no primeiro pedido e, em seguida, comprimir e coloca em cache o ficheiro para pedidos subsequentes. Ficheiros com a Cache-Control: cabeçalho de cache não nunca são comprimidos. |
| Não comprimida |Comprimidos |Não comprimida |A descompressão efetua a CDN |
| Não comprimida |Não comprimida |Não comprimida | |
| Não comprimida |Não colocar em cache |Não comprimida | |

## <a name="media-services-cdn-compression"></a>Serviços de multimédia compressão de CDN
Para pontos finais ativados para a CDN de serviços de multimédia de transmissão em fluxo, a compressão está ativada por predefinição para os seguintes tipos MIME: 
- application/vnd.ms-sstr+xml 
- aplicação/dash + xml
- application/vnd.apple.mpegurl
- aplicação/f4m + xml. 

Não é possível ativar ou desativar a compressão para estes tipos MIME com o portal do Azure.  

## <a name="see-also"></a>Consulte também
* [Troubleshooting CDN file compression](cdn-troubleshoot-compression.md) (Resolver problemas de compressão de ficheiros da CDN)    

