# booty: the biggest thing to happen to ECMAScript since Netscape

[![license](https://img.shields.io/badge/license-BSD-3)](LICENSE.md)

# SUMMARY

booty is a convention for writing development tasks for JavaScript (Node.js) projects, via [npm run](https://docs.npmjs.com/cli/v10/using-npm/scripts) hooks.

# EXAMPLE

* [example](example)

# ABOUT

Tasks are implemented using modern JavaScript code. We can use a directory like `tasks/` to house task definitions.

## tasks/groceries

Here, a `groceries` task

```js
#!/usr/bin/env node
'use strict';

import child_process from 'node:child_process';
import process from 'node:process';

export default function groceries() {
    child_process.execSync('echo milk...', { stdio: 'inherit' });
    child_process.execSync('echo eggs...', { stdio: 'inherit' });
    child_process.execSync('echo just browsing', { stdio: 'inherit' });
}

function main() {
    groceries();
}

if (import.meta.url === `file://${process.argv[1]}`) { main(); }
```

Take care to avoid colliding with [conventional](https://docs.npmjs.com/cli/v10/using-npm/scripts) NPM life cycle task names.

## tasks/clean

It's often a good idea to configure a `clean` task to automate resetting the development environment.

```js
#!/usr/bin/env node
'use strict';

import fs from 'node:fs';
import process from 'node:process';

export default function clean() {
    console.log('removing junk files...');

    fs.rmSync('nosuchdirectory', { force: true, recursive: true });
    fs.rmSync('nosuchfile.dat', { force: true });
}

function main() {
    clean();
}

if (import.meta.url === `file://${process.argv[1]}`) { main(); }
```

## tasks/all

Subtasks can be aggregated together into higher level tasks.

Let's prepare `all` our errands to trigger together.

```js
#!/usr/bin/env node
'use strict';

import groceries from './groceries';
import clean from './clean';

import process from 'node:process';

function main() {
    groceries();
    clean();
}

if (import.meta.url === `file://${process.argv[1]}`) { main(); }
```

You can group or divide tasks into hierarchy shapes as needed, depending on the complexity of the tasks and the frequency of low level tasks.

Then, wire up each task to the `npm run` system.

## package.json

```json
{
    "name": "@mcandre/hello-booty",
    "scripts": {
        "all": "./tasks/all",
        "groceries": "./tasks/groceries",
        "clean": "./tasks/clean"
    },
    "type": "module"
}
```

# NOTES

Compared to modern task runners, the `npm run` system has some quirks:

* `npm run` accepts only one task name at a time. To trigger multiple tasks, either submit separate `npm run <task a>`, `npm run <task b>` commands, or create an aggregate task that invokes the subtasks.
* `npm run` has no concept of a default task. We can adopt the convention `npm run all`, in reverence to traditional makefiles.

## Why not _____?

* Grunt is largely unmaintained, and increases the attack surface.
* Gulp is terrible at CLI tasks, and increases the attack surface.
* Make isn't JavaScript.

# REQUIREMENTS

* [Node.js](https://nodejs.org/en/) 20+

# RESOURCES

Prior art, personal plugs, and tools for developing software (including non-JS projects)!

* Inspiration from [nobuild](https://github.com/tsoding/nobuild) - a convention for C/C++ build systems
* [bashate](https://github.com/openstack/bashate) - shell script linter
* [Gradle](https://gradle.org/) - JVM build system
* [lake](https://luarocks.org/modules/steved/lake) - Lua task runner
* [Leiningen](https://leiningen.org/) + [lein-exec](https://github.com/kumarshantanu/lein-exec) - Clojure task runner
* [Mage](https://magefile.org/) - Go task runner
* [mcandre/linters](https://github.com/mcandre/linters) - curated linter collection
* [mcandre/tinyrick](https://github.com/mcandre/tinyrick) - Rust task runner
* [npm](https://www.npmjs.com/), [Grunt](https://gruntjs.com/) - Node.js task runners
* [POSIX make](https://pubs.opengroup.org/onlinepubs/009695299/utilities/make.html) - general purpose task runner
* [Rake](https://ruby.github.io/rake/) - Ruby task runner
* [Rebar3](https://www.rebar3.org/) - Erlang build system
* [sbt](https://www.scala-sbt.org/index.html) - Scala build system
* [Shake](https://shakebuild.com/) - Haskell task runner

🍑
