## Disallow usage of unknown wire adapters (no-unknown-wire-adapters)

This rule offers a way to restrict the usage of unknown wire adapters. It can be used to ensure that all wired properties are statically analyzable in order to support use cases such as prefetching and offline.

## Rule details

This rule can be configured as follows:

```js
{
    adapters: [
        // for modules exporting wire adapters using named exports
        { module: '<module name>', identifier: '<adapter identifier>' },

        // for modules exporting wire adapters using default exports
        { module: '<module name>', identifier: 'default' },
    ];
}
```

Any usage of the `@wire` decorator will be flagged as an error unless a list of known adapters has been provided.

```js
/*eslint lwc/no-unexpected-wire-adapters: ["error", {"adapters": [{"module": "myAdapters", "identifier": "fooAdapter"}]}]*/

import { LightningElement, wire } from 'lwc';
import defaultAdapter, { fooAdapter, barAdapter } from 'myAdapters';

export default class Example extends LightningElement {
    @wire(fooAdapter) // valid
    foo;

    @wire(barAdapter) // invalid, missing adapter in config: {"module": "myAdapters", "identifier": "barAdapter"}
    bar;

    @wire(defaultAdapter) // invalid, missing adapter in config: {"module": "myAdapters", "identifier": "default"}
    default;
}
```
