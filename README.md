# Description

The SPID authentication module allows users to authenticate against the Italian SPID system.

# Installation

Install the module as usual.

# Usage

[WIP]

## Configuration

* start [SPID SAML Check](https://github.com/italia/spid-saml-check) (`docker run -tidp 8443:8443 --network=ddev-drupal9_default italia/spid-saml-check:1.10.4`)
* create a folder outside the webroot to host the certificates (like `var/www/html/spid/certs`)
* generate certificates (`openssl req -x509 -nodes -sha256 -days 365 -newkey rsa:2048 -subj "/C=IT/ST=Italy/L=Milan/O=myservice/CN=localhost" -keyout sp.key -out sp.crt`)
* create a folder outside the webroot to host the metadata (like `var/www/html/spid/metadata`)
* Navigate to `/admin/config/people/spid` and fill the form with all the data related to your environment:
  * Entity ID: usually the url of your site
  * Certificate file path: something like `var/www/html/spid/certs/sp.crt`
  * Certificate key file path: something like `var/www/html/spid/certs/sp.key`
  * Metadata file path: something like `var/www/html/spid/metadata/`
  * Organization name: the name of your organization
  * Organization display name: the name of your organization
* go to `/admin/config/people/spid/metadata_idp` and click `Download metadata` to download public idp metadata
* download the test idp metadata from [here](https://localhost:8443/demo/metadata.xml) and save it to the metadata folder (like `var/www/html/spid/metadata/`) as `test.xml`
* login to SPID SAML Check ([here](https://localhost:8443/#/login)) with credential: validator/validator
* import the SP metadata to SPID SAML Check (path `https://web/spid/metadata`)
* Place the *SPID SP Access Button* block somewhere in you layout (only visible to anonymous users and on `/user/login` path)
* Add a logout link to the user menu (or the menu you want) that points to `/spid/logout`
* Set session cookie sameSite to `None` (add a `services.yml` file in `web/sites/default` with the following content (because of this [change record](https://www.drupal.org/node/3275352)):

```yaml
parameters:
  session.storage.options:
    cookie_samesite: None

```

* login with SPID SP Access Button using the test credentials (available users [here](https://localhost:8443/demo/users))
