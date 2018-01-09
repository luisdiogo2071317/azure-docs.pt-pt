---
title: "Configurar o codificador Telestream Wirecast para enviar uma transmissão em fluxo em direto | Microsoft Docs"
description: "Este tópico mostra como configurar o codificador em direto Wirecast para enviar uma transmissão em fluxo para canais de AMS que estão ativados para live encoding. "
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: 0e4fb0b7c915969da1760eaccc77aa399030752e
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Utilizar o codificador Wirecast para enviar uma transmissão em fluxo em direto
> [!div class="op_single_selector"]
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [Elementar em direto](media-services-configure-elemental-live-encoder.md)
> * [Transcodificadores](media-services-configure-tricaster-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Este artigo mostra como configurar o [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) codificador em direto para enviar uma transmissão em fluxo para AMS canais de consumo que está ativado para live encoding.  Para obter mais informações, consulte [Trabalhar com Canais Ativados para Realizar Live Encoding com Media Services do Azure](media-services-manage-live-encoder-enabled-channels.md).

Este tutorial mostra como gerir os serviços de suporte de dados do Azure (AMS) com a ferramenta Explorador de serviços de suporte de dados do Azure (AMSE). Esta ferramenta é executada apenas em PCs Windows. Se estiver no Mac ou Linux, utilize o portal do Azure para criar [canais](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) e [programas](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Pré-requisitos
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

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Especifique um nome de canal, o campo de descrição é opcional. Em definições de canal, selecione **padrão** para a opção Live Encoding, com o protocolo de entrada definido como **RTMP**. Pode deixar a todas as outras definições conforme está.

    Certifique-se de que o **iniciar agora o novo canal** está selecionada.

3. Clique em **criar canal**.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> O canal pode demorar até 20 minutos para iniciar.
>
>

Enquanto está a iniciar o canal, pode [configurar codificador](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

> [!IMPORTANT]
> A faturação é iniciada assim que o canal entra no estado pronto. Para obter mais informações, consulte [Estados do canal](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_wirecast_rtmp></a>Configurar o codificador Telestream Wirecast
Neste tutorial, são utilizadas as seguintes definições de saída. As restantes desta secção descreve os passos de configuração mais detalhadamente.

**Vídeo**:

* Codec: 264
* Perfil: Alta (nível 4.0)
* Velocidade de transmissão: kbps 5000
* Keyframe: 2 segundos (60 segundos)
* Taxa de moldura: 30

**Áudio**:

* O codec: AAC (LC)
* Velocidade de transmissão: kbps 192
* Frequência de amostragem: kHz 44.1

### <a name="configuration-steps"></a>Passos de configuração
1. Abra a aplicação de Telestream Wirecast sobre o computador que está a ser utilizado e estar configurada para RTMP de transmissão em fluxo.
2. Configurar o resultado ao navegar até o **saída** separador e selecionando **definições de saída...** .

    Certifique-se de que o **saída destino** está definido como **RTMP servidor**.
3. Clique em **OK**.
4. Na página Definições, defina o **destino** campo ser **Media Services do Azure**.

    O perfil de codificação é pré-seleccionado para **Azure 264 720 p 16:9 (1280 x 720)**. Para personalizar estas definições, selecione o ícone de engrenagem à direita da lista pendente e, em seguida, escolha **predefinido novo**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. Configure predefinições de codificador.

    A predefinição de nome e verificar as seguintes definições recomendadas:

    **Vídeo**

   * Codificador: MainConcept 264
   * Fotogramas por segundo: 30
   * Taxa média de bits: 5000 kbits/seg (pode ser ajustado com base no limitações de rede)
   * Perfil: principal
   * Fotograma chave cada: 60 frames

    **Áudio**

   * Taxa de bits de destino: 192 kbits/seg
   * Frequência de amostragem: kHz 44.100

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. Prima **guardar**.

    O campo de codificação tem agora o perfil recentemente criado disponível para seleção.

    Certifique-se que o novo perfil está selecionado.
7. Obter o canal de entrada URL para poder atribuí-la para o Wirecast **RTMP Endpoint**.

    Navegue de volta para a ferramenta AMSE e verificar o estado de conclusão de canal. Depois do estado mudou de **inicial** para **executar**, pode obter o URL de entrada.

    Quando o canal está em execução, faça duplo clique o nome de canal, navegue para baixo até paire sobre **URL de entrada de copiar para área de transferência** e, em seguida, selecione **primário URL de entrada**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. No Wirecast **definições saída** janela, colar estas informações no **endereço** campo da secção de saída e atribuir um nome de fluxo.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. Selecione **OK**.
2. No principal **Wirecast** ecrã, confirme as origens de entrada para as vídeo e áudio, estará pronto e, em seguida, prima **fluxo** no canto superior esquerdo.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> Antes de clicar em **fluxo**, **tem** Certifique-se de que o canal está pronto.
> Além disso, certifique-se não manter o canal no estado pronto sem uma entrada contribuição feed durante um período superior > 15 minutos.
>
>

## <a name="test-playback"></a>Reprodução de teste

Navegue para a ferramenta AMSE e faça duplo clique o canal a ser testada. No menu, coloque o cursor sobre **reprodução a pré-visualização** e selecione **com o Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Se a sequência aparecer num leitor de, em seguida, o codificador foi configurado corretamente para ligar ao AMS.

Se não for recebido um erro, o canal tem de ser reposto e definições de codificador ajustado. Consulte o [resolução de problemas](media-services-troubleshooting-live-streaming.md) artigo para obter orientações.  

## <a name="create-a-program"></a>Criar um programa
1. Depois de reprodução de canal é confirmada, crie um programa. Sob o **em direto** separador a ferramenta AMSE, faça duplo clique na área de programa e selecione **criar um novo programa**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. Nome do programa e, se necessário, ajuste o **duração da janela de arquivo** (que está predefinida para quatro horas). Também pode especificar uma localização de armazenamento ou deixe como predefinição.  
3. Verifique o **iniciar agora o programa** caixa.
4. Clique em **criar programa**.  

   >[!NOTE]
   >Criação de programa demora menos tempo que a criação do canal.
       
5. Assim que estiver a executar o programa, confirmar reprodução ao clicar o programa e navegar até **reprodução de programas** e, em seguida, selecionar **com o Azure Media Player**.  
6. Depois de confirmar, faça duplo clique novamente o programa e selecione **copie o URL de saída para a área de transferência** (ou obter estas informações do **definições e as informações do programa** opção no menu).

O fluxo está agora pronto para ser incorporado num leitor, distribuída ou para um público-alvo para uma visualização em direto.  

## <a name="troubleshooting"></a>Resolução de problemas
Consulte o [resolução de problemas](media-services-troubleshooting-live-streaming.md) artigo para obter orientações.

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
