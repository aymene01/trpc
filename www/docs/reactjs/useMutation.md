---
id: mutations
title: useMutation()
sidebar_label: useMutation()
slug: /react-mutations
---

> The hooks provided by `@trpc/react` are a thin wrapper around React Query. For in-depth information about options and usage patterns, refer to their docs on [Mutations](https://react-query.tanstack.com/guides/mutations).

Works like react-query's mutations - [see their docs](https://react-query.tanstack.com/guides/mutations).

### Example

<details><summary>Backend code</summary>

```tsx title='server/routers/_app.ts'
import { initTRPC } from '@trpc/server'
import { z } from 'zod';

export const t = initTRPC()()

export const appRouter = t.router({
  // Create procedure at path 'login'
  // The syntax is identical to creating queries
  login: t
    .procedure
    // using zod schema to validate and infer input values
    .input( 
      z.object({
        name: z.string(),
      })
    )
    .mutation(({ input }) => (
      // Here some login stuff would happen
      return {
        user: {
          name: input.name,
          role: 'ADMIN'
        },
      };
     ))
})
```

</details>

```tsx
import { trpc } from '../utils/trpc';

export function MyComponent() {
  // This can either be a tuple ['login'] or string 'login'
  const mutation = trpc.proxy.login.useMutation();

  const handleLogin = async () => {
    const name = 'John Doe';

    mutation.mutate({ name });
  };

  return (
    <div>
      <h1>Login Form</h1>
      <button onClick={handleLogin} disabled={mutation.isLoading}>Login</button>

      {mutation.error && <p>Something went wrong! {mutation.error.message}</p>}
    </div>
  );
}
```