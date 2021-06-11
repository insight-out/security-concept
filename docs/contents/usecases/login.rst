*****
Login
*****

.. mermaid::

    sequenceDiagram;
        autonumber
        Client->>+Server: authenticate(email, password)
        Server-->>-Client: cookie, secured
        opt is not secured
        Note over Client,Server: processes/create-keypair
        end
        par GetSecret
        Note over Client,Server: processes/get-secret
        and GetPrivateKey
        Note over Client,KeyServer: processes/get-privkey
        end
        Client->>Client: decrypt(encryptedKey, encryptedSecret)
        Note over Client,Server: processes/session-challenge


1. regular authentication request
2. creates the session cookie as well as an indicator ``secured`` if the user has actual key material

If the user is not secured, key material is generated, see :ref:`processes-create-keypair`.
Otherwise, the following processes run in parallel:
* :ref:`processes-get-secret`
* :ref:`processes-get-privkey`

3. decryption and validation of key and secret:
   * ``encryptedPrivateKey`` gets decrypted with the users password.
   * ``encryptedSecret`` gets decrypted with the ``privateKey``.

As a final step, the client requests a :ref:`processes-session-challenge` to validate the private key.