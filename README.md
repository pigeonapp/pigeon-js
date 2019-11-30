# pigeon-js

To install pigeon-js to your web application, paste this snippet into the head of your site:

```
<script type="text/javascript">
  (function(){

    // Create a queue, but don't obliterate an existing one!
    var pigeonJS = window.pigeonJS = window.pigeonJS || [];

    // If the pigeon.js is already loaded on the page return.
    if (pigeonJS.initialize) return;

    // If the snippet was invoked already show an error.
    if (pigeonJS.invoked) {
      if (window.console && console.error) {
        console.error('pigeonJS snippet included twice.');
      }
      return;
    }

    // Invoked flag, to make sure the snippet
    // is never invoked twice.
    pigeonJS.invoked = true;

    // A list of the methods in pigeonJS.js to stub.
    pigeonJS.methods = [
      'track'
    ];

    // Define a factory to create stubs. These are placeholders
    // for methods in Pigeon library so there is no need to wait for
    // Pigeon library to load completely in order to execute methods.
    pigeonJS.factory = function(method){
      return function(){
        var args = Array.prototype.slice.call(arguments);
        args.unshift(method);
        pigeonJS.push(args);
        return pigeonJS;
      };
    };

    // For each of Pigeon library methods, generate a queueing stub.
    for (var i = 0; i < pigeonJS.methods.length; i++) {
      var key = pigeonJS.methods[i];
      pigeonJS[key] = pigeonJS.factory(key);
    }

    // Create an async script element to load Pigeon asynchronously.
      var script = document.createElement('script');
      script.type =   'text/javascript';
      script.async = true;
      script.src = 'https://cdn-url/pigeon.min.js';

      // Insert the script before the first script element.
      var first = document.getElementsByTagName('script')[0];
      first.parentNode.insertBefore(script, first);

  })();

</script>
```

