# Salesforce-bundle
Symfony bundle for Salesforce REST client which is base on username,password authentication

###Package instalation with composer

```console
$ composer require genesis-global/salesforce-bundle
```


### Enable bundle

Then, enable the bundle by adding it to the list of registered bundles
in the `app/AppKernel.php` file of your project:

```php
<?php
// app/AppKernel.php

// ...
class AppKernel extends Kernel
{
    public function registerBundles()
    {
        $bundles = array(
            // ...

            new GenesisGlobal\Salesforce\SalesforceBundle\SalesforceBundle(),
        );

        // ...
    }

    // ...
}
```

### Required configuration:
```yaml
salesforce:
    authentication:
        endpoint: "https://login.salesforce.com/"
        client_id: "id"
        client_secret: "secret"
        username: "name"
        password: "pass",
        security_token: '22s2sd233e3'
    rest:
        version: "v35.0"
        endpoint: "https://yourinstance.salesforce.com"

```

### Use in controller:

#### CREATE RECORD
```php
// custom class which implements SobjectInterface
$case = new Sobject();
$case->setName('Case');
$case->setContent(['someField' => 'someValue']);

$result = $this->get('salesforce.service')->create($case);  

// get salesforce id
$id = $result->getId();
```

#### UPDATE RECORD
```php

try {
    $this->get('salesforce.service')->update('Account', '001D000000INjVe', [ 'someField' => 'someValue' ]);

} catch (UpdateSobjectException $e) {

    // update failed
    echo $e->getMessage();
}
```

#### UPSERT RECORD
```php
// account object
$account = new \stdClass();
$account->Current_Balance__c = '21023';
$account->Current_Count_of_Deposit__c = 0;
$account->Number_of_Active_Days__c = 12;

// account sObject ready to send to salesforce
$accountSobject = new Sobject();
$accountSobject->setName('Account');
$accountSobject->setContent($account);

$result = $this->get('salesforce.service')->upsert($sObject, 'Player_Account__c', '123132');
```
#### GET METADATA

```php
$metaData = $this->get('salesforce.service')->getMetaDataForSobject('Account');

```

#### FIND RECORDS
```php
$fields = ['Id', 'Name', 'Phone'];
$result = $this->get('salesforce.service')->query($fields, 'Account', ["Name != 'Joe Blogs'", "Phone = '1234-5678'"]);
```
