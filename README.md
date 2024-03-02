# SPID authentication

The SPID authentication module allows users to authenticate against the Italian
SPID system.

# Requirements

The SPID authentication module needs to be installed via Composer, which will
also download the required
library: [italia/spid-php-lib](https://github.com/italia/spid-php-lib).

Look at [Using Composer with Drupal](https://www.drupal.org/node/2404989) for
further information.

# Installation

Install as you would normally install a contributed Drupal module.
For further information,
see [Installing Drupal Modules](https://www.drupal.org/docs/extending-drupal/installing-drupal-modules).

# Configuration

* Start [SPID SAML Check](https://github.com/italia/spid-saml-check) (
  ex: `docker run -tidp 8443:8443 --network=network-where-webserver italia/spid-saml-check:1.10.4`)
* Create a folder outside the webroot to host the certificates (
  like `/var/www/html/spid/certs`)
* Generate
  certificates (`openssl req -x509 -nodes -sha256 -days 365 -newkey rsa:2048 -subj "/C=IT/ST=Italy/L=Milan/O=myservice/CN=localhost" -keyout /var/www/html/spid/certs/sp.key -out /var/www/html/spid/certs/sp.crt`)
* Create a folder outside the webroot to host the metadata (
  like `/var/www/html/spid/metadata`)
* Navigate to `/admin/config/people/spid` and fill the form with all the data
  related to your environment:
    * Entity ID: usually the url of your site
    * Certificate file path: something like `/var/www/html/spid/certs/sp.crt`
    * Certificate key file path: something
      like `/var/www/html/spid/certs/sp.key`
    * Metadata file path: something like `/var/www/html/spid/metadata/`
    * Organization name: the name of your organization
    * Organization display name: the name of your organization
* Go to `/admin/config/people/spid/metadata_idp` and click `Download metadata`
  to download public idp metadata
* Download the test idp metadata
  from [SPID SAML Check Demo](https://[SPID SAML Check]/demo/metadata.xml) and
  save it to the metadata folder (like `/var/www/html/spid/metadata/`)
  as `test.xml`
* Login to SPID SAML
  Check ([SPID SAML Check Demo](https://[SPID SAML Check]/#/login)) with
  credential: `validator/validator`
* Insert the url for download the SP metadata
  into [Metadata Service Provider](https://[SPID SAML Check]/#/metadata-sp-download)
  of SPID SAML Check (ex. path `https://[DRUPAL-SITE]/spid/metadata`)
* Place the *SPID SP Access Button* block somewhere in you layout (only visible
  to anonymous users and on `/user/login` path)
* Add a logout link to the user menu (or the menu you want) that points
  to `/spid/logout`
* Set session cookie sameSite to `None` (add a `services.yml` file
  in `web/sites/default` with the following content (because of
  this [change record](https://www.drupal.org/node/3275352)):
  ```yaml
  parameters:
    session.storage.options:
      cookie_samesite: None
  ```
* Login with SPID SP Access Button using the test credentials (available
  users [here](https://[SPID SAML Check]/demo/users))
