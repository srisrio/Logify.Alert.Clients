# Logify Alert for JavaScript

A JavaScript client for reporting exceptions to [Logify Alert](https://logify.devexpress.com/).

## Links to script

The latest version of the JavaScript client is available by the following links:
```
https://logifyjs.devexpress.com/logifyAlert.js - full version
https://logifyjs.devexpress.com/logifyAlert.min.js - minified version
```

Additionally, there are clients for specific Logify Alert versions. For example:
```
https://logifyjs.devexpress.com/0.0.4/logifyAlert.js - full version
https://logifyjs.devexpress.com/0.0.4/logifyAlert.min.js - minified version
```

## Quick Start

```javascript
<script type="text/javascript" src="https://logifyjs.devexpress.com/logifyAlert.min.js"/>

<script type="text/javascript">
	var client = new logifyAlert("SPECIFY_YOUR_API_KEY_HERE");
	client.applicationName = "Test application";
	client.startHandling();
</script>
```

## API

### Fields

#### applicationName

Specifies the application name. This name is shown in generated reports.

```javascript
client.applicationName = 'My Application';
```

#### applicationVersion

Specifies the application version. This version is shown in generated reports. 

```javascript
client.applicationVersion = '1.0.1';
```

#### breadcrumbsMaxCount

Integer. Specifies the maximum allowed number of breadcrumbs attached to one crash report. The default value is 1000 instances (or 3 MB).

```javascript
logify.breadcrumbsMaxCount = 2000;
```

#### collectBreadcrumbs

Boolean. Specifies whether automatic breadcrumbs collecting is enabled. The default value is **false**.  
The total breadcrumbs size is limited by 1000 instances (or **3 Mb**) per one crash report by defaul. To change the maximum allowed size of attached breadcrumbs, use the *breadcrumbsMaxCount* property. 

```javascript
var logify = new logifyAlert("YOUR_APIKEY_HERE");
logify.collectBreadcrumbs = true;
logify.startHandling();
```

#### collectCookies

Boolean. Specifies whether the Logify Alert client collects cookies. The default value is **false**.

```javascript
client.collectCookies = true;
```

#### collectInputs

Boolean. Specifies whether the Logify Alert client collects all inputs passed to a web page (except for passwords). The collected information stores an input's type, identifier, tag name and value. The default value is **false**.

```javascript
client.collectInputs = true;
```

#### collectLocalStorage

Boolean. Specifies whether the Logify Alert client collects local storage data. The default value is **false**.

```javascript
client.collectLocalStorage = true;
```

#### collectSessionStorage

Boolean. Specifies whether the Logify Alert client collects session storage data. The default value is **false**.

```javascript
client.collectSessionStorage = true;
```

#### customData

A dictionary that contains custom data sent with a generated report. The first key can only consists of a-z, A-Z, 0-9, and _ characters.

```javascript
client.customData = {FIRST_KEY:  "FIRST DATA", SECOND_KEY: "SECOND DATA"};
```

#### sensitiveDataFilters

An array of keys specifying fields whose values should be excluded from crash reports sent to Logify Alert. These keys are applied to breadcrumbs, inputs passed to a web page, local and session storage data. Pass strings (case-insensitive) or RegEx objects as keys to the array.

```javascript
client.sensitiveDataFilters = ["password", /credit.*/];
```

#### tags

A dictionary that contains tags specifying additional fields from a raw report, which will be used in auto ignoring, filtering or detecting duplicates. A key is a tag name (a string that consists of a-z, A-Z, 0-9, and _ characters), and a value is a tag value that is saved to a report. A new tag is added with **Allow search** enabled.

```javascript
client.tags["OS"] = "Win8";
```

#### userId

Specifies a unique user identifier that corresponds to a sent report.

```javascript
client.userId = 'unique user id';
```

### Methods for automatic reporting

Logify Alert allows you to automatically listen to uncaught exceptions and rejections, and deliver crash reports. For this purpose, use the methods below.

#### startHandling()

Commands Logify Alert to start listening to uncaught exceptions and rejections and send reports for all processed exceptions. 

```javascript
client.startHandling();
```

#### stopHandling()

Commands Logify Alert to stop listening to uncaught exceptions and rejections. 

```javascript
client.stopHandling();
```

### Methods for manual reporting

Alternatively, Logify Alert allows you to catch required exceptions manually, generate reports based on caught exceptions and send these reports only. For this purpose, use the methods below.

#### sendException(errorMsg, url, lineNumber, column, errorObj)

Sends information about the caught exception to the Logify Alert server.

```javascript
client.sendException(errorMsg, url, lineNumber, column, errorObj);
```

#### sendRejection(reason, promise)

Sends information about the caught rejection to the Logify Alert server.

```javascript
client.sendRejection(reason, promise);
```

### Methods

#### addBreadcrumbs(breadcrumb)

Adds a new instance to a collection of manual breadcrumbs attached to a report. The total breadcrumbs size is limited by 100 instances (or **3 Mb**) per one crash report by default. To change the maximum allowed size of attached breadcrumbs, use the *breadcrumbsMaxCount* property.

```javascript
client.addBreadcrumbs({ "dateTime": new Date().toUTCString(), "event": "manual", "message": "A manually added breadcrumb" });
```

### Callbacks

#### afterReportException(error)

Specifies a delegate to be called after sending exceptions and rejections to the Logify Alert server.

The *error* parameter holds an error text that indicates the report sending result. The *undefined* value specifies that the crash report has been successfully sent. The "*The report was not sent*" value specifies that the crash report has not been sent.

```javascript
client.afterReportException = function (error) {
        if (error == undefined) {
            // The report has been successfully sent
        }
        
        if (error == "The report was not sent") {
            // The report has not been sent
        }
}
```

#### beforeReportException(customData)

Specifies a delegate to be called before sending exceptions and rejections to the Logify Alert server.

The *customData* parameter holds custom data sent along with an exception or a rejection. The default value is *undefined*. If you change a parameter value within a callback, a new value will be stored and passed to a callback the next time you call it in your application.

```javascript
client.beforeReportException = function (customData) {
        if (customData == undefined) {
            customData = {};
        }
        customData["test key"] = "test value";
        return customData;
}
```

## Custom Clients
If the described client is not suitable for you, you can create a custom one. For more information, refer to the [Custom Clients](https://github.com/DevExpress/Logify.Alert.Clients/blob/develop/CustomClients.md) document.
