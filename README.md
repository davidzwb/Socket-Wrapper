# Socket Wrapper
## Usage:

### TCP

for client:

```c++
InetAddress addr = InetAddress("127.0.0.1", 27777);
TcpStream tcpstream = TcpStream(addr, false); //false indicating client, which is active
if(!tcpstream.start())
{
  std::cout << "client start failed." << std::endl;
  return 0;
}

tcpstream.writeALL("Hello World!", strlen("Hello World!"));
```

for server:

```c++
InetAddress addr = InetAddress("", 27777);
TcpStream tcpstream = TcpStream(addr, true);//true indicating passive listending

if (!tcpstream.start())
{
  std::cout << "server start failed." << std::endl;
  return 0;
}

char readline[128];

while (1)
{
  ::memset(readline, 0, 128);

  TcpStream connstream = TcpStream();//create a new tcpstream to accept new connection

  if (!tcpstream.accept(connstream))
    return 0;

  int num = connstream.readSome(readline, 128);
  readline[num] = '\0';

  std::cout << readline << std::endl;
}
```

### UDP

for client:

```c++
InetAddress addr("127.0.0.1", 27777);

UdpDatagram udpdatagram(addr, false);

udpdatagram.start();

char hello[] = "Hello UDP!";

udpdatagram.send(hello, strlen(hello));
```

for server:

```c++
InetAddress addr("", 27777);

UdpDatagram udpdatagram(addr, true);

udpdatagram.start();

char buf[BUFSIZE];

int nr = udpdatagram.receive(buf, BUFSIZE);

if (nr < 0)
  return 0;

buf[nr] = '\0';

std::cout << "server: received: " << buf << std::endl;
```

