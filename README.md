1. How many data your publlsher program will send to the message broker in one run?

    Pada main.rs function main menunjukkan terdapat 5 message yang dikirimkan ke message broker. Yaitu pada kode berikut:
    ```rust
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "1" to_owned(), user_name: "2206810042-Amir".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "2" to_owned(), user_name: "2206810042-Budi".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "3" to_owned(), user_name: "2206810042-Cica".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "4" to_owned(), user_name: "2206810042-Dira".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "5" to_owned(), user_name: "2206810042-Emir".to_owned() });
    ```


2. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?

    Ya. keduanya merupakan url yang sama. Hal ini menunjukkan bahwa service publisher dan subscriber berjalan pada infrastruktur komunikasi yang sama. Dengan kata lain, service publisher dan subscriber berkomunikasi melalui message broker yang sama. Pada kasus ini, service publisher dan subscriber berkomunikasi melalui message broker RabbitMQ yang berjalan pada localhost:5672.

#### Rabbit MQ
![alt text](images/rabbitmq-overview.png)

#### Create communication between publisher and subscriber on the connection
![alt text](images/event-sent.png)

#### Message rates
![alt text](images/message-chart.png)

spike yang ada pada RabbitMQ management console menunjukkan bahwa message rate yang dikirimkan oleh publisher ke subscriber berjalan dengan baik. Rate awal hingga 4 message per second karena saya melakukan spam cargo run pada console publisher. Setelah itu, message rate berkurang menjadi 1 message per ketika console run dilakukan dalam rentang waktu yang lebih longgar.

Setiap kali publisher program berjalan, ia akan memberikan koneksi ke RabbitMQ melalui url `amqp://guest:guest@localhost:5672`. Kemudian, publisher melakukan publish_event. Flow yang terjadi adalah sebagai berikut:

1. *Connection Established*: Publisher akan membuat koneksi ke RabbitMQ melalui url `amqp://guest:guest@localhost:5672`.
2. *Message Published*: Publisher akan mengirimkan message ke RabbitMQ dengan memanggil fungsi `publish_event`.
3. *New Activity on RabbitMQ*: RabbitMQ akan menerima message yang dikirimkan oleh publisher.
4. *Message Delivered to Subscriber*: Subscriber akan menerima message yang dikirimkan oleh publisher.

Pada saat aktivitas terdeteksi oleh RabbitMQ, maka akan terjadi spike pada grafik message rate yang terdapat pada RabbitMQ management console.

#### Slow rate simulation
![alt text](images/slow-rate.png)

Terlihat bahwa peak activity menunjukkan angka 20, hal ini disebabkan saya menjalankan program publisher sebanyak 4 kali dimana setiap 1 program mengirimkan 5 buah message, sehingga total message yang dikirimkan adalah 20. 

Selain itu, terlihat bahwa message rate yang dikirimkan oleh publisher ke subscriber terhambat pada angka 1 message per second. Hal ini disebabkan oleh waktu yang dibutuhkan oleh publisher untuk mengirimkan message ke RabbitMQ membutuhkan ten milisecond.