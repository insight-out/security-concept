***********************
Asymmetric Key Exchange
***********************

.. _processes-create-keypair:

Create keypair
---------------

.. mermaid::

    sequenceDiagram;
        autonumber
        Client->>Client: createKeys()
        Client->>Client: encryptKey(privateKey, password)
        Client->>+Server: putPublicKey(publicKey)
        Server-->>-Client: success
        Client->>+Server: getAccessToken()
        Server-->>-Client: accessToken
        Client->>+KeyServer: putPrivateKey(accessToken, userID, encryptedPrivateKey, iv, salt)
        KeyServer->>+Server: validateUser(userID, accessToken)
        Server-->>-KeyServer: valid
        KeyServer-->>-Client: success

1. createKeys
2. encryptKeys
3. the public key is stored on the server
4. obligatory success status
5. request accessToken, see :ref:`API <api-server-accesstoken>`
6. create and send accessToken
7. store encryptedPrivateKey on the KeyServer along with initialization vector, salt and userID, see :ref:`API <api-keyserver-newuser>`
8. validation of accessToken, see :ref:`API <api-server-validate>`
9. obligatory success status
10. obligatory success status

.. _processes-get-privkey:

Get private key
---------------

.. mermaid::

    sequenceDiagram;
        autonumber
        Client->>+Server: getAccessToken()
        Server-->>-Client: (accessToken)
        Client->>+KeyServer: getEncryptedPrivateKey(userID, accessToken)
        KeyServer->>+Server: validateUser(userID, accessToken)
        Server->>-KeyServer: valid
        KeyServer-->>-Client: (encryptedPrivateKey, iv, salt)

1. request new accessToken, see :ref:`API <api-server-accesstoken>`
2. create and send accessToken
3. request private key from KeyServer, see :ref:`API <api-keyserver-getkey>`
4. validate accessToken, see :ref:`API <api-server-validate>`
5. obligatory success status
6. return encryptedPrivateKey, salt and iv