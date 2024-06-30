[[SignalR]]

```js
//подключение signalR на клиенте
<script src="https://cdnjs.cloudflare.com/ajax/libs/microsoft-signalr/7.0.9/signalr.min.js"></script>
<script>
// создание соединения на  /custom
let connetion = new signalR.HubConnectionBuilder().withUrl("/custom").build();
// обработка событий(до connection.start())
connetion.on("Recieve",(data) => console.log(data))
// отправка сообщений
const send = ()=>{
	connetion.send("send","my message")
}
 connetion.start().then(() => {
	console.log("connection succesfully")
})

```