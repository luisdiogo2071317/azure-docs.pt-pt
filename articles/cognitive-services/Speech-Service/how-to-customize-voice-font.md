---
title: O que é de voz personalizada? -Serviços cognitivos as do azure
description: Este artigo proporciona uma visão geral Microsoft texto em voz voz personalização, que lhe permite criar uma voz de marca reconhecível, um do-única.
services: cognitive-services
author: noellelacharite
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2018
ms.author: nolach
ms.openlocfilehash: 84493ae83515c0458bf5b9e9cf44603300a8b4f7
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284892"
---
# <a name="creating-custom-voice-fonts"></a>Criar tipos de voz personalizada

Personalização de voz de Microsoft texto em fala (TTS) permite-lhe criar uma voz reconhecível, um do-única para sua marca: um *tipo de voz.* 

Para criar o seu tipo de voz, fazer uma gravação de studio e carregar os scripts associados como os dados de treinamento. O serviço, em seguida, cria um modelo de voz única ajustado para a gravação. Em seguida, pode utilizar este tipo de voz para sintetizar voz. 

Pode começar a utilizar com uma pequena quantidade de dados para uma prova de conceito. Mas os dados mais fornecer, quanto mais natural e professional pode parecer a sua voz.

Personalização de voz está disponível para inglês Americano (em inglês) e continental chinês (zh-CN).

## <a name="prerequisites"></a>Pré-requisitos

A funcionalidade de personalização de voz de texto em voz está atualmente em pré-visualização privada. [Preencha o formulário de aplicação](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0N8Vcdi8MZBllkZb70o6KdURjRaUzhBVkhUNklCUEMxU0tQMEFPMjVHVi4u) para ser considerado para o acesso.

Também precisa de uma conta do Azure e uma subscrição para o serviço de voz. [Criar um](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) se ainda não o fez. Ligue-se a sua subscrição para o portal de voz personalizada da seguinte forma.

1. Inicie sessão no [portal de voz personalizada](https://customvoice.ai) com a mesma conta Microsoft que utilizou para aplicam-se para obter acesso.

2. Vá para "Subscrições" no nome da sua conta no canto superior direito.

    ![Subscrições](media/custom-voice/subscriptions.png)

3. Na página "Subscrições", escolha "Subscrição existente do Connect".

     ![Ligar a subscrição existente](media/custom-voice/connect-existing-sub.png)

4. Cole a chave de subscrição na tabela, conforme mostrado abaixo. Cada subscrição tem duas chaves, e pode usar qualquer um deles.

     ![Adicionar subscrição](media/custom-voice/add-subscription.png)

Está pronto para começar!

## <a name="prepare-recordings-and-transcripts"></a>Preparar gravações e transcrições

Um conjunto de dados de formação de voz consiste num conjunto de arquivos de áudio, junto com um arquivo de texto que contém as transcrições de todos esses arquivos de áudio.

Preparar-se esses arquivos em ambas as direções: um escrever um script e fazê-lo, lidos pelo talentos de voz, ou usar áudio publicamente disponíveis e transcrever para texto. No último caso, edite disfluencies dos arquivos de áudio, como "um" e outros sons de preenchimento, stutters, mumbled palavras ou mispronunciations.

Para produzir um tipo de voz bom, é importante que as gravações são feitas num ambiente silencioso com um microfone de alta qualidade. Volume consistente, falando taxa, fala pitch e expressivas mannerisms de voz são essenciais para criar uma voz digital excelente. Para criar uma voz para uso em produção, recomendamos que usar o talento de studio e de voz de gravação profissional. Para obter mais detalhes, consulte [exemplos de como grave voz para uma voz personalizada](record-custom-voice-samples.md).

### <a name="audio-files"></a>Arquivos de áudio

Cada arquivo de áudio deve conter uma única expressão (por exemplo, uma sentença única ou uma única mão de um sistema de caixa de diálogo). Todos os ficheiros têm de estar no mesmo idioma (multilíngües vozes personalizadas não são suportadas). Os arquivos de áudio devem também ter, cada um nome de ficheiro exclusivo numérico feita com a extensão de nome de ficheiro `.wav`.

Arquivos de áudio devem estar preparados da seguinte forma. Outros formatos não são suportados e serão rejeitados.

| **Propriedade** | **Valor** |
| ------------ | --------- |
| Formato do Ficheiro  | RIFF (WAV)|
| Taxa de Amostragem| pelo menos de 16 000 Hz |
| Formato de Exemplo| PCM, 16 bits |
| Nome de Ficheiro    | Numérico, com `.wav` extensão |
| Formato do Arquivo| Zip      |
| Tamanho Máximo do Arquivo|200 MB|

Coloque o conjunto de arquivos de áudio numa única pasta sem subdiretórios e todo o conjunto como um único arquivo de ficheiro ZIP do pacote.

> [!NOTE]
> Ficheiros de onda com uma taxa de amostragem mais baixo do que 16.000 Hz serão rejeitados. Nos casos em que um ficheiro zip contém ondas com taxas de amostragem diferentes, serão importados apenas esses igual ou maior do que 16.000 Hz.
> O portal atualmente importa ZIP arquiva até 200 MB. No entanto, podem ser carregados vários arquivos. O número máximo de conjuntos de dados permitidos é de que 10 arquivos ZIP gratuitamente utilizadores de subscrição e 50 para utilizadores de subscrição standard.

### <a name="transcripts"></a>Transcrições

O arquivo de transcrição é um ficheiro de texto sem formatação (ANSI/UTF-8/UTF-8-BOM/UTF-16-LE/UTF-16-BE). Cada linha do arquivo de transcrição tem de ter o nome de um arquivo de áudio, seguido de um caractere do separador (ponto de código 9) e, finalmente, a respetiva transcrição. Não existem linhas em branco são permitidas.

Por exemplo:

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

O sistema de voz personalizada normaliza transcrições ao converter o texto em minúsculas e remover pontuação estranhas. É importante que as transcrições são 100% precisa para gravações de áudio correspondentes.

> [!TIP]
> Quando criar produção voz vozes, selecionadas expressões de com (ou escrever scripts) Considerando cobertura fonética e eficiência.

## <a name="upload-your-datasets"></a>Carregar os conjuntos de dados

Depois de preparar o arquivo do arquivo de áudio e transcrições, carregá-los através da [Portal de serviço de voz personalizada](https://customvoice.ai).

> [!NOTE]
> Não não possível editar os conjuntos de dados após eles foram carregados. Caso se tenha esquecido incluem transcrições de alguns dos arquivos de áudio, por exemplo, ou acidentalmente, escolha o sexo errado, tem de carregar todo o conjunto de dados novamente. Verificar o conjunto de dados e definições minuciosamente antes de iniciar o carregamento.

1. Inicie sessão no portal.

2. Escolher **dados** em voz personalizada na página principal. 

    ![Meus projetos](media/custom-voice/my-projects.png)

    É apresentada a tabela de meus dados de voz. Está vazio, se ainda não carregou quaisquer conjuntos de dados de voz.

3. Clique em **importar dados** para abrir a página para carregar um novo conjunto de dados.

    ![Importar dados de voz](media/custom-voice/import-voice-data.png)

4. Introduza um nome e descrição nos campos fornecidos. 

5. Selecione a região para seus tipos de voz. Certifique-se de que as informações de localidade correspondente ao idioma dos dados de gravação e os scripts. 

6. Selecione o género do falante cujo voz estiver a utilizar.

7. Escolha o script e arquivos de áudio para carregar. 

8. Clique em **importação** para carregar os dados. Para conjuntos de dados maiores, a importação pode demorar vários minutos.

> [!NOTE]
> Os utilizadores de subscrição gratuita podem carregar dois conjuntos de dados cada vez. Utilizadores de subscrição Standard podem carregar cinco conjuntos de dados em simultâneo. Se atingir o limite, aguardar até que pelo menos um dos seus conjuntos de dados termina a importação, em seguida, tente novamente.

Quando o carregamento estiver concluído, a tabela de meus dados de voz é exibida novamente. Deverá ver uma entrada que corresponde ao conjunto de dados carregado apenas. 

Conjuntos de dados são validados automaticamente após o carregamento. Validação de dados inclui uma série de verificações nos arquivos de áudio para verificar se seu formato de ficheiro, o tamanho e a frequência de amostragem. Verificações nos arquivos de transcrição Verifique se o formato de arquivo e executam alguns normalização do texto. As expressões são transcrito usando o reconhecimento de voz e o texto resultante é comparado com a transcrição que forneceu.

![Meus dados de voz](media/custom-voice/my-voice-data.png)

A tabela seguinte mostra os Estados de processamento de conjuntos de dados importados. 

| Estado | Significado
| ----- | -------
| `NotStarted` | O conjunto de dados foi recebido e é colocada na fila para processamento
| `Running` | O conjunto de dados está sendo validado
| `Succeeded` | O conjunto de dados foi validado e agora pode ser utilizado para criar um tipo de voz

Depois de concluída a validação, pode ver o número total de expressões correspondentes para cada um dos seus conjuntos de dados da coluna de expressão.

Pode transferir um relatório para verificar as pontuações de pronúncia e o nível de ruído para cada um dos seus gravações. A pronúncia classificação vai de 0 a 100; uma pontuação inferior a 70 normalmente indica um erro de correspondência do erro ou um script de voz. Um destaque pesada pode reduzir sua pontuação pronúncia e afetar a voz digital gerada.

Um rácio de sinal e ruído superior (SNR) indica inferior ruído em sua áudio. Normalmente, pode entrar SNR 50 + por gravação por meio de estúdios profissionais. Áudio com um SNR abaixo de 20 pode resultar em óbvio ruído em sua voz gerado.

Considere novamente gravar qualquer expressões com pontuações de pronúncia baixa ou fraco rácios de sinal e ruído. Se não for possível gravar novamente, pode impedir que essas expressões com seu conjunto de dados.

## <a name="build-your-voice-font"></a>Criar o seu tipo de voz

Depois do conjunto de dados foi validado, pode usá-lo para criar o seu tipo de voz personalizada. 

1. Escolher **modelos** no menu pendente "Voz personalizada". 
 
    É apresentada a tabela de meus tipos de voz, a listagem quaisquer tipos de voz personalizada que já criou.

1. Clique em **criar vozes** sob o título da tabela. 

    É apresentada a página para a criação de um tipo de voz. A Localidade atual é mostrada na primeira linha da tabela. Altere a Localidade para criar uma voz em outro idioma. A localidade tem de ser o mesmo que os conjuntos de dados a ser utilizados para criar a voz.

1. Como fez ao carregar o conjunto de dados, introduza um nome e descrição para ajudar a identificar este modelo. 

    O nome que introduzir aqui será o nome utilizado para especificar a voz no seu pedido para síntese de fala como parte da entrada SSML, pelo que deve escolher cuidadosamente. Apenas letras, números e carateres de pontuação alguns como '-', '_' "(',')" são permitidos.

    Uma utilização comum do campo de descrição é registrar os nomes dos conjuntos de dados que foram utilizados para criar o modelo.

1. Escolha o género do seu tipo de voz. Tem de corresponder o género do conjunto de dados.

1. Selecione os conjuntos de dados que pretende utilizar para o tipo de voz de formação. Todos os conjuntos de dados utilizados tem de ser do mesmo falante.

1. Clique em **criar** para começar a criar o seu tipo de voz.

    ![Criar modelo](media/custom-voice/create-model.png)

Seu novo modelo é apresentada na tabela meus tipos de voz. 

![Meus tipos de voz](media/custom-voice/my-voice-fonts.png)

O estado apresentado reflete o processo de conversão o conjunto de dados para um tipo de voz, como mostrado aqui.

| Estado | Significado
| ----- | -------
| `NotStarted` | O seu pedido de criação de tipo de letra de voz é colocada na fila para processamento
| `Running` | O tipo de voz está a ser criado
| `Succeeded` | O tipo de voz foi criado e pode ser implementado

O tempo de treinamento varia dependendo do volume de dados de áudio processados. Vezes típicos variam entre sobre 30 minutos para centenas de expressões e 40 horas para expressões 20.000 com.

> [!NOTE]
> Os utilizadores de subscrição gratuita podem preparar um tipo de voz ao mesmo tempo. Utilizadores de subscrição Standard podem preparar os três vozes em simultâneo. Se atingir o limite, aguarde até que seja, pelo menos, um dos tipos de voz concluída treinamento e tente novamente.

## <a name="test-your-voice-font"></a>Testar o seu tipo de voz

Depois do tipo de voz criado com êxito, pode testá-lo antes de implementar para ser utilizado. Clique em **teste** na coluna de operações. É apresentada a página de teste para o tipo de voz selecionado. A tabela estiver vazia se ainda não tiver submetido quaisquer pedidos de teste para a voz.

![Meus tipos de voz, parte 2](media/custom-voice/my-voice-fonts2.png)

Clique em **teste com texto** botão sob o título da tabela para exibir um menu pop-up de envio de solicitações de texto. Pode submeter o pedido de teste no texto sem formatação ou SSML. O tamanho máximo de entrada é 1024 carateres, incluindo todas as etiquetas para o pedido SSML. O idioma do seu texto deve ser o mesmo que o idioma do seu tipo de voz.

![Teste de tipo de letra de voz](media/custom-voice/voice-font-testing.png)

Depois de preencher a caixa de texto e confirmar o modo de entrada, clique em **Sim** para submeter o pedido de teste e voltar para a página de teste. A tabela inclui agora uma entrada que corresponde ao seu novo pedido e a coluna de status agora conhecido. Pode demorar alguns minutos para sintetizar voz. Quando a coluna Estado ler com êxito, pode baixar a entrada de texto (um `.txt` arquivo) e saída de áudio (um `.wav` ficheiro) e audition a última opção de qualidade.

![Teste de tipo de letra, parte 2 de voz](media/custom-voice/voice-font-testing2.png)

## <a name="create-and-use-a-custom-endpoint"></a>Criar e utilizar um ponto final personalizado

Depois de ter criado e testado seu modelo de voz com êxito, implementá-la num ponto de extremidade de texto em voz personalizada. Em seguida, utilizar este ponto final em vez do ponto final comum quando são efetuados pedidos de texto em voz através da API REST. O ponto final personalizado pode ser chamado apenas pela subscrição que utilizou para implementar o tipo de letra.

Para criar um novo ponto final personalizado, escolha **pontos de extremidade** no menu de voz personalizada na parte superior da página. A página meus vozes implementado é apresentada, com sua tabela de pontos finais de voz personalizada atual, se houver. A Localidade atual será refletida na primeira linha da tabela. Para criar uma implementação para um idioma diferente, altere a localidade apresentada. (Tem de corresponder a voz que está a implementar).

Clique nas **implementar vozes** botão para criar um novo ponto final. Introduza o nome e descrição do seu ponto final personalizado.

No menu de subscrição, escolha a subscrição que pretende utilizar. Os utilizadores de subscrição gratuita podem ter apenas um modelo implementado por vez. Utilizadores de subscrição Standard podem criar até 20 pontos de extremidade, cada um com sua própria voz personalizada.

![Criar ponto final](media/custom-voice/create-endpoint.png)

Depois de selecionar o modelo de implementação, clique em **criar**. A página meus vozes implementado será exibida novamente, agora com uma entrada para o ponto final de novo. Pode demorar alguns minutos para instanciar um novo ponto final. Quando o estado da implementação é concluído com êxito, o ponto final está pronto para ser utilizado.

![Meu vozes implementados](media/custom-voice/my-deployed-voices.png)

Quando o estado de implementação é concluído com êxito, o ponto final do seu tipo de voz implementado é apresentada na tabela My vozes implementado. Pode utilizar este URI diretamente numa solicitação HTTP.

Testes online do ponto de extremidade também estão disponível através do portal de voz personalizada. Para testar o ponto final, escolha **pontos finais de teste** no menu de lista pendente de voz personalizada. O ponto final de teste de página é apresentada. Escolha uma voz personalizada implementada e introduza o texto seja falado (no texto sem formatação ou formato SSML) na caixa de texto.

> [!NOTE] 
> Quando utilizar SSML, o `<voice>` etiqueta tem de especificar o nome que deu a sua voz personalizada quando o criou.

Clique em **reproduzir** ouvir o texto falado em seu tipo de voz personalizada.

![Teste de ponto final](media/custom-voice/endpoint-testing.png)

O ponto final personalizado é funcionalmente idêntico ao ponto final do padrão usado para solicitações de voz. Ver [REST API](rest-apis.md) para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes

- [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)
