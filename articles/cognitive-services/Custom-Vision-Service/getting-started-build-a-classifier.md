---
title: Criar um classificador com o serviço de visão personalizada - serviços cognitivos do Azure | Microsoft Docs
description: Saiba como utilizar o serviço de visão personalizada para criar um classificador que pode discernir objetos em fotografias.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: 6dc271c13f53a445c7d1101f5264d890208bd03c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352909"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Como criar um classificador com problemas de visão personalizada

Para utilizar o serviço de visão personalizada, primeiro tem de criar um classificador. Neste documento, saiba como criar um classificador através do web browser.

## <a name="prerequisites"></a>Pré-requisitos

Para criar um classificador, tem de ter:

- Um [conta Microsoft](https://account.microsoft.com/account) ou um Azure Active Directory OrgID ("conta escolar ou profissional"), pelo que pode iniciar sessão no customvision.ai e começar a utilizar.

    > [!IMPORTANT] 
    > O início de sessão OrgID para utilizadores do Azure Active Directory (Azure AD) de [nuvens nacionais](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud) não é atualmente suportado.

- Uma série de imagens para preparar o classificador (com um mínimo de 30 imagens por tag).

- Algumas imagens para testar o classificador depois do classificador está preparado.

- Opcional: Uma subscrição do Azure associada à sua Account Microsoft ou OrgID. Se não tiver uma subscrição do Azure, pode criar um [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

    > [!IMPORTANT]
    > Sem uma subscrição do Azure, só poderá criar __limitada avaliação__ projetos. Se tiver uma subscrição do Azure, será solicitado para criar recursos de formação de serviço de visão personalizada e predição no [portal do Azure](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) durante a criação do projeto.   

## <a name="create-a-new-project"></a>Criar um novo projeto

Para criar um novo projeto, utilize os seguintes passos:

1. No seu browser, navegue para o [página web de visão personalizada](https://customvision.ai). Selecione __sessão__ para começar a utilizar o serviço.

    ![Imagem da página de início de sessão](./media/getting-started-build-a-classifier/custom-vision-web-ui.png)

    > [!NOTE]
    > Depois de se inscrever serviço de visão personalizada, é-lhe apresentada uma lista de projetos. Fora "limitada versão de avaliação" dois projetos para fins de teste, projetos estão associados um recurso do Azure. Se for um utilizador do Azure, verá todos os projetos associados [recursos Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#grant-access-to-resources) para o qual tem acesso. 

2. Para criar o primeiro projeto, selecione **novo projeto**. Para o seu primeiro projeto, é-lhe pedido para aceitar os termos de serviço. Selecione a caixa de verificação e, em seguida, selecione o **concordo** botão. O **novo projeto** é apresentada a caixa de diálogo.

    ![A caixa de diálogo novo projeto tem campos de nome, descrição e domínios.](./media/getting-started-build-a-classifier/new-project.png)

3. Introduza um nome e uma descrição para o projeto. Em seguida, selecione um dos domínios disponíveis. Cada domínio otimiza o classificador de tipos específicos de imagens, conforme descrito na seguinte tabela:

    |Domínio|Objetivo|
    |---|---|
    |__Genérico__| Otimizado para uma vasta gama de tarefas de classificação de imagem. Se nenhum dos outros domínios são adequadas ou se não souber de qual o domínio para escolher, selecione o domínio genérico. |
    |__Prato__|Otimizado para fotografias de dishes como seria vê-los no menu restaurante. Se pretende classificar fotografias de fruta individuais ou vegetables, utiliza o domínio prato.|
    |__Landmarks__|Otimizado para reconhecível landmarks, naturais e artificial. Este domínio funciona melhor quando o landmark é claramente visível na fotografia. Este domínio funciona mesmo se o landmark é ligeiramente obstructed por pessoas existentes à frente dele.|
    |__Revenda__|Otimizado para imagens que se encontram num catálogo compras ou Web site de compras. Se quiser precisão elevada classificar entre dresses, pants e shirts, utilize este domínio.|
    |__Adulto__|Otimizado para definir melhor a conteúdo para adultos e conteúdo não adulto. Por exemplo, se pretender bloquear as imagens de pessoas bathing de encriptação, este domínio permite-lhe criar um classificador personalizado para o fazer.|
    |__Domínios compacto__| Otimizado para as restrições de classificação em tempo real em dispositivos móveis. Os modelos gerados por domínios compact podem ser exportados para executar localmente.|

    Pode alterar o domínio mais tarde, se pretender.

4. Selecione um grupo de recursos. A lista pendente grupo de recursos mostra todos os seus grupos de recursos do Azure que incluem um recurso de serviço de visão personalizada. Também pode criar selecione __limitada avaliação__. A entrada de avaliação limitada é o grupo de recursos apenas que um utilizador do Azure não será possível à sua escolha.

    Para criar o projeto, selecione __criar projeto__.

## <a name="upload-and-tag-images"></a>Carregamento e imagens de etiqueta

1. Para adicionar o classificador de imagens, utilize o __adicionar imagens__ botão e, em seguida, selecione __procurar ficheiros locais__. Selecione __abra__ para mover a etiquetagem.

    > [!TIP]
    > Depois de selecionar as imagens, tem de etiquetá-los. A etiqueta é aplicada para o grupo de imagens que selecionou para carregar, pelo que pode ser mais fácil carregar imagens por etiquetas que planeia utilizar. Também pode alterar a etiqueta selecionadas imagens depois de serem marcados e carregados.

    > [!TIP]
    > Carregue as imagens com os ângulos de câmara diferentes, iluminação da, em segundo plano, tipos, os estilos, grupos, tamanhos, etc. Utilize uma variedade de tipos de fotografias para garantir que o classificador não está totalmente direcionado e pode generalizar bem.

    Serviço de visão personalizada aceita imagens de formação. jpg, PNG e o formato de *.bmp, 6 MB por imagem. (Imagens de predição podem ser até 4 MB por imagem). Recomendamos que imagens ser 256 pixels no limite mais curto. Quaisquer imagens mais curtos do que 256 pixels no limite mais curto são expandidos pelo serviço de visão personalizada.

    ![O controlo de imagens de adicionar é apresentado no canto superior esquerdo e, como um botão no centro da parte inferior.](./media/getting-started-build-a-classifier/add-images01.png)

    ![É apresentado no botão de ficheiros locais procurar quase center na parte inferior.](./media/getting-started-build-a-classifier/add-images02.png)

    >[!NOTE] 
    > A API REST pode ser utilizada para carregar as imagens de formação de URLs.

2. Para definir a etiqueta, introduza o texto a __etiquetas My__ campo e, em seguida, utilizar o __+__ botão. Para carregar as imagens e marcá-los, utilize o __carregar ficheiros [número]__ botão. Pode adicionar mais do que uma etiqueta para as imagens. 

    > [!NOTE]
    > O tempo de carregamento varia consoante o número e tamanho das imagens que selecionou.

    ![Imagem da etiqueta e o carregamento de página](./media/getting-started-build-a-classifier/add-images03.png)

3. Selecione __feito__ depois das imagens foram carregadas.

    ![A barra de progresso mostra todas as tarefas concluídas.](./media/getting-started-build-a-classifier/add-images04.png)

4. Para carregar a outro conjunto de imagens, volte ao passo 1. Por exemplo, se pretender que seja possível distinguir dogs e ponies, carregar e imagens de ponies de etiqueta.

## <a name="train-and-evaluate-the-classifier"></a>Dar formação e avaliar o classificador

Para preparar o classificador, selecione o **preparar** botão.

![O botão de formação está perto da parte superior direita da janela do browser.](./media/getting-started-build-a-classifier/train01.png)

Demora apenas alguns minutos para preparar o classificador. Durante este período, são apresentadas informações sobre o processo de preparação.

![O botão de formação está perto da parte superior direita da janela do browser.](./media/getting-started-build-a-classifier/train02.png)

Depois de formação, o __desempenho__ é apresentado. Indicadores de precisão e devolução de chamada indicam como boa que o classificador, baseada no teste automática. Serviço de visão personalizada utiliza as imagens que foi submetido para formação para calcular estes números, utilizando um processo denominado [k subconjuntos de validação validação cruzada](https://en.wikipedia.org/wiki/Cross-validation_(statistics)).

![Os resultados de formação mostram a precisão geral e devolução de chamada e a precisão e recuperar para cada etiqueta no classificador.](./media/getting-started-build-a-classifier/train03.png)

> [!NOTE] 
> Sempre que selecionou o **formação** botão, criar uma nova iteração do seu classificador. Pode ver todos os seus iterações antigas no **desempenho** separador e pode eliminar os que poderá estar obsoleto. Quando elimina uma iteração, pode ficar eliminar quaisquer imagens associados exclusivamente.

O classificador utiliza todas as imagens para criar um modelo que identifica cada etiqueta. Para testar a qualidade do modelo, o classificador tenta cada imagem de modelo para ver o que se localiza o modelo.

Qualities dos resultados classificador são apresentados.

|Termo|Definição|
|---|---|
|__Precisão__|Quando classifica uma imagem, como provável é o classificador corretamente classificar a imagem? Fora de todas as imagens que são utilizadas para preparar o classificador (dogs e ponies), que percentagem modelo obtiveram correto? 99 etiquetas corretas fora 100 imagens proporciona uma precisão de 99%.|
|__Recuperar__|Fora de todas as imagens que devem ter sido corretamente classificadas, quantos o classificador identificar corretamente? Devolução de chamada de 100% significa que, se existirem 38 imagens de preguiçoso as imagens que foram utilizadas para preparar o classificador, o classificador encontrar 38 dogs.|

## <a name="next-steps"></a>Passos Seguintes

[Testar e reparametrização do modelo](test-your-model.md)

