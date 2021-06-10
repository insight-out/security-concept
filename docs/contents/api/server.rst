****************
Server endpoints
****************

protected
=========

These routes require an already authenticated user session

.. http:get:: /api/user/accessToken

    Requests a userAccessToken for the current user session

    **Example request**:

    .. sourcecode:: http

        GET /api/user/accessToken HTTP/1.1
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: text/javascript

        "someAccessToken"

    :status 200: if accessToken was generated
    :status 400: if something went wrong

.. http:get:: /api/user/(string:id)/publicKey

    Requests the public key of a given user

    **Example request**:

    .. sourcecode:: http

        GET /api/user/abc7665df75878a/publicKey HTTP/1.1
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: text/javascript

        "some public key"

    :status 200: if publicKey was found
    :status 400: if `id` was invalid

.. http:post:: /api/user/publicKey

    Updates the publicKey of the current session user

    **Example request**:

    .. sourcecode:: http

        POST /api/user/publicKey HTTP/1.1
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: text/javascript

        "some public key"

    :form publicKey: the new public key
    :status 200: if publicKey was saved
    :status 400: if something went wrong

.. http:get:: /api/facility/(string:id)/secret

    Requests the user-specific encrypted secret of a specific facility. The user must be a member of this facility.

    **Example request**:

    .. sourcecode:: http

        GET /api/facility/abc123sdasd/secret HTTP/1.1
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: text/javascript

        "encrypted secret"

    :status 200: user is member and secret was sent
    :status 400: something went wrong

.. http:get:: /api/facility/(string:id)/iv

    Requests the initilization vector of a specific facility. The user must be a member of this facility.

    **Example request**:

    .. sourcecode:: http

        GET /api/facility/abc123sdasd/iv HTTP/1.1
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: text/javascript

        "a very long initialization vector"

    :status 200: user is member and iv was sent
    :status 400: something went wrong


keyserver-only
==============

These endpoints are only available for the key.box

.. http:post:: /keyserver/user/validate

    Validates a given userAccessToken against the corresponding user data.

    **Example request**:

    .. sourcecode:: http

        GET /keyserver/user/validate HTTP/1.1
        Content-Type: application/json

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: text/javascript

        "hereBeToken"

    :status 200: for a successfully created token
    :status 403: for invalid requests (no user, demo user, ...)