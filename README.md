# DTLS Examples for OpenSSL
This repository contains examples for DTLS UDP.
Each application in `src` can be used as client or server.

Our examples are developed against the OpenSSL 1.1.x API.

## UDP Examples
**DTLS Echo Server and Client**  
This sample includes a multi-threaded echo server and client sending messages over an SCTP/UDP connection encrypted with DTLS.

```
Usage: dtls_udp_echo [options] [address]
Options:
        -l      message length (default: 100 Bytes)
        -L      local address
        -p      port (default: 23232)
        -n      number of messages to send (default: 5)
        -v      verbose
        -V      very verbose
```

**DTLS Character Generator Server and Client**  
This sample includes a multi-threaded character generator server and client sending as many messages as possible to each other over an SCTP/UDP connection encrypted with DTLS for a given time.
Statistics how many messages have been sent and received and how many got lost are printed at the end.

```
Usage: dtls_udp_chargen [options] [address]
Options
        -l      message length (default: 100 Bytes)
        -L      local address
        -p      port (default: 23232)
        -t      time to send (default: 10 sec)
        -v      verbose
        -V      very verbose
```

**DTLS Discard Server and Client**  
This sample includes a multi-threaded discard server and client sending messages over an SCTP/UDP connection encrypted with DTLS.

```
Usage: dtls_udp_discard [options] [address]
Options:
        -l      message length (Default: 100 Bytes)
        -L      local address
        -p      port (default: 23232)
        -t      time to send (Default: 10 sec)
        -v      verbose
        -V      very verbose
```

## Build the Examples
Before calling `make` in the `src` directory, it may be necessary to specify custom *library* and *include* paths.
This is either done by modifying the *Makefile* or by providing the path as a command line argument.

```
$ make
```

## OpenSSL Certificates
In order to run the example programs, the required server and client certificates should be located in a `certs` subfolder.
* client-cert.pem
* client-key.pem
* server-cert.pem
* server-key.pem

The following commands create signed certificates for client and server of the samples above.
```
touch ca-db-index
echo 01 > ca-db-serial

# Certificate Authority
openssl req -nodes -x509 -newkey rsa:2048 -days 365 -keyout ca-key.pem -out ca-cert.pem

# Server Certificate
openssl req -nodes -new -newkey rsa:2048 -keyout server-key.pem -out server.csr

# Sign Server Certificate
openssl ca -config ca.conf -days 365 -in server.csr -out server-cert.pem

# Client Certificate
openssl req -nodes -new -newkey rsa:2048 -keyout client-key.pem -out client.csr

# Sign Client Certificate
openssl ca -config ca.conf -days 365 -in client.csr -out client-cert.pem
```

You can create your own [ca.conf](ca.conf) file or use a minimal sample.

## Usage with OpenSSL `s_client` / `s_server`
The examples are not limited to be used with each other, they may also be used with the built-in OpenSSL application.

The example below starts a DTLS echo server.
The client connects via OpenSSL's `s_client` application and sends input read from `stdin` to the server.
The server echos received messages.

```
$ dtls_udp_echo -V -L 127.0.0.1
```
```
$ openssl s_client -dtls -connect 127.0.0.1:23232
```
