[![Build Status](https://github.com/axetroy/deno_cross_env/workflows/test/badge.svg)](https://github.com/axetroy/deno_cross_env/actions)

### Generic pool for Deno

> Manage the resource pool, like connection...

### Features

- [x] Graceful to create/destroy the resource
- [x] Easy to Use
- [x] test cover

### Usage

```typescript
import { Pool } from "https://deno.land/x/std/pool/mod.ts";

// fake db
interface Db {
  query(params: string): Promise<any>; // query data
  disconnect(): void; // disconnect
}

function connectToDataBase(): Db {
  return {
    query: async (params: string) => {
      return "axetroy";
    },
    disconnect: () => {}
  };
}

const pool = new Pool<Db>({
  min: 1,
  max: 10,
  creator: async (pool, resourceID) => {
    return connectToDataBase();
  },
  destroyer: async (pool, resource) => {
    return resource.resource.disconnect();
  }
});

// get resource from pool
const conn = await pool.get();

const name: string = await conn.query("name");

console.log("My name is: ", name);
```

## License

The [MIT License](LICENSE)
