## YORB 2020 could use **your** help!  *Mine?*  Yes, yours!  

YORB 2020 is built in javascript and node.js from a number of different tools.

## Components:

* [THREE.js](https://threejs.org/) is a javascript library for 3D rendering. YORB uses three.js for all 3D rendering, player controls, and interactions between users and 3D objects.

* [Socket.io](https://socket.io/) is a javascript library for *"real-time, bidirectional and event-based communication."*  YORB uses socket.io for the following purposes:
    * Creating a multiplayer environment: each client uses sockets to share the following information: when they enter and leave the space and the position and rotation of their character in 3D space
      
    * Allowing audio/video communication by managing [Mediasoup signaling](https://mediasoup.org/documentation/v3/communication-between-client-and-server): each client uses sockets to set up communication between their [Mediasoup Device](https://mediasoup.org/documentation/v3/mediasoup-client/api/#Device) and the Mediasoup [Router](https://mediasoup.org/documentation/v3/mediasoup/api/#Router) on the server

* [Mediasoup](https://mediasoup.org/) is a *Selective Forwarding Unit* (SFU). This allows more participants to connect to one another than would be possible with direct peer-to-peer connections.  More specifically, *[an SFU](https://webrtcglossary.com/sfu/) is capable of receiving multiple media streams and then decide which of these media streams should be sent to which participants.*   Also, because Mediasoup is a C++ based SFU with node.js bindings and a javascript client side API, we are able to do all of our programming in Javascript and Node.js.

## Files:

**Server-side:**

* [server.js](/server.js): this file contains all of the server logic
* [config.js](/config.js): this file contains Mediasoup Router configuration settings

**Client-side (in `/src` directory):**

These files get built into a file called `bundle.js` which resides in the `/public` directory.

* [index.js](/src/index.js): this file contains all of the client side socket setup and Mediasoup signaling
* [scene.js](/src/scene.js): this file exposes the `Scene` class which contains all of the three.js scene logic (interaction in the 3D space)

## Local Developement Setup:


1. Clone or fork the repository and download a local copy:
    ```bash
    git clone https://github.com/AidanNelson/YORB2020.git
    ```
2. Navigate into the repository and install dependencies (note that it may take some time to build Mediasoup):
    ```bash
    cd YORB2020
    npm install
    ```
3. YORB relies on a secure (HTTPS) server, and as such requires that you set up certificates.  On MacOS, you can run the following commands to generate self-signed certificates.  These certificates will work for local development, but are not certified by a certificate authority and therefore will require you to make a security exception in your browser:
    ```bash
    # make a directory for the new certificates
    mkdir certs 
    openssl req  -nodes -new -x509  -keyout certs/privkey.pem -out certs/fullchain.pem
    ```
    
4. In [config.js](/config.js), set values for the `httpIp` and `webRtcTransport.listenIps` to match your IP on the local network.  For instance, if your IP on the local network is `192.168.1.145`, those values would look like this:

    ```
    httpIp: '192.168.1.145'  

    ...

    webRtcTransport: {
        listenIps: [
        { ip: '192.168.1.145', announcedIp: null } 
        ],
        ...
    }
    ```
    Note that it may be necessary to set `announdedIp` value to your remote IP as per [this mediasoup documentation](https://mediasoup.org/documentation/v3/mediasoup/api/#TransportListenIp).

5. Start the build system and node server with sudo:
    ```
    sudo npm start
    ```


## Hosting YORB on a Server:

* To do... fill out this documentation