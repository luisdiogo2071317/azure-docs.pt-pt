---
title: Segurança no Azure IoT Edge | Documentos da Microsoft
description: Segurança, a autenticação e autorização de dispositivos do IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4292dc3abf775d48ee33cc7a48793d080156efb1
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568942"
---
# <a name="securing-azure-iot-edge"></a>Proteger o Azure IoT Edge

A proteção de um edge inteligente é necessário para a concessão de confiança na operação de uma solução de IoT ponto a ponto. O Azure IoT Edge foi concebido para segurança, que é extensível para perfis de risco diferentes, cenários de implantação e oferece a mesma proteção que se espera de todos os serviços do Azure.

O Azure IoT Edge é executado num hardware diferente, suporta o Linux e Windows e é aplicável para cenários de implementação diferentes.  Risco avaliado depende de muitos considerações, incluindo a propriedade de solução, área geográfica de implementação, confidencialidade de dados, privacidade, requisitos regulamentares e vertical do aplicativo.  Em vez de oferecer soluções concretas para cenários específicos, faz sentido criar uma estrutura de segurança extensível baseada nos princípios mantermos bem concebidos para dimensionamento. 
 
Este artigo fornece uma descrição geral da estrutura de segurança. Para obter mais informações, consulte [proteger um edge inteligente](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

## <a name="standards"></a>Padrões

Padrões de promovem a facilidade de exame detalhado e facilidade de implementação, que são a marca de segurança.  Uma solução de segurança de rede bem arquitetada deve é adaptado para exame detalhado em avaliação para criar a confiança e não deve ser um obstáculo para a implementação.  O design da estrutura para proteger o Azure IoT Edge emanates de comprovada e protocolos de segurança comprovada pelo setor tirar partido do conhecimento prévio e reutilização. 

## <a name="authentication"></a>Autenticação

É fundamental no desenvolvimento de confiança a saber, sem dúvida, quais actors, dispositivos e componentes estão a participar no fornecimento de valor por meio de uma solução de IoT ponto a ponto.  Esses dados de conhecimento oferece a responsabilidade de segura de participantes para ativar a base para a admissão.  O Azure IoT Edge atinge esse conhecimento através da autenticação.  O mecanismo primário para a autenticação para a plataforma do Azure IoT Edge é a autenticação baseada em certificados.  Esse mecanismo é derivada de um conjunto de normas que regem a infraestrutura de chave pública (PKiX) pelo IETF Internet Engineering Task Force ().     

A estrutura de segurança do Azure IoT Edge chama de identidades de certificado exclusivo para todos os dispositivos, módulos (contentores que encapsulam a lógica dentro do dispositivo) e atores interagir com o dispositivo Azure IoT Edge seja fisicamente ou através de uma ligação de rede.  Nem todo componente ou cenário poderá é adaptado para autenticação baseada em certificado para o qual a extensibilidade da estrutura de segurança oferece caminhos seguros para accommodation. 

## <a name="authorization"></a>Autorização

A capacidade de delegar a autoridade e controlar o acesso é crucial para alcançar um princípio fundamental da segurança – o princípio de privilégio mínimo.  Dispositivos, módulos e atores podem ganhar acesso apenas aos recursos e os dados dentro do respetivo âmbito de permissão e apenas quando é aceitável em termos de arquitetura.  Isso significa que algumas permissões são configuráveis com privilégios e outros impostos em termos de arquitetura suficientes.  Por exemplo, enquanto um módulo pode ser autorizado por meio da configuração com privilégios para iniciar uma ligação para o IoT Hub do Azure, não há nenhuma razão por que um módulo num dispositivo Azure IoT Edge deve acessar o duplo do módulo no outro dispositivo Azure IoT Edge.  Por esse motivo, a segunda opção seria possível em termos de arquitetura impedida. 

Outros esquemas de autorização incluem direitos de assinatura de certificado e o controlo de acesso baseado em funções (RBAC).  A extensibilidade da estrutura de segurança permite que a adoção de outros esquemas de autorização madura. 

## <a name="attestation"></a>Atestado

Atestado assegura a integridade dos bits de software.  É importante para a deteção e prevenção de software maligno.  A estrutura de segurança do Azure IoT Edge classifica atestado em três categorias principais:

* Atestado estático
* Atestado de tempo de execução
* Atestado de software

### <a name="static-attestation"></a>Atestado estático

Atestado estático é a verificação da integridade de todos os bits de software, incluindo os sistemas operativos, todos os tempos de execução, e informações de configuração no dispositivo. o power-up.  Ele é frequentemente referido como o arranque seguro.  A estrutura de segurança para dispositivos do Azure IoT Edge expande para fornecedores de silício e incorpore os recursos de hardware de segurança incluída a fim de garantir os processos de atestado estático. Esses processos incluem a inicialização segura e firmware seguro processos de atualização.  Trabalhar em colaboração fechar com fornecedores de silício elimina as camadas de firmware supérfluo assim minimizar a superfície de ameaças. 

### <a name="runtime-attestation"></a>Atestado de tempo de execução

Depois de um sistema concluiu um processo de inicialização validados e está a funcionar e em execução, bem estruturados sistemas seguros detectaria tenta injetar o software maligno através de interfaces e portas de sistemas e tomar as contramedidas adequadas.  Para dispositivos de um edge inteligente no custódia físico de atores maliciosos, é possível inserir malcontent através de meios que não a interfaces de dispositivo, como a adulteração e ataques de canal de lado.   Tal malcontent, que pode ser na forma de software maligno ou alterações de configuração não autorizado, normalmente não seria detetado pelo processo de arranque seguro porque eles ocorrerem após o processo de inicialização.  Medidas preventivas oferecidos ou imposto pelo hardware do dispositivo bastante contribui para a evitando essas ameaças.  A estrutura de segurança do Azure IoT Edge explicitamente chama para extensões para combatting ameaças de tempo de execução.     

### <a name="software-attestation"></a>Atestado de software

Todos os sistemas de bom estado de funcionamento, incluindo sistemas de um edge inteligente tem de ser abertos a patches e atualizações.  A segurança é importante para os processos de atualização caso contrário, que eles podem ser potenciais vetores de ameaças.  A estrutura de segurança do Azure IoT Edge chamadas por atualizações medido e assinado pacotes para garantir a integridade e autenticar-se a origem dos pacotes.  Isto é aplicável a todos os sistemas operativos e bits de software de aplicação. 

## <a name="hardware-root-of-trust"></a>Raiz de hardware de confiança

Para muitas implementações de dispositivos de um edge inteligente, especialmente aqueles implantados em lugares em que os atores maliciosos potenciais têm acesso físico ao dispositivo, a segurança oferecida pelo hardware do dispositivo de é o último defesa para proteção.  Por esse motivo, a ancoragem de confiança no hardware resistente a adulterações é mais crucial para implementações deste tipo.  A estrutura de segurança do Azure IoT Edge envolve a colaboração segura silicon os fornecedores de hardware para oferecer diferentes tipos de raiz de hardware de confiança para acomodar vários perfis de risco e cenários de implementação. Estes incluem o uso de silicon segura a cumprir os padrões de protocolo de segurança comuns, como o Trusted Platform Module (ISO/IEC 11889) e Device identificador Composition Engine (DICE da Trusted Computing Group).  Estas instruções também incluem tecnologias de enclave seguro como TrustZones e extensões de proteção de Software (SGX). 

## <a name="certification"></a>Certificação

Para ajudar a tomar decisões informadas ao adquirir dispositivos Azure IoT Edge para a implantação de clientes, a estrutura de Azure IoT Edge inclui os requisitos de certificação.  Certificações relacionadas à segurança afirmações e certificações relativas a validação da implementação de segurança, são fundamentais para esses requisitos.  Por exemplo, uma certificação de afirmação da segurança informa que o dispositivo do IoT Edge usa hardware de segurança conhecido para resistir a ataques de arranque. Uma certificação de validação informa que o hardware de segurança foi implementado corretamente para oferecer esse valor no dispositivo.  Para manter o princípio de simplicidade, o framework oferece a visão de manter a carga de certificação mínima.   

## <a name="extensibility"></a>Extensibilidade

A extensibilidade é um cidadão de primeira classe, a estrutura de segurança do Azure IoT Edge.  Com a tecnologia de IoT impulsionar diferentes tipos de transformações de negócios, é razoável dizer que segurança deve evoluir de forma totalmente integrada ao mesmo tempo para endereço emergentes de cenários.  A estrutura de segurança do Azure IoT Edge é iniciado com uma base sólida na qual ele se baseia na extensibilidade em dimensões diferentes para incluir: 

* Primeira serviços de segurança de terceiros como o serviço de aprovisionamento de dispositivos para o IoT Hub do Azure.
* Serviços de terceiros, como serviços de segurança gerida para mercados verticais de aplicação diferente (como industriais ou cuidados de saúde) e do foco de tecnologia (como a segurança de monitorização na malha redes ou serviços de atestado de hardware de silicon) através de uma rede avançada de parceiros.
* Sistemas herdados para incluir retroajustar com estratégias de segurança alternativas, gosto de usar tecnologia segura que não sejam certificados para gestão de identidades e autenticação.
* Proteger o hardware para a adoção totalmente integrada de tecnologias emergentes de hardware de segurança e silicon contribuições de parceiro.

Esses são apenas alguns exemplos de dimensões de extensibilidade e a estrutura de segurança do Azure IoT Edge foi concebida para ser seguro desde o início para suportar essa extensibilidade. 

No final, o maior sucesso na proteção de um edge inteligente resulta da colaboração contribuições de uma Comunidade de open orientado pelo interesse comuns na proteção de IoT.  Nessas contribuições poderão ser na forma de tecnologias seguras ou serviços.  A estrutura de segurança do Azure IoT Edge oferece uma base sólida de segurança que é extensível para a máxima cobertura oferecer o mesmo nível de confiança e a integridade de um edge inteligente como com a cloud do Azure.  

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como é o Azure IoT Edge [proteger um edge inteligente](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).
