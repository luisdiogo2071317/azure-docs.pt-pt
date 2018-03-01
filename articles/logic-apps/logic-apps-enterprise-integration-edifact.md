---
title: "Mensagens EDIFACT para a integração do enterprise B2B - Azure Logic Apps | Microsoft Docs"
description: "Mensagens de Exchange EDIFACT no formato EDI para integração do enterprise B2B com Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 2257d2c8-1929-4390-b22c-f96ca8b291bc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/26/2016
ms.author: LADocs; jonfan
ms.openlocfilehash: 4b1ea9966add3cf0d5f75988f11cda57fa4e4cf6
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="exchange-edifact-messages-for-enterprise-integration-with-logic-apps"></a>Mensagens de Exchange EDIFACT para a integração empresarial com logic apps

Podem trocar mensagens EDIFACT para Azure Logic Apps, tem de criar um contrato de EDIFACT e armazenar esse contrato na sua conta de integração. Eis os passos sobre como criar um contrato de EDIFACT.

> [!NOTE]
> Esta página abrange as funcionalidades EDIFACT para Azure Logic Apps. Para obter mais informações, consulte [X12](logic-apps-enterprise-integration-x12.md).

## <a name="before-you-start"></a>Antes de começar

Segue-se os itens que precisa de:

* Um [conta integração](logic-apps-enterprise-integration-create-integration-account.md) que já foi definida e associados à subscrição do Azure  
* Pelo menos dois [parceiros](logic-apps-enterprise-integration-partners.md) que já são definidas na sua conta de integração

> [!NOTE]
> Quando cria um contrato, o conteúdo nas mensagens que receber ou enviar e para o parceiro tem de corresponder ao tipo de contrato.

Depois de [criar uma conta de integração](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) e [adicionar parceiros](logic-apps-enterprise-integration-partners.md), pode criar um contrato de EDIFACT seguindo estes passos.

## <a name="create-an-edifact-agreement"></a>Crie um contrato de EDIFACT 

1. Inicie sessão no [portal do Azure](http://portal.azure.com "portal do Azure"). 

2. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "integração" e, em seguida, selecione **contas de automatização**.

   ![Localizar a conta de integração](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

   > [!TIP]
   > Se **todos os serviços** não aparecer, poderá ter de expanda o menu primeiro. Na parte superior do menu fechado, selecione **Mostrar etiquetas de texto**.

3. Em **contas de automatização**, selecione a conta de integração onde pretende criar o contrato.

   ![Selecione onde criar o contrato de conta de integração](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. Escolha **contratos**. Se não tiver um mosaico de contratos, adicione primeiro o mosaico.   

   ![Escolha o que mosaico "Contratos"](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. Na página de contratos, escolha **adicionar**.

   ![Escolha "Adicionar"](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. Em **adicionar**, introduza um **nome** para o contrato. Para **tipo de contrato**, selecione **EDIFACT**. Selecione o **anfitrião parceiro**, **identidade do anfitrião**, **convidado parceiro**, e **identidade de convidado** para o contrato.

   ![Forneça os detalhes do contrato](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

   | Propriedade | Descrição |
   | --- | --- |
   | Nome |Nome do contrato |
   | Tipo de contrato | Deve ser EDIFACT |
   | Parceiro do Anfitrião |Um contrato tem de parceiro de um anfitrião e convidado. O parceiro de anfitrião representa a organização que configura o contrato. |
   | Identidade do Anfitrião |Um identificador para o parceiro de anfitrião |
   | Parceiro Convidado |Um contrato tem de parceiro de um anfitrião e convidado. O parceiro de convidado representa a organização que está a decorrer empresas com o parceiro de anfitrião. |
   | Identidade do Convidado |Um identificador para o parceiro de convidado |
   | Definições de Receção |Estas propriedades aplicam-se a todas as mensagens recebidas por um contrato. |
   | Definições de Envio |Estas propriedades aplicam-se a todas as mensagens enviadas por um contrato. |
   ||| 

## <a name="configure-how-your-agreement-handles-received-messages"></a>Configurar a forma como os identificadores de contrato receberam mensagens

Agora que definiu as propriedades de contrato, pode configurar como o presente contrato identifica e processa mensagens a receber foi recebidas pelo seu parceiro através deste contrato.

1. Em **adicionar**, selecione **receber definições**.
Configure estas propriedades com base no seu contrato com o parceiro que trocas de mensagens consigo. Para descrições das propriedades, consulte as tabelas desta secção.

   **Receber definições** está organizado estas secções: identificadores, confirmação, esquemas, números de controlo, validação e definições interno.

   ![Configurar "Receber definições"](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. Depois de terminar, certifique-se guardar as definições ao escolher **OK**.

O contrato está agora pronto para processar mensagens a receber em conformidade com as definições selecionadas.

### <a name="identifiers"></a>Identificadores

| Propriedade | Descrição |
| --- | --- |
| UNB6.1 (Palavra-passe de Referência do Destinatário) |Introduza um valor alfanumérico vão entre 1 e 14 carateres. |
| UNB6.2 (Qualificador de Referência do Destinatário) |Introduza um valor com um mínimo de um caráter e um máximo de dois carateres alfanumérico. |

### <a name="acknowledgments"></a>Em que as confirmações

| Propriedade | Descrição |
| --- | --- |
| Receção de Mensagem (CONTRL) |Selecione esta caixa de verificação para devolver uma confirmação (CONTRL) técnica para o remetente intercâmbio. A confirmação é enviada para o remetente de intercâmbio com base nas definições enviar para o contrato. |
| Reconhecimento (CONTRL) |Selecione esta caixa de verificação para devolver uma confirmação (CONTRL) funcional para o remetente de intercâmbio de confirmação é enviada para o remetente de intercâmbio com base nas definições enviar para o contrato. |

### <a name="schemas"></a>Esquemas

| Propriedade | Descrição |
| --- | --- |
| UNH2.1 (TIPO) |Selecione um tipo de conjunto de transação. |
| UNH2.2 (VERSÃO) |Introduza o número de versão de mensagem. (Mínimo, um caráter; máximo, três carateres). |
| UNH2.3 (VERSÃO) |Introduza o número de versão de mensagem. (Mínimo, um caráter; máximo, três carateres). |
| UNH2.5 (CÓDIGO DE ATRIBUÍDO ASSOCIADO) |Introduza o código atribuído. (O máximo, seis carateres. Tem de ser alfanumérico). |
| UNG2.1 (ID DO REMETENTE DE APLICAÇÃO) |Introduza um valor com um mínimo de um caráter e um máximo de 35 carateres alfanumérico. |
| UNG2.2 (QUALIFICADOR DE CÓDIGO DO REMETENTE DE APLICAÇÃO) |Introduza um valor alfanumérico, com um máximo de quatro caracteres. |
| SCHEMA |Selecione o esquema previamente carregado que pretende utilizar da sua conta de integração associado. |

### <a name="control-numbers"></a>Números de Controlo
| Propriedade | Descrição |
| --- | --- |
| Não permitir o número de controlo intercâmbio duplicados |Para bloquear interchanges duplicados, selecione esta propriedade. Se selecionado, a ação de descodificar EDIFACT verifica que o número de controlo de intercâmbio (UNB5) para o intercâmbio recebido não corresponde a um número de controlo do intercâmbio anteriormente processados. Se for detetada uma correspondência, o intercâmbio não foi processado. |
| Verificar UNB5 duplicados a cada (dias) |Se optar por não permitir números de controlo de intercâmbio de duplicados, pode especificar o número de dias para efetuar a verificação, concedendo o valor adequado para esta definição. |
| Não permitir duplicados de números de controlo de Grupo |Para bloquear interchanges com números de controlo do grupo de duplicados (UNG5), selecione esta propriedade. |
| Não permitir duplicados de números de controlo do conjunto de Transação |Para bloquear interchanges com números de controlo de conjunto de transação duplicado (UNH1), selecione esta propriedade. |
| Número de Controlo de Reconhecimento do EDIFACT |Para designar os números de referência de conjunto de transação para utilização numa confirmação, introduza um valor para o prefixo, um intervalo de números de referência e um sufixo. |

### <a name="validations"></a>Validações

Quando concluir cada linha de validação, o outro é adicionado automaticamente. Se não especificar quaisquer regras, validação utiliza a linha "Predefinida".

| Propriedade | Descrição |
| --- | --- |
| Tipo de Mensagem |Selecione o tipo de mensagem EDI. |
| Validação de EDI |Efetue a validação de EDI em tipos de dados, tal como definido pelo esquema EDI propriedades, restrições de comprimento, os elementos de dados vazio e separadores à direita. |
| Validação Expandida |Se o tipo de dados não EDI, é o requisito de elemento de dados e validação permitido repetição, enumerações e dados de validação de comprimento de elemento (mínimo/máximo). |
| Permitir Zeros à Esquerda/Direita |Manter quaisquer adicionais esquerda ou à direita zero e carateres de espaço. Não remova estes carateres. |
| Cortar Zeros à Esquerda/Direita |Remova zero direita ou à esquerda e carateres de espaço. |
| Política de Separador Decimal |Gere separadores à direita. <p>Selecione **não permitido** para proibir delimitadores à direita e de separadores no intercâmbio recebido. Se tiver o intercâmbio delimitadores à direita e de separadores, o intercâmbio está declarado não válido. <p>Selecione **opcional** para aceitar interchanges com ou sem os delimitadores à direita e de separadores. <p>Selecione **obrigatório** quando o intercâmbio recebido tem de ter os delimitadores à direita e de separadores. |

### <a name="internal-settings"></a>Definições Internas

| Propriedade | Descrição |
| --- | --- |
| Criar etiquetas XML vazias se forem permitidos separadores decimais |Selecione esta caixa de verificação para que o remetente de intercâmbio incluem as etiquetas XML vazias para separadores à direita. |
| Dividir Intercâmbio como conjuntos de transação - suspender conjuntos de transação com erro|Analisa a cada transação definida num intercâmbio para um documento XML separado aplicando o envelope adequado para o conjunto de transação. Suspenda apenas os conjuntos de transação que não obedeçam a validação. |
| Dividir Intercâmbio como conjuntos de transação - suspender intercâmbio com erro|Analisa a cada transação definida num intercâmbio para um documento XML separado aplicando o envelope adequado. Suspenda o intercâmbio completo quando um ou mais conjuntos de transação o intercâmbio falharem a validação. | 
| Preservar intercâmbio - suspender conjuntos de transação com o erro |Deixa o intercâmbio intactas, cria um documento XML para o intercâmbio de batch de todo. Suspenda apenas os conjuntos de transação que não obedeçam a validação, ao continuar a processar todos os outros conjuntos de transação. |
| Preservar Intercâmbio - suspender intercâmbio com erro |Deixa o intercâmbio intactas, cria um documento XML para o intercâmbio de batch de todo. Suspenda o intercâmbio completo quando um ou mais conjuntos de transação o intercâmbio falharem a validação. |

## <a name="configure-how-your-agreement-sends-messages"></a>Configurar como o seu contrato envia mensagens

Pode configurar a forma como este contrato identifica e processa mensagens de saída que enviam para os parceiros através deste contrato.

1.  Em **adicionar**, selecione **enviar definições**.
Configure estas propriedades com base no seu contrato com o seu parceiro a quem trocas de mensagens consigo. Para descrições das propriedades, consulte as tabelas desta secção.

    **Enviar definições** está organizado estas secções: identificadores, confirmação, esquemas, Envelopes, conjuntos de carateres e separadores, números de controlo e validações.

    ![Configurar a "Definições de envio"](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. Depois de terminar, certifique-se guardar as definições ao escolher **OK**.

O contrato está agora pronto para processar mensagens de saída que está em conformidade com as definições selecionadas.

### <a name="identifiers"></a>Identificadores

| Propriedade | Descrição |
| --- | --- |
| UNB1.2 (versão sintaxe) |Selecione um valor entre **1** e **4**. |
| UNB2.3 (Endereço de Encaminhamento Inverso do Remetente) |Introduza um valor com um mínimo de um caráter e um máximo de 14 carateres alfanumérico. |
| UNB3.3 (Endereço de Encaminhamento Inverso do Destinatário) |Introduza um valor com um mínimo de um caráter e um máximo de 14 carateres alfanumérico. |
| UNB6.1 (Palavra-passe de Referência do Destinatário) |Introduza um valor com um mínimo de um e um máximo de 14 carateres alfanumérico. |
| UNB6.2 (Qualificador de Referência do Destinatário) |Introduza um valor com um mínimo de um caráter e um máximo de dois carateres alfanumérico. |
| UNB7 (ID de Referência da Aplicação) |Introduza um valor com um mínimo de um caráter e um máximo de 14 carateres alfanumérico |

### <a name="acknowledgment"></a>Confirmação
| Propriedade | Descrição |
| --- | --- |
| Receção de Mensagem (CONTRL) |Selecione esta caixa de verificação se o parceiro alojado espera receber uma confirmação (CONTRL) técnica. Esta definição especifica que o parceiro alojado, o que está a enviar a mensagem, solicita uma confirmação de parceiro de convidado. |
| Reconhecimento (CONTRL) |Selecione esta caixa de verificação se o parceiro alojado espera receber uma confirmação (CONTRL) funcional. Esta definição especifica que o parceiro alojado, o que está a enviar a mensagem, solicita uma confirmação de parceiro de convidado. |
| Gerar ciclo SG1/SG4 para conjuntos de transações aceites |Se tiver escolhido solicitar uma confirmação funcional, selecione esta caixa de verificação para forçar a geração de ciclos de SG1/SG4 no funcionais em que as confirmações CONTRL para conjuntos de transação foi aceite. |

### <a name="schemas"></a>Esquemas
| Propriedade | Descrição |
| --- | --- |
| UNH2.1 (TIPO) |Selecione um tipo de conjunto de transação. |
| UNH2.2 (VERSÃO) |Introduza o número de versão de mensagem. |
| UNH2.3 (VERSÃO) |Introduza o número de versão de mensagem. |
| SCHEMA |Selecione o esquema a utilizar. Esquemas estão localizadas na sua conta de integração. Para aceder à sua esquemas, primeiro associe a sua conta de integração para a sua aplicação lógica. |

### <a name="envelopes"></a>Envelopes
| Propriedade | Descrição |
| --- | --- |
| UNB8 (Código de Prioridade de Processamento) |Introduza um valor alfabético que não é mais do que um caráter. |
| UNB10 (Contrato de Comunicação) |Introduza um valor com um mínimo de um caráter e um máximo de 40 carateres alfanumérico. |
| UNB11 (Indicador de Teste) |Selecione esta caixa de verificação para indicar que o intercâmbio que gerou dados de teste |
| Aplicar Segmento UNA (Conselho de Cadeia de Serviço) |Selecione esta caixa de verificação para gerar um segmento UNA para intercâmbio para serem enviados. |
| Aplicar Segmentos UNG (Cabeçalho do Grupo de Função) |Selecione esta caixa de verificação para criar segmentos de agrupamento no cabeçalho de grupo funcional as mensagens enviadas para o parceiro de convidado. Os seguintes valores são utilizados para criar segmentos UNG: <p>Para **UNG1**, introduza um valor com um mínimo de um caráter e um máximo de seis carateres alfanumérico. <p>Para **UNG2.1**, introduza um valor com um mínimo de um caráter e um máximo de 35 carateres alfanumérico. <p>Para **UNG2.2**, introduza um valor alfanumérico, com um máximo de quatro caracteres. <p>Para **UNG3.1**, introduza um valor com um mínimo de um caráter e um máximo de 35 carateres alfanumérico. <p>Para **UNG3.2**, introduza um valor alfanumérico, com um máximo de quatro caracteres. <p>Para **UNG6**, introduza um valor com um mínimo de um e um máximo de três carateres alfanumérico. <p>Para **UNG7.1**, introduza um valor com um mínimo de um caráter e um máximo de três carateres alfanumérico. <p>Para **UNG7.2**, introduza um valor com um mínimo de um caráter e um máximo de três carateres alfanumérico. <p>Para **UNG7.3**, introduza um valor com um mínimo de 1 caráter e um máximo de 6 carateres alfanumérico. <p>Para **UNG8**, introduza um valor com um mínimo de um caráter e um máximo de 14 carateres alfanumérico. |

### <a name="character-sets-and-separators"></a>Conjuntos de Carateres e Separadores

Que o conjunto de caracteres, pode introduzir um conjunto diferente de delimitadores a ser utilizado para cada tipo de mensagem. Se um conjunto de carateres não está especificado para um esquema de message especificada, o conjunto de carateres predefinido é utilizado.

| Propriedade | Descrição |
| --- | --- |
| UNB1.1 (Identificador de Sistema) |Selecione o caráter EDIFACT definido para ser aplicado o intercâmbio de saída. |
| Esquema |Selecione um esquema a partir da lista pendente. Depois de concluir cada linha, é automaticamente adicionada uma nova linha. Para o esquema selecionado, selecione o conjunto de separadores que pretende utilizar, com base nas descrições do separador abaixo. |
| Tipo de Entrada |Selecione um tipo de entrada da lista pendente. |
| Separador de Componente |Para separar os elementos de dados composto, introduza um único caráter. |
| Separador do Elemento de Dados |Para separar os elementos de dados simples dentro de elementos de dados composto, introduza um único caráter. |
| Terminador de Segmento |Para indicar o fim de um segmento EDI, introduza um único caráter. |
| Sufixo |Selecione o caráter que é utilizado com o identificador de segmento. Se designar um sufixo, o elemento de dados do segmento terminador pode estar vazio. Se o terminador de segmento for deixado em branco, tem de designar um sufixo. |

### <a name="control-numbers"></a>Números de Controlo
| Propriedade | Descrição |
| --- | --- |
| UNB5 (Número de Controlo de Nó) |Introduza um prefixo, um intervalo de valores para o número de controlo do intercâmbio e um sufixo. Estes valores são utilizados para gerar um intercâmbio de saída. O prefixo e o sufixo são opcionais, enquanto o número de controlo é necessário. O número de controlo é incrementado para cada nova mensagem; o prefixo e o sufixo se alteram. |
| UNG5 (Número de Controlo de Grupo) |Introduza um prefixo, um intervalo de valores para o número de controlo do intercâmbio e um sufixo. Estes valores são utilizados para gerar o número de controlo de grupo. O prefixo e o sufixo são opcionais, enquanto o número de controlo é necessário. O número de controlo é incrementado para cada nova mensagem de erro até que o valor máximo é atingido; o prefixo e o sufixo se alteram. |
| UNH1 (Número de Referência do Cabeçalho de Mensagem) |Introduza um prefixo, um intervalo de valores para o número de controlo do intercâmbio e um sufixo. Estes valores são utilizados para gerar o número de referência de cabeçalho de mensagem. O prefixo e o sufixo são opcionais, enquanto o número de referência é necessário. O número de referência é incrementado para cada nova mensagem; o prefixo e o sufixo se alteram. |

### <a name="validations"></a>Validações

Quando concluir cada linha de validação, o outro é adicionado automaticamente. Se não especificar quaisquer regras, validação utiliza a linha "Predefinida".

| Propriedade | Descrição |
| --- | --- |
| Tipo de Mensagem |Selecione o tipo de mensagem EDI. |
| Validação de EDI |Efetue a validação de EDI em tipos de dados, tal como definido pelas propriedades EDI do esquema, restrições de comprimento, os elementos de dados vazio e separadores à direita. |
| Validação Expandida |Se o tipo de dados não EDI, é o requisito de elemento de dados e validação permitido repetição, enumerações e dados de validação de comprimento de elemento (mínimo/máximo). |
| Permitir Zeros à Esquerda/Direita |Manter quaisquer adicionais esquerda ou à direita zero e carateres de espaço. Não remova estes carateres. |
| Cortar Zeros à Esquerda/Direita |Remova esquerda ou à direita zero carateres. |
| Política de Separador Decimal |Gere separadores à direita. <p>Selecione **não permitido** para proibir delimitadores à direita e de separadores no intercâmbio enviado. Se tiver o intercâmbio delimitadores à direita e de separadores, o intercâmbio está declarado não válido. <p>Selecione **opcional** enviar interchanges com ou sem os delimitadores à direita e de separadores. <p>Selecione **obrigatório** se o intercâmbio enviado tem de ter os delimitadores à direita e de separadores. |

## <a name="find-your-created-agreement"></a>Encontrar o contrato criado

1.  Depois de concluir a definição em todas as propriedades de contrato, o **adicionar** página, escolha **OK** para concluir a criação do seu contrato e voltar à sua conta de integração.

    O contrato recém-adicionado agora aparece na sua **contratos** lista.

2.  Também pode ver os contratos na descrição geral da sua integração conta. No menu de conta a integração, escolha **descrição geral**, em seguida, selecione o **contratos** mosaico. 

    ![Escolha o que mosaico "Contratos"](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="view-swagger-file"></a>Ver o ficheiro Swagger
Para ver os detalhes do Swagger para o conector EDIFACT, consulte [EDIFACT](/connectors/edifact/).

## <a name="learn-more"></a>Saiba mais
* [Saiba mais sobre o Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Saiba mais sobre o pacote de integração do Enterprise")  

