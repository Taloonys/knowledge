![PR-14648_image1](image-storage/PR-14648_image1.png)

[сначала см. Модель OSI](resources/osi.md)
# link layer 
> Здесь решается, какая информация в каком виде куда хочет попасть
# internet layer
> Здесь решается кому отправка будет и идентифицируется получатель
# transport layer
> Решается способо и каким протоколом будет осуществлена отправка сообщений
# application layer
> Решается, как именно будет получаться дата через интернет, какими технологиями и как она предстанет перед пользователем

![Pasted image 20241026141341](image-storage/Pasted%20image%2020241026141341.png)
## protocols
![Pasted image 20241026142511](image-storage/Pasted%20image%2020241026142511.png)
### HTTP
> **Hypertext transfer protocol**, серверу не нужно хранить никаких состояний, вся информации в сообщении, в сообщении так же указывается код состояния, направленный одностороннй протокол соединения.
![Pasted image 20241026143036](image-storage/Pasted%20image%2020241026143036.png)

![Pasted image 20241026143148](image-storage/Pasted%20image%2020241026143148.png)

![Pasted image 20241026143206](image-storage/Pasted%20image%2020241026143206.png)

### Web sockets
> Протокол двунаправленного канала связи, позволяющий поддерживать постоянную непрерывную передачу данных.

### Email Related Protocols
* SMTP - Протокол для передачи сообщений между серверами
* IMAP - Протокол для изъятия сообщений с сервера, например, для нескольких устройств одного аккаунта
* POP3 - Протокол для выгрузки сообщений на клиент с сервера.
### File Transfer
* FTP - непосредственно передача файлов на сервер или выгрузка с него
* SSH - передача файлов по защищённому логином и паролем каналу
### Real-time communication
* WebRTC - протокол для передачи данных между браузерами/приложениями на разных устройствах, используется в видео общениях и file sharing
* MQTT - легковесный протокол передачи сообщений и телеметрии, чаще всего используется в IoT
* AMQP - протокол для передачи сообщений в контексте ПО, например, можно встретить в брокерах сообщений
