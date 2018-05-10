---
title: Cobertura do Azure Maps de composição | Microsoft Docs
description: Saiba mais sobre a cobertura de composição Maps do Azure
services: azure-maps
keywords: ''
author: jinzh-azureiot
ms.author: jinzh
ms.date: 03/07/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: ab05277c4541ae859f79b1108c4cf8a7beb29271
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="azure-maps-render-coverage"></a>Cobertura de composição de mapas do Azure

Mapas do Azure utiliza quadrícula mosaicos e vetor mosaicos para criar o maps. Na sua resolução mais baixa, o mundo inteiro encaixa num mosaico único. A sua resolução máxima, um único mosaico representa 38 medidores quadrados. Como ampliar num mapa, por conseguinte, pode ver cada vez mais mais detalhes sobre continentes, regiões, cidades e streets individuais. Para obter mais informações, consulte [níveis de Zoom e mosaico grelha](zoom-levels-and-tile-grid.md).

No entanto, o Maps não tem o mesmo nível de informações e a precisão para todas as regiões. As tabelas seguintes fornecem informações sobre o nível de detalhe composto pode esperar da cada região.

## <a name="legend"></a>Legenda

| Símbolo | Significado |
|--------|---------|
| ✓ | Região é representada com dados detalhados.   |
| Ø | Região é representada com dados simplificados. |


## <a name="africa"></a>África 


| Região | Mosaicos de quadrícula unificados | Mosaicos de vetor unificados |
| ------ | :------------------: | :------------------: |
| Argélia                          | ✓ | ✓ |
| Angola                           | ✓ | ✓ |
| Benim                            | ✓ | ✓ |
| Botsuana                         | ✓ | ✓ |
| Burkina Faso                     | ✓ | ✓ |
| Burundi                          | ✓ | ✓ |
| Cabo Verde                       |   | ✓ |
| Camarões                         | ✓ | ✓ |
| República Centro-Africana         |   | Ø |
| Chade                             |   | Ø |
| Comores                          |   | Ø |
| República do Congo                            | ✓ | ✓ |
| República Democrática do Congo | ✓ | ✓ |
| Costa do Marfim (Côte d’Ivoire)                    |   | Ø |
| Jibuti                         |   | Ø |
| Egito                            | ✓ | ✓ |
| Guiné Equatorial                |   | Ø |
| Eritreia                          |   | Ø |
| Etiópia                         |   | Ø |
| Gabão                            | ✓ | ✓ |
| Gâmbia                           |   | Ø |
| Gana                            | ✓ | ✓ |
| Guiné                           |   | Ø |
| Guiné-Bissau                    |   | Ø |
| Quénia                            | ✓ | ✓ |
| Lesoto                          | ✓ | ✓ |
| Libéria                          |   | Ø |
| Líbia                            |   | Ø |
| Madagáscar                       |   | Ø |
| Malavi                           | ✓ | ✓ |
| Mali                             | ✓ | ✓ |
| Mauritânia                       | ✓ | ✓ |
| Maurícia                        | ✓ | ✓ |
| Maiote                          | ✓ | ✓ |
| Marrocos                          | ✓ | ✓ |
| Moçambique                       | ✓ | ✓ |
| Namíbia                          | ✓ | ✓ |
| Níger                            | ✓ | ✓ |
| Nigéria                          | ✓ | ✓ |
| Reunião                          | ✓ | ✓ |
| Ruanda                           | ✓ | ✓ |
| Santa Helena, Ascensão e Tristão da Cunha |   | Ø |
| São Tomé e Príncipe            |   | Ø |
| Senegal                          | ✓ | ✓ |
| Serra Leoa                     |   | Ø |
| Somália                          |   | Ø |
| África do Sul                     | ✓ | ✓ |
| Sudão do Sul                      |   | Ø |
| Sudão                            |   | Ø |
| Suazilândia                        | ✓ | ✓ |
| Unidas República Tanzania      | ✓ | ✓ |
| Togo                             | ✓ | ✓ |
| Tunísia                          | ✓ | ✓ |
| Uganda                           | ✓ | ✓ |
| Zâmbia                           | ✓ | ✓ |
| Zimbabué                         | ✓ | ✓ |

## <a name="americas"></a>Américas

| Região | Mosaicos de quadrícula unificados | Mosaicos de vetor unificados |
| ------ | :------------------: | :------------------: |
| Anguila                  | ✓ | ✓ |
| Antígua e Barbuda       | ✓ | ✓ |
| Argentina                 | ✓ | ✓ |
| Aruba                     | ✓ | ✓ |
| Baamas                   | ✓ | ✓ |
| Barbados                  | ✓ | ✓ |
| Belize                    | ✓ | ✓ |
| Bermudas                   |   | ✓ |
| Estado de Plurinational de Bolivia |   | ✓ |
| Bonaire, Santo Eustáquio e Saba |   | ✓ |
| Brasil                    | ✓ | ✓ |
| Canadá                    | ✓ | ✓ |
| Ilhas Caimão            | ✓ | ✓ |
| Chile                     | ✓ | ✓ |
| Clipperton ilha         |   | ✓ |
| Colômbia                  | ✓ | ✓ |
| Costa Rica                |   | ✓ |
| Cuba                      | ✓ | ✓ |
| Curaçao                   | ✓ | ✓ |
| Domínica                  | ✓ | ✓ |
| República Dominicana        | ✓ | ✓ |
| Equador                   |   | ✓ |
| Ilhas Falkland (Malvinas) |   | ✓ |
| Guiana Francesa             | ✓ | ✓ |
| Gronelândia                 |   | Ø |
| Granada                   | ✓ | ✓ |
| Guadalupe                | ✓ | ✓ |
| Guatemala                 |   | ✓ |
| Guiana                    | ✓ | ✓ |
| Haiti                     | ✓ | ✓ |
| Honduras                  | ✓ | ✓ |
| Jamaica                   | ✓ | ✓ |
| Martinica                | ✓ | ✓ |
| México                    | ✓ | ✓ |
| Monserrate                | ✓ | ✓ |
| Nicarágua                 | ✓ | ✓ |
| Ilhas Marianas do Norte  |   | ✓ |
| Panamá                    | ✓ | ✓ | 
| Paraguai                  |   | ✓ |
| Peru                      | ✓ | ✓ |
| Porto Rico               | ✓ | ✓ |
| Quebec (Canadá)           |   | ✓ |
| São Bartolomeu          | ✓ | ✓ |
| São Cristóvão e Neves     | ✓ | ✓ |
| Santa Lúcia               | ✓ | ✓ |
| São Martin (francês)     | ✓ | ✓ |
| São Pedro e Miquelão |   | ✓ |
| São Vicente e Granadinas | ✓ | ✓ |
| Santo Maarten (Neerlandês)      | ✓ | ✓ |
| Ilhas Geórgia do Sul e Sandwich do Sul |   | ✓ |
| Suriname                  |   | ✓ |
| Trinidad e Tobago       | ✓ | ✓ |
| Ilhas Turcas e Caicos  | ✓ | ✓ |
| Estados Unidos             | ✓ | ✓ |
| Uruguai                   | ✓ | ✓ |
| Venezuela                 | ✓ | ✓ |
| Ilhas Virgens Britânicas   | ✓ | ✓ |
| Ilhas Virgens, E. U. A.      | ✓ | ✓ |

## <a name="asia"></a>Ásia 

| Região | Mosaicos de quadrícula unificados | Mosaicos de vetor unificados |
| ------ | :------------------: | :------------------: |
| Afeganistão               |   | Ø |
| Bahrain                   | ✓ | ✓ |
| Bangladeche                |   | Ø |
| Butão                    |   | Ø |
| Território Britânico do Oceano Índico |   | Ø |
| Brunei                    | ✓ | ✓ |
| Camboja                  |   | Ø |
| China                     |   | Ø |
| Ilhas dos Cocos (Keeling)   |   | Ø |
| República Democrática Popular da Coreia do Norte |   | Ø |
| Dokdo e Takeshima       |   | Ø |
| RAE de Hong Kong                 | ✓ | ✓ |
| Indonésia                 | ✓ | ✓ |
| Irão                      |   | Ø |
| Iraque                      | ✓ | ✓ |
| Israel                    |   | ✓ |
| Japão                     |   | Ø |
| Jordânia                    | ✓ | ✓ |
| Cazaquistão                |   | ✓ |
| Kuwait                    | ✓ | ✓ |
| Quirguizistão                |   | Ø |
| República Democrática Popular do Laos |   | Ø |
| Líbano                   | ✓ | ✓ |
| Macau                     | ✓ | ✓ |
| Malásia                  | ✓ | ✓ |
| Maldivas                  |   | Ø |
| Mongólia                  |   | Ø |
| Mianmar                   |   | Ø |
| Napal                     |   | Ø |
| Omã                      | ✓ | ✓ |
| Paquistão                  |   | Ø |
| Filipinas               | ✓ | ✓ |
| Catar                     | ✓ | ✓ |
| República da Coreia         | ✓ | Ø |
| Arábia Saudita              | ✓ | ✓ |
| Ilhas Senkaku           |   | ✓ |
| Singapura                 | ✓ | ✓|
| Sri Lanka                 |   | Ø |
| República Árabe Síria      |   | Ø |
| Taiwan                    | ✓ | ✓ |
| Tajiquistão                |   | Ø |
| Tailândia                  | ✓ | ✓ |
| Timor-Leste               |   | Ø |
| Turquemenistão              |   | Ø |
| Emirados Árabes Unidos      | ✓ | ✓ |
| Ilhas Menores Afastadas dos Estados Unidos |   | Ø |
| Usbequistão                |   | Ø |
| Vietname                   | ✓ | ✓ |
| Iémen                     | ✓ | ✓ |

## <a name="oceania"></a>Oceânia

| Região | Mosaicos de quadrícula unificados | Mosaicos de vetor unificados |
| ------ | :------------------: | :------------------: |
| Samoa Americana            |   | ✓ |
| Austrália                 | ✓ | ✓ |
| Ilhas Cook              |   | Ø |
| Fiji                      |   | Ø |
| Polinésia Francesa          |   | Ø |
| Guame                      | ✓ | ✓ |
| Quiribati                  |   | Ø |
| Ilhas Marshall          |   | Ø |
| Micronésia                |   | Ø |
| Nauru                     |   | Ø |
| Nova Caledónia             |   | Ø |
| Nova Zelândia               | ✓ | ✓ |
| Niue                      |   | Ø |
| Ilha Norfolk            |   | Ø |
| Ilhas Palaus                     |   | Ø |
| Papua-Nova Guiné          |   | Ø |
| Pitcairn                  |   | Ø |
| Estado Independente de Samoa                     |   | Ø |
| Ilhas Salomão           |   | Ø|
| Toquelau                   |   | Ø |
| Tonga                     |   | Ø |
| Tuvalu                    |   | Ø |
| Vanuatu                   |   | Ø |
| Wallis e Futuna         |   | Ø |


## <a name="europe"></a>Europa

| Região | Mosaicos de quadrícula unificados | Mosaicos de vetor unificados |
| ------ | :------------------: | :------------------: |
| Albânia                   | ✓ | ✓ |
| Andorra                   | ✓ | ✓ |
| Arménia                   |   | Ø |
| Áustria                   | ✓ | ✓ |
| Azerbaijão                |   | Ø |
| Bielorrússia                   | Ø | ✓ |
| Bélgica                   | ✓ | ✓ |
| Marco convertível        | ✓ | ✓ |
| Bulgária                  | ✓ | ✓ |
| Croácia                   | ✓ | ✓ |
| Chipre                    | ✓ | ✓ |
| República Checa            | ✓ | ✓ |
| Dinamarca                   | ✓ | ✓ |
| Estónia                   | ✓ | ✓ |
| Ilhas Faroé             |   | Ø |
| Finlândia                   | ✓ | ✓ |
| França                    | ✓ | ✓ |
| Geórgia                   |   | Ø |
| Alemanha                   | ✓ |✓ |
| Gibraltar                 | ✓ |   |
| Grécia                    | ✓ | ✓ |
| Guernsey                  |   | ✓ |
| Hungria                   | ✓ | ✓ |
| Islândia                   | ✓ | ✓ |
| Irlanda (República das)     | ✓ | ✓ |
| Ilha de Man               |   | ✓ |
| Itália                     | ✓ | ✓ |
| Jan Mayen                 |   | ✓ |
| Jersey                    |   | ✓ |
| Letónia                    | ✓ | ✓ |
| Liechtenstein             | ✓ | ✓ |
| Lituânia                 | ✓ | ✓ |
| Luxemburgo                | ✓ | ✓ |
| Macedónia                 | ✓ | ✓ |
| Malta                     | ✓ | ✓ |
| Moldova                   | ✓ | ✓ |
| Mónaco                    | ✓ | ✓ |
| Montenegro                | ✓ | ✓ |
| Países Baixos               | ✓ | ✓ |
| Noruega                    | ✓ | ✓ |
| Polónia                    | ✓ | ✓ |
| Portugal                  | ✓ | ✓ |
| Roménia                   | ✓ | ✓ |
| Federação Russa        | ✓ | ✓ |
| São Marino                | ✓ | ✓ |
| Sérvia                    | ✓ | ✓ |
| Eslováquia                  | ✓ | ✓ |
| Eslovénia                  | ✓ | ✓ |
| Kurils Austrais           |   | ✓ |
| Espanha                     | ✓ | ✓ |
| Svalbard                  |   | ✓ |
| Suécia                    | ✓ |   |
| Suíça               | ✓ | ✓ |
| Turquia                    | ✓ | ✓ |
| Ucrânia                   | ✓ | ✓ |
| Reino Unido            | ✓ | ✓ |
| Cidade do Vaticano              | ✓ | ✓ |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a composição de mapas do Azure, consulte [níveis de Zoom e mosaico grelha](zoom-levels-and-tile-grid.md).

Saiba mais sobre o [cobertura as áreas para os serviço de encaminhamento de Maps](routing-coverage.md). 