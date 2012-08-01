Coupons Installation
====================

Installation instructions for coupons web application.

Dependencies
------------

* php >= 5.3.x
* mysql >= 5.1.x *(also tested with MariaDB >= 5.2.x)*
* python-sphinx *(to build documentation)*
* git *(to clone directly from the repository)*

Required php modules:

* gd
* ldap
* mysql-pdo
* openssl
* soap

Get source code
---------------

Get the source code from the git repository *(clone)* or a release tag.
Also, the CakePHP bootstrap-helper is required, see `Deploy`_ on how to
download it as a git submodule or from a relsease tag.

Repositories:

* `Coupons (contains CakePHP framework) <http://git.edu.teiath.gr/coupons.git>`_
* Coupons on github *(pending)*
* `Coupons documentation <http://git.edu.teiath.gr/coupons-docs.git>`_
* `CakePHP framework <https://github.com/cakephp/cakephp>`_
* `CakePHP bootstrap-helper <https://github.com/loadsys/twitter-bootstrap-helper>`_

Deploy
------

Application setup: list of required configuration files and a list of the most important options.

Configuration files
^^^^^^^^^^^^^^^^^^^

Rename each ``*.php.default`` file to ``*.php`` and edit accordingly if required.

* **bootstrap.php.default**

* **configuration.php.default**

    * ``APP_URL``: set the application url `(eg: http://coupons.edu.teiath.gr)` **no** trailing slash here.
    * ``ISSUE_URL``: redmine url
    * ``ISSUE_TOKEN``: redmine's user access token
    * ``ISSUE_PROJECT_ID``: redmine project id to post issues to `(project must have an issue tracker)`.
    * ``ADMIN_EMAIL``: email for application's administrator
    * ``MAX_OFFER_IMAGES``: maximum number of images allowed in offers.
    * ``MAX_COMPANY_IMAGES``: maximum number of images allowed in company profile.

* **core.php.default**

    * contains salt and hash algorithm settings - keep it safe.

* **database.php.default**

    * database settings - set database name/credentials
    * ``encoding``: set to ``utf8``.

* **email.php.default**

    * configure application email address / email charset etc

* **ldap.php.default**

    * ldap settings - set ldap credentials here


Database
--------

Database setup.

From command line
^^^^^^^^^^^^^^^^^

Run the setup script found in ``schema/reset-db.sh`` and provide the root username/password
for your database when prompted. The database scripts assume you have the right to create
databases. The default database created is named ``opendeals``.

From PHPMyAdmin
^^^^^^^^^^^^^^^

#. import ``opendeals.sql``
#. import ``days.sql``
#. import ``counties.csv``, skiping the 1st row
#. import ``municipalities.csv`` skipping the 1st row and re-arranging columns properly
#. import ``offercategories.sql``



Cron scripts
------------

* **new_offers.sh**

    * notify users for new offers
    * run: ``once / day`` a little after ``00:00``.

* **notify_improper.sh**

    * notify users for offers marked as spam (only if user has related coupons)
    * run every: ``6 hours``.

* **remove_old_visits.sh**

    * cleanup old status
    * run every: ``3 days`` or longer.

* **update_state.sh**

    * check offer end time and mark finished offers as "finished"
    * run at least every: ``15 min``.

* **update_total_stats.sh**

    * calculate total stats for previous day
    * run: ``once / day`` a little after ``00:00``.

