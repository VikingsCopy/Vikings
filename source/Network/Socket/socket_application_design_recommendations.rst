==========================
Socket 应用设计建议
==========================

.. list-table:: Socket design applications
    :widths: 10 10 30
    :header-rows: 1
    
    * - Recommendation
      - Reason
      - Best used in
    * - Use asynchronous I/O
      - Asynchronous I/O used in a threaded server model is preferable over the more conventional select() model.
      - Socket server applications which handle numberous concurrent clients.
    * - When using asynchronous I/O, adjust the number of threads in the process to an optimum number for the number of clients to be processed.
      - If too few threads are defined then some clients might time out before being handled. If too many threads are defined then some system resource are not used efficiently. **Note:** It is better to have too many threads than too few threads.
      - Socket applications using asynchronous I/O.
    * - Design socket application to avoid the use of the pstflag on all start operations for asynchronous I/O.
      - Avoids the performance overhead of transition to a completion port if the operation has already been satisfied synchronously.
      - Socket applications using asynchronous I/O.
    * - Use send() and recv() over read() and write().
      - send() and recv() APIs provide a small performance and serviceability improvement over read() and write().
      - Any socket program that knows it uses a socket descriptor and not a file descriptor.
    * - Use the receive low water(SO_RCVLOWAT) socket option to avoid looping on a receive operation until all data has arrived.
      - Allows your application to wait for a minimum amount of data to be received on the socket before satisfying a blocked receive operation.
      - Any socket application that receives data
    * - Use the MSG_WAITALL flag to avoid looping on a receive operation until all data has arrived.
      - Allows your application to wait for the entire buffer provided on the receive operation to be received before satisfying a blocked receive operation.
      - Any socket application that receives data and knows in advance how much it expects to arrive.
    * - Use sendmsg() and recvmsg() over givedescriptor() and takedescriptor().
      - Portability, Communication of control information, performance, Pool of worker jobs Unknown worker job ID, Adaptive server design, Inactive worker job and Pass more than one descriptor at a time have more advantages.
      - Any socket application passing socket or file descriptors between processes.
    * - Use poll() over select().
      - The poll() API allows the application to pass an array of structures rather than an array of bits. Because each pollfd structure can contain up to 8 bytes, the application only needs to pass one structure for each descriptor, even if descriptor numbers are very large.
      - Any socket application using select() to poll the status of a socket.
    * - When using select(), try to avoid a large number of descriptors in the read, write or exception set. **Note:** If you have a large number of descriptors being used for select() processing see the asynchronous I/O recommendation above.
      - When there are a large number of descriptors in a read, write or exception set, considerable redundant work occurs each time select() is called. As soon as a select() is satisfied the actual socket function must still be performed. Asynchronous I/O APIs combine the notification that something has occurred on a socekt with the actual I/O operation.
      - Applications that have a large(>50) number of descriptors active for select().
    * - Save your copy of the read, write and exception sets before using select() to avoid rebuilding the sets for every time you must reissue the select().
      - This saves the overhead of rebuilding the read, write, or rebuilding the read, write, or exception sets every time you plan to issue the select().
      - Any socket application where you are using select() with a large number of socket descriptors enabled for read, write or exception processing.
    * - Do not use select() as a timer. Use sleep() instread. **Note:** If granularity of the sleep() timer is not adequate, you might need to use select() as a timer. In this case, set maximum descriptor to 0 and the read, write, and exception set to NULL.
      - Better timer response and less system overhead.
      - Any socket application where you are using select() just as a timer.
    * - If your socekt application has increased the maximum number of file and socket descriptors allowed per process using DosSetRelMaxFH() and you are using select() in this same application, be careful of the affect this new maximum value has on the size of the read, write and exception sets used for select() processing.
      - If you allocate a descrptor outside the range of read, write or exception set, as specified by FD_SETSIZE, then you can overwrite and destroy storage. Ensure your set sizes are at least large enough to handle whatever the maximum number of descriptors are set for the process and the maximum descriptor value specified on the select() API.
      - Any application or process where you use DosSetRelMaxFH() and select()
    * - Set all socket descriptors in the read or write sets to nonblocking. When a descriptor becomes enabled for read or write, loop and consume or send all of the data until EWOULDBLOCK is returned.
      - This allow you to minimize the number of select() calls when data is still available to be processed or read on a descriptor.
      - Any socket application where you are using select().
    * - Only specify the sets that you need to use for select() processing.
      - Most applications do not need to specify the exception set or write set.
      - Any socket application where you are using select().
    * - Use GSKit APIs instread of SSL_APIs.
      - Both the Global Security Kit(GSkit) and SSL_APIs allow you to develop secure AF_INET or AF_INET6, SOCK_STREAM socket applications. Because the GSKit APIs are supported across IBM systems, they are the preferred APIs to secure an application. The SSL_APIs exist only in the IBM i operating system.
      - Any socket application that nneds to be enabled for SSL or TLS processing.
    * - Avoid using signals.
      - The performance overhead of signal(on all platforms, not just the IBM i platform) is expensive. It is better to design your socket application to use Asynchronous I/O or select() APIs.
      - Any socket application that uses signals.
    * - Use protocol independent routines when available, such as inet_ntop(), inet_pton(), getaddrinfo(), getnameinfo().
      - Even if you are not yet ready to support IPv6, use these APIs,(instead of inet_ntoa(), inet_addr(), gethostbyname() and gethostbyaddr()) to prepare you for easier migration.
      - Any AF_INET or AF_INET6 application that uses network routines.
    * - Use sockaddr_storage to declare storage for any address family address.
      - Simplifies writing code portable across multiple address families and platforms. Declares enough storage to hold the largest address families and platforms. Declares enough storage to hold the largest address family and ensures the correct boundary alignment.
      - Any socket application that stores addresses.

