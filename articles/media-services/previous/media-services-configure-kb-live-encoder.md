---
title: Configurar o codificador de Haivision KB para enviar uma transmissão em fluxo em direto para o Azure | Microsoft Docs
description: Este tópico mostra como configurar o codificador em direto Haivision KB para enviar uma transmissão em fluxo para canais de AMS que estão ativados para live encoding.
services: media-services
documentationcenter: ''
author: dbgeorge
manager: vsood
editor: ''
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 02/02/2018
ms.author: juliako;dbgeorge
ms.openlocfilehash: 25077cd9338a2764c6dff9e755812033685f6641
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="use-the-haivision-kb-live-encoder-to-send-a-single-bitrate-live-stream"></a>Utilizar o codificador em direto Haivision KB para enviar uma transmissão em fluxo em direto
> [!div class="op_single_selector"]
> * [Elementar em direto](media-services-configure-elemental-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
> * [Haivision](media-services-configure-kb-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [wirecast](media-services-configure-wirecast-live-encoder.md)

Este tópico mostra como configurar o [codificador em direto Havision KB](https://www.haivision.com/products/kb-series/) codificador para enviar uma transmissão em fluxo para AMS canais de consumo que está ativado para live encoding. Para obter mais informações, consulte [Trabalhar com Canais Ativados para Realizar Live Encoding com Media Services do Azure](media-services-manage-live-encoder-enabled-channels.md).

Este tutorial mostra como gerir os serviços de suporte de dados do Azure (AMS) com a ferramenta Explorador de serviços de suporte de dados do Azure (AMSE). Esta ferramenta é executada apenas em PCs Windows. Se estiver no Mac ou Linux, utilize o portal do Azure para criar [canais](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) e [programas](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Pré-requisitos
*   Acesso a um codificador Haivision KB, executar SW v5.01, ou superior.
* [Criar uma conta de Media Services do Azure](media-services-portal-create-account.md)
* Certifique-se existe um ponto de final de transmissão em fluxo em execução. Para obter mais informações, consulte [gerir transmissão em fluxo pontos finais numa conta de Media Services](media-services-portal-manage-streaming-endpoints.md)
* Instale a versão mais recente do [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) ferramenta.
* Inicie a ferramenta e ligue à sua conta de AMS.

## <a name="tips"></a>Sugestões
* Sempre que possível, utilize uma ligação de internet hardwired.
* É uma boa regra prática ao determinar os requisitos de largura de banda duplo a forma de transmissão em fluxo. Embora não seja um requisito obrigatório, ajuda a mitigar o impacto de congestionamento de rede.
* Quando utilizar codificadores baseada em software, feche enviados quaisquer programas desnecessários.

## <a name="create-a-channel"></a>Criar um canal
1. Na ferramenta AMSE, navegue para o **em direto** separador e faça duplo clique na área de canal. Selecione **canal de criar...** no menu.
[Haivision](./media/media-services-configure-kb-live-encoder/channel.png)
2. Especifique um nome de canal, o campo de descrição é opcional. Em definições de canal, selecione **padrão** para a opção Live Encoding, com o protocolo de entrada definido como **RTMP**. Pode deixar a todas as outras definições conforme está. Certifique-se de que o **iniciar agora o novo canal** está selecionada.
3. Clique em **criar canal**.
[Haivision](./media/media-services-configure-kb-live-encoder/livechannel.png)

> [!NOTE]
> O canal pode demorar até 20 minutos para iniciar.

## <a name="configure-the-haivision-kb-encoder"></a>Configurar o codificador Haivision KB
Neste tutorial, são utilizadas as seguintes definições de saída. As restantes desta secção descreve os passos de configuração mais detalhadamente.

Vídeo:
-   Codec: 264
-   Perfil: Alta (nível 4.0)
-   Velocidade de transmissão: kbps 5000
-   Keyframe: 2 segundos (60 segundos)
-   Taxa de moldura: 30

Áudio:
-   O codec: AAC (LC)
-   Velocidade de transmissão: kbps 192
-   Frequência de amostragem: kHz 44.1

## <a name="configuration-steps"></a>Passos de configuração
1.  Início de sessão para a interface de utilizador Haivision KB.
2.  Clique em de **botão do Menu** no Centro de controlo do canal e selecione **adicionar canal**  
    ![Captura de 2017-08-14 em 9.15.09 de ecrã AM.png](./media/media-services-configure-kb-live-encoder/step2.png)
3.  Tipo de **nome do canal** no nome do campo e clique em seguinte.  
    ![Captura de 2017-08-14 em 9.19.07 de ecrã AM.png](./media/media-services-configure-kb-live-encoder/step3.png)
4.  Selecione o **origem de entrada do canal** do **origem de entrada** pendente e clique em seguinte.
    ![Captura de 2017-08-14 em 9.20.44 de ecrã AM.png](./media/media-services-configure-kb-live-encoder/step4.png)
5.  Do **codificador modelo** pendente escolha **H264-720 AAC 192** e clique em seguinte.
    ![Captura de 2017-08-14 em 9.23.15 de ecrã AM.png](./media/media-services-configure-kb-live-encoder/step5.png)
6.  Do **selecione nova saída** pendente escolha **RTMP** e clique em seguinte.  
    ![Captura de 2017-08-14 em 9.27.51 de ecrã AM.png](./media/media-services-configure-kb-live-encoder/step6.png)
7.  Do **canal de saída** janela, preencher as informações do Azure stream. Colar o **RTMP** ligação de configuração do canal inicial no **servidor** área. No **nome de saída** área escreva. o nome do canal. Na área de modelo de nome de fluxo, utilize o modelo RTMPStreamName_ % video_bitrate % nome o fluxo.
    ![Captura de 2017-08-14 em 9.33.17 de ecrã AM.png](./media/media-services-configure-kb-live-encoder/step7.png)
8.  Clique em seguinte e, em seguida, clique em concluído.
9.  Clique em de **botão Reproduzir** para iniciar o canal de codificador.  
    ![Haivision KB.png](./media/media-services-configure-kb-live-encoder/step9.png)

## <a name="test-playback"></a>Reprodução de teste
Navegue para a ferramenta AMSE e faça duplo clique o canal a ser testada. No menu, coloque o cursor sobre reprodução a pré-visualização e selecione com Media Player do Azure.

Se a sequência aparecer num leitor de, em seguida, o codificador foi configurado corretamente para ligar ao AMS.

Se não for recebido um erro, o canal tem de ser reposto e definições de codificador ajustado. Consulte o artigo para obter orientações sobre a resolução de problemas.

## <a name="create-a-program"></a>Criar um programa
1.  Depois de reprodução de canal é confirmada, crie um programa. No separador em direto na ferramenta AMSE, faça duplo clique na área de programa e selecione criar um novo programa.
[Haivision](./media/media-services-configure-kb-live-encoder/program.png)
1.  O programa de nome e, se necessário, ajuste a duração da janela de arquivo (que está predefinida para quatro horas). Também pode especificar uma localização de armazenamento ou deixe como predefinição.
2.  Caixa de verificação do início da programa agora.
3.  Clique em criar programa.
4.  Assim que estiver a executar o programa, confirme reprodução clicando com o programa e navegar para reproduzir os programas e, em seguida, selecionar com Media Player do Azure.
5.  Assim que confirmar, faça duplo clique novamente o programa e selecione a copiar o URL de saída para a área de transferência (ou obter estas informações a partir da opção de definições a partir do menu e informações sobre o programa).

O fluxo está agora pronto para ser incorporado num leitor, distribuída ou para um público-alvo para uma visualização em direto.

> [!NOTE]
> Criação de programa demora menos tempo que a criação do canal.