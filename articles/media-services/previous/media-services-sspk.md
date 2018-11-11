---
title: Licenciamento Microsoft® Smooth Streaming cliente Kit de migração
description: Saiba mais sobre como para o Microsoft® Smooth Streaming cliente portar Kit de licenciamento.
services: media-services
documentationcenter: ''
author: xpouyat
manager: cfowler
editor: ''
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: xpouyat
ms.openlocfilehash: 71cb4ff7c5948067dfb039caf69241cfa9e22408
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243901"
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Licenciamento Microsoft® Smooth Streaming cliente Kit de migração
## <a name="overview"></a>Descrição geral
Microsoft Smooth Streaming cliente portar Kit (**SSPK** para abreviar) é uma implementação cliente Smooth Streaming, que é otimizada para o ajudar a fabricantes de dispositivos incorporados, cabo e operadoras de celular, fornecedores de serviços de conteúdo, aparelho fabricantes, fornecedores independentes de software (ISVs) e fornecedores de soluções para criar os produtos e serviços para transmissão em fluxo adaptável conteúdo no formato de transmissão em fluxo uniforme. SSPK é um dispositivo e independente de plataforma de implementação do cliente de transmissão em fluxo uniforme de mensagens em fila que pode ser portado por licenciado para qualquer plataforma e dispositivo. 

Incluída abaixo é uma arquitetura de alto nível e caixa do IIS Smooth Streaming portar Kit é a implementação de cliente de transmissão em fluxo uniforme fornecida pela Microsoft e inclui toda a lógica central para reprodução de conteúdo de transmissão em fluxo uniforme. Este conteúdo, em seguida, é transportado por parceiros para um dispositivo específico ou a plataforma implementando interfaces apropriadas. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a>Descrição
SSPK está licenciado nos termos que oferecem o valor comercial excelente. Licença SSPK fornece o setor com:

* Origem de Kit de migração de transmissão em fluxo uniforme em C++ 
  * implementa a funcionalidade de cliente de transmissão em fluxo uniforme
  * Adiciona o formato de análise heurística, colocação em memória intermédia lógica, etc.
* Aplicação de leitor de APIs 
  * interfaces de programação para interação com uma aplicação de leitor de multimédia
* Interface de (PAL) de camada de Abstração de plataforma 
  * interfaces de programação para interação com o sistema operativo (threads, soquetes)
* Interface de (HAL camada) de Abstração de hardware 
  * interfaces de programação para interação com o hardware de R / decodificadores V (decodificação, composição)
* Interface de gestão (DRM) de direitos digitais 
  * interfaces de programação para a manipulação de DRM por meio de camada de Abstração de DRM (DAL)
  * Kit de migração do Microsoft PlayReady funciona separadamente, mas integra-se por meio dessa interface. Para obter mais detalhes sobre o licenciamento do Microsoft PlayReady Device, clique em [aqui](https://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
* Exemplos de implementação 
  * exemplo de implementação de PAL para Linux
  * implementação de HAL do exemplo de GStreamer

## <a name="licensing-options"></a>Opções de Licenciamento
Microsoft Smooth Streaming cliente portar Kit fica disponível para máquinas em dois contratos de licença distintos: um para o desenvolvimento uniforme produtos de Interim cliente de transmissão em fluxo e outro para a distribuição uniforme de transmissão em fluxo Final produtos de cliente para os utilizadores finais.

* Para os fabricantes de chipset, integradores de sistemas ou independentes de software de (ISVs fornecedores) que precisam de uma origem de código portagem kit de desenvolvimento de produtos até lá, um Microsoft Smooth Streaming cliente portar Kit **licença de produto do Interim** deve ser executado.
* Para fabricantes de dispositivos ou os ISVs que precisam de direitos de distribuição para Smooth Streaming Final produtos de cliente para os utilizadores finais, Microsoft Smooth Streaming cliente portar Kit **licença do produto Final** deve ser executado.

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Smooth Streaming de licença de produto do Interim do Kit de migração de cliente da Microsoft
Sob esta licença, a Microsoft oferece um Smooth Streaming cliente portar Kit e os direitos de propriedade intelectual necessárias para desenvolver e distribuir Smooth produtos de Interim cliente de transmissão em fluxo para outras máquinas de dispositivo de Smooth Streaming cliente portar Kit que Distribua Smooth Streaming Final produtos de cliente.

#### <a name="fee-structure"></a>Estrutura de taxa
Uma taxa de licença de uso individual de US $ 50 000 fornece acesso para o Smooth Streaming cliente portar Kit. 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Smooth Streaming de licença do produto Final do Kit de migração de cliente da Microsoft
Sob esta licença, a Microsoft oferece a todos os direitos de propriedade intelectual necessário para receber Smooth Streaming Interim produtos de cliente de outras máquinas de Smooth Streaming cliente portar Kit e distribuir com a marca da empresa Smooth Streaming cliente Final Produtos para os utilizadores finais.

#### <a name="fee-structure"></a>Estrutura de taxa
O cliente Smooth Streaming produto Final é oferecido num modelo de direitos autorais como em:

* US $0,10 por implementação do dispositivo fornecida
* O royalties está limitada a US $ 50 000 por ano
* Nenhum royalty para os primeiros 10 000 implementações de dispositivo por ano 

## <a name="licensing-procedure-and-sspk-access"></a>Procedimento de licenciamento e SSPK acesso
Correio eletrónico [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) para todas as consultas de licenciamento.

O [portal de distribuição de SSPK](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) está acessível para máquinas de até lá registadas.

Até lá e Final SSPK licenciados podem enviar perguntas técnicas para [ smoothpk@microsoft.com ](mailto:smoothpk@microsoft.com).

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Smooth Streaming licenciados de contrato de produto provisória de cliente da Microsoft
* Soluções de negócios adroit, Inc.
* SA de difusão Digital avançadas
* AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
* Albis Technologies Ltd.
* Alticast Corporation
* Serviços de Digital Amazon, Inc.
* Tecnologia de Arion, Inc.
* AVC multimídia Software Co., Ltd.
* Cavium, Inc.
* EchoStar Corporation de compra
* Enseo, Inc.
* Fluendo da A.S.
* HANDAN BroadInfoCom co., Ltd.
* Infomir GMBH
* Inc. de EUA Irdeto
* iWEDIA da A.S. 
* Liberdade BV de serviços globais
* MediaTek Inc.
* MStar Co, Lda.
* Nintendo co., Ltd.
* OpenTV, Inc.
* Saffron Digital limitado
* Sichuan Changhong Electric Co., Ltd
* SoftAtHome
* Sony Corporation
* Tatung Technology Inc.
* TCL tecnologia Electronics (Huizhou) Co., Ltd.
* Parte superior Victory investimentos, Ltd.
* Vestel Elektronik Sanayi ve Ticaret A.S.
* VisualOn, Inc.
* ZTE Corporation

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Smooth Streaming licenciados de contrato de produto Final do cliente da Microsoft
* SA de difusão Digital avançadas
* AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
* Albis Technologies Ltd.
* Serviços de Digital Amazon, Inc.
* Tecnologia de AmTRAN co., Ltd.
* Arcadyan Technology Corporation
* Tecnologia de Arion, Inc.
* ATMACA ELEKTRONİK SAN. VE TİC. A.Ş
* Colúmbia céu difusão limitado
* CastPal Technology Inc., Shenzhen
* Compal Electronics, Inc.
* Dongguan Digital AV tecnologia Corp., Ltd.
* EchoStar Corporation de compra
* Enseo, Inc.
* Filmflex filmes limitado
* Fluendo da A.S.
* Gibson inovações limitado
* Informações de Haier Applicantion S.R.L
* HANDAN BroadInfoCom co., Ltd.
* Hisense International co., Ltd. 
* Homecast co., Ltd
* Está Hai precisão Industry Co., Ltd.
* Infomir GMBH
* Kaonmedia co., Ltd.
* KDDI Corporation
* Nintendo co., Ltd.
* Cor de laranja SA
* Saffron Digital limitado
* Sagemcom banda larga SAS
* Shenzhen Coship Electronics Co., LTD
* Shenzhen Jiuzhou Electric Co., Ltd
* Shenzhen Skyworth Digital tecnologia co., Ltd
* Sichuan Changhong Electric Co., Ltd.
* Corp. de Industrial Skardin
* Céu Deutschland Fernsehen GmbH & co. KG
* SmarDTV da A.S.
* SoftAtHome
* Sony Corporation
* Limitado de Marketing no exterior do TCL (especificamente comercial de Macau)
* Tecnologias de entrega de Technicolor, SAS
* Ltd. Global de Tongfang
* Parte superior Victory investimentos, Ltd.
* Produtos de estilo de vida de Toshiba & corporação de serviços
* Corporation de suporte de dados universal /Slovakia/ s.r.o.
* VIZIO, Inc.
* Wistron Corporation
* ZTE Corporation


## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

