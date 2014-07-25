angular-rollbar
===============

A tiny Angular wrapper for for [Rollbar JS API](https://rollbar.com/docs/notifier/rollbar.js/)

```javascript
    var module = angular.module('example.com/myapp', ['ng', 'anotherpit/angular-rollbar']);

    module.config(['$rollbarProvider', function($rollbarProvider) {
        // Be sure to replace POST_CLIENT_ITEM_ACCESS_TOKEN with your project's post_client_item access token, which you can find in the Rollbar.com interface.
        $rollbarProvider.config.accessToken = 'POST_CLIENT_ITEM_ACCESS_TOKEN';
        $rollbarProvider.config.captureUncaught = true;
        $rollbarProvider.config.payload = {
            environment : 'production'
        };
    }]);

    module.run(['$rollbar', function($rollbar) {
        // Caught errors
        try {
          doSomething();
        } catch (e) {
          $rollbar.error("Something went wrong", e);
        }

        // Arbitrary log messages. 'critical' is most severe; 'debug' is least.
        $rollbar.critical("Connection error from remote Payments API");
        $rollbar.error("Some unexpected condition");
        $rollbar.warning("Connection error from Twitter API");
        $rollbar.info("User opened the purchase dialog");
        $rollbar.debug("Purchase dialog finished rendering");

        // Can include custom data with any of the above.
        // It will appear as `custom.postId` in the Occurrences tab
        $rollbar.info("Post published", {postId: 123});

        // Callback functions
        $rollbar.error(e, function(err, data) {
          if (err) {
            console.log("Error while reporting error to Rollbar: ", e);
          } else {
            console.log("Error successfully reported to Rollbar. UUID:", data.uuid);
          }
        });
    });
```
