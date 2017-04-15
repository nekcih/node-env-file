# NODE-ENV-FILE-SUBST

Parse and load environment files including substitutions (containing ENV variable exports) into Node.js environment, i.e. `process.env`.


## Example

**`.env`**

```bash
  # some env variables

  FOO=foo1
  BAR=bar1
  BAZ=1
  QUX=
  # QUUX=
```

**`.env2`**

```bash
  # some env variables using exports syntax

  exports FOO=foo2
  exports BAR=bar2
  exports BAZ=2
  exports QUX=
  # exports QUUX=

```

**`index.js`**

```javascript
  var assert = require('assert');
  var env = require('node-env-file');

  process.env.FOO = "defaultfoo";

  // Load any undefined ENV variables from a specified file.
  env(__dirname + '/.env');
  assert.equal(process.env.FOO, "defaultfoo");
  assert.equal(process.env.BAR, "bar1");
  assert.equal(process.env.BAZ, "1");
  assert.equal(process.env.QUX, "");
  assert.equal(process.env.QUUX, undefined);

  // Load another ENV file - and overwrite any defined ENV variables.
  env(__dirname + '/.env2', {overwrite: true});
  assert.equal(process.env.FOO, "foo2");
  assert.equal(process.env.BAR, "bar2");
  assert.equal(process.env.BAZ, "2");
  assert.equal(process.env.QUX, "");
  assert.equal(process.env.QUUX, undefined);

  // Support substitutions
  env(__dirname + '/.env3', {overwrite: true, substitutions: true})
  assert.equal(process.env.BAR, "foo") // uses FOO
  assert.equal(process.env.BAZ, "substituted") // tries to use FOO_2 but defaults to "substituted"

  // Load any undefined ENV variables from a specified file, but don't crash if the file doesn't exist
  // Usefull for testing env vars in development, but using "real" env vars in production
  envfile(__dirname + '/.env', {raise: false});

```


## API

* `(filepath)`

    ```javascript
    env('./path/to/.env');
    ```

* `(filepath, options)`

    ```javascript
    env('./path/to/.env', {
        verbose: true,
        substitutions: true,
        overwrite: true,
        raise: false,
        logger: console
    });
    ```


## Installation

```shell
  $ npm install node-env-file-subst
```


## Test

**Local tests:**

```shell
  $ make test
```


## Examples

**Local examples:**

```shell
  $ make example
```


## Related Libraries

* **[node-env-flag](http://github.com/grimen/node-env-flag)**


## License

Released under the MIT license.
