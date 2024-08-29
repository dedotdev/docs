# String

### `stringToU8a`

Converts a string to Uint8Array.

```typescript
import { stringToU8a } from 'dedot/utils';

stringToU8a('hello world') // new Uint8Array([0x68, 0x65, 0x6c, 0x6c, 0x6f, 0x20, 0x77, 0x6f, 0x72, 0x6c, 0x64])
```

### `stringToHex`

Converts a string to hex format

```typescript
import { stringToHex } from 'dedot/utils';

stringToHex('hello world') // '0x68656c6c6f20776f726c64'
```

### `stringCamelCase`

Converts a string to `camelCase`

```typescript
import { stringCamelCase } from 'dedot/utils';

stringCamelCase('hello world') // helloWorld
stringCamelCase('HelloWorld') // helloWorld
stringCamelCase('helloWorld') // helloWorld
stringCamelCase('hello_world') // helloWorld
stringCamelCase('hello-world') // helloWorld
```

### `stringPascalCase`

Converts a string to `PascalCase`

```typescript
import { stringPascalCase } from 'dedot/utils';

stringPascalCase('hello world') // HelloWorld
stringPascalCase('HelloWorld') // HelloWorld
stringPascalCase('helloWorld') // HelloWorld
stringPascalCase('hello_world') // HelloWorld
stringPascalCase('hello-world') // HelloWorld
```

### `stringSnakeCase`

Converts a string to `snake_case`

```typescript
import { stringSnakeCase } from 'dedot/utils';

stringSnakeCase('hello world') // hello_world
stringSnakeCase('HelloWorld') // hello_world
stringSnakeCase('helloWorld') // hello_world
stringSnakeCase('hello_world') // hello_world
stringSnakeCase('hello-world') // hello_world
```

### `stringDashCase`

Converts a string to `dash-case`

```typescript
import { stringDashCase } from 'dedot/utils';

stringDashCase('hello world') // hello-world
stringDashCase('HelloWorld') // hello-world
stringDashCase('helloWorld') // hello-world
stringDashCase('hello_world') // hello-world
stringDashCase('hello-world') // hello-world
```

### `stringLowerFirst`

Converts to first letter of a string to lower case

```typescript
import { stringLowerFirst } from 'dedot/utils';

stringLowerFirst('ABC') // aBC
stringLowerFirst('abc') // abc
```

### `stringUpperFirst`

Converts to first letter of a string to upper case

```typescript
import { stringUpperFirst } from 'dedot/utils';

stringLowerFirst('ABC') // ABC
stringLowerFirst('abc') // Abc
```
