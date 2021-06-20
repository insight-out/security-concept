Add member
==========

.. mermaid:: 

    sequenceDiagram;
        autonumber
        AdminClient->>Server: newUser(email)
        UserClient->>UserClient: createKeyPair()
        UserClient->>UserClient: encryptPrivateKey()
        UserClient->>Server: register(password)
        Server->>UserClient: accessToken
        UserClient->>KeyServer: addKey(accessToken, encPrivKey)
        UserClient->>Server: addPubKey(pubKey)
        KeyServer->>+Server: validateNewUser(accessToken, cert)
        Server->>-KeyServer: userID
        KeyServer->>UserClient: success (Waiting for completion)
        Server->>+AdminClient: userRegistered(userPubKey)
        AdminClient->>AdminClient: encSecret(userPubKey)
        AdminClient->>Server: putUserEncSecret(encSecret)
        Server->>UserClient: encSecret

1. Admin läd neuen user ein
2. neuer user bekommt email mit link zur registrierung (mit token)
3. neue user füllt die registrierung aus (mail is vorgegeben) -> public und private key werden erzeugt und hinterlegt
4. admin muss neuen user bestätigen (beim bestätigen werden dann alle relevanten secrets and den publickey des neuen users verschlüsselt) Bei diesem Step können weitere sicherheitsmaßnahmen ergriffen werden (telefonat/persönliches nachfragen/geheimfrase)
