# nextjs-starter-repo

NextJS starter repository with TailwindCSS & TypeScript

## Usage

1. Fork this repository
2. `git clone https://github.com/[your-username]/nextjs-starter-repo.git`
3. `cd nextjs-starter-repo && yarn install`
4. `vercel link`
5. Get a token from Vercel [here](https://vercel.com/account/tokens).
6. Add `VERCEL_ORG_ID`, `VERCEL_PROJECT_ID`, & `VERCEL_TOKEN` to your GitHub Secrets [here](https://github.com/rockchalkwushock/nextjs-starter-repo/settings/secrets/actions). (`ordId` & `projectId` come from the `.vercel/` generated when running `vercel link`)
7. Update configurations, generators, templates as needed.
8. Build dope things! 🎉

## Under The Hood

This repository is setup to opt-in to Webpack 5 by default and is opted-in to strict mode for `react`. It also makes use of a lot of dev tools and configurations:

### `@commitlint`, `commitizen`, & `cz-conventional-changelog`

These three packages are used in tandem to create good commits so you don't end up with something like this:

```text
abc123 did a thing
def456 did another thing
```

Instead proper conventions will be enforced so you end up with something like:

```text
abc123 feat(pages): added about page
def456 test(pages/about): added tests for about page
```

This is all done by running the command `yarn commit`. Running to normal `git commit -m` will result in an error so you are forced to run this command.

If you like aliasing commands add the below to your `.bashrc` or `.zshrc`:

```bash
alias yc="yarn commit"
```

- [`@commitlint` Docs](https://commitlint.js.org/#/)
- [`commitizen` Docs](http://commitizen.github.io/cz-cli/)
- [`cz-conventional-changelog` Docs](https://github.com/conventional-changelog/conventional-changelog)

The configuration for `commitlint` and `commitizen` can be found in the `package.json`.

### `husky` & `lint-staged`

`husky` is used to run git hooks and `lint-staged` is used to format, lint, and test any code `pre-commit`.

In this repository you can see that we committing a laundry list of files will be formatted via `prettier` and all our `.(js|ts|tsx)` files will be linted and tested.

- [`husky` Docs](https://typicode.github.io/husky/#/)
- [`lint-staged` Docs](https://github.com/okonet/lint-staged)

The configuration for `lint-staged` can be found in the `package.json`.

### `dot-plop` & `plop`

`dot-plop` and `plop` give the repository the ability to have generators. `dot-plop` specifically gives the TypeScript typings for easier use of `plop`. You can read more about the generator and templates in this repository further below.

#### Generators FTW 🎉🎉🎉

> "Automate away the boring stuff."
>
> [Insert developer name] after doing the same thing over and over again.

In this repository there is one generator called `feature`. Running `yarn new feature` will give you a list of options to choose from. The output below is just an example of what is possible. You can update the templates as desired and expand the current generator or add as many as you feel like just make note each generator must have a unique name so if you add a generator say `helloWorld` the command will be `yarn new helloWorld`

```shell
~/Desktop/Projects/nextjs-starter-repo on setup-repository > yarn new feature                                                                        at 16:08:59
yarn run v1.22.10
$ plop feature
? What will this feature require? API Endpoint, Component, Hook, Typings, Service, Page
? What should the api endpoint be named? sign-in
? What should the path to the endpoint be? (no need to include /api/) auth
? What should the component be named? SignInForm
? What should the hook be named? useSignIn
? What should the interface be named? sign-in
? What should the service be named? sign-in
? What should the page be named? SignIn
? What is the path to the page? (i.e. pages/hello/world/)
✔  ++ /pages/api/auth/sign-in.ts
✔  ++ /components/SignInForm.tsx
✔  ++ /hooks/useSignIn.ts
✔  ++ /interfaces/signIn.ts
✔  ++ /lib/signIn.ts
✔  ++ /pages/signin.tsx
✨  Done in 52.01s.
```

- [`plop` Docs](https://plopjs.com/)

### `prettier`

`prettier` is used for formatting the code in the repository. The configuration can be updated in the `package.json`.

To manually format the code base run `yarn format`.

- [`prettier` Docs](https://prettier.io/)

### `@testing-library/jest-dom`, `@testing-library/react`, `jest`, & `ts-jest`

There are a few scripts for testing:

```shell
yarn test
yarn test:ci
yarn test:coverage
yarn test:watch
```

I cannot for the life of me find the article I read about using `maxWorkers` and `runInBand` to give credit to that individual. the TL;DR is these flags will speed up the test suites.

The configuration for `jest` can be found in the `package.json`. Please note the `babel` config is exposed in the `package.json` as well this is there for the reason of `jest` understanding the code should be passed through `babel` before executing the tests. Hence the `ts-jest.babelConfig: true` piece of the configuration.

- [`@testing-library` Docs](https://testing-library.com/)
- [`jest` Docs](https://jestjs.io/)
- [`ts-jest` Docs](https://kulshekhar.github.io/ts-jest/)

### `sass` & `tailwindcss`

This repository uses `sass` and `tailwindcss` for styling. As per the Tailwind docs `postcss` is also being used.

You can find the configurations for `postcss` in the `postcss.config.js` and `tailwindcss` in the `tailwind.config.js`.

- [`sass` Docs](https://sass-lang.com/)
- [`tailwindcss` Docs](https://tailwindcss.com/)

### `typescript`

`typescript` is what is used for type-checking the code base.

The repository is making use of _aliasing_ via TypeScript. Below is a table of the alias mapping:

| Alias           | Path              | Usage                 |
| --------------- | ----------------- | --------------------- |
| `@components/*` | `components/*`    | `@components/Footer`  |
| `@hooks/*`      | `hooks/*`         | `@hooks/useAnalytics` |
| `@interfaces/*` | `interfaces/*`    | `@interfaces/blog`    |
| `@layouts/*`    | `layouts/*`       | `@layouts/PostLayout` |
| `@lib/*`        | `lib/*`           | `@lib/mdx`            |
| `@pages/*`      | `pages/*`         | `@pages/index`        |
| `@test-utils`   | `utils/testUtils` | `@test-utils`         |
| `@utils/*`      | `utils/*`         | `@utils/dateTime`     |

It is important to note any changes made to the `compilerOptions.paths` property in the `tsconfig.json` **MUST** be reflected in the `jest.moduleNameMapper` or the test suite will fail.

```json
{
  "compilerOptions": {
    "paths": {
      "@newAlias": ["dir/*"]
    }
  }
}
```

```json
{
  "jest": {
    "moduleNameMapper": {
      "^@newAlias(.*)$": "<rootDir>/dir/$1"
    }
  }
}
```

The configuration can be found in the `tsconfig.json`.

To manually check the code base run: `yarn type-check`.

- [`typescript` Docs](https://www.typescriptlang.org/)

### GitHub Actions

This repository makes use of [GitHub Actions](https://github.com/features/actions) for all the continuous integration & development needs. Under `.github/` you will find a `dependabot.yml` which is the configuration for [Dependabot](https://dependabot.com/) which keeps all the dependencies (including GitHub Actions) up-to-date.

You will also find `.github/workflows/` this is where the CI/CD workflows are kept. There are specifically three workflows:

1. Feature
2. Staging
3. Release

The repository is meant for you to follow a git flow similar to the below:

```shell
git branch --list
# You should have production and staging.
# checkout from staging to create a feature-branch
git checkout -b feature-one
# build a cool feature
git add .
# don't forget to use this command!
yarn commit
git push
# Open a PR against staging
# feature.yml will then run.
# If all checks pass then you can merge to staging.
```

#### feature.yml

This workflow will `lint`, `type-check`, `test`, and run the `build` to make sure the code is building properly.

If all of these checks pass then the [Vercel Action](https://github.com/marketplace/actions/vercel-action) will execute and as long as you provide teh `VERCEL_*` environment variables referenced above you will end up with a preview url of the current code that will look similar to this:

```text
https://[project-name]-[hash]-[vercel-username].vercel.app
```

You can change the preview url to look like the below by going into the `feature.yml` and uncommenting the `alias-domain` property and updating as necessary:

```text
https://pr-1.[project-name]
```

#### staging.yml

The `staging` workflow is pretty much identical to the `feature` workflow however the Vercel Action can create a preview url like below with the same setup as above:

```text
https://staging.[project-name]
```

### release.yml

This workflow simply releases your app into a production environment and can be aliased to a domain on Vercel or use the base domain Vercel gives you i.e. (`https:/[project-name].vercel.app`)

> Please note if you wish to use different branch names you will need to change them in the each workflow file so they run correctly as well as update branch names on GitHub.

## Contributing

Please visit [CONTRIBUTING.md](https://github.com/rockchalkwushock/nextjs-starter-repo/blob/master/CONTRIBUTING.md)

## License

[MIT](https://github.com/rockchalkwushock/nextjs-starter-repo/blob/master/LICENSE) © [Cody Brunner](https://codybrunner.dev)
