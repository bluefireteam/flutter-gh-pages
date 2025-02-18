# Flutter GH Pages

Automates the build and deployment of your Flutter web app on Github gh pages

# Action

To use this action, create an action like the following on your workflows folder

```yml
name: Gh-Pages

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - uses: subosito/flutter-action@v2
      - uses: bluefireteam/flutter-gh-pages@v7
```

In order to push the changes, the **GITHUB_TOKEN** needs to have **write** permissions.
This can be set in Repository 🠚 Settings 🠚 Actions 🠚 General.
See the [documentation](https://docs.github.com/en/actions/security-guides/automatic-token-authentication#modifying-the-permissions-for-the-github_token).
![image](https://github.com/bluefireteam/flutter-gh-pages/assets/564768/22e3ed49-03bf-4b30-ad96-e16dc6613106)

To build a project in a folder other that the root, use the `workingDir` property

```yml
      ...
      - uses: bluefireteam/flutter-gh-pages@v7
        with:
          workingDir: example
```

By default, the action will use the auto setting for web renderers, to change that you can use the `webRenderer` property.
<br>
Note: the --web-renderer argument is deprecated in flutter 3.29.0 hence it will be ignored flutter version is or above 3.29.0

More on web renderers here: https://flutter.dev/docs/development/tools/web-renderers

```yml
      ...
      - uses: bluefireteam/flutter-gh-pages@v7
        with:
          webRenderer: canvaskit
```

You can also specify if you want to build to WebAssembly with the `compileToWasm` property. If specified the `webRenderer` property will be ignored.

More on WebAssembly here: https://docs.flutter.dev/platform-integration/web/wasm

```yml
      ...
      - uses: bluefireteam/flutter-gh-pages@v7
        with:
          compileToWasm: true
```

By default, the action will send the files to the `gh-pages` branch, which is the default used by Github Pages.
If you need to change that, the `targetBranch` property can be used

```yml
      ...
      - uses: bluefireteam/flutter-gh-pages@v7
        with:
          targetBranch: my-gh-pages-branch
```

By default, the generated page only works on `User or Organization sites` ex:`user.github.io/`. 
You can change that by specifying the `baseHref` argument, so the site will work on a `Project Site`, ex:`user.github.io/repoName`.

The parameter `baseHref` must start and end with a forward slash `"/"`.
(For projects created on flutter version earlier than `2.5.0`, please manually edit the file `web/index.html`, changing the line `<base href="/">` to `<base href="$FLUTTER_BASE_HREF">`)

```yml
      ...
      - uses: bluefireteam/flutter-gh-pages@v7
        with:
          baseHref: /my-repo/
```

To pass arguments to the builder with `--dart-define` the `customArgs` property can be used

```yml
      ...
      - uses: bluefireteam/flutter-gh-pages@v7
        with:
          customArgs: --dart-define="simple=example"
```


And consumed in the code via (**const** is mandatory!):
```dart
void main() async {
  String arg = const String.fromEnvironment('simple'); // arg = "example"
  ...
}
```


To use github pages with a custom domain, add a file named `CNAME` to the
`<project>/web` folder whose contents is your domain, like:
> subdomain.domain.com
