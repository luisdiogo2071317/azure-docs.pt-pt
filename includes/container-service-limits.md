| Recurso | Limite Predefinido |
| --- | :--- |
| Número máximo de nós por cluster | 100 |
| Número máximo de pods por nó ([redes básicas com o Kubenet][basic-networking]) | 110 |
| Número máximo de pods por nó ([redes avançadas com o Azure CNI][advanced-networking]) | 30<sup>1</sup> |
| Número máximo de clusters por subscrição | 100 |

<sup>1</sup> Este valor pode ser personalizado através da implementação do modelo ARM. Veja exemplos [aqui][arm-deployment-example].<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[arm-deployment-example]: https://github.com/Azure/AKS/blob/master/examples/vnet/02-aks-custom-vnet.json#L64-L69
