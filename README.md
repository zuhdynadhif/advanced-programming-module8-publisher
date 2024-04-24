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