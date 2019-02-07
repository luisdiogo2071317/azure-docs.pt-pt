---
title: O que é a API do Azure para pré-visualização de FHIR - API do Azure para pré-visualização do FHIR
description: Este artigo descreve a API do Azure para FHIR.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: overview
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 327c28b70c564f7e5099ada14a0d4653b7f0db6c
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823558"
---
# <a name="what-is-azure-api-for-fhirreg-preview"></a>O que é a API do Azure para FHIR&reg; pré-visualização?
API do Azure para FHIR permite a troca rápida de dados através de APIs de FHIR, apoiada por um plataforma como um serviço gerida (PaaS) na cloud. Isso torna mais fácil para todos aqueles que trabalham com dados de estado de funcionamento para ingerir, gerir e manter as informações de estado de funcionamento protegida [PHI](https://www.hhs.gov/answers/hipaa/what-is-phi/index.html) na cloud: 

- Serviço FHIR, aprovisionado na cloud em minutos gerido 
- Nível empresarial, com base em FHIR® o ponto de extremidade no Azure para aceder aos dados e armazenamento no formato de FHIR®
- Elevado desempenho, baixa latência
- Gestão segura de dados de estado de funcionamento protegida (PHI) num ambiente de cloud em conformidade
- SMART no FHIR para dispositivos móveis e implementações de web
- Controlar os dados em escala com controlo de acesso baseado em funções (RBAC)
- Registo de controlo de acesso, a criação, modificação e leituras dentro de cada arquivo de dados de auditoria

A API do Azure para FHIR permite-lhe criar e implementar um serviço FHIR em apenas alguns minutos para tirar partido do dimensionamento flexível da cloud.  Paga apenas o débito e armazenamento de que precisa. Os serviços do Azure que impulsionam o API do Azure para FHIR destinam-se para obter um desempenho rápido, não importa o que estiver a gerir os conjuntos de dados do tamanho.  A camada de persistência de dados na API do Azure para FHIR tira partido do Azure Cosmos DB, que garante latências no percentil de 99 e garante uma elevada disponibilidade com capacidades multi-homing. 

A API de FHIR e o arquivo de dados em conformidade permitem-lhe ligar e interagir com qualquer sistema que utiliza FHIR APIs com segurança.  A Microsoft leva em operações, manutenção, atualizações e os requisitos de conformidade na oferta de PaaS, pelo que pode liberar seus próprios recursos de desenvolvimento e operacionais. 

## <a name="leveraging-the-power-of-your-data-with-fhir"></a>**Tirar partido do poder dos seus dados com FHIR**
O setor de cuidados de saúde está a transformar rapidamente dados de estado de funcionamento para o padrão de emergente de [FHIR&reg; ](https://hl7.org/fhir) (rapidamente recursos de interoperabilidade de cuidados de saúde). FHIR permite um modelo de dados robusta e extensível com o exchange padronizada de semântica e os dados que permite que todos os sistemas usando FHIR trabalhem em conjunto.  Transformar os seus dados para FHIR permite-lhe ligar a origens de dados existentes, como os sistemas de registos médicos Eletrónicos ou pesquisar bases de dados rapidamente. FHIR também permite a troca rápida de dados em implementações modernas do mobile e o desenvolvimento de web. Mais importante, FHIR pode simplificar a ingestão de dados e acelerar o desenvolvimento com ferramentas de aprendizagem automática e análise.  

### <a name="securely-manage-health-data-in-the-cloud"></a>**Gerir de forma segura a dados de estado de funcionamento na cloud**
A API do Azure para FHIR permite a troca de dados por meio de consistente, RESTful, APIs de FHIR com base na especificação HL7 FHIR. Sustentado por um PaaS geridos oferta no Azure, também fornece um ambiente escalável e seguro para a gestão e o armazenamento de dados de informações de estado de funcionamento protegida (PHI) em formato FHIR nativo.  

### <a name="free-up-your-resources-to-innovate"></a>**Libertar os recursos para inovar**
Poderiam investir recursos de criação e execução de seu próprio serviço FHIR, mas com a API do Azure para FHIR, a Microsoft leva na carga de trabalho de operações, manutenção, atualizações e os requisitos de conformidade, permitindo que a liberar seus próprios operacionais e desenvolvimento recursos.

### <a name="enable-interoperability-with-fhir"></a>**Permitir a interoperabilidade com FHIR** 
Usando a API do Azure para FHIR permite a se conectar com qualquer sistema que tira partido das APIs de FHIR para leitura, escrita, pesquisa e outras funções.  Pode ser utilizado como uma ferramenta poderosa para consolidar, normalizar e aplicar a aprendizagem automática com dados clínicos desde registos médicos Eletrónicos, clinician e dashboards dos pacientes, programas de monitorização remotos, ou com bases de dados fora do seu sistema com APIs de FHIR.

### <a name="control-data-access-at-scale"></a>**Controlar o acesso de dados em escala**
Controle os seus dados. Controlo de acesso baseado em funções (RBAC) permite-lhe gerir a forma como os seus dados são armazenados e acedidos.  Fornecendo aumentou a segurança e reduzir a carga de trabalho administrativa, o que determina quem tem acesso para os conjuntos de dados que criar, com base nas definições de função que criou para o seu ambiente.  

### <a name="audit-logs-and-tracking"></a>**Registos de auditoria e controlo**
Rapidamente controle onde será seus dados com registos de auditoria incorporadas. Controlar o acesso, a criação, modificação e leituras dentro de cada arquivo de dados.

### <a name="secure-your-data"></a>**Proteger os seus dados** 
Protege a PHI com inteligência de segurança sem igual.  Os dados para uma base de dados exclusivo por instância de API em ambiente isolados e protegidos com ativação pós-falha de várias regiões. A API do Azure para FHIR implementa uma defesa em camadas e aprofundada e a proteção avançada contra ameaças para os seus dados.  

## <a name="applications-for-a-fhir-service"></a>**Aplicações para um serviço FHIR**
Os servidores FHIR são as principais ferramentas de interoperabilidade de dados de estado de funcionamento.  A API do Azure para FHIR foi concebida enquanto uma API e o serviço que pode criar, implementar e começar a utilizar rapidamente.  Como o padrão FHIR expande-se nos cuidados de saúde, casos de utilização continuará a crescer, mas alguns aplicativos de cliente inicial em que a API do Azure para FHIR é útil estão abaixo: 

- **Desenvolvimento de aplicações e inicialização/IOT:**  Os clientes a desenvolver uma aplicação direcionada ao paciente ou fornecedor (móvel ou web) pode tirar partido de API do Azure para FHIR como um serviço de back-end totalmente gerido. A API do Azure para FHIR fornece um recurso valioso em que os clientes podem gerir dados e troca de dados num ambiente de cloud segura foi concebido para os dados de estado de funcionamento, tirar partido do SMART nas diretrizes de implementação de FHIR e ativar a sua tecnologia a ser utilizado por todos sistemas de fornecedor (por exemplo, a maioria dos EHRs tem ativado o FHIR ler APIs).   
- **Ecossistemas de cuidados de saúde:**  Embora existam EHRs como principal "fonte verdadeira", em muitas configurações clínicos, não é incomum para fornecedores de ter várias bases de dados que não estão ligadas entre si ou armazenam dados em diferentes formatos.  Utilizar a API do Azure para FHIR como um serviço que se aproveita esses sistemas permite padronizar os dados no formato FHIR.  Isto ajuda a habilitar a troca de dados em vários sistemas com um formato de dados consistente. 

- **Pesquisa:** Pesquisadores de cuidados de saúde encontrará o padrão FHIR em geral e a API do Azure para FHIR útil pois normaliza os dados em torno de um modelo de dados FHIR comum e reduz a carga de trabalho de machine learning e a partilha de dados.
Troca de dados através da API do Azure para FHIR fornece registos de auditoria e acesso controla esse controle ajuda o fluxo de dados e quem tem acesso a quais tipos de dados. 

## <a name="fhir-from-microsoft"></a>FHIR da Microsoft

Capacidades FHIR da Microsoft estão disponíveis em duas configurações:

* A API do Azure para FHIR – PaaS de uma oferta no Azure, facilmente aprovisionado no portal do Azure e gerida pela Microsoft.
* Servidor de FHIR para o Azure – um projeto de código-fonte aberto que pode ser implementado na sua subscrição do Azure, disponível no GitHub em https://github.com/Microsoft/fhir-server.

Para utilização casos que requer estender ou personalizar o servidor FHIR ou necessitar de aceder aos serviços subjacentes — por exemplo, a base de dados — sem passar através das APIs de FHIR, os desenvolvedores devem escolher o servidor de FHIR de código-fonte aberto para o Azure.   Para implementação de uma API de FHIR de chave na mão, prontos para produção e o serviço de back-end onde os dados persistentes só devem ser acessados por meio da API de FHIR, os desenvolvedores devem escolher a API do Azure para FHIR

## <a name="get-started"></a>Introdução

Para começar a trabalhar com o serviço de FHIR, siga o guia de introdução de 5 minutos:

* Implementar com o servidor de FHIR de origem aberto [PowerShell](fhir-oss-powershell-quickstart.md)

## <a name="next-steps"></a>Passos Seguintes

Depois de configurar o serviço FHIR, siga os passos importantes para configurar e testar o serviço:

* [Serviço de acesso a FHIR com o Postman](access-fhir-postman-tutorial.md)

FHIR é a marca registada da HL7 e é utilizado com a permissão da HL7.
