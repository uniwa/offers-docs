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
        -H "Accept: application/xml" \
        -d "<User><username>foo</username><password>bar</password></User>" \
        -c /tmp/cookie

Sample JSON request::

    $ curl -X POST http://url/users/login \
        -H "Content-Type: application/json" \
        -H "Accept: application/json" \
        -d '{"User": {"username": "foo", "password": "bar"}}' \
        -c /tmp/cookie

.. note::

    Τα παραπάνω request δημιουργούν ένα αρχείο cookies ``cookie`` στον κατάλογο ``tmp``.
    Χρησιμοποιείται για τα requests τα οποία απαιτούν αυθεντικοποίηση.

.. note::

    Χρησιμοποιήστε μόνο διπλά quotes ``"`` στο JSON. Εξωτερικά απαιτούνται μονά quotes ``'`` για αποφυγή expansion από το shell.

Sample JSON response on success::

    {
        "message": "Η αυθεντικοποίηση ολοκληρώθηκε με επιτυχία",
        "status_code": 200
    }

and on wrong credenials::

    {
        "message": "Δώστε έγκυρο όνομα και κωδικό χρήστη",
        "status_code": 403
    }


Offer Index
-----------

====== ========================= ===========
Method Rest URI                  Description
====== ========================= ===========
GET    url/**offers**            Retrieve list of publicly available offers. By default returns 1st page of results only.
------ ------------------------- -----------
GET    url/**index**/**offers**  Same as above, alternative URI.
====== ========================= ===========

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


Request options

============== =============== ===========
Sort keyword   Sort value      Description
============== =============== ===========
orderby        *recent*        Sort by recent additions
-------------- --------------- -----------
orderby        *rank*          Sort by vote results (sum of votes)
-------------- --------------- -----------
orderby        *votes*         Sort by vote number (count)
-------------- --------------- -----------
orderby        *distance*      Sort by user distance, available only when user coordinates are set
-------------- --------------- -----------
page           *<num>*         Show only results page number = *<num>*
============== =============== ===========

.. note::

    Από προεπιλογή επιστρέφεται μόνο η πρώτη σελίδα αποτελεσμάτων.

Sample JSON request::

    $ curl -X GET http://url/offers/index/orderby:rank/page:2 -H "Accept: application/json"


.. note::

    Για την χρήση παραμέτρων ταξινόμησης απαιτείται στο URI το **index**


Offer Types
-----------

====== ======================== ===========
Method Rest URI                 Description
====== ======================== ===========
GET    url/**offers/happyhour** Retrieve list of publicly available **Happy Hour** offers.
------ ------------------------ -----------
GET    url/**offers/coupons**   Retrieve list of publicly available **Coupons** offers.
------ ------------------------ -----------
GET    url/**offers/limited**   Retrieve list of publicly available **Limited** offers.
====== ======================== ===========

Η απάντηση που επιστρέφουν τα παραπάνω URIs είναι αντίστοιχη με της ενότητας `Offer Index`_ .

Offer View
----------

====== ========================== ===========
Method Rest URI                   Description
====== ========================== ===========
GET    url/**offers**/*{offerId}* Retrieve info of offer with id *offerId*
====== ========================== ===========

The following table describes the URI parameters.

============== ========================== ===========
required parameters
-----------------------------------------------------
Parameter Name Data type                  Description
============== ========================== ===========
offerId        string                     ID of offer
============== ========================== ===========

Sample JSON request::

    $ curl -X GET http://url/offers/17 -H "Accept: application/json"


Sample JSON response (offer type **HappyHour**)::

    {
        "company": {
            "address": null, 
            "afm": "011111111", 
            "created": "2012-04-17 12:29:45", 
            "fax": null, 
            "id": "8", 
            "image_count": "0", 
            "is_enabled": true, 
            "latitude": null, 
            "longitude": null, 
            "modified": "2012-04-17 12:29:45", 
            "municipality_id": null, 
            "name": "\u0386\u03c1\u03c9\u03bc\u03b1 \u0392\u03cd\u03bd\u03b7\u03c2", 
            "phone": "2100000000", 
            "postalcode": null, 
            "service_type": null, 
            "user_id": "13"
        }, 
        "offer": {
            "autoend": null, 
            "autostart": null, 
            "company_id": "8", 
            "coupon_count": "0", 
            "coupon_terms": null, 
            "created": "2012-04-17 12:35:51", 
            "description": "\u03a4\u03b9\u03bc\u03ad\u03c2 \u03c3\u03b5 Happy Hours\r\n\r\n\u039f\u03b9 \u00ab\u03c7\u03b1\u03c1\u03bf\u03cd\u03bc\u03b5\u03bd\u03b5\u03c2 \u03ce\u03c1\u03b5\u03c2\u00bb \u03ad\u03c7\u03bf\u03c5\u03bd \u03ba\u03b1\u03b8\u03b9\u03b5\u03c1\u03c9\u03b8\u03b5\u03af \u03c0\u03bb\u03ad\u03bf\u03bd \u03c3\u03c4\u03b1 \u03c0\u03b5\u03c1\u03b9\u03c3\u03c3\u03cc\u03c4\u03b5\u03c1\u03b1 \u03bc\u03b1\u03b3\u03b1\u03b6\u03b9\u03ac \u03c4\u03b7\u03c2 \u03c0\u03cc\u03bb\u03b7\u03c2. \u03a3\u03c4\u03bf \u0386\u03c1\u03c9\u03bc\u03b1 \u0392\u03cd\u03bd\u03b7\u03c2, \u03bf\u03b9 Happy Hours \u03be\u03b5\u03ba\u03b9\u03bd\u03bf\u03cd\u03bd \u03c3\u03c4\u03b9\u03c2 11 \u03c4\u03bf \u03b2\u03c1\u03ac\u03b4\u03c5 \u03ba\u03b1\u03b9 \u03b4\u03b9\u03b1\u03c1\u03ba\u03bf\u03cd\u03bd \u03bc\u03ad\u03c7\u03c1\u03b9 \u03ba\u03b1\u03b9 \u03c4\u03bf \u03ba\u03bb\u03b5\u03af\u03c3\u03b9\u03bc\u03bf. \u0395\u03bd\u03c4\u03cc\u03c2 \u03c4\u03c9\u03bd Happy Hours \u03bf\u03b9 \u03c4\u03b9\u03bc\u03ad\u03c2 \u03c0\u03ad\u03c6\u03c4\u03bf\u03c5\u03bd \u03b1\u03ba\u03cc\u03bc\u03b1 \u03c0\u03b5\u03c1\u03b9\u03c3\u03c3\u03cc\u03c4\u03b5\u03c1\u03bf:\r\n\r\n    \u0392\u03b1\u03c1\u03b5\u03bb\u03af\u03c3\u03b9\u03b1 \u03bc\u03c0\u03cd\u03c1\u03b1 330ml \u039c\u039f\u039d\u039f 2,5\u20ac\r\n    \u0392\u03b1\u03c1\u03b5\u03bb\u03af\u03c3\u03b9\u03b1 \u03bc\u03c0\u03cd\u03c1\u03b1 500ml MONO 4,5\u20ac\r\n    \u039c\u03c0\u03cd\u03c1\u03b1 \u03bc\u03b5 \u03c4\u03bf \u03bc\u03ad\u03c4\u03c1\u03bf, \u03c3\u03c4\u03b1 3 \u03bb\u03af\u03c4\u03c1\u03b1 \u03c4\u03bf \u03ad\u03bd\u03b1 \u03bb\u03af\u03c4\u03c1\u03bf \u0394\u03a9\u03a1\u039f!", 
            "ended": null, 
            "id": "14", 
            "image_count": "0", 
            "is_spam": false, 
            "max_per_student": null, 
            "modified": "2012-04-17 12:35:51", 
            "offer_category": "\u03a6\u03b1\u03b3\u03b7\u03c4\u03cc", 
            "offer_hours": [
                {
                    "day_id": "1", 
                    "ending": "04:00:00", 
                    "starting": "23:00:00"
                }, 
                {
                    "day_id": "2", 
                    "ending": "04:00:00", 
                    "starting": "23:00:00"
                }, 
                {
                    "day_id": "3", 
                    "ending": "04:00:00", 
                    "starting": "23:00:00"
                }, 
                {
                    "day_id": "4", 
                    "ending": "04:00:00", 
                    "starting": "23:00:00"
                }, 
                {
                    "day_id": "5", 
                    "ending": "04:00:00", 
                    "starting": "23:00:00"
                }, 
                {
                    "day_id": "6", 
                    "ending": "04:00:00", 
                    "starting": "23:00:00"
                }, 
                {
                    "day_id": "7", 
                    "ending": "04:00:00", 
                    "starting": "23:00:00"
                }
            ], 
            "offer_state": "active", 
            "offer_type": "happy hour", 
            "started": "0000-00-00 00:00:00", 
            "tags": "\u03ac\u03c1\u03c9\u03bc\u03b1 \u03b2\u03cd\u03bd\u03b7\u03c2", 
            "title": "\u0386\u03c1\u03c9\u03bc\u03b1 \u0392\u03cd\u03bd\u03b7\u03c2 Happy Hours", 
            "total_quantity": null, 
            "vote_count": "0", 
            "vote_sum": "0"
        }, 
        "status_code": 200
    }

Sample JSON response (offer type **Coupons**)::

    {
        "company": {
            "address": "test address 28", 
            "afm": "011111111", 
            "created": null, 
            "fax": "0987654321", 
            "id": "1", 
            "image_count": "1", 
            "is_enabled": true, 
            "latitude": null, 
            "longitude": null, 
            "modified": null, 
            "municipality_id": "13", 
            "name": "company_test_1", 
            "phone": "1234567890", 
            "postalcode": "12345", 
            "service_type": "estiatorio", 
            "user_id": "5"
        }, 
        "offer": {
            "autoend": null, 
            "autostart": null, 
            "company_id": "1", 
            "coupon_count": "0", 
            "coupon_terms": "", 
            "created": "2012-05-22 12:15:25", 
            "description": "100 \u03ba\u03bf\u03c5\u03c0\u03cc\u03bd\u03b9\u03b1 \u03b3\u03b9\u03b1 \u03ad\u03ba\u03c0\u03c4\u03c9\u03c3\u03b7 \u03c3\u03b5 \u03b5\u03af\u03b4\u03b7 \u03b3\u03c1\u03b1\u03c6\u03b5\u03af\u03bf\u03c5.", 
            "ended": null, 
            "id": "18", 
            "image_count": "0", 
            "is_spam": false, 
            "max_per_student": "0", 
            "modified": "2012-05-22 12:15:25", 
            "offer_category": "\u03a0\u03c1\u03bf\u03ca\u03cc\u03bd\u03c4\u03b1", 
            "offer_hours": [], 
            "offer_state": "active", 
            "offer_type": "coupons", 
            "started": "2012-05-20 14:00:00", 
            "tags": "\u03b3\u03c1\u03b1\u03c6\u03b5\u03af\u03bf", 
            "title": "test", 
            "total_quantity": "100", 
            "vote_count": "0", 
            "vote_sum": "0"
        }, 
        "status_code": 200
    }

Sample JSON response (offer type **Limited**)::

    {
        "company": {
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
        }, 
        "offer": {
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
            "max_per_student": null, 
            "modified": "2012-04-17 12:49:56", 
            "offer_category": "\u03a6\u03b1\u03b3\u03b7\u03c4\u03cc", 
            "offer_hours": [], 
            "offer_state": "active", 
            "offer_type": "limited", 
            "started": "2012-01-01 00:00:00", 
            "tags": "pizza fan \u03c0\u03af\u03c4\u03c3\u03b1", 
            "title": "Pizza Fan 40 \u03c4\u03bf\u03b9\u03c2 \u03b5\u03ba\u03b1\u03c4\u03cc", 
            "total_quantity": null, 
            "vote_count": "0", 
            "vote_sum": "0"
        }, 
        "status_code": 200
    }



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

