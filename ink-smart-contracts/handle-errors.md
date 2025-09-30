# Handle errors

Interacting with a contract often resulting in errors at runtime level ([DispatchError](https://docs.rs/frame-support/latest/frame_support/pallet_prelude/enum.DispatchError.html)) or contract-level ([LangError](https://use.ink/4.x/faq/migrating-from-ink-3-to-4#add-support-for-language-level-errors-langerror)). Whenever running into these errors, Dedot will throw an Error containing specific context about the problem so developers can handle this accordingly.

### Handle errors when deploying contracts

```typescript
import {
  isContractInstantiateDispatchError, isContractInstantiateLangError
} from "dedot/contracts";
import { FlipperContractApi } from "./flipper";

const ALICE = '...';

try {
  // Dry-run contract construction
  const dryRun = await deployer.query.new(true, { caller: ALICE })

  // ...
} catch (e: any) {
  if (isContractInstantiateDispatchError<FlipperContractApi>(e)) {
    // Getting a runtime level error (e.g: Module error, Overflow error ...)
    const { dispatchError, raw } = e;
    const errorMeta = client.registy.findErrorMeta(dispatchError);
    // ...
  }

  if (isContractInstantiateLangError<FlipperContractApi>(e)) {
    const { langError, raw } = e;
    console.log('LangError', langError);
  }

  // Other errors ...
}
```

### Handle errors when interacting with contracts

```typescript
import {
  isContractDispatchError, isContractLangError
} from "dedot/contracts";
import { FlipperContractApi } from "./flipper";

const ALICE = '...';

try {
  // Dry-run mutable contract message
  const dryRun = await contract.query.flip({ caller: ALICE })

  // ...
} catch (e: any) {
  if (isContractDispatchError<FlipperContractApi>(e)) {
    // Getting a runtime level error (e.g: Module error, Overflow error ...)
    const { dispatchError, raw } = e;
    const errorMeta = client.registy.findErrorMeta(dispatchError);
    // ...
  }

  if (isContractLangError<FlipperContractApi>(e)) {
    const { langError, raw } = e;
    console.log('LangError', langError);
  }

  // Other errors ...
}
```
