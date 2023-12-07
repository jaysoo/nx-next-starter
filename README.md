# Nx Next.js starter

Adapted from https://vercel.com/templates/next.js/turborepo-next-basic

This is an example of using Next.js and ESLint with virtually zero-config. It is actually negative configuration compared to the original Turborepo example.
Check out the `apps/web` and `apps/docs` apps, and you'll see no `project.json` or any mentions of Nx.

Try it!

```shell
npx nx dev web
npx nx dev docs

# or
npx nx run-many -t=dev
```

Caching works too!

```
npx nx build web

# remove build ouputs
rm -rf apps/web/.next

# the second build will be cached, but outputs are restored
npx nx build web

# see that the ouputs are restored when the build is read from cache
ls -lsa apps/web/.next
```

## How does this work?

We're using new capabilities that will soon be available in Nx and Nx plugins. Every plugin has the ability to register tasks depending on what configuration files are detected. In this repo we see `next.config.js` and `.eslintrc.js` files, which are handled by `@nx/next` and `@nx/eslint` _automatically_.

The magic is in the `nx.json` file, where we see the following line:

```json5
"plugins": ["@nx/next/plugin", "@nx/eslint/plugin"]
```

That line instructs Nx to use the two plugins to automatically infer tasks for us. The `@nx/next` plugin will find all `next.config.js` and configure the `build`, `dev`, `start` tasks automatically, with the correct `outputs` being cached. The `@nx/eslint` plugin finds all `.eslintrc` files and configures the `lint` task for each of the matching projects.

## Mental model shift and ease of adoption

Nx plugins were very restricted in teams doing things the "Nx way". Even though Nx itself doesn't care about your architecture, most of the plugins want your code to be structured in specific ways.

The new `plugins` setup allows users to benefit from Nx and Nx plugins without having to restructure existing code.


