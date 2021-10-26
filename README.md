# mproxy

mproxy is a lightweight http proxy server implementation. It is created by [@examplecode](https://github.com/examplecode), and now maintained by [@ArchLinuxStudio](https://github.com/ArchLinuxStudio).

- Only supports linux system.
- Support http proxy.
- Support http tunnel proxy.
- Simple obfuscation http tunnel proxy through XOR.
- Support chained connection.

## HTTP proxy mode

mproxy can work in a normal HTTP proxy mode, only forward the request, which cannot be used for censorship circumvention.

```bash
./mproxy -l 8000 -d
```

Note: The "-d" parameter indicates that the program serves as a deamon service to prevent the terminal from exiting the program.

This way of working is of little significance to us, it is only a function display, and the focus is on the following.

## HTTP proxy tunnel mode with obfuscation(censorship-circumvention mode)

This working mode needs to be used in conjunction with the mproxy-client and mproxy-server, that is, mproxy acts as the client and server respectively, and forms a simple encrypted tunnel between the client and the server, thereby avoiding the detection of GFW. The specific working mode is shown in the figure below:

 <pre>   
   +----------+        +-----------+       +----------+      +----------+
   |          |        |           |       |          |      |          |
   |          |        |           |       |          |      |          |
   |  APP     +------->| mproxy    |+------> mproxy   +------> Web      |
   |          |        |           |       |          |      |          |
   |          |        | client    |       | server   |      |          |
   +----------+        +-----------+       +----------+      +----------+
</pre>

The deployment steps are as follows:

### step1 : Start mproxy on the remote server as a remote proxy

```bash
./mproxy  -l 8081 -D -d
```

-D Specify to accept data for decryption, and its corresponding parameter'-E' is applied to the local agent

### step2 : Start mproxy locally as a local proxy, and specify the transmission method to encrypt

Start a mporxy locally and specify the server address and port number deployed remotely in the previous step.

```bash
./mproxy  -l 8080 -h xxx.xxx.xxx.xxx:8081 -E
```

-l Specify the local listening port
-h Specify the server address and port number of the remote next hop. If you need censorship circumvention, you need to designate a server that has not been censorship.
-E Encrypt when sending data

## Star History

[![Stargazers over time](https://starchart.cc/ArchLinuxStudio/mproxy.svg)](https://starchart.cc/ArchLinuxStudio/mproxy)

## Ref

1. https://blog.csdn.net/starboybenben/article/details/49803315
2. https://blog.csdn.net/JenaeLi/article/details/73292559
3. https://imququ.com/post/the-proxy-connection-header-in-http-request.html
