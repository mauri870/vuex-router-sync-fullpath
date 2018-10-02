# vuex-router-sync-fullpath

> Sync vue-router's current $route.fullPath as part of vuex store's state. This package differs from the original vue-router-sync because it not uses cyclic objects.

### Usage

``` bash
# the latest version works only with vue-router >= 2.0
npm install vuex-router-sync-fullpath
```

``` js
import { sync } from 'vuex-router-sync-fullpath'
import store from './vuex/store' // vuex store instance
import router from './router' // vue-router instance

const unsync = sync(store, router) // done. Returns an unsync callback fn

// bootstrap your app...

// During app/Vue teardown (e.g., you only use Vue.js in a portion of your app and you 
// navigate away from that portion and want to release/destroy Vue components/resources)
unsync() // Unsyncs store from router
```

You can optionally set a custom vuex module name:

```js
sync(store, router, { moduleName: 'RouteModule' } )
```

### How does it work?

- It adds a `route` module into the store, which contains the state representing the current route:

  ``` js
  store.state.route.fullPath
  ```

- When the router navigates to a new route, the store's state is updated.

- **`store.state.route` is immutable, because it is derived state from the URL, which is the source of truth**. You should not attempt to trigger navigations by mutating the route object. Instead, just call `$router.push()` or `$router.go()`. Note that you can do `$router.push({ query: {...}})` to update the query string on the current path.

### License

[MIT](http://opensource.org/licenses/MIT)
