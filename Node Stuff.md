# Node Stuff

## Creating local node packages
It is possible to create local node packages and then install them (and all their dependencies) in a different local module.
`npm pack` will create a .tgz file. You can then run `npm install {relative path to tgz file}` in the directory where you want to install that module and all it's dependencies.
