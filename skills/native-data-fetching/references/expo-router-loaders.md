- **`expo-router`**; `useLoaderData` hook
- **`expo-router/server`**; current `LoaderFunction` type; **`expo-server`**; runtime utilities such as `StatusError` and `setResponseHeaders`. Verify installed exports.
`useLoaderData` is typed via `typeof loader`; the generic parameter infers the return type.
- Loaders cannot be used in `_layout` files; only in route files
- The request object is immutable; use optional chaining (`request?.headers`) as it may be `undefined` in static mode