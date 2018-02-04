---
title: Crie e registe os conectores SOAP - Azure Logic Apps | Microsoft Docs
description: "Configurar conectores SOAP para utilização no Azure Logic Apps"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 031762e5639fc52e0b0a6a5bf8d12db25da25e12
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/03/2018
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Crie e registe os conectores SOAP no Azure Logic Apps

Para integrar serviços SOAP nos fluxos de trabalho das suas aplicações lógicas, pode criar e registar um conector personalizado Simple Object Access Protocol (SOAP) com a Linguagem WSDL (Web Services Description Language) que descreve os serviços SOAP. Os conectores SOAP funcionam como conectores pré-criados, pelo que pode utilizá-los da mesma forma que os outros conectores nas suas aplicações lógicas.


## <a name="prerequisites"></a>Pré-requisitos

Para registar o conector SOAP, precisa dos itens seguintes:

* Uma subscrição do Azure. Se não tiver uma subscrição, pode começar com uma [conta do Azure gratuita](https://azure.microsoft.com/free/). Caso contrário, inscreva-se [numa subscrição Pay As You Go](https://azure.microsoft.com/pricing/purchase-options/).

* Qualquer item aqui:
  * Um URL para um WSDL que define o seu serviço SOAP e as APIs
  * Um ficheiro WSDL que define o seu serviço SOAP e as APIs

  Neste tutorial, pode utilizar o nosso exemplo, [Orders SOAP Service](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl).

* Opcional: uma imagem que vai ser utilizada como ícone do conector personalizado


## <a name="1-create-your-connector"></a>1. Criar o conector

1. No portal do Azure, no menu principal do Azure, escolha **Novo**. Na caixa de pesquisa, introduza "conector do logic apps" como o filtro e prima Enter.

   ![Novo, procurar “conector do logic apps”](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. Na lista de resultados, escolha **Conector do Logic Apps** > **Criar**.

   ![Criar conector do Logic Apps](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Indique os detalhes para registar o conector, conforme descrito na tabela. Quando tiver terminado, escolha **Afixar ao dashboard** > **Criar**.

   ![Detalhes do conector personalizado da Aplicação Lógica](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Propriedade | Valor sugerido | Descrição | 
   | -------- | --------------- | ----------- | 
   | **Nome** | *nome-do-conector-soap* | Indique um nome para o conector. | 
   | **Subscrição** | *nome-da-subscrição-do-Azure* | Selecione a sua subscrição do Azure. | 
   | **Grupo de recursos** | *nome-do-grupo-de-recursos-do-Azure* | Crie ou selecione um grupo do Azure para organizar os seus recursos do Azure. | 
   | **Localização** | *região-da-implementação* | Selecione uma região de implementação para o conector. | 
   |||| 

   Quando o Azure implementar o conector, o menu do conector do Logic Apps abre-se automaticamente. 
   Se não, escolha o conector SOAP a partir do dashboard do Azure.

## <a name="2-define-your-connector"></a>2. Definir o conector

Agora, especifique o ficheiro WSDL ou o URL para criar o conector, a autenticação que este utiliza e as ações e os acionadores que o conector SOAP fornece


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Indique o ficheiro WSDL ou o URL do conector

1. No menu do seu conector, se ainda não estiver selecionado, escolha **Conector do Logic Apps**. Na barra de ferramentas, escolha **Editar**.

   ![Editar conector personalizado](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. Escolha **Geral**, para indicar os detalhes nestas tabelas para criar, proteger e definir as ações e os acionadores para o conector SOAP.

   1. Em **Conectores personalizados**, selecione **SOAP** no **Ponto Final da API**, de modo a poder fornecer o ficheiro WSDL que descreve a sua API.

      ![Indicar o ficheiro WSDL da API](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Opção | Formato |Descrição | 
      | ------ | ------ | ----------- | 
      | **Carregar WSDL a partir do ficheiro** | *ficheiro-WSDL* | Navegue para a localização do ficheiro WSDL e selecione-o. | 
      | **Carregar o WSDL a partir do URL** | http://*caminho-para-o-ficheiro-wsdl* | Indique o URL do ficheiro WSDL do seu serviço. | 
      | **SOAP em REST** |   | Transforme as APIs no serviço SOAP em APIs REST. | 
      |||| 

   2. Para **Informações gerais**, carregue um ícone para o conector. 
   Normalmente, os campos **Descrição**, **Anfitrião** e **URL Base** são preenchidos automaticamente a partir do ficheiro WSDL. 
   Contudo, se isso não acontecer, adicione estas informações, conforme descrito na tabela, e escolha **Continuar**. 

      ![Detalhes do conector](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Opção ou definição | Formato | Descrição | 
      | ----------------- | ------ | ----------- | 
      | **Carregar Ícone** | *ficheiro-png-ou-jpg-com-menos-de-1-MB* | Um ícone que representa o conector <p>Cor: preferencialmente, um logótipo branco num fundo colorido. <p>Dimensões: logótipo com cerca de 160 pixels num quadrado com 230 pixels | 
      | **Cor de fundo do ícone** | *código-hexadecimal-da-cor-da-marca-do-ícone* | <p>A cor por trás do ícone que corresponde à cor de fundo no ficheiro do ícone. <p>Formato: hexadecimal. Por exemplo, #007ee5 representa a cor azul. | 
      | **Descrição** | *descrição-do-conector* | Indique uma descrição breve do conector. | 
      | **Anfitrião** | *anfitrião-do-conector* | Indique o domínio anfitrião do seu serviço SOAP. | 
      | **URL Base** | *URL-base-do-conector* | Indique o URL base do seu serviço SOAP. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Descrever a autenticação que o conector utiliza

1. Agora, escolha **Segurança**, para rever ou descrever a autenticação que o seu conector utiliza. A autenticação certifica-se de que as identidades dos seus utilizadores fluem adequadamente entre o seu serviço e os clientes.

   Por predefinição, o **Tipo de autenticação** do conector está definido como **Sem autenticação**.
   
   ![Tipo de autenticação](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Para alterar o tipo de autenticação, escolha **Editar**. Pode selecionar **Autenticação básica**. Para utilizar etiquetas de parâmetros que não sejam os valores predefinidos, atualize-as em **Etiqueta do parâmetro**.

   ![Autenticação básica](./media/logic-apps-soap-connector-create-register/security.png)

   
2. Para guardar o conector depois de introduzir as informações de segurança, na parte superior da página, escolha **Atualizar conector** e **Continuar**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Rever, atualizar ou definir ações e acionadores para o conector

1. Agora, escolha **Definição**, para rever, editar ou definir ações e acionadores novos que os utilizadores podem adicionar aos respetivos fluxos de trabalho.

   As ações e acionadores baseiam-se nas operações definidas no seu ficheiro WSDL, que preenchem automaticamente a página **Definição** e incluem os valores de pedido e resposta. Assim, se as operações necessárias já aparecerem aqui, pode avançar para o passo seguinte do processo de registo sem fazer alterações nesta página.

   ![Definição do conector](./media/logic-apps-soap-connector-create-register/definition.png)

2. Opcionalmente, se pretender editar ações existentes e acionadores ou adicionar novos, [continuar com estes passos](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Concluir a criação do conector

Quando estiver pronto, escolha **Atualizar Conector**, para poder implementá-lo. 

Parabéns! Agora, quando criar uma aplicação lógica, pode localizar o conector no Estruturador de Aplicações Lógicas e adicioná-lo à sua aplicação lógica.

![No Estruturador de Aplicações Lógicas, localize o seu conector](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Partilhar o seu conector com outros utilizadores do Logic Apps

Os conectores personalizados registados, mas não certificados, funcionam como os conectores geridos pela Microsoft, mas estão visíveis e disponíveis *apenas* para os respetivos autores e para os utilizadores que têm o mesmo inquilino do Azure Active Directory e a mesma subscrição do Azure para as aplicações lógicas na região onde estas estão implementadas. Embora a partilha seja opcional, poderá deparar-se com cenários em que queira partilhar os seus conectores com outros utilizadores. 

> [!IMPORTANT]
> Se partilhar um conector, as outras pessoas poderão começar a depender do mesmo. 
> ***A eliminação do conector elimina todas as ligações ao mesmo.***
 
Para partilhar o seu conector com utilizadores externos fora estes limites, por exemplo, com todos os utilizadores das Logic Apps, [submeter o seu conector para certificação Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>FAQ

**P:** O conector SOAP está disponível geralmente (GA)? </br>
**R:** O conector SOAP está em **Pré-visualização** e ainda não é um serviço em GA.

**P:** Existem restrições e problemas conhecidos no conector SOAP? </br>
**R:** Sim, veja [SOAP connector restrictions and known issue](../api-management/api-management-api-import-restrictions.md#wsdl) (Restrições e problemas conhecidos do conector SOAP).

**P:** Existem limites para os conectores personalizados? </br>
**R:** Sim, veja os [limites dos conectores personalizados aqui](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Obter suporte

* Para obter suporte com o desenvolvimento e a integração ou para pedir funcionalidades que não estão disponíveis no assistente de registo, contacte [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com). A Microsoft monitoriza esta conta quanto a perguntas e problemas com que os programadores se deparam e encaminha-as para a equipa adequada.

* Para fazer ou responder a perguntas ou para ver o que os outros utilizadores do Azure Logic Apps estão a fazer, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Para ajudar a melhorar o Logic Apps, vote ou submeta ideais no [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Passos Seguintes

* [Opcional: Confirme do seu conector](../logic-apps/custom-connector-submit-certification.md)
* [Custom connector FAQ](../logic-apps/custom-connector-faq.md) (FAQ dos conectores personalizados)