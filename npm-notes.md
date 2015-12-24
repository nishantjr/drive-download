npm-notes
=========

In our second go at this, we're playing with the JavaScript API, run via
node.js, and using the npm package manager to pull in the appropriate
libraries. The npm package manager is not well understood by us, so
these are notes to try to bring us towards a document that can properly
explain what we're supposed to be doing in relation to that and how we
do it.


Installing node.js/npm/etc.
---------------------------

Avoid Linux system packages; they're ancient. We're using the
latest stable version (5.3.0 as of this writing) from the
[node.js](https://nodejs.org/en/) website.

In the spirit of developers being able to work without root access,
unpack the above download under `$HOME/local` or whatever turns your
crank, and "install" it by prependin `$HOME/local/bin` to your path.

When you do package installs ("global" and/or "local"), there are two
directories that will be modifed: `$HOME/local` and `$HOME/.npm`. Feel
free to put these under revision control with git or whatever if you
want to see what these are doing. It seems that, in short:

* `local` (the node install directory) includes installed
  packages. Much of these go under `lib/node_modules`, but other
  standard directories are also used: e.g., command-line tools (such
  as `npm` itself) will go under `local/bin`. There doesn't appear to
  be any package archive data or meta-information here, aside from the
  standard package.json.
* `$HOME/.npm` contains package archives (in `.tgz` format) and probably
  meta-information related to download packages and the like. This
  appears to be all cached material that can be replace should it all be
  deleted.
* `node_modules` when under an arbitrary directory contains "locally"
  installed modules, which is how you add modules to individual
  projects. The standard [node.js require search path][node_modules]
  will find these.
  
[node_modules]: https://nodejs.org/api/modules.html#modules_loading_from_node_modules_folders

There are [instructions for dealing with permissions
problems](https://docs.npmjs.com/getting-started/fixing-npm-permissions)
in the npm docs, if you need to do a global install somewhere (perhaps
for Windows?).


Documentation Summary
---------------------

The [npm docs](https://docs.npmjs.com/) are a little awkward to follow,
being 1-2 paragarphs per page (plus a video, which cjs did not watch)
with no next/back buttons. Here's what you need to know from the first
30 or so pages.

For the commands below, `npm help _command_` will show the manpage.

#### Updating npm

npm is included with node.js, but you can use npm to update to the
latest version:

The command to upgrade is slightly different between _Getting Started_
section 2 and the common text stuff at the bottom of each page of that
guide:

    npm install npm -g          # Section 2
    npm install npm@latest -g   # Stuff at bottom

For the moment, use whichever you like until we figure out what each
means. (They do produce different results in the package.json files.)

#### Installing Packages

Packages can be installed 'globally' (in the node/npm installation
directory) or 'locally' (under a `node_modules` directory in the current
directory). See the section above for notes on directories used for
this.

Basic syntax is:

    npm install [options] [<@scope>/]<name>@<version range>

In place of the _`name`_ you may also use an HTTP or a Git remote URL,
`github:`, `gist:`, etc.

e.g.,

    npm install lodash      # local install
    npm install -g lodash   # global install

Flags:
* `--dry-run`: show what would be done without doing it.
* `--save`: package will appear in **dependencies**
* `--save-dev`: package will appear in **dev-dependencies**
* `--save-exact`: saves exact rather than minimum version
* `--production`: do not install **devDependencies**

There's also a 'shrinkwrap' thing that needs further research.

#### npm Configuration

`npm config` gets and sets configuration information. The `ls` or `list`
subcommand shows important and/or non-default config info; the `-l` option
to that includes all defaults. `get` will print a specific config item.

#### package.json
