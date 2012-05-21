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

.. note::

    Χρησιμοποιήστε μόνο διπλά quotes ``"`` στο JSON.


Offer Index
-----------

====== =================== ===========
Method Rest URI            Description
====== =================== ===========
GET    url/**offers**      Retrieve list of publicly available offers.
====== =================== ===========

Sample JSON request::

    $ curl -X GET http://url/offers -H "Accept: application/json"

Sample JSON response::

    {
        "companies": [
            {
                "address": null, 
                "afm": "011111111", 
                "created": "2012-04-17 09:24:38", 
                "fax": null, 
                "id": "4", 
                "image_count": "0", 
                "is_enabled": true, 
                "latitude": null, 
                "longitude": null, 
                "modified": "2012-04-17 09:24:38", 
                "municipality_id": null, 
                "name": "Pizza Fan", 
                "phone": "2108112000", 
                "postalcode": null, 
                "service_type": null, 
                "user_id": "9"
            }
        ],
        "offers": [
            {
                "autoend": null, 
                "autostart": null, 
                "company_id": "4", 
                "coupon_count": "0", 
                "coupon_terms": null, 
                "created": "2012-04-17 12:49:56", 
                "description": "40% \u03b3\u03b9\u03b1 \u03c4\u03bf\u03c5\u03c2 \u03c6\u03bf\u03b9\u03c4\u03b7\u03c4\u03ad\u03c2.", 
                "ended": null, 
                "id": "17", 
                "image_count": "0", 
                "is_spam": false, 
                "max_per_student": "0", 
                "modified": "2012-04-17 12:49:56", 
                "offer_category": "\u03a6\u03b1\u03b3\u03b7\u03c4\u03cc", 
                "offer_hours": [], 
                "offer_state": "active", 
                "offer_type": "limited", 
                "started": "2012-01-01 00:00:00", 
                "tags": "pizza fan \u03c0\u03af\u03c4\u03c3\u03b1", 
                "title": "Pizza Fan 40 \u03c4\u03bf\u03b9\u03c2 \u03b5\u03ba\u03b1\u03c4\u03cc", 
                "total_quantity": "0", 
                "vote_count": "0", 
                "vote_sum": "0"
            },
        ],
        "status_code": 200
    }

.. note::

    Για κάθε προσφορά επιστρέφονται και τα αντίστοιχα στοιχεία της επιχείρησης.
    Τα στοιχεία αυτά επιστρέφονται σε μια δεύτερη λίστα με το όνομα ``companies``.


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

