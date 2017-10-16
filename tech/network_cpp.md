用于disables the synchronization between the C and C++ standard streams：

	ios_base::sync_with_stdio(false);

This unties cin from cout. Tied streams ensure that one stream is flushed automatically before each I/O operation on the other stream：

	cin.tie(NULL);

Structures for handling internet addresses:

	#include <netinet/in.h>

	struct sockaddr_in {
    	short            sin_family;   // e.g. AF_INET
    	unsigned short   sin_port;     // e.g. htons(3490)
    	struct in_addr   sin_addr;     // see struct in_addr, below
    	char             sin_zero[8];  // zero this if you want to
	};

	struct in_addr {
    	unsigned long s_addr;  // load with inet_aton()
	};

<sys/socket.h> makes available a type, socklen_t, which is an unsigned opaque integral type of length of at least 32 bits

	#include <sys/socket.h>

memset：void * memset ( void * ptr, int value, size_t num );

	char str[] = "almost every programmer should know memset!";
	memset (str,'-',6);

	str = "------ every programmer should know memset!"

