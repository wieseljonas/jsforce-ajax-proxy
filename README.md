# JSforce AJAX Proxy

A proxy server to access Salesforce API from JSforce JavaScript apps served outside of Salesforce.

As the same origin policy restricts communication to the Salesforce API from outer domain,
you should serve cross-domain proxy server when you build a app using JSforce outside of Salesforce.

This app will become obsolete immediately when Salesforce API supports CORS.
I hope the day will come soon.

## Usage

Start proxy server in your server environment which can run Node.js app (Heroku is the one you might choose).

Install required packages :

```
$ npm install
```

Run proxy server :

```
$ node proxy.js
```

When you use JSforce in your JavaScript app, set `proxyUrl` when creating `Connection` instance. 

```
var conn = jsforce.Connection({
  accessToken: '<access_token>',
  instanceUrl: '<instance_url>',
  proxyUrl: 'https://your-ajax-proxy-service.herokuapp.com/proxy/'
});

conn.query('SELECT Id, Name FROM Account', function(err, res) {
  // ...
});
```
If you have a public webservice you can just add the url to the publicapiurl variable

```
 var publicapiurl = "https://certprep-developer-edition.ap1.force.com/services/apexrest/Exam";
```
Here below a Jquery example to call the proxy

```
$.ajax({
        url: "http://localhost:3123/proxy/Example",
        type: "POST",
        contentType: "application/json",
        data: requestdata,
        success : function (data) {
          window.console.log('success');
          window.console.log(data);
          //....process
        },
        error : function (jqXHR, textStatus, errorThrown) {
            window.console.log(jqXHR);
            window.console.log(textStatus);
            window.console.log(errorThrown);
        } 
        //beforeSend: function(xhr){xhr.setRequestHeader('Access-Control-Allow-Origin', '*');    
      });
```

## Note

You don't have to use this app when you are building a JSforce app in Visualforce,
because it works in the same domain as Salesforce API.
