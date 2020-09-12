---
layout: single
classes: wide
title: Websocket
excerpt: "What I have learned from websocket."
date: 2020-09-12
tags: [about]
---

# Websocket

## What is websocket?

![f4390d12c641c468dcfb743dc64ae7cb.png](/assets/images/fde9d0e2ee684942b04f6a719fc94e3b.png)  
Websocket is a TCP commmunation protocol that provides communication between a server and a client to transfer data   real-time. It also supports Cross Origin Communication which means it allows communication between different servers.

### Example usages of a websocket

1.  Communication between a game client and a website to update leaderboards.
2.  Chatting function
3.  Event feed
4.  Notifications

## How a websocket request and its response looks like.

**HTTP Request**

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Origin: http://example.com
Sec-WebSocket-Protocol: chat
Sec-WebSocket-Version: 13
```

**HTTP Response**

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
Sec-WebSocket-Protocol: chat
```

With the headers presented at the request and the response.

1.  `Upgrade: websocket` means that we are telling the server to upgrade our connection to websocket.
2.  `Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==` is used to tell the server that the following request is a valid websocket handshake request coming from a websocket client which means it is used to identify whether its coming from a websocket client not from non-websocket clients like HTTP.
3.  `Sec-WebSocket-Protocol: chat` is used to tell the server that we are going to use a structure(subbprotocol) whether the server & client will support it.
4.  `Sec-WebSocket-Version: 13` tells what websocket version we are using.
5.  `Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=` is used to tell that the handshake request was accepted.
6.  `Origin: http://example.com` tells the server that the upcoming request is from `http://example.com`.

## Security issue with Websocket
Due to the fact of the intentional behaviour of websocket, Same-Origin-Policy can't be applied because of that it is possible for a attacker to make a opening handshake request using his/her website to a endpoint that uses websocket if the server doesn't check whether the `Origin` of the request is coming from a trusted-source combined with sending this malicious website to a victim behalf of them we can access his/her information. This attack is what we called `Cross-site WebSocket hijacking`.

### Cross-site WebSocket hijacking
> Cross-site WebSocket hijacking (also known as cross-origin WebSocket hijacking) involves a cross-site request forgery (CSRF) vulnerability on a WebSocket handshake. It arises when the WebSocket handshake request relies solely on HTTP cookies for session handling and does not contain any CSRF tokens or other unpredictable values. - Portswigger

It means that an attacker can make a malicious page/website that sends a opening handshake request to a websocket endpoint, if the attacker was able to send the link of the malicious page/website to a victim that uses a vulnerable website and is currently logged on to that vulnerable website, the attacker can gain information or do actions behalf of the victim. This happens because the server is not properly validating the `Origin` header and it only relies on `session cookie` for session handling.
```
GET /chat HTTP/1.1
Host: normal-website.com
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: wDqumtseNBJdhkihL6PW7w==
Connection: keep-alive, Upgrade
Cookie: session=KOsEJNuflw4Rd9BDNrVmvwBF9rEijeE2
Upgrade: websocket
```


## Mitigation
The best way to mitigate this is to have a proper validation on the `Origin` header and a unique token generated for the request.
