# 为开发自己js库做的一些准备

## 1.以分析[locache](https://github.com/d0ugal-archive/locache/blob/0.4.0/locache.js)为例

我们来分析如下代码：
```javascript
// A defer implementation to avoid IO access blocking the current
// thread. This is exposed on the LocacheCache prototype, simply so it
// can be accessed from within the unit tests. It's not intended for
// public use.
var defer = (LocacheCache.prototype._defer = (function() {
  // Store of the pending functions
  var timeouts = [];
  // Message name used to identify posts related to the defer
  var messageName = "function-defer-message";

  // Add a message event listener. If its not from this window or doesn't
  // have the message name defined above, don't do anything and allow it
  // to propagate to other handlers. Otherwise, its meant for us so stop
  // the event.
  var addEventListener;
  if (root.addEventListener) {
    addEventListener = root.addEventListener;
  } else if (root.attachEvent) {
    addEventListener = root.attachEvent;
  }

  addEventListener(
    "message",
    function(event) {
      if (event.source !== root || event.data !== messageName) {
        return;
      }
      event.stopPropagation();

      // Make sure we have some pending functions, otherwise return.
      if (timeouts.length === 0) {
        return;
      }

      // take the oldest from the 'queue' and call that functions.
      var fn = timeouts.shift();
      fn();

      // Return false, to make sure the event isn't propagated
      return false;
    },
    true
  );

  // Constructor for the Defer, takes a function object and stores it
  // on itself.
  function Deferred(fn) {
    this.fn = fn;
  }

  // The defer method runs the function and stores the result. Upon
  // finishing, it looks for a finishedFunction, that if exists, is
  // called and passed the result.
  Deferred.prototype.defer = function() {
    this.resultValue = this.fn();
    if (this.hasOwnProperty("finishedFunction")) {
      this.finishedFunction(this.resultValue);
    }
  };

  // Return if the defer has finished. THis is determined by the
  // existence of the resultValue on the object.
  Deferred.prototype.hasFinished = function() {
    return this.hasOwnProperty("resultValue");
  };

  // The finished function takes another function and assigns that to
  // be called when the deferred function has finished.
  Deferred.prototype.finished = function(fn) {
    this.finishedFunction = fn;
    // Check to see if the deferred function has finished, this can
    // happen if its very quick or the finished function is assigned
    // late. If it is, call it straight away.
    if (this.hasFinished()) {
      this.finishedFunction(this.resultValue);
    }
    // Make this object chainable.
    return this;
  };

  // Wrapper utility function that provides access to the Deferred
  // implementation and makes it very simple to use.
  function defer(fn) {
    // Create the defer instance that wraps the function
    var d = new Deferred(fn);
    // Add the defer method on the Deferred object, with the instance
    // bound to the queue.
    timeouts.push(bind(d.defer, d));
    // post a message to the window that can be received by the
    // message handler.
    root.postMessage(messageName, "*");
    // Finally, return the deferred object instance.
    return d;
  }

  // Only return the defer function, this is all we want to be publicly
  // accessible.
  return defer;
})());

```