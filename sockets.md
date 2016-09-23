# Sockets and Suspension

When working with sockets on iOS you need to take some extra care to app and device suspension. 

This document assumes knowledge of iOS and low-level sockets

App suspension
---
This normally happens when your app is put into the background unless you ask for background execution time.

When the application is suspended the operating system stores received data in the small buffers as usual until the application is ready to receive again. This goes for both connected TCP sockets and TCP sockets listening for connections.

When the application is revived from suspension it can read available data and accept pending connections. Mind that there might be an amount of data lost if the buffers gets filled up.

It might be a good idea to let the remote side know that you will not be able to process messages for a while to prevent data loss. 

System suspension
---
The system puts itself in a suspended state when you lock the screen of your device and nothing is running in the background.

When the system is suspended sockets and their buffers will be closed and freed.

When your app wakes up again the next `read()` and `accept()` will with return -1 and you will need to re-connect/bind.