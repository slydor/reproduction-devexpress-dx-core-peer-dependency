# Update
The core issue is fixed with v4.0.9 and this repository is not needed anymore. See the tracking issue of DevExpress: https://supportcenter.devexpress.com/ticket/details/t1250987/reactive-yn0002-warning-appears-on-an-attempt-to-restore-the-dx-react-chart-package-with

# Reproduction repository

This project reproduces an issue of peer dependencies in `@devexpress` packages in combination with yarn workspace packages.

When run `yarn install`, it will warn with:

```sh
➤ YN0000: ┌ Resolution step
➤ YN0002: │ @devexpress/dx-react-chart@npm:4.0.8 [3a687] doesn't provide @devexpress/dx-core (pab92a), requested by @devexpress/dx-chart-core
➤ YN0000: │ Some peer dependencies are incorrectly met; run yarn explain peer-requirements <hash> for details, where <hash> is the six-letter p-prefixed code
```

And `yarn explain peer-requirements pab92a` yields:
```sh
➤ YN0000: @devexpress/dx-react-chart@npm:4.0.8 [3a687] doesn't provide @devexpress/dx-core, breaking the following requirements:

➤ YN0000: @devexpress/dx-chart-core@npm:4.0.8 [8b493] → 4.0.8 ✘
```

## Workaround

Adding this to [.yarnrc.yml](./.yarnrc.yml) hot fixes the issue:

```yaml
packageExtensions:
  # This is a workaround until https://supportcenter.devexpress.com/ticket/details/t1250227/peer-dependency-not-specified-properly-from-dx-react-chart-to-dx-core is fixed.
  '@devexpress/dx-react-chart@4.0.8':
    peerDependencies:
      '@devexpress/dx-core': '4.0.8'
```
