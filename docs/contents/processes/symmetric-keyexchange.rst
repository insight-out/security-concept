**********************
Symmetric Key Exchange
**********************

.. _processes-create-secret:

Create secret
-------------

.. mermaid::

    sequenceDiagram;
        autonumber
        opt facility creation
        Client->>Client: createSecret()
        end
        Client->>+Server: getPublicKeyForUser(userID)
        Server-->>-Client: (publicKey)
        Client->>Client: encryptSecret(publicKey)
        Client->>+Server: putEncryptedSecret()
        Server-->>-Client: success

1. in case a facility secret was not already created, the corresponding secret is created here.
2. OBSOLETE: the public key must not be requested as it is part of the security store in VueX
3. OBSOLETE
4. encrypt the facility secret
5. send memberSecret to the server
6. obligatory success message

.. _processes-get-secret:

Get secret
----------

.. mermaid::

    sequenceDiagram;
        autonumber
        Client->>+Server: getMyEncryptedSecret(facilityID)
        Server-->>-Client: (encryptedSecret)

1. request the users encrypted facility member secret (one per facility)
2. server sends the secret