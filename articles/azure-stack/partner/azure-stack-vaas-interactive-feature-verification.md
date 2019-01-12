---
title: Verificação de recurso interativo de teste na validação de pilha do Azure como um serviço | Documentos da Microsoft
description: Saiba como criar testes de verificação de recurso interativo para o Azure Stack com validação como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/07/2019
ms.author: mabrigg
ms.reviewer: johnhas
ROBOTS: NOINDEX
ms.openlocfilehash: 5b20e170388aa6e9b73b7c5c26a286ec51b52209
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2019
ms.locfileid: "54245842"
---
# <a name="interactive-feature-verification-testing"></a>Teste de verificação de recurso interativo  

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Pode utilizar a estrutura de testes de verificação de recurso interativo para pedir testes para o seu sistema. Quando solicita um teste, a Microsoft utiliza o framework para preparar os testes que requerem passos manuais de interativos. Microsoft pode utilizar a estrutura em conjunto encadear vários testes de autónomo automatizado.

Este artigo descreve um cenário simples de manual. O teste verifica a substituir um disco no Azure Stack. O framework recolhe registos de diagnóstico em cada passo. Pode depurar problemas à medida que os encontrar. A estrutura também permite a partilha dos registos produzidos por outras ferramentas ou processos e permite-lhe fornecer comentários sobre o cenário.

## <a name="overview-of-a-test-pass"></a>Descrição geral de uma aprovação de teste

Um teste para a substituição do disco é um cenário comum. Neste exemplo, o teste tem sete etapas:

1.  Criar uma nova **aprovação de teste** fluxo de trabalho.
2.  Selecione o **teste de identificação de disco**.
3.  Inicie o teste.
4.  Escolha as ações no cenário de verificação interativo.
5.  Verifique o resultado do cenário.
6.  Envie o resultado do teste para a Microsoft.
7.  Verificar o estado no portal do VaaS.

## <a name="create-a-new-test-pass"></a>Criar uma nova passagem de teste

1.  Navegue para o [portal do Azure Stack validação](https://www.azurestackvalidation.com) e iniciar sessão.

2.  Criar uma nova solução ou escolha um já existente.

3.  Selecione **começar** sobre o **aprovação de teste** mosaico.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image1.png)

4.  Introduza um nome para o **aprovação de teste** fluxo de trabalho.

5.  Introduza o ambiente e os parâmetros comuns de teste ao seguir as instruções no [parâmetros comuns do fluxo de trabalho para a validação de pilha do Azure como um serviço](azure-stack-vaas-parameters.md) artigo.

6.  A cadeia de ligação de diagnóstico tem de seguir o formato especificado no [parâmetros de teste](azure-stack-vaas-parameters.md#test-parameters) secção a [parâmetros comuns do fluxo de trabalho](azure-stack-vaas-parameters.md) artigo.

7.  Introduza os parâmetros necessários para o teste.

8.  Selecione o agente para executar a execução de teste interativa.

> [!Note]  
> Utilizador de administrador de domínio e palavra-passe tem de ser especificados para o teste de verificação de recurso interativo de identificação de disco.

![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image2.png)

## <a name="select-the-test"></a>Selecione o teste

1.  Selecione **teste de identificação de disco\<versão >**.

    > [!Note]  
    > A versão do teste irá incrementar quando são feitas melhorias para os acessórios de teste. Sempre deve ser utilizada a versão mais recente, a menos que a Microsoft contrária.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image4.png)

2.  Fornecer o utilizador de administrador de domínio e palavra-passe, selecionando **editar**.

3.  Selecione o teste apropriado execução agente/DVM para iniciar o teste.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image5.png)

4.  Selecione **submeter** para iniciar o teste.

![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image6.png)

## <a name="start-the-test"></a>Iniciar o teste

Mostram as instruções de teste de identificação de disco no computador que executa o agente de VaaS. Normalmente, isso é o DVM ou a Jumpbox para a instância do Azure Stack.

![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image8.png)

## <a name="choose-the-actions"></a>Escolha as ações

1.  Siga os **documentação** e **validação** links para rever as instruções da Microsoft sobre como efetuar este cenário.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image9.png)

2.  Selecione **Seguinte**.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image10.png)

3.  Siga as instruções para executar o script precheck.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image11.png)

4.  Depois do script precheck é concluído com êxito, execute o cenário manual (substituição do disco), de acordo a **documentação** e **validação** liga-se do **informações**separador.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image12.png)

5.  Não feche a caixa de diálogo enquanto estiver a executar o cenário manual.

6.  Quando tiver terminado de executar o cenário manual, siga as instruções para executar o script de verificação de post.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image13.png)

7.  A conclusão com êxito do cenário manual (substituição do disco), selecione **seguinte**.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image14.png)

> [!Important]  
> Se fechar a janela, o teste irá parar antes de terminar.

## <a name="check-the-status"></a>Verificar o Estado

1.  Quando o teste estiver concluído, será solicitado para fornecer comentários.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image15.png)

2.  Estas perguntas ajudará a avaliar a qualidade de taxa de e a versão de sucesso do cenário de Microsoft.

## <a name="send-the-test-result"></a>Enviar o resultado do teste

1.  Anexe quaisquer ficheiros de registo que pretende enviar à Microsoft.

    ![Texto alternativo](media/azure-stack-vaas-interactive-feature-verification/image16.png)

2.  Aceite o EULA de envio de comentários.

3.  Selecione **submeter** para enviar os resultados à Microsoft.

## <a name="next-steps"></a>Passos Seguintes

- [Monitorizar e gerir os testes no portal do VaaS](azure-stack-vaas-monitor-test.md)
