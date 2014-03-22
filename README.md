bigquery
----

This is a simple sdk for using google bigquery.

# Installation

```
npm install bigquery
```

# Apply service account

Follow the doc: http://gappsnews.blogspot.tw/2013/10/connect-cloud-platform-bigquery-using.html

# Convert p12 key
From admin console, create a service account, save the client_secrets.json and it's key
ex: Translate p12 to pem
```
openssl pkcs12 -in privatekey.p12 -out privatekey.pem -nocerts
openssl rsa -in privatekey.pem -out key.pem
```

# Initial

Load bigquery lib, specify your project id then setup the service account and the client_secret.json file path, pem key file path for auth use.

```
var bq = require('bigquery')
  , fs = require('fs')
  , prjId = 'your-bigquery-project-id'; //you need to modify this

bq.init({
  client_secret: '/path-to-client_secret.json',
  privatekey_pem: '/path-to-privatekey.pem',
  key_pem: '/path-to-key.pem'
});
```

# Do Query

Query the dataset that you have. The sample bellow is to query the public dataset of wikipedia.

```
bq.job.query(prjId, 'select count(*) from publicdata:samples.wikipedia', function(e,r,d){
  if(e) console.log(e);
  console.log(JSON.stringify(d));
});
```

# Load data

Before load data, you must create your dataset and table first. After that, you can use the following code to load data. (In this case test is the dataset id, testtb1 is the table name.)

```
var data = [
 {
   "insertId": "201403221228",
   "json": {
     "name": "simon3",
     "sex": "M",
     "age": 35
   }
 }
];
bq.job.load(prjId, 'test', 'testtb1', data, function(e,r,d){
  if(e) console.log(e);
  console.log(JSON.stringify(d));
})
```

