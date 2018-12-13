---
title: Instalar pacotes num bloco de notas do Jupyter no Azure
description: Como instalar o Python, R, e F# pacotes de dentro de um bloco de notas do Jupyter em execução no Azure.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 6f089c12-128b-4dbd-96e3-1320d37eeba4
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: ed57cb458cc1f71c5d6fd2475c54cc712257dd84
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084877"
---
# <a name="install-packages-from-within-a-notebook"></a>Instalar pacotes a partir de um bloco de notas

Embora seja possível configurar o [ambiente para o bloco de notas no nível de projeto](configure-manage-azure-notebooks-projects.md#configure-the-project-environment), pode querer instalar pacotes diretamente dentro de um bloco de notas individual.

Pacotes instalados do bloco de notas aplicam-se apenas para a atual sessão do servidor. Instalações de pacotes não são mantidas depois do servidor está a ser encerrado.

## <a name="python"></a>Python

Pacotes em Python podem ser instalados através do pip ou conda utilizando os comandos dentro das células de código:

```bash
!pip install <package_name>

!conda install <package_name> -y
```

Se a saída do comando indica que o requisito é satisfeito já, em seguida, blocos de notas do Azure pode incluir o pacote por predefinição. O pacote também pode ser instalado através de um [o passo de configuração do ambiente de projeto](configure-manage-azure-notebooks-projects.md#configure-the-project-environment).

## <a name="r"></a>R

Pacotes de R podem ser instalados partir CRAN ou do Github com o `install.packages` função numa célula de código:

```r
install.packages("package_name")
```

Também pode instalar versões de pré-lançamento e de outros pacotes de desenvolvimento do Github com a biblioteca de devtools:

```r
options(unzip = 'internal')
library(devtools)
install_github('<user>/<repo>')
```

## <a name="f"></a>F#

Pacotes no F# pode ser instalada a partir [nuget.org](https://www.nuget.org) chamando a dependência de Paket manager a partir de dentro das células de código. Primeiro, carregue o Gestor de Paket:

```fsharp
#load "Paket.fsx"
```

Em seguida, instale pacotes:

```fsharp
Paket.Package
[ "MathNet.Numerics"
"MathNet.Numerics.FSharp"
]
```

## <a name="next-steps"></a>Passos Seguintes

- [Como: configurar e gerir projetos](configure-manage-azure-notebooks-projects.md)
- [Como: apresentar uma apresentação de diapositivos](present-jupyter-notebooks-slideshow.md)
