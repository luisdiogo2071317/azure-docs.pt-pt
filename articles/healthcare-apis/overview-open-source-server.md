---
title: O que é o servidor de FHIR para o Azure - FHIR Server para o Azure
description: Este artigo descreve FHIR Server para o Azure.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.topic: overview
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 20b29ba4c23da90b4941b37d02a1d3f42310dd40
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823571"
---
# <a name="what-is-fhir-server-for-azure"></a>O que é o servidor de FHIR para o Azure

FHIR&reg; Server para o Azure é uma implementação de código-fonte aberto do emergentes [especificação HL7 rápida cuidados de saúde interoperabilidade recursos (FHIR)](https://www.hl7.org/fhir/) criada para a cloud da Microsoft. A especificação de FHIR define o estado de funcionamento como clinical dados podem ser feitos interoperáveis entre sistemas. Servidor FHIR para o Azure ajuda a facilitar a essa interoperabilidade na cloud. O objetivo deste projeto Microsoft Healthcare é permitir que os desenvolvedores implantem rapidamente o serviço FHIR.
 
Com os dados no formato FHIR, o servidor de FHIR para programadores do Azure permite ingerir e gerir conjuntos de dados FHIR na cloud, rapidamente controlar e gerir o acesso a dados e normalizar dados para cargas de trabalho do machine learning. Servidor de FHIR para o Azure está otimizado para o ecossistema do Azure: 
* Scripts e modelos ARM estão disponíveis para o aprovisionamento de imediato na Cloud da Microsoft 
* Scripts estão disponíveis para mapear para o Azure AAD e ativar o controlo de acesso baseado em funções (RBAC) 

Servidor FHIR para o Azure baseia-se com a separação lógica, permitindo que os desenvolvedores a flexibilidade para modificar como ele é implementado e estende seus recursos, conforme necessário. As camadas de lógica do servidor FHIR são: 

* Camada – de hospedagem oferece suporte de alojamento em ambientes diferentes, com uma configuração personalizada dos contentores de inversão de controle (IoC). 
* Camada API rESTful – a implementação de APIs definidas pela especificação HL7 FHIR. 
* Núcleos de camada de lógica – a implementação da lógica FHIR principal. 
* Camada de persistência – um provedor de persistência conectável, permitindo que o servidor de FHIR para ligar a praticamente qualquer utilitário de persistência de dados. FHIR Server para o Azure inclui um provedor de persistência de dados prontos a utilizar para o Azure Cosmos DB (um serviço de base de dados replicada globalmente que oferece consultas avançadas sobre os dados). 

Servidor FHIR para o Azure capacita os desenvolvedores – poupar tempo quando é necessário integrar rapidamente o servidor de FHIR às suas próprias aplicações ou fornecendo uma base no qual eles podem personalizar seu próprio serviço FHIR. Como um projeto de código-fonte aberto, contribuições e comentários da Comunidade de desenvolvedores FHIR continuará a melhorar este projeto. 

Privacidade e segurança são principais prioridades e o servidor de FHIR para o Azure foi desenvolvido para oferecer suporte a requisitos de informações de estado de funcionamento protegida (PHI). Todos os serviços do Azure utilizados no FHIR Server para o Azure [cumprem os requisitos de conformidade para Protected Health Information](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings).

Este projeto de código aberto totalmente é apoiado pela equipe do Microsoft Healthcare, mas nós sabemos que esse projeto apenas obterá ainda melhor com os seus comentários e contribuições. Podemos estão liderando os desenvolvimentos desta base de código e teste de compilações e implementações diariamente. 