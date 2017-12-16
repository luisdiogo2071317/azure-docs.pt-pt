
GPU com otimização de VM são especializadas máquinas de virtuais disponíveis com único ou vários GPUs NVIDIA. Estes tamanhos foram concebidos para cargas de trabalho de computação intensiva, intensivas de gráficos e visualização. Este artigo fornece informações sobre o número e tipo de GPUs, vCPUs, discos de dados e NICs, bem como armazenamento débito e a rede de largura de banda para cada tamanho neste agrupamento. 

* **NC, NCv2 e ND** tamanhos estão otimizados para aplicações intensivas de computação e rede intensivas e algoritmos, incluindo aplicações baseadas em CUDA e OpenCL e simulações, AI e Learning profunda. 
* **NV** tamanhos são otimizados e foi concebidos para visualização remota, transmissão em fluxo, jogos, codificação e cenários VDI através das estruturas, tais como tendo sido igualmente e DirectX.  


## <a name="nc-instances"></a>Instâncias NC

As instâncias de NC são com tecnologia de [NVIDIA Tesla K80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) cartão. Os utilizadores podem crunch através de dados mais rápidos através do aproveitamento CUDA para aplicações de exploração de energia, simulações de falhas, ray rastreada composição, learning avançada e muito mais. A configuração de NC24r fornece uma baixa latência, a interface de rede de alto débito otimizado para paralelo fortemente conjugado cargas de trabalho de computação.


| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | GPU | Discos de dados máximos |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 24 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 |
| Standard_NC24 |24 |224 | 1440 | 4 | 64 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 64 |

1 GPU = metade de uma placa K80.

*Com capacidade RDMA

## <a name="ncv2-instances"></a>Instâncias de NCv2

Instâncias de NCv2 são a próxima geração das máquinas de série de NC, com tecnologia [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPUs. Estes GPUs podem fornecer 2x mais do que o desempenho de capacidade de cálculo da série de NC atual. Os clientes podem tirar partido destes GPUs atualizados para tradicionais cargas de trabalho HPC, tais como a modelação de reservatório, DNA sequenciação, protein análise, simulações Monte Carlo e outras pessoas. Como a série de NC, a série NCv2 oferece uma configuração com uma latência baixa, a interface de rede de alto débito otimizado para paralelo fortemente conjugado cargas de trabalho de computação.

> [!IMPORTANT]
> Para esta família de tamanho, a quota de vCPU (principal) na sua subscrição inicialmente está definida para 0 em cada região. [Pedir um aumento de quota vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) para esta família num [região disponível](https://azure.microsoft.com/regions/services/).
>

| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | GPU | Discos de dados máximos |
| --- | --- | --- | --- | --- | --- |
| Standard_NC6_v2 |6 |112 | 336 | 1 | 12 |
| Standard_NC12_v2 |12 |224 | 672 | 2 | 24 |
| Standard_NC24_v2 |24 |448 | 1344 | 4 | 32 |
| Standard_NC24r_v2 * |24 |1448 | 1344 | 4 | 32 |

1 GPU = um cartão de P100.

*Com capacidade RDMA

## <a name="nd-instances"></a>Instâncias de ND

As máquinas virtuais de série ND são uma nova adição para a família GPU concebida para cargas de trabalho AI e aprendizagem profunda. Oferecem um excelente desempenho para formação e inferência. Instâncias de ND são utiliza a tecnologia de [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPUs. Estas instâncias fornecem excelente desempenho de precisão simples flutuante ponto operações, para cargas de trabalho de AI saber como utilizar o Toolkit de cognitivos, TensorFlow, Caffe e outras estruturas. A série ND também oferece um tamanho de memória GPU muito maior (24 GB), que permite adaptar modelos de rede neuronal muito maiores. Como a série de NC, a série ND oferece uma configuração com uma rede de latência baixa e alta débito secundária através de RDMA e a conectividade de InfiniBand para poder executar tarefas de formação em grande escala GPUs muitos de expansão.

> [!IMPORTANT]
> Para esta família de tamanho, a quota de vCPU (principal) por região na sua subscrição inicialmente está definida para 0. [Pedir um aumento de quota vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) para esta família num [região disponível](https://azure.microsoft.com/regions/services/).
>

| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | GPU | Discos de dados máximos |
| --- | --- | --- | --- | --- | --- |
| Standard_ND6 |6 |112 | 336 | 1 | 12 |
| Standard_ND12 |12 |224 | 672 | 2 | 24 |
| Standard_ND24 |24 |448 | 1344 | 4 | 32 |
| Standard_ND24r * |24 |1448 | 1344 | 4 | 32 |

1 GPU = um cartão de P40.

*Com capacidade RDMA

## <a name="nv-instances"></a>Instâncias NV



As instâncias de NV são utiliza a tecnologia de [NVIDIA Tesla M60 ](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPUs e NVIDIA grelha a tecnologia de ambiente de trabalho acelerados aplicações e ambientes de trabalho virtuais onde os clientes são capazes de visualizar os seus dados ou simulações. Os utilizadores têm capacidade para visualizar os seus fluxos de trabalho intensivas de gráficos nas instâncias do NV para obter a capacidade de gráficos superior e adicionalmente executar única precisão cargas de trabalho, tais como a codificação e composição. 

| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | GPU | Discos de dados máximos |
| --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 24 |
| Standard_NV12 |12 |112 |680 | 2 | 48 |
| Standard_NV24 |24 |224 |1440 | 4 | 64 |

1 GPU = metade de uma placa M60.


