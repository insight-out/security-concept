********
Glossary
********

.. glossary::

    userID
       A 12-byte random generated identifier from MongoDB (`specification <https://docs.mongodb.com/manual/reference/method/ObjectId/>`_) for documents in mongo collections.
       The userID is just the ObjectId from a user document.
 
    userAccessToken
       Whenever a client wants to retrieve data from the key.box, an user-specific one-time access token is required.
       The client can request such a token from the server, as long as valid user session exists. The token is valid for 60 seconds and gets invalidated after it was used once.