*****************
key.box endpoints
*****************
Have a look at the `README <https://git.insio.de/insio/key.box/src/branch/master/README.md>`_ of the repo for a more up to date version of endpoint documentation.

In general all key.box endpoints are publicly available, as long as the request contents a valid `userAccessToken`.

.. http:post:: /getKey

  Requesting the encrypted key data for a given userAccessToken

  **Example request**:

  .. sourcecode:: http

    POST /getKey HTTP/1.1
    Content-Type: application/json

  **Example response**:

  .. sourcecode:: http

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
      "foo": 123
    }

  :form userAccessToken: generated access token for one request
  :status 200: if token was valid
  :status 404: when parameters are missing or invalid

.. http:post:: /newUser

.. http:post:: /pendingChange

.. http:post:: /applyPendingChange