nedb-promise
============

A promise wrapper for [NeDB](https://github.com/louischatriot/nedb).

Installation
============

Install with NPM:

`$ npm i --save nedb-promise`

Usage
=====

Example using ES7 async/await:
```
import { datastore } from 'nedb-promise'

async function doDatabaseStuff() {
  let DB = new datastore({
     // these options are passed through to nedb.Datastore

     filename: 'my-db.json',

     autoload: true // so that we don't have to call loadDatabase()
  })

  await DB.insertAsync([{
    num: 1, alpha: 'a'
  }, {
    num: 2, alpha: 'b'
  }])

  let document = await DB.findOneAsync({ num: 1 })

  // use NeDB cursors:
  let documents = await DB.cfind({})
    .projection({ num: 1, _id: 0 })
    .execAsync()
}

doDatabaseStuff()
```

API
===

## datastore(options)

Returns an extended `nedb.Datastore` instance (`options` are passed through to the NeDB constructor), with promisified methods (suffixed with 'Async').

For example, you may use `findAsync(...)`, `insertAsync(...)`, etc.

It also includes the two extension methods `cfind(...)`, and `cfindOne(...)` that return promisified cursors, so that you may do:

```
let results = await myDataStore.cfindA({ moo: 'goo' })
  .projection({ moo: 1, _id: 0 }) // see NeDB cursor methods
  .exec()
```

Building
========

Build with gulp:

```
$ gulp
```

Testing
=======

Run tests with mocha (after building):

```
$ cd build/
$ mocha
```

License
=======
Copyright (c) 2015, Jonathan Apodaca <jrapodaca@gmail.com>
Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted, provided that the above copyright notice and this permission notice appear in all copies.
THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
