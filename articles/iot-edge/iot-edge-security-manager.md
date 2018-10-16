---
title: O Gestor de segurança de IoT Edge do Azure | Documentos da Microsoft
description: Gerencia a postura de segurança de dispositivo do IoT Edge e a integridade dos serviços de segurança.
services: iot-edge
keywords: segurança, o elemento seguro, o enclave, TEE, IoT Edge
author: eustacea
manager: timlt
ms.author: eustacea
ms.date: 07/30/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: f0e548cdd1c59dc894899ddbac127dd76db7db26
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321191"
---
# <a name="azure-iot-edge-security-manager"></a>Gestor de segurança de IoT Edge do Azure

O Gestor de segurança do Azure IoT Edge é um principal de segurança bem vinculado para proteger o dispositivo IoT Edge e todos os seus componentes ao abstrair o hardware de silicon seguro. É o ponto focal para proteção de segurança e fornece o ponto de integração de tecnologia para fabricantes de dispositivos original (OEM).

![Gestor de segurança de IoT Edge do Azure](media/edge-security-manager/iot-edge-security-manager.png)

Gestor de segurança de IoT Edge tem como objetivo para defender a integridade do dispositivo IoT Edge e todas as operações de software inerente.  Ele faz isso fazendo a transição de confiança de raiz de hardware subjacente de hardware de confiança (se disponível) para inicializar o runtime do IoT Edge e continuar a monitorizar a integridade das suas operações de forma segura.  Essencialmente, o Gestor de segurança de IoT Edge é composto por software trabalhar em conjunto com o hardware de silicon segura onde estiver disponível e ativado para o ajudar a fornecer as garantias de segurança mais elevadas possíveis.  

As responsabilidades do Gestor de segurança de IoT Edge incluem (mas não limitado a):

* Protegidos e medidas de inicialização do dispositivo Azure IoT Edge.
* Provisionamento de identidade de dispositivo e fazer a transição de confiança quando aplicável.
* Alojar e proteger os componentes de dispositivo de serviços cloud como o serviço aprovisionamento de dispositivos.
* Aprovisione em segurança os módulos do IoT Edge com uma identidade exclusiva do módulo.
* Controlador de chamadas para a raiz de hardware do dispositivo de confiança através dos serviços de notary.
* Monitorize a integridade das operações de IoT Edge no tempo de execução.

Gestor de segurança de IoT Edge é composto por três componentes principais:

* Daemon de segurança de IoT Edge.
* Segurança módulo plataforma abstração de hardware (HSM PAL) de camada.
* Raiz de silicon opcional mas recomendado altamente hardware de HSM ou de confiança.

## <a name="the-iot-edge-security-daemon"></a>O daemon de segurança de IoT Edge

O daemon de segurança de IoT Edge é o software responsável pelas operações lógicas do Gestor de segurança de IoT Edge.  Ele é composto por uma parte significativa da base de computação confiável do dispositivo IoT Edge. 

### <a name="design-principles"></a>Princípios de conceção

Adequadamente e praticamente realizar suas responsabilidades, o daemon de segurança de IoT Edge tem como objetivo de estar em conformidade com os seguintes princípios de núcleos:

#### <a name="maximize-operational-integrity"></a>Maximizar a integridade operacional

O daemon de segurança de IoT Edge foi concebido para operar com a integridade mais alta possível dentro a capacidade de defesa de qualquer dado de raiz de hardware de confiança. Com a integração adequada, a raiz de hardware de confiança mede e monitoriza o daemon de segurança estaticamente e em tempo de execução para resistir a adulteração.

Acesso físico é sempre uma ameaça para dispositivos de IoT em geral.  Raiz de hardware de confiança, por conseguinte, desempenha um papel importante na defesa a integridade do daemon de segurança de IoT Edge.  Raiz de hardware de confiança vêm em duas versões:

* elementos seguros para a proteção de informações confidenciais, como chaves criptográficas e segredos.
* enclaves seguras para a proteção de segredos, como chaves e cargas de trabalho confidenciais, como a medição e faturação.

Existência desses dois modelos para utilizar a raiz de hardware de confiança dá origem a dois tipos de ambientes de execução:

* O ambiente de execução padrão ou avançado (REE) que se baseiam no uso de elementos seguros para proteger informações confidenciais.
* O ambiente de execução fidedigno (TEE) que se baseiam no uso da tecnologia de enclave segura para proteger informações confidenciais e oferecer proteção para a execução de software.

Para dispositivos com o enclaves seguros como raiz de hardware de confiança, a lógica confidencial dentro do daemon de segurança de IoT Edge é esperada que residem no enclave.  O daemon de segurança partes não sensíveis podem residir fora o TEE.  De qualquer maneira, espera-se de fabricantes de design original (ODM) e de fabricantes de equipamento original (OEM) para expandir a confiança do seu HSM para medir e defender-se a integridade do daemon de segurança de borda no arranque e de tempo de execução.

#### <a name="minimize-bloat-and-churn"></a>Minimizar o inchaço e alterações a dados

Outro princípio básico para o daemon de segurança de borda é minimizar o volume de alterações.  Para o nível mais alto de confiança, o daemon de segurança de IoT Edge pode rigidamente acoplar com a raiz de hardware do dispositivo de confiança que estiverem disponíveis e operar como código nativo.  É comum para estes tipos de realizations para atualizar o software de daemon por meio da raiz de hardware da atualização segura dos caminhos de fidedignidade (por oposição a mecanismos de atualização de SO fornecido), que pode ser um desafio, dependendo do cenário específico de hardware e de implementação.  Embora a renovação de segurança seja uma recomendação veemente para dispositivos de IoT, é razoável dizer que os requisitos de atualização excessiva ou payloads de atualizações de grandes dimensões podem expandir a superfície de ameaças de várias maneiras.  Os exemplos incluem a ignorar de atualizações para maximizar a disponibilidade operacional ou raiz de hardware de confiança restrita demasiado para processar cargas de atualizações de grandes dimensões.  Como tal, o design do daemon de segurança de IoT Edge é conciso para manter o volume e, por conseguinte, a pequena base de computação confiável e para minimizar os requisitos de atualização.

### <a name="architecture-of-iot-edge-security-daemon"></a>Arquitetura do daemon de segurança de IoT Edge

![Daemon de segurança do Azure IoT Edge](media/edge-security-manager/iot-edge-security-daemon.png)

O daemon de segurança de IoT Edge foi concebido para tirar partido de qualquer raiz de hardware disponível da tecnologia de confiança para proteção de segurança.  Mais, por isso, foi concebido para permitir a operação de divisão-mundo entre um ambiente de execução padrão/Rich (REE) e um ambiente de execução fidedigno (TEE) para tirar partido das tecnologias de hardware que oferecem a ambientes de execução fidedigno (TEE).  Núcleo para a arquitetura do daemon de segurança de IoT Edge é interfaces específicas de função para permitir a interação dos componentes principais do Edge, para garantir a integridade do IoT Edge, dispositivo e as respetivas operações.

#### <a name="cloud-interface"></a>Interface na cloud

A interface de nuvem permite que o daemon de segurança de IoT Edge aceder aos serviços cloud, por exemplo, com cumprimentos da cloud para segurança de dispositivo, como de renovação de segurança.  Por exemplo, o daemon de segurança de IoT Edge utiliza atualmente essa interface para acessar o IoT Hub do Azure [serviço de aprovisionamento de dispositivo (DPS)](https://docs.microsoft.com/azure/iot-dps/) para a gestão de ciclo de vida de identidade de dispositivo.  

#### <a name="management-api"></a>API de Gestão

O daemon de segurança de IoT Edge oferece uma API de gestão, que é chamado pelo agente do Edge quando criar/iniciar/parar/remover um módulo de borda. O daemon de segurança de IoT Edge armazena "registos" para todos os módulos do Active Directory. Estes registos de mapeiam a identidade de um módulo para algumas propriedades do módulo. Alguns exemplos para estas propriedades são o identificador de processo (pid) do processo em execução no contentor ou o hash do conteúdo do contentor de docker.

Estas propriedades são usadas pela carga de trabalho (descrito abaixo) para atestar que o chamador está autorizado a executar uma ação de API.

A API de gestão é uma API com privilégios, que pode ser chamada apenas a partir do agente do IoT Edge.  Uma vez que o daemon de segurança de IoT Edge inicializa e inicia o agente do IoT Edge, pode criar um registo implícito para o agente do IoT Edge, depois que ele tem atestado que o agente do IoT Edge não foi adulterado. O mesmo processo de atestado que utiliza a API de carga de trabalho é utilizado para restringir o acesso para a API de gestão para apenas o agente do IoT Edge.

#### <a name="container-api"></a>Contentor de API

O daemon de segurança de IoT Edge oferece a interface do contêiner para interagir com o sistema de contentores utilizado como Moby e a Docker para a instanciação do módulo.

#### <a name="workload-api"></a>API da carga de trabalho

A API de carga de trabalho é um daemon de segurança de IoT Edge API acessível para todos os módulos, incluindo o agente do IoT Edge. Ele fornece uma prova de identidade, na forma de um HSM Root o token assinado ou X509 certificado e a confiança correspondente do pacote para um módulo. O pacote de confiança contém certificados de AC para todos os outros servidores que os módulos devem confiar.

O daemon de segurança de IoT Edge utiliza um processo de atestado para proteger esta API. Quando um módulo de chama esta API, o daemon de segurança de IoT Edge tenta encontrar um registo para a identidade. Se tiver êxito, ele usa as propriedades de registo para medir o módulo. Se tiver iniciado o resultado das correspondências de processo de medição que Root o registro, um HSM novo token ou X509 certificado é gerado e o correspondente certificados de AC (pacote de confiança) são devolvidos para o módulo.  O módulo utiliza este certificado para ligar ao IoT Hub, outros módulos ou iniciar um servidor. Quando o token assinado ou o certificado está prestes a expirar, é da responsabilidade do módulo para pedir um novo certificado.  Considerações adicionais são incorporadas ao design para tornar o processo de renovação de identidade o mais direto possível.

### <a name="integration-and-maintenance"></a>Integração e manutenção

A Microsoft mantém o código principal base para o [daemon de segurança de IoT Edge no GitHub](https://github.com/Azure/iotedge/tree/master/edgelet).

#### <a name="installation-and-updates"></a>Instalação e atualizações

A ser um processo nativo, instalação e atualizações do daemon de segurança de IoT Edge é gerido através do sistema de gestão do pacote do sistema operacional.  No entanto, é esperado que IoT Edge dispositivos com raiz de hardware de confiança e fornecer proteção adicional para a integridade do daemon por meio do gerenciamento de ciclo de vida por meio dos dispositivos seguros arranque e atualizações de sistemas de gestão.  É deixado aos fabricantes de dispositivos para explorar esses recursos em conformidade com os seus recursos do respectivo dispositivo.

#### <a name="versioning"></a>Controlo de versões

O runtime do IoT Edge controla e reporta a versão do daemon de segurança de IoT Edge. A versão é reportada como o *runtime.platform.version* relatados de atributo do módulo de agente do Edge de propriedade.

### <a name="hardware-security-module-platform-abstraction-layer-hsm-pal"></a>Camada de abstração de plataforma de módulo de segurança do hardware (HSM PAL)

A PAL HSM abstrai todos os raiz de hardware de confiança para isolar o desenvolvedor ou do usuário do IoT Edge de suas complexidades.  Ele é composto por uma combinação de Interface de programação de aplicativo (API) e trans procedimentos de comunicações de domínio, por exemplo, comunicação entre um ambiente de execução padrão e um enclave seguro.  A implementação real da HSM PAL depende o hardware de segurança específico em utilização.  Sua existência permite o uso de praticamente qualquer hardware de silicon seguras em todo o ecossistema de IoT.

## <a name="secure-silicon-root-of-trust-hardware"></a>Raiz de silicon segura de hardware de confiança

Silicon segura é necessário âncora de confiança dentro do hardware do dispositivo IoT Edge.  Silicon seguro são fornecidos em várias para incluir o Trusted Platform Module (TPM), elemento seguro incorporado (eSE), ARM TrustZone, Intel SGX e tecnologias de silicon seguro personalizado.  A utilização de raiz de silicon segura de confiança nos dispositivos é altamente recomendável tendo em conta as ameaças associadas fisicamente a acessibilidade de dispositivos IoT.

## <a name="iot-edge-security-manager-integration-and-maintenance"></a>Integração de Gestor de segurança de IoT Edge e manutenção

Um dos maiores objetivos do Gestor de segurança de IoT Edge é a identidade e isolar os componentes a tarefa de defesa a segurança e a integridade da plataforma do Azure IoT Edge para proteção personalizada.  Como tal, espera-se de terceiros, como os fabricantes de dispositivos para que utilize dos recursos de segurança personalizadas disponíveis no seu hardware do dispositivo.  Veja a secção de passos seguinte abaixo em ligações de exemplo sobre como proteger o Gestor de segurança de IoT do Azure com o Trusted Platform Module (TPM) em plataformas Linux e Windows.  Estes exemplos usar o software ou virtual TPMs mas aplica-se diretamente ao uso de dispositivos TPM discretos.  

## <a name="next-steps"></a>Passos Seguintes

Leia o blog sobre [proteger um edge inteligente](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

Criar e aprovisionar um dispositivo de limite com um [TPM virtual numa máquina virtual Linux](how-to-auto-provision-simulated-device-linux.md).

Criar e aprovisionar um [dispositivo de periferia de TPM simulado no Windows](how-to-auto-provision-simulated-device-windows.md).

<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/
