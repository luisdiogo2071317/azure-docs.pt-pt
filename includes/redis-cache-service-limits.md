| Recurso | Limite |
| --- | --- |
| Tamanho da cache |530 GB |
| Bases de Dados |64 |
| Máx. de clients conectados |40,000 |
| Redis réplicas de Cache (para elevada disponibilidade) |1 |
| Partições horizontais numa cache premium com clustering |10 |

Limita a Cache de Redis do Azure e os tamanhos são diferentes para cada escalão de preço. Para ver os escalões de preço e seus tamanhos associados, consulte [preços do Cache de Redis do Azure](https://azure.microsoft.com/pricing/details/cache/).

Para obter mais informações sobre os limites de configuração de Cache de Redis do Azure, consulte [configuração do servidor predefinido Redis](../articles/redis-cache/cache-configure.md#default-redis-server-configuration).

Porque a configuração e gestão de instâncias de Cache de Redis do Azure é feito pela Microsoft, nem todos os comandos da Redis são suportados na Cache de Redis do Azure. Para obter mais informações, consulte [Redis comandos não suportados na Cache de Redis do Azure](../articles/redis-cache/cache-configure.md#redis-commands-not-supported-in-azure-redis-cache).

