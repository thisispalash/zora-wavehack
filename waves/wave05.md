# :computer: Wave 05
> Refactor to monorepo

As mentioned in the [README](../README.md), the primary work done this wave was beginning a 
refactor to a monorepo. The reason is because there were a lot of repeated components as well a 
need for a common authentication layer. The approach taken is that of OAuth (while the platform 
will deploy and manage smart wallets for the users on the backend).

You can see the repo here ~ [`thisispalash/semicolon-monorepo`](https://github.com/thisispalash/semicolon-monorepo)

## The Plan

- [x] Turborepo for all domains, the auth layer, and shared components
- [x] OAuth Flow for user registration and logins
- [ ] Implement new UI for writing portion
- [ ] Semicolon Backend, for deployments, LLM calls, and indexing

## Updates

- Turborepo still has some build issues
- OAuth flow is ~90% complete
- `apps` and backend is still pending, moved to [wave06](./wave06.md)