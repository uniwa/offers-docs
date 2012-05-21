Coupons API reference
=====================

Login
-----

====== =================== ===========
Method Rest URI            Description
====== =================== ===========
POST   url/**users/login** Login user in service.
====== =================== ===========

XML Request body::

    <User>
        <username>foo</username>
        <password>bar</password>
    </User>

JSON Request body::

    {
        "User": {
            "username": "foo",
            "passsword": "bar"
        }
    }

Sample XML request::

    $ curl -X POST http://url/users/login \
        -H "Content-Type: application/xml" \
        -d "<User><username>foo</username><password>bar</password></User>" \
        -c /tmp/cookie

Sample JSON request::

    $ curl -X POST http://url/users/login \
        -H "Content-Type: application/json" \
        -d "{"User": {"username": "foo", "password": "bar"}}" \
        -c /tmp/cookie

.. note::

    Τα παραπάνω request δημιουργούν ένα αρχείο cookies ``cookie`` στον κατάλογο ``tmp``.
    Χρησιμοποιείται για τα requests τα οποία απαιτούν αυθεντικοποίηση.


Offer Index
-----------

Offer View
----------

Offer Create
------------

Offer Update
------------

Offer Delete
------------
TODO

Coupon View
-----------

Coupon Get
----------

