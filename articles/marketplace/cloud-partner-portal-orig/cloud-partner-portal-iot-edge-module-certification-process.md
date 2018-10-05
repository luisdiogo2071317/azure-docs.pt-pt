---
title: Certificação de módulo do IoT Edge | Documentos da Microsoft
description: Certificar o módulo do IoT Edge para o Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: pbutlerm
ms.openlocfilehash: c11995a3db1987de7969ee476a1c69a1025be5fd
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810268"
---
# <a name="the-iot-edge-module-certification-process"></a>O processo de certificação do módulo do IoT Edge

Este artigo descreve os passos e requisitos para certificar um módulo do IoT Edge para publicação no Azure Marketplace. 

>[!Note]
>Este é um documento temporário. O mercado de módulo do IoT Edge está a ser criado em paralelo e este artigo irá ser substituído por documentação pública.

## <a name="understanding-an-iot-edge-module"></a>Noções básicas sobre o módulo do IoT Edge

Terminologia do módulo:

-   R **imagem módulo** é um pacote que contém o software que define um módulo.

-   R **instância de módulo** é a unidade específica de computação em execução a imagem de módulo num dispositivo IoT Edge. A instância de módulo é iniciada pelo runtime do IoT Edge.

Módulos podem também incluir o módulo de IoT SDK, que utiliza a seguinte terminologia:

-   R **identidade do módulo** é uma parte da informação (incluindo credenciais de segurança) armazenada num IoT Hub, que está associada a cada instância de módulo.

-   R **módulo duplo** são documentos JSON armazenados num IoT Hub, que contém informações de estado para uma instância de módulo, incluindo os metadados, configurações e condições.

-   **SDKs** são utilizados para desenvolver módulos personalizados em vários idiomas, tais como: C\#, C, Python, Java e node. js.

## <a name="the-onboarding-process-for-an-iot-edge-module"></a>O processo de integração de um módulo do IoT Edge

Captura de ecrã seguinte mostra o processo de um módulo do IoT Edge para se tornar público no Azure Marketplace.

![Processo de certificação](./media/cloud-partner-portal-iot-edge-module-certification-process/onboarding-process.png)

### <a name="onboarding-step-details"></a>Detalhes dos passos de integração

-   **Passo 1** -parceiros submeter as suas ofertas com o tipo de oferta **módulo do IoT Edge** na ferramenta de Cloud Partner Portal pela equipa do Azure Marketplace. Tem de ser um parceiro de MS oficial para acessar a ferramenta de Portal de parceiros da Cloud. Para obter mais informações, consulte a [Guia do publicador](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

-   **Passo 2** - o Marketplace executa automaticamente o software antivírus e antimalware verifica. A equipe de IoT pode executar os testes personalizados ingestão automatizados durante esta fase.

-   **Passo 3** -o módulo é público. É listada no marketplace e o contentor pode ser puxado anonimamente partir de uma URL. Por exemplo, *marketplace.azurecr.io/module-identifier*.

## <a name="iot-edge-module-certification-criteria"></a>Critérios de certificação de módulo do IoT Edge

Seguem-se os principais requisitos que têm de ser cumpridos para um módulo do IoT Edge para serem certificados e publicada no Azure Marketplace.

>[!Note]
>Estes requisitos podem ser alterados. Será notificado com antecedência e ser dada altura para atualizar os contentores, para que eles podem cumprir os requisitos de atualizado.

### <a name="technical-requirements"></a>Requisitos técnicos

**Ser um módulo do IoT Edge**

-   Apenas os contentores de docker-compatíveis atualmente são suportados como módulos do IoT Edge. O módulo tem de ser executado [Moby](https://mobyproject.org/). 

    >[!Note]
    >Contentores do docker irão provável que funcionem com Moby porque Moby é o projeto de código-fonte aberto subjacente para o Docker.

-   O parceiro tem de fornecer as seguintes predefinições: 

    -   Um padrão **marca** (que não pode estar vazio.

    -   Um padrão **createOptions** (que pode estar vazio.)

    -   Se utilizar o SDK de módulo do IoT, um padrão **duplo** (que pode estar vazio.)

    -   Se utilizar o SDK de módulo do IoT, um padrão **rotas** (que pode estar vazio.)

**Suporte de plataforma**

-   Apenas as plataformas que sejam **em disponibilidade geral** na camada 1 do IoT Edge (como registadas no [suporte do Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/support)) precisam de suporte. Por exemplo, isso atualmente significa que suporte as combinações de arquitetura e OS seguintes:

    -   Ubuntu Server 18.04 para x64

    -   Ubuntu Server 16.04 para x64

    -   Raspbian-stretch para arm32 (armhf)

**O dimensionamento do dispositivo**

-   Qualquer dispositivo com as dimensões equivalentes (CPU/RAM/armazenamento/GPU/e etc.) de um Raspberry Pi ou superior, pode ser um dispositivo IoT Edge. Se um módulo só funciona em restrições de dimensão específicos, essas restrições tem de ser especificadas na descrição do módulo.

**Predefinições**

-   Um módulo deve começar com os parâmetros de predefinição prontos a utilizar para cada plataforma suportada (SO + arquitetura).

-   Definições de configuração devem ser claramente documentadas (etiquetas, variáveis de ambiente, duplo, rotas.) Como parte da lista, os parceiros podem definir uma descrição para o módulo que oferece suporte a marcação HTML básica ou aponta para uma página da Web externa.

**Controlo de versões**

-   Os clientes tem de ser capazes de escolher se querem atualização automática de um módulo proveniente do marketplace ou se pretende utilizar uma versão exata tiver testado. Módulos de Marketplace têm de seguir o mesmo padrão de controlo de versões que o runtime do IoT Edge. Por exemplo:

    -   Sem interrupção marcas como "1.0", podem ser atualizados.

    -   Marcas de versão secundária, como "1.3.24", não podem ser atualizadas.

**Telemetria**

-   Módulos com o SDK do módulo do IoT tem de definir o identificador de módulo exclusivo atribuído pelo marketplace para fins de telemetria. Isto permite que o Azure Marketplace identificar o número de instâncias de módulo que estejam a executar. Este identificador exclusivo é o nome de contentor atribuído pelo marketplace na ingestão. Utilize os métodos dos SDKs seguintes para definir este identificador:
    - [C\#](https://hub.docker.com/_/mysql/)
    - [C](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Python](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/Iothub_sdk_options.md)
    - [Java](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.sdk.iot.device._product_info?view=azure-java-stable)

-   Para módulos que não utilizam o SDK de módulo do IoT, informações menos precisas estão disponíveis através do Portal de parceiro de Cloud. Por exemplo, o número de downloads.

**Segurança**

-   Contentores devem ter o acesso com privilégios mínimos para o host quanto possível. Os contentores "Privilegiados" devem ser extremamente raros.

-   Parceiros devem manter seu módulo proteger como parte do suporte que fornecem.

**Atualizações**

-   Parceiros devem manter seu módulo, atualizados e funcional. Eles vai ser notificados com antecedência se existirem quaisquer alterações de última hora planejadas para o Runtime do IoT Edge.

**SDK do módulo do IoT**

-   Incluindo o SDK de módulo do IoT, não é um pré-requisito para a certificação.
    No entanto, incluindo o SDK do módulo de IoT pode fornecer uma melhor experiência de utilizador. Por exemplo, para suportar o encaminhamento, enviar mensagens para a Cloud e assim por diante. O SDK do módulo de IoT é necessário para obter telemetria sobre o número de instâncias de módulo em execução.

**Requisito subjetivo**

-   Tem de cumprir mundo real, pelo menos, um caso de utilização do IoT Edge. Por exemplo, um contentor de WordPress não deve ser carregada, a menos que um cliente está disposto a utilizá-lo a partir do IoT Edge.

**Requisitos legais (da política de AMP)**

-   O módulo deve estar em conformidade com as políticas e contratos do Microsoft Azure Marketplace. Para obter mais informações, consulte o contrato de publicador anexados e [políticas de participação](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

-   Além da contratos do Azure Marketplace e as políticas, pode haver requisitos legais adicionais específicos para um tipo de oferta do módulo do IoT Edge. Isso está sendo revisão.

-   O módulo tem de ter uma *termos de utilização* e uma *política de privacidade*.

-   O módulo também deve ter um de terceiros aviso se o módulo usa qualquer linha de terceiros.

**Requisitos de suporte (da política de AMP)**

-   Os parceiros são responsáveis por suporte de seus módulos do IoT Edge. Eles serão Certifique-se de que as opções de suporte dadas na descrição do módulo do IoT Edge continuam disponíveis e que a opção de suporte, pelo menos, um sempre está disponível. (Consulte a secção 4 do contrato de publicador do AMP para obter mais detalhes).

**Categorização**

-   Todos os módulos do IoT Edge irão aparecer na categoria **Internet das coisas \>módulo do IoT Edge**. Cabe-lhe para o parceiro para escolher a melhor categoria secundárias para seus produtos.
