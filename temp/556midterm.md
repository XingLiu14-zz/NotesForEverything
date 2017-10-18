## 556 Midterm

### DNS(Domain Name System)

Every host knows a local DNS server, every local DNS server knows the ROOT server  
Iterated DNS Query, Recursive DNS Query：把负载放到了中间的server  

NDS Resourse Records: DNS query has name & type
Resourse record is the response to a query: name & value & type & TTL

### Network Application

Reliable byte stream network communication service  
Big Endian: first byte sent/received is the most significant  
Bit ordering is handled by hardware  
htons(), htonl(), htobe64(), ntohs(), ntohl(), be64toh()

Socket API:  
client: socket->connect->send/recv->close  
server: socket->bind->listen->connect-send/recv->close  
How to support mutiple connection: Event-driven concurrency & multi-threading -> Maintain a pool of descriptors  
int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);

### Reliability

Packet are identified by sequence number, received packets send a ACK  
Stop and wait: need 1 sequence number  
Sliding window: if sender receive ACK of outside packet, ignore it. 每次ACK已经送过且没有gap的最新一位  
windows size最好占满整个rtt，需要2n个sequence number来表示一个n的window size  
Two dementional parity: 四个bit错误有可能检查不出  

Ethernet uses cyclic redundancy check(CRC); Internet protocols uses 1's complement sum:  
CRC: represent n-bit message as an n-1 degree polynomial, we want to send M(x), choose a divisor k-degree polynomial C(x), T(x) = M(x)*x^k - R(x) = A(x)*C(x)。sender把T(x)发过去，先把要传输的值乘C(x)的指数，再减去这个数除C(x)的余数，再检查可不可以被C(x)整除  
1‘s complement sum: 用反码来算减法

### Encoding and Framing

Non-Return to Zero (NRZ): when there's long sequence of 1 or 0, hard to do clock recovery  
Non-Return to Zero Inverted (NRZI): solve the long sequence of 1, not long sequence of 0  
Manchester: 1->high to low transition, 0->low to high transition; efficiency = 50%  
4bits/5bits: use 5 bits encode 4 bits make no 3 consecutive 0, use NRZI transimit, efficiency = 80%

Byte-Oriented Protocol (Sentinel Approach): STX(start of text), ETX(end of text), if ETX appears in the date, introduce a DLE(Data Link Escape) before it  
Bit-Oriented Protocol(HDLC: High-level Data Link Protocol): bit stuffing

### Broadcast network access control

Media Access Control Strategies: Channel pertition, Taking-turns, Contention

ALOHAnet: 有包就发，没收到ACK就重发。每个packet的脆弱时间是两个传输时间F。每个host发送概率遵从泊松分布，p是冲突概率，G是平均包传送率：G=入+pG；p=1-e^-(2GF). F入=ρ=Re^-(2R)=FGe^-(2R),当R=0.5时，ρmax=0.18

Ethernet：最小数据46bytes，总共64bytes。Network length <= (min_packet_size)*(propagation_speed)/(2*bandwidth)。Exponential backoff algorithms（冲突之后等待的时间） Carrier-sense multiple access/Collision Detect

RTS: request to send, CTS: clear to send.当你听到CTS，不要传，直到听到ACK

### Scaling ethernet

Ethernet switch called bridges, each bridge maintain a forwarding database with entries <MAC address, port, age>  
bridge只会记住packet从哪个port来的

Spanning tree:  
ConfigurationBridge Protocol Data Unit (BPDU), root bridge, designated bridge, (designated port) root port  
先比root ID, 然后是path cost，然后是自己的ID。B的root port和所有以B作为root bridge是通的，B只给所有高的port发消息

### IP

