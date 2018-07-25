---
title: Criar um classificador com o serviço de visão personalizada - serviços cognitivos do Azure | Documentos da Microsoft
description: Saiba como utilizar o serviço de visão personalizada para criar um classificador que pode enxergar a objetos em fotografias.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: c5183078d2f9d5eb16abef4f5df240f77eea6b8b
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223374"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Como criar um classificador com visão personalizada

Para utilizar o serviço de visão personalizada, primeiro tem de criar um classificador. Neste documento, saiba como criar um classificador por meio de seu navegador da web.

## <a name="prerequisites"></a>Pré-requisitos

Para criar um classificador, primeiro tem de ter:

- Válido [conta Microsoft](https://account.microsoft.com/account) ou um Azure Active Directory OrgID ("conta escolar ou profissional"), para que pode iniciar sessão no customvision.ai e começar a utilizar.

    > [!IMPORTANT] 
    > O início de sessão OrgID para os utilizadores do Azure Active Directory (Azure AD) de [clouds nacionais](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud) não é atualmente suportada.

- Uma série de imagens para treinar o seu classificador (com um mínimo de 30 imagens por marca).

- Algumas imagens para testar o seu classificador depois do classificador é preparado.

- Opcional: Uma subscrição do Azure associada ao seu Microsoft Account ou OrgID. Se não tiver uma subscrição do Azure, pode criar uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

    > [!IMPORTANT]
    > Sem uma subscrição do Azure, apenas será possível criar __limitado a versão de avaliação__ projetos. Se tiver uma subscrição do Azure, será solicitado a criar recursos de treinamento de serviço de visão personalizada e predição no [portal do Azure](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) durante a criação do projeto.   

## <a name="create-a-new-project"></a>Criar um novo projeto

Para criar um novo projeto, utilize os seguintes passos:

1. No seu browser, navegue para o [página da web de visão personalizada](https://customvision.ai). Selecione __iniciar sessão__ para começar a utilizar o serviço.

    ![Imagem da página de início de sessão](./media/getting-started-build-a-classifier/custom-vision-web-ui.png)

    > [!NOTE]
    > Depois de iniciar sessão serviço de visão personalizada, é apresentada uma lista de projetos. Fora "limitado versão de avaliação" dois projetos para fins de teste, os projetos estão associados um recurso do Azure. Se for um utilizador do Azure, verá todos os projetos associados [recursos do Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#grant-access-to-resources) para o qual tem acesso. 

2. Para criar seu primeiro projeto, selecione **novo projeto**. Para o seu primeiro projeto, é-lhe perguntado para aceitar os termos de serviço. Selecione a caixa de verificação e, em seguida, selecione o **concordo** botão. O **novo projeto** é apresentada a caixa de diálogo.

    ![A caixa de diálogo novo projeto tem campos de nome, descrição e domínios.](./media/getting-started-build-a-classifier/new-project.png)

3. Introduza um nome e uma descrição para o projeto. Em seguida, selecione um dos domínios disponíveis. Cada domínio otimiza o classificador para tipos específicos de imagens, conforme descrito na tabela a seguir:

    |Domínio|Objetivo|
    |---|---|
    |__Genérico__| Otimizado para uma ampla variedade de tarefas de classificação de imagens. Se nenhum dos outros domínios são adequadas ou se tiver a certeza de qual escolher o domínio, selecione o domínio genérico. |
    |__Comida__|Otimizado para fotos de pratos conforme os veria num menu do restaurante. Se quiser classificar fotografias de frutas individuais ou vegetables, utilize o domínio de alimento.|
    |__Pontos de referência__|Otimizado para pontos de referência reconhecíveis, naturais e artificiais. Este domínio funciona melhor quando o ponto de referência é claramente visível na fotografia. Este domínio funciona mesmo que o ponto de referência é um pouco obstructed por pessoas na sua frente.|
    |__Revenda__|Otimizado para imagens que se encontram num catálogo de compra ou o Web site de compra. Se pretender que o serviço de alta precisão classificar entre dresses, intuição e shirts, utilize este domínio.|
    |__Adulto__|Otimizado para definir melhor o conteúdo para adultos e conteúdo não adulto. Por exemplo, se deseja bloquear imagens de pessoas em bathing de encriptação, este domínio permite-lhe criar um classificador personalizado para fazer isso.|
    |__Domínios compactos__| Otimizado para as restrições de classificação em tempo real em dispositivos móveis. Os modelos de gerados por domínios compactos podem ser exportados para executar localmente.|

    Pode alterar o domínio mais tarde se desejar.

4. Selecione um grupo de recursos. A lista pendente do grupo de recursos mostra-lhe todos os grupos de recursos do Azure que incluem um recurso de serviço de visão personalizada. Também pode criar select __limitado a versão de avaliação__. A entrada de avaliação limitada é o grupo de recursos apenas que um utilizador não pertencente ao Azure poderá à sua escolha.

    Para criar o projeto, selecione __criar projeto__.

## <a name="upload-and-tag-images"></a>Carregar e imagens de etiqueta

1. Para adicionar imagens para o classificador, utilize o __adicionar imagens__ e, em seguida, selecione __navegar nos ficheiros locais__. Selecione __aberto__ para mover para a marcação.

    > [!TIP]
    > Após a seleção de imagens, tem de etiquetá-los. A etiqueta é aplicada ao grupo de imagens que selecionou para carregar, pelo que pode ser mais fácil carregar imagens pelas marcas de que pretende utilizar. Também pode alterar a etiqueta para imagens selecionadas depois de serem marcados e carregados.

    > [!TIP]
    > Carregar imagens com ângulos de câmera diferentes, iluminação, em segundo plano, tipos, estilos, grupos, tamanhos, etc. Utilize uma variedade de tipos de fotografia para garantir que o seu classificador é medida não e pode generalizar bem.

    Serviço de visão personalizada aceita imagens de formação em. jpg,. PNG e o formato. bmp, até 6 MB por imagem. (Imagens de predição podem ser até 4 MB por imagem). Recomendamos que as imagens sejam 256 pixels na extremidade mais curta. Quaisquer imagens mais curtas do que 256 pixels na extremidade mais curta aumentar verticalmente até ao serviço de visão personalizada.

    ![O controle de imagens de add é mostrado no canto superior esquerdo e, como um botão no centro na parte inferior.](./media/getting-started-build-a-classifier/add-images01.png)

    >[!NOTE] 
    > A API REST pode ser utilizada para carregar imagens de formação de URLs.

2. Para definir a etiqueta, introduza o texto no __minhas marcas__ campo e, em seguida, utilizar o __+__ botão. Para carregar as imagens e marcá-los, utilize o __carregar ficheiros [número]__ botão. Pode adicionar mais do que uma etiqueta para as imagens. 

    > [!NOTE]
    > O tempo de carregamento varia consoante o número e tamanho das imagens que selecionou.

    ![Imagem da etiqueta e o carregamento de página](./media/getting-started-build-a-classifier/add-images03.png)

3. Selecione __feito__ assim que as imagens foram carregadas.

    ![A barra de progresso mostra todas as tarefas concluídas.](./media/getting-started-build-a-classifier/add-images04.png)

4. Para carregar outro conjunto de imagens, regresse ao passo 1. Por exemplo, se quiser fazer a distinção entre cães e ponies, carregar e marque as imagens de ponies.

## <a name="train-and-evaluate-the-classifier"></a>Dar formação e avaliar o classificador

Para treinar o classificador, selecione o **treinar** botão.

![O botão de comboio é perto da parte superior direita da janela do navegador.](./media/getting-started-build-a-classifier/train01.png)

Demora apenas alguns minutos para treinar o classificador. Durante este período, são apresentadas informações sobre o processo de treinamento.

![O botão de comboio é perto da parte superior direita da janela do navegador.](./media/getting-started-build-a-classifier/train02.png)

Depois de treinamento, o __desempenho__ é apresentado. Os indicadores de precisão e lembre-se indicam-lhe quão bom que seu classificador é, com base no teste automático. O serviço de visão personalizada utiliza as imagens que submetidas para formação para calcular esses números, utilizando um processo chamado [fases k validação cruzada](https://en.wikipedia.org/wiki/Cross-validation_(statistics)).

![Os resultados de treinamento mostram a precisão geral e lembre-se e a precisão e lembre-se para cada etiqueta no classificador.](./media/getting-started-build-a-classifier/train03.png)

> [!NOTE] 
> Sempre que selecione o **Train** botão, cria uma nova iteração do seu classificador. Pode ver todas as iterações antigas na **desempenho** separador e poderá excluir qualquer que podem ser obsoletos. Quando elimina uma iteração, acaba a eliminar quaisquer imagens que estão associadas exclusivamente com o mesmo.

O classificador utiliza todas as imagens para criar um modelo que identifica cada marca. Para testar a qualidade do modelo, o classificador tenta cada imagem no modelo para ver o que se encontra o modelo.

São apresentadas as qualidades dos resultados do classificador.

|Termo|Definição|
|---|---|
|__Precisão__|Quando classifica uma imagem, o quão provável é o seu classificador para classificar corretamente a imagem? Fora de todas as imagens que são usadas para treinar o classificador (cães e ponies), que percentagem o modelo de obteve correto? 99 etiquetas corretas fora de 100 imagens dá uma precisão de 99%.|
|__Lembre-se__|Fora de todas as imagens que devem ter sido classificadas corretamente, quantos seu classificador identificar corretamente? Um recall de 100% significa que, se existirem 38 imagens de dog nas imagens que foram usadas para treinar o classificador, o classificador encontrado 38 cães.|

## <a name="next-steps"></a>Passos Seguintes

[Testar e voltar a preparar o modelo](test-your-model.md)

