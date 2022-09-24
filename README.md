# SPA Configuration - React `envsubst`

Example of using a separate JS file and `envsubst` to inject deploy-time configuration to a React single-page app.

## Notes

- Bootstrapped using [Create React App]
- If `envsubst` isn't available in your environment you can install it as part of the [`gettext`][gettext] package with
  e.g. `apt-get install gettext`.

## Usage

This demo includes two sets of configuration:

- development (in `src/configuration.js`); and
- production (in `build/config.js`).

The latter is _generated_ from `config.template.js` (which is copied from `public/` into `build/` by `react-scripts`)
using the NPM `configure` script. Any `${VAR_NAME}` in the template will be replaced with the corresponding value from
the local environment. This script assumes the `build/` directory will exist and needs to be re-run after any rebuild:

```shell
$ npm run build

> react-envsubst@0.1.0 build
> react-scripts build

Creating an optimized production build...
Compiled successfully.

File sizes after gzip:

  46.64 kB  build/static/js/main.36b327cb.js
  1.79 kB   build/static/js/787.54cb1c77.chunk.js
  541 B     build/static/css/main.073c9b0a.css

The project was built assuming it is hosted at /.
You can control this with the homepage field in your package.json.

The build folder is ready to be deployed.
You may serve it with a static server:

  npm install -g serve
  serve -s build

Find out more about deployment here:

  https://cra.link/deployment

$ MESSAGE=Production npm run configure

> react-envsubst@0.1.0 configure
> envsubst < build/config.template.js > build/config.js

```

This may seem closer to build-time than run-time configuration, but note that rather than rebuilding the whole
application we're switching a single trivial file. You don't even need Node/NPM installed to do this, let alone all of
the package's dependencies; the script is provided as a convenience for the local developer, but you can just use
`envsubst` directly in e.g. CI environments. You just store the `build/` directory, which includes the template, as
your single artefact and regenerate configuration at _deploy_ time as it's promoted through your environments.

[Create React App]: https://create-react-app.dev/
[gettext]: https://www.gnu.org/software/gettext/
