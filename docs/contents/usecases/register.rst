Register
========

**This process applies only for facility admins registering a new account while creating a facility.**

MATZE: hier müssen wir nocheinmal trennen zwischen Praxisleiter, B2C und der Erstellung neuer Praxen ohne Registrierung!

.. mermaid:: 

    sequenceDiagram
        autonumber
        Client->>Server: register(email, password)
        Server->>Client: userID, accessToken
        Client->>Client: createKeys()
        Client->>Client: encryptKey(privateKey, password)
        Client->>+KeyServer: newUser(accessToken, userID, encryptedPrivateKey)
        KeyServer->>+Server: validateUser(userID, accessToken, certificate)
        Server->>-KeyServer: valid
        KeyServer-->-Client: success-response
        Client->>Client: createSecrets()
        Client->>Client: encryptSecrets(secrets, publicKey)
        Client->>Client: encryptInsioSecrets(secrets, insioPublicKey)
        Client->>Server: secureUser(encryptedSecrets, encryptedInsioSecrets, publicKey)
        Note over Client,Server: Email verification process

1. ``register(email, password)``: creates new user on the server
2. ``userToken``: used to identify this request (secret token of a user)
3. ``createSecrets``: creates multiple randomly generated secrets, these secrest are used to encrypt all data for this client, we need multiple keys to be able to allow multiple clients of one organisation to be able to have different roles/functionalities/permissions
4. ``createKeys()``: Creates a public - private Keypair
5. ``encryptKey(privateKey, password)``: encrypt the private key so it can be stored on the KeyServer without risk and the user can access it again, using his password
6. ``newUser(userToken, encryptedPrivateKey)``: tells the keyserver that there is a new user (to be able to match the user on Server and KeyServer, we user the ``userToken``) and gives the ``encryptedPrivateKey`` to the Keyserver
7. The KeyServer asks the Server to validate the userToken
8. If the userToken was found in the database, some sort of success message is returned
9. KeyServer must send some sort of success, so we know we can proceed - otherwise, something went wrong and we need to restart the process.
10. ``encryptSecrets(secrets, publicKey)``: use the public key to encrypt all the secrets.
11. ``encryptInsioSecrets(secrets, insioPublicKey)``: use the Insio publicKey to encrypt all the secrets
12. ``secureUser(encryptedSecrets, encryptedInsioSecrets, publicKey)``: Tell the Server the encrypted secrets and the publicKey. From here on, the user is considered 'secure'