---
title: Através de perfis de versão de API com o Python na pilha do Azure | Microsoft Docs
description: Saiba como utilizar perfis de versão de API com o Python na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: mabrigg
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: a4fe62ba8c0732745326831b977e8975e1210436
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>Utilize perfis de versão de API com o Python na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

## <a name="python-samples-for-azure-stack"></a>Exemplos de Python para a pilha do Azure 

Pode utilizar os seguintes exemplos de código para executar tarefas comuns de gestão para máquinas virtuais na pilha do Azure.

Os exemplos de código mostram-lhe:

- Crie máquinas virtuais:
    - Criar uma Máquina Virtual do Linux
    - Criar uma máquina virtual do Windows
- Atualize uma máquina virtual:
    - Expanda uma unidade
    - Marcar uma máquina virtual
    - Anexe discos de dados
    - Desanexar discos de dados
- Operar uma máquina virtual:
    - Iniciar uma máquina virtual
    - Parar uma máquina virtual
    - Reiniciar uma máquina virtual
- Máquinas virtuais de lista
- Eliminar uma máquina virtual

Para rever o código para realizar estas operações, veja o **run_example()** função no script de Python **Hybrid/unmanaged-disks/example.py** no repositório GitHub [ virtual máquinas-python-gerir](https://github.com/viananth/virtual-machines-python-manage/tree/8643ed4bec62aae6fdb81518f68d835452872f88).

Cada operação é claramente etiquetada com um comentário e uma função de impressão.
Os exemplos não são necessariamente pela ordem apresentada na lista acima.


## <a name="run-the-python-sample"></a>Executar o exemplo de Python

1.  Se ainda não tiver, [instalar o Python](https://www.python.org/downloads/).

    Este exemplo (e o SDK) são compatível com o Python 2.7, 3.4, 3.5 e 3.6.

2.  Recomendações gerais para desenvolvimento do Python é utilizar um ambiente Virtual. 
    Para obter mais informações, consulte https://docs.python.org/3/tutorial/venv.html
    
    Instalar e inicializar o ambiente virtual com o módulo "venv" no Python 3 (tem de instalar [virtualenv](https://pypi.python.org/pypi/virtualenv) para Python 2.7):

    ````bash
    python -m venv mytestenv # Might be "python3" or "py -3.6" depending on your Python installation
    cd mytestenv
    source bin/activate      # Linux shell (Bash, ZSH, etc.) only
    ./scripts/activate       # PowerShell only
    ./scripts/activate.bat   # Windows CMD only
    ````

3.  Clone o repositório.

    ````bash
    git clone https://github.com/Azure-Samples/virtual-machines-python-manage.git
    ````

4.  Instale as dependências através do pip.

    ````bash
    cd virtual-machines-python-manage\Hybrid
    pip install -r requirements.txt
    ````

5.  Criar um [principal de serviço](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals) para trabalhar com a pilha do Azure. Certifique-se o principal de serviço tem [função de contribuinte/proprietário](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal) na sua subscrição.

6.  Defina as seguintes variáveis e exportar estas variáveis de ambiente para o shell atual. 

`Note: provide an explanation of where these variables come from?`

    ````bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  Tenha em atenção que para executar este exemplo, Ubuntu 16.04-LTS e imagens de Datacenter de R2 de 2012 do Windows tem de estar presentes na pilha do Azure Marketplace. Estes podem ser [transferido a partir do Azure](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-download-azure-marketplace-item) ou [adicionados ao repositório de imagens de plataforma](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-add-vm-image).


8. Execute o exemplo.

    ```
    python unmanaged-disks\example.py
    ```

## <a name="notes"></a>Notas

Poderá estar tempted para tentar obter o disco do SO da VM utilizando `virtual_machine.storage_profile.os_disk`.
Em alguns casos, isto pode fazer o que pretende, mas tenha em atenção de que proporciona um `OSDisk` objeto.
Para atualizar o tamanho do disco do SO, como `example.py` , não tem um `OSDisk` objeto, mas um `Disk` objeto.
`example.py` obtém o `Disk` objeto com o seguinte:

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## <a name="next-steps"></a>Passos Seguintes

- [O Centro de desenvolvimento do Python do Azure](https://azure.microsoft.com/develop/python/)
- [Documentação de Virtual Machines do Azure](https://azure.microsoft.com/services/virtual-machines/)
- [Percurso de aprendizagem para máquinas virtuais](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- Se não tiver uma subscrição do Microsoft Azure pode obter uma conta de avaliação gratuita [aqui](http://go.microsoft.com/fwlink/?LinkId=330212).
