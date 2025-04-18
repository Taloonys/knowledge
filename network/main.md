* [Network base](resources/netbase.md)
* [Модель OSI](resources/osi.md)
* [Модель TCP/IP](resources/tcpip.md)
* [Entities & services](resources/network-entities.md)
* [Web общение](resources/web-communication.md)

**Источники**
* [https://aws.amazon.com/ru/what-is/api/](https://aws.amazon.com/ru/what-is/api/)
* [https://www.atlassian.com/ru/microservices/microservices-architecture](https://www.atlassian.com/ru/microservices/microservices-architecture)
# FAQ
## NAT vs Bridged vs Host only
![](resources/image-storage/Pasted%20image%2020250418125405.png)
### NAT (Network Address Translation)
> Механизм TCP/IP сетей, который подменяет ip локального пк на ip хост пк.
* Есть хост пк (это физический пк, с котором мы работаем)
* Есть виртуальные машины на этом ПК (например несколько операционок в VMware или VirtualBox)
* Выбран режим NAT сети
* Виртуалка обращается к сети -> запрос идёт к VMware, например
* VMware меняет: IP адресс виртуалки в загаловке запроса => IP адрес физической машины
* Запрос в интернет отправляется с IP хост пк в заголовке, не смотря на то, что запрос отправила виртуалка
* Вместе с этим во временной таблице сохраняется информация о том, кто кого заменил, поэтому входящий обратно запрос из внешней сети может "расшифроваться обратно"
> *Вообще это ещё частично решает проблему с тем, что кол-во IPv4 адрессов ограничено*
## Bridged
> Объединяет 2 устройства "абстрактным мостом", машины могут общаться теперь друг с другом + имеют доступ оба в интернет под своими собственными IP

![](resources/image-storage/Pasted%20image%2020250418125051.png)
# Host only
>  Дочернему ПК (или виртуалке) будет присвоен IP адресс, и доступ к этому адрессу будет только у хоста.