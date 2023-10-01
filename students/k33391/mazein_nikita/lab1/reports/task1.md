#Задача
Реализовать клиентскую и серверную часть приложения. Клиент отсылает серверу
сообщение «Hello, server». Сообщение должно отразиться на стороне сервера.
Сервер в ответ отсылает клиенту сообщение «Hello, client». Сообщение должно
отобразиться у клиента.

##Реализация
1. server.py

```python
import socket
import signal

# Create a UDP socket object
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(('localhost', 1337))
signal.signal(signal.SIGINT, signal.SIG_DFL)

while True:
    # Set timeout
    s.settimeout(60)

    try:
        # Receive data from client
        data, addr = s.recvfrom(1024)
        print('Received data from client:', data.decode('utf-8'))

        # Send data to client
        data = bytes('Hello, client!', 'utf-8')
        s.sendto(data, addr)
        print('Sent data to client:', data.decode('utf-8'))

    except socket.timeout:
        print('No data received from client after 60 seconds')
        break

    except KeyboardInterrupt:
        print("Server terminated by user.")
        break

# Close socket
s.close()
```

2. client.py

```python
import socket

# Create a UDP socket object
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.connect(('localhost', 1337))

# Send data to server
data = bytes('Hello, server!', 'utf-8')
s.send(data)
print('Sent data to server:', data.decode('utf-8'))

# Receive data from server
data = s.recv(1024)
print('Received data from server:', data.decode('utf-8'))
```

##Демонстрация работы
