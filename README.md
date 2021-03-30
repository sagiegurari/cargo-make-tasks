# cargo-make-tasks

[![Release](https://img.shields.io/github/v/release/sagiegurari/cargo-make-tasks)](https://github.com/sagiegurari/cargo-make-tasks/releases)
[![license](https://img.shields.io/github/license/sagiegurari/cargo-make-tasks)](https://github.com/sagiegurari/cargo-make-tasks/blob/master/LICENSE)

> Reusable makefiles for [cargo-make](https://sagiegurari.github.io/cargo-make/).

* [Overview](#overview)
* [Usage](#usage)
* [Contributing](.github/CONTRIBUTING.md)
* [Release History](CHANGELOG.md)
* [License](#license)

<a name="overview"></a>
## Overview
The cargo-make-tasks repository contains makefiles with tasks specific for a programing language or environment.<br>
While cargo-make comes with many built in tasks, those are either generic tasks (such as git actions) or specific for rust (such as cargo actions).<br>
However, this repository aims to provide additional tasks that can be used as is for projects that have different requirements.

<a name="usage"></a>

The following example does the following:

* Pulls the cmake.toml from the cargo-make-tasks repository
* Pulls several generic tomls from cargo-make repository (git, github and toml)
* Loads all makefiles so all tasks defined in them are now available

```toml
# Include the requested makefile (in this example, cmake.toml) and optionally also
# other generic makefiles from cargo-make directly.
extend = [
  { path = "./target/cargo-make/git.toml" },
  { path = "./target/cargo-make/github.toml" },
  { path = "./target/cargo-make/toml.toml" },
  { path = "./target/cargo-make/cmake.toml" }
]

[config]
load_script = '''
#!@duckscript
# setup directory for external makefiles
mkdir ./target/cargo-make

# load the makefile from the cargo-make-tasks repository (in this example, cmake.toml)
file = set cmake
content = http_client --method GET https://raw.githubusercontent.com/sagiegurari/cargo-make-tasks/master/src/${file}.toml
writefile ./target/cargo-make/${file}.toml ${content}

# Optionally, load additional generic makefiles from cargo-make repository
files = array git github toml
for file in ${files}
  content = http_client --method GET https://raw.githubusercontent.com/sagiegurari/cargo-make/master/src/lib/descriptor/makefiles/${file}.toml
  writefile ./target/cargo-make/${file}.toml ${content}
end
release ${files}
'''
```

## Contributing
See [contributing guide](.github/CONTRIBUTING.md)

<a name="history"></a>
## Release History

See [Changelog](CHANGELOG.md)

<a name="license"></a>
## License
Developed by Sagie Gur-Ari and licensed under the Apache 2 open source license.
