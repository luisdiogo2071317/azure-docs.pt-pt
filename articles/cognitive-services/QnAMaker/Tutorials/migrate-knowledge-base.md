---
title: Migrar bases de dados de conhecimento de pré-visualização - Qna Maker
titleSuffix: Azure Cognitive Services
description: Como importar uma base de dados de conhecimento
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: a06a04ba992c8d7e9691e4838d38faaafd48de7a
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48853917"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrar uma base de dados de conhecimento através de importação-exportação
A ferramenta QnA Maker anunciámos a disponibilidade geral de 7 de Maio de 2018 no \\\build\ conferência. Disponibilidade geral do QnA Maker tem uma nova arquitetura baseada no Azure. Criado com a pré-visualização gratuita do QnA Maker de bases de dados de conhecimento, terá de ser migrados para GA. do QnA Maker Pré-visualização do QnA Maker vão ser preterido em Novembro de 2018. Para obter mais informações sobre as alterações em disponibilidade geral do QnA Maker, veja o anúncio de disponibilidade geral do QnA Maker [mensagem de blogue](https://aka.ms/qnamakerga-blog).

A ferramenta QnA Maker tem agora uma [modelo de preços](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/qna-maker/).

Pré-requisitos
> [!div class="checklist"]
> * Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
> * Configurar um novo [serviço QnA Maker](../How-To/set-up-qnamaker-service-azure.md)

## <a name="migrate-a-knowledge-base-from-qna-maker-preview-portal"></a>Migrar uma base de dados de conhecimento a partir do portal de pré-visualização do QnA Maker
1. Navegue para [portal de pré-visualização do QnA Maker](https://aka.ms/qnamaker-old-portal
) e clique em **meus serviços**.
2. Selecione a base de dados de conhecimento que quer migrar ao clicar no ícone de edição.

    ![Editar a base de dados de conhecimento](../media/qnamaker-how-to-migrate-kb/preview-editkb.png)

3. Clique em **transferir a base de dados de conhecimento** transferir um ficheiro. tsv que contém os conteúdos da sua base de dados de conhecimento - perguntas, obter respostas, metadados, e a origem de dados nomes que foram extraídos.

    ![Transferir a base de dados de conhecimento](../media/qnamaker-how-to-migrate-kb/preview-download.png)

4. Iniciar sessão para o [portal do QnA Maker](https://qnamaker.ai) com as suas credenciais do azure e clique em **criar uma base de dados de conhecimento**.
    
5. Se ainda não tenha criado um serviço QnA Maker, selecione **criar um serviço QnA**. Caso contrário, escolha um serviço QnA Maker de listas pendentes no passo 2. Selecione o serviço QnA Maker que irá alojar a Base de dados de conhecimento.

    ![Configurar o serviço QnA](../media/qnamaker-how-to-create-kb/setup-qna-resource.png)

6. Criar uma base de dados de conhecimento vazia 

    ![Origens de dados do conjunto](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Dê o seu serviço um **nome.** São suportados nomes duplicados e carateres especiais são também suportados.
    - Ignorar carregamento de ficheiros ou URLs que pretende utilizar os dados da sua base de dados de conhecimento de pré-visualização. Por agora, irá criar uma base de dados de conhecimento vazia.

7. Selecione **Criar**.

    ![Criar KB](../media/qnamaker-how-to-create-kb/create-kb.png)

8. Nesta nova base de dados de conhecimento, abra a **configurações** separador e clique em **base de dados de conhecimento de importação**. Importa as perguntas, respostas e metadados e mantém os nomes de origem de dados do qual foram extraídos.

   ![Base de dados de conhecimento de importação](../media/qnamaker-how-to-migrate-kb/Import.png)

9. **Teste** a nova base de dados de conhecimento com o painel de teste. Saiba como [testar a sua base de dados de conhecimento](../How-To/test-knowledge-base.md).
10. **Publicar** a base de dados de conhecimento. Saiba como [publicar a sua base de dados de conhecimento](../How-To/publish-knowledge-base.md).
11. Utilize o ponto final abaixo no código da aplicação ou bot. Veja aqui como [criar um bot QnA](../Tutorials/create-qna-bot.md).

    ![Valores do QnA Maker](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

Neste momento, todo o conhecimento conteúdo - perguntas, respostas e metadados, junto com os nomes dos arquivos de origem e os URLs são importados para a nova base de dados de conhecimento. 

## <a name="chatlogs-and-alterations"></a>Chatlogs ou alterações
Alterações (sinónimos) não são importadas automaticamente. Utilize o [APIs de V2](https://aka.ms/qnamaker-v2-apis) para exportar as alterações da pilha de pré-visualização e o [V4 APIs](https://aka.ms/qnamaker-v4-apis) substituir na pilha de novo.

Não é possível migrar chatlogs, uma vez que a nova pilha utiliza o Application Insights para armazenar chatlogs. No entanto pode transferir o chatlogs do [portal de pré-visualização](https://aka.ms/qnamaker-old-portal).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Editar uma base de dados de conhecimento](../How-To/edit-knowledge-base.md)
