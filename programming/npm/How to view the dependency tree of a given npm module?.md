#npm #node 

You can generate NPM dependency trees without the need of installing a dependency by using the command

```
npm ls --all
```

This will generate a dependency tree for the project at the current directory and print it to the console. (The `all` option shows all transitive dependencies, not just those directly depended upon by the current project - see [the documentation](https://docs.npmjs.com/cli/v7/commands/npm-ls#all).)

You can get the dependency tree of a specific dependency like so:

```
npm ls [dependency]
```

You can also set the maximum depth level by doing

```
npm ls --depth=[depth]
```

Note that you can only view the dependency tree of a dependency that you have installed either globally, or locally to the NPM project.

Alternative

```
npm view <PACKAGE> dependencies
```

It prints **only the direct dependencies**, not the whole tree.