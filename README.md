# Keystone 6 on Heroku

A simple example project showing how one might develop and deploy a [KeystoneJS 6](https://keystonejs.com)
backend to [Heroku](https://www.heroku.com/).

The app code in this project is based heavily on the
[`with-auth` example project](https://github.com/keystonejs/keystone/tree/master/examples/with-auth) from the main
[Keystone repo](https://github.com/keystonejs/keystone).
It demonstrates some of the powerful APIs and tools Keystone provides and the gratifying developer experience it enables.

If you haven't heard about [Keystone](https://keystonejs.com), it's a powerful GraphQL-based headless CMS, written in TypeScript
It has some terrific features out of the box, is easy to extend, and a joy to use.
There's [documentation](https://keystonejs.com/docs) covering all the
[APIs](https://keystonejs.com/docs/apis) and
[field types](https://keystonejs.com/docs/apis/fields) used in this project, as well as
[guides](https://keystonejs.com/docs/guides) to take you further.

If you get stuck, hit us up on the [KeystoneJS Slack](https://community.keystonejs.com) and search (or post) under the
[`[keystonejs]` tag](https://stackoverflow.com/questions/tagged/keystonejs) on Stack Overflow.

## How to Use This Repo

There are different ways to approach this repo depending on what you're trying to achieve:

* **[Deploy directly to Heroku](#deploy-directly-to-heroku) (for free!)** –
Click the "Deploy" button below to build and run a live copy of this app on your own Heroku account.
This is a good way to play around with the Keystone Admin UI without setting up a local dev environment.
* **[Download the code to experiment locally](#develop-locally)** –
Git clone or download the source code to experiment with Keystone 6 on your own machine.
Customise the lists and Admin UI, play with the GraphQL endpoint, setup access control and more.
* **[Start a new project](#start-a-new-project)** –
Want to use this repo as a template for your own project?
Fork it to your GitHub account and start hacking.
The instructions below guide you through creating a Heroku app using the "push to deploy" workflow.
You'll have a powerful and immensely flexible GraphQL backend, live on the Internet, in no time
* **Just look around** –
Don't need to run the app?
Browse the source code here on GitHub as a reference implementation.
We've tried to include helpful comments inline and the `with-auth` example project (on which this app is based) has further
[documentation](https://github.com/keystonejs/keystone/tree/master/examples/with-auth).

## Deploy Directly to Heroku

[![Deploy Directly to Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/keystonejs/keystone-6-heroku-example)

This magical button 👆 takes a copy of this repo, builds it into an image and runs it on a Heroku dyno with the Postgres addon.
All you need to get started is a [Heroku account](https://signup.heroku.com).

These resources are created on the [free tier](https://www.heroku.com/pricing) so, naturally,
have some performance limitations and will "sleep" if no request are made for a few minutes.
With this option you also won't be able to modify the source code or data schema (for that, you'll need to setup a local dev environment, see below).
Regardless this is a great, low investment way to play with the Keystone Admin UI from a user perspective.

## Develop Locally

This codebase can be run and developed on your local machine by setting up a simple development environment.

First, ensure you have the following tools installed:

* [git](https://git-scm.com/downloads)
* [NodeJS](https://nodejs.org/en/download) (v14.13 or higher)
* [Yarn](https://classic.yarnpkg.com/en/docs/install) (or `npm` will also work)
* [PostgreSQL](https://www.postgresql.org/download)

If you're on MacOS and use [Homebrew](https://brew.sh), you can install all these at once:

```sh
brew install git node yarn postgresql
```

If you're on a different platform, click though the links above and follow the relevant instructions.

Once the prerequisites are installed we can download the app code:

```sh
# Download the repo
git clone https://github.com/keystonejs/keystone-6-heroku-example
cd keystone-6-heroku-example

# Install the node packages
yarn

# Start the app
yarn dev
```

Then, point your browser to [localhost:3000](http://localhost:3000).
You'll be prompted to set an email address and password before being taken to the Keystone Admin UI.

### Modifying the App

This app is extremely simple –
it has only a two lists and intentionally avoids the more advanced Keystone functionality (like
[hooks](https://keystonejs.com/docs/guides/hooks), the
[document field](https://keystonejs.com/docs/guides/document-fields),
[access control](https://keystonejs.com/docs/guides/access-control), etc.).
This has let us organised most of the Keystone code into two files:

* `schema.ts` defines the [Keystone schema](https://keystonejs.com/docs/apis/schema) –
the set of lists, fields and relationships on which your Keystone app operates.
* `keystone.ts` controls the [system configuration](https://keystonejs.com/docs/apis/config) –
how Keystone runs and connects to other services, as well as authentication and sessions.

You'll need to restart the Keystone process before changes made to the source code are reflected in the app.
To do so simply stop the process (<kbd>Ctrl</kbd> + <kbd>c</kbd>) and re-run `yarn dev`.

## Start a New Project

There are many way of creating a Keystone application.
You can install packages manually to create something from scratch (ie. `yarn add @keystone-next/keystone`) or use a guided process like
[`create-keystone-app`](https://keystonejs.com/docs/walkthroughs/getting-started-with-create-keystone-app).
Alternatively, you can copy and modify an existing app like this repo.

To do so, [fork](https://github.com/keystonejs/keystone-6-heroku-example/fork)
or otherwise copy these files to your own repository then follow the instructions above to
[setup a local development environment](#develop-locally).

Note, Keystone is usually used as a [headless CMS](https://en.wikipedia.org/wiki/Headless_content_management_system),
meaning your users will visit a separate, frontend application or website, which uses the Keystone API behind the scenes.
The creation and deployment of this frontend app is beyond the scope of this guide.

### Deployment

Heroku supports a number of [deployment strategies](https://blog.heroku.com/six-strategies-deploy-to-heroku)
all of which can be used with Keystone.
You've already seen the "Deploy to Heroku" button above.
It's a fun demo but isn't suited for actually developing and repeatedly deploying an application to production.
Here we'll use the ["push to deploy" strategy](https://devcenter.heroku.com/articles/git).
This process uses the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
to create and configure a new app then git to deploy changes.

First, ensure you have the Heroku CLI installed and authenticated to your Heroku account.
If you're on MacOS the suggested method is Homebrew (for other platforms, see the [download options](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)):

```sh
# Get the Heroku cli (if you don't have it already)
brew tap heroku/brew && brew install heroku

# Follow the prompts to authenticate
heroku auth:login
```

Once the CLI tooling is authenticated, run these commands from the root of your app repo.
You'll need to replace `my-keystone-app` with a key that will identify you app.
This will be used to identify your app later and must be globally unique.

```sh
# Create the new app and configure a free postgres addon
# This also adds a git remote to your repo called "heroku"
heroku create my-keystone-app --addons=heroku-postgresql:hobby-dev

# Adding a SESSION_SECRET env var will allow sessions to persist between dyno restarts and deploys
heroku config:set SESSION_SECRET=$(head -c50 /dev/urandom | base64 | tr -dc 'A-Za-z0-9' | head -c50)

# Deploy your latest commit by pushing the main branch to the heroku remote
git push heroku main
```

It'll take a few minutes to build and deploy, then your app will appear online.
The last section of the build output will give you the public URL, similar to this:

```
remote: -----> Launching...
remote:        Released v1
remote:        https://my-keystone-app.herokuapp.com/ deployed to Heroku
```

### Migrations

If you're developing an app, sooner or later, you're going to need to change the database structure.

The first time you ran `yarn dev` on your dev machine Keystone created a database and setup the initial schema
by applying the SQL in the `/migrations/20210825070616_initial_schema` directory.
This migration was generated by Keystone based on the contents of `schema.ts`.
You can leverage this same functionality to create your own migrations.

For example, lets open `schema.ts` and add a `phone` field to the Person list, like this:

```js
Person: list({
  fields: {
    name: text({ isRequired: true }),
    phone: text(),  // <- New!

    // Existing fields ...
  },
});
```

Re-running `yarn dev` will prompt us to create and apply a migration representing this change:

```
✨ There has been a change to your Keystone schema that requires a migration
✔ Name of migration … adding phone numbers
✨ A migration has been created at migrations/20210825230709_adding_phone_numbers
✔ Would you like to apply this migration? … yes
✅ The migration has been applied
```

Behind the scenes, this magic is being performed by
[Prisma](https://www.prisma.io) and [Prisma Migrate](https://www.prisma.io/docs/concepts/components/prisma-migrate).
The resultant SQL (in `/migrations`) can be committed to git like any other file.
Once it's applied, your database, GraphQL schema and Admin UI will all reflect the updated list schema.

This codebase is setup to automatically applied outstanding migrations as part of the build process executed by Heroku.
You can see this in `package.json` – the `build` script is `keystone-next build && keystone-next prisma migrate deploy`.

In practise this means database changes are run against your Heroku database a short time before the new version of the application is rolled out.
As such, you should ensure your database changes are backwards compatible with the previous release of your app (or risk runtime errors while the deployments are occurring).

There are many strategies for deploying and coordinating app updates and migrations.
The approach we've taken here is simple and works sufficiently well for this infrastructure setup but your app may have different needs.
See the [CLI docs](https://keystonejs.com/docs/guides/cli) for the underlying commands used.

### Next Steps

For ideas on what's possible with Keystone,
see the [guides](https://keystonejs.com/docs/guides) section of the website,
checkout the [roadmap](https://keystonejs.com/updates/roadmap) and
stay tuned to the project [updates](https://keystonejs.com/updates).
