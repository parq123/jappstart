### Getting Started Guide ###

  1. Configure the following properties in filter-local.properties, filter-dev.properties, and filter-prod.properties.
    * google.app.id
    * google.jsapi.http.key
    * google.jsapi.https.key
    * application.hostname
    * mail.fromAddress (see [Sending Mail](http://code.google.com/appengine/docs/java/mail/overview.html#Sending_Mail) for details regarding e-mail address restrictions)
  1. Create a settings.xml and a settings-security.xml in your local .m2 folder. See [Maven - Password Encryption](http://maven.apache.org/guides/mini/guide-encryption.html) for instructions on how to encrypt your GAE password.
```
<!-- settings.xml -->
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
        http://maven.apache.org/xsd/settings-1.0.0.xsd">
  
    <servers>
        <server>
            <id>yourapp.com</id> <!-- this should match the google.app.id property and is the GAE application id -->
            <username>you@yourapp.com</username> <!-- GAE username -->
            <password>encryptedpass</password> <!-- Encrypted GAE password -->
        </server>    
    </servers>

</settings>
```
```
<!-- settings-security.xml -->
<settingsSecurity>
    <master>masterpass</master> <!-- Encrypted master password -->
</settingsSecurity>
```
  1. Run _mvn clean package -P local_ to create a new local build
  1. Run _mvn gae:run -P local_ to run locally
  1. Run _mvn gae:stop -P local_ to stop the local jetty server
  1. Run _mvn clean package -P dev_ to create a new dev build
  1. Run _mvn gae:deploy -P dev_ to deploy to your dev app engine app
  1. Run _mvn clean package -P prod_ to create a new prod build
  1. Run _mvn gae:deploy -P prod_ to deploy to your prod app engine app

Notes:
  1. jsapi keys can be obtained [here](http://code.google.com/apis/ajaxlibs/documentation/index.html#sign_up_for_an_api_key).
  1. To demonstrate the localization functionality just append the following to any url: _/some/url?locale=en\_US_ or _/some/url?locale=tl\_PH_. To support additional locales just create a new _messages\_xx\_YY.properties_ and set the new locale via _/some/url?locale=xx\_YY_.

### JRebel Usage ###
  1. Install the [JRebel Nightly Build](http://www.zeroturnaround.com/jrebel/early-access/).
  1. Define a REBEL\_HOME environment variable
  1. Run _mvn clean compile -P local_ to create a new local build
  1. Run _mvn gae:run -P local-jrebel_ to run locally with JRebel support

### Remote API/Bulk Loader Usage ###
Below is an example of how to use the bulk loader to copy all data from the production server to the local development server. Omit the "--kind" option to dump/restore all kinds. See [this](http://code.google.com/appengine/docs/python/tools/uploadingdata.html) for more info.
  * Backup:
```
    appcfg.py download_data --application=<app-id> --kind=UserAccount --url=https://<app-id>.appspot.com/remote_api --filename=backup.dat
```
  * Restore:
```
    appcfg.py upload_data --application=<app-id> --kind=UserAccount --url=http://localhost:8080/remote_api --filename=backup.dat
```

### Google Plugin for Eclipse Usage ###
  1. See the [Google App Engine Blog](http://googlewebtoolkit.blogspot.com/2010/08/how-to-use-google-plugin-for-eclipse.html).