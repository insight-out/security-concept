*****
Login
*****

.. mermaid::

    sequenceDiagram;
        autonumber
        Client->>+Server: authenticate(email, password)
        Server-->>-Client: cookie, meta, encryptedSecret, accessToken
        Client->>+KeyServer: getKey(cookie with sessionID, accessToken)
        KeyServer->>+Server: validate(certificate, accessToken, sessionID)
        Server-->>-KeyServer: userID
        KeyServer-->>-Client: encryptedPrivateKey
        Client->>Client: decrypt(encryptedKey, encryptedSecret)

1. regular authentication request
2. send the ``encryptedSecret`` and ``userToken`` from DB
3. fetch key from KeyServer, the cookie is part of the HTTPS request and contains the sessionID, the ``userToken`` is also send
4. KeyServer must check if the sessionID and userToken are valid. 
    1. The test.box has to check the certificate of the KeyServer, so nobody except our KeyServer can use this endpoint
    2. Plausibility checks regarding the sessionID and userToken
5. test.box will return some sort of success message
6. KeyServer can now lookup the enryptedPrivateKey via the userToken
7. Client can decrypt the key by
    1. ``secret = d(encryptedSecret, password)``
    2. ``privateKey = d(encryptedPrivateKey, password)``

Some content
============

More content
------------

Und nun?