# Publishing

First, I set up webpack following the instruction on [this](https://code.visualstudio.com/api/working-with-extensions/bundling-extension) page.

To publish an extension, I have done the following:
1. `npm install -g vsce`, installs a tool for publishing extensions
2. Sign in to marketplace at `https://marketplace.visualstudio.com/VSCode` 
3. Click name on the navbar
4. Press `Create new organization`
5. Go to `User settings` -> `Personal access tokens` 
6. Create new token
    * Name is extension name (kebab case),
    * Select `All accessible organizations`, must do this to avoid 401
    * Scopes: `Marketplace` -> `Acquire`, `Publish`, `Manage`
7. Copy token
8. `vsce create-publisher YOUR-PUBLISH-NAME`,
9. `vsce publish -p YOUR-PAT`

Won't probably have to do any of these steps after they are done once.

To publish subsequest version, I do the following:
1. Change version in `package.json`. It is also possible to use `vsce publish {minor/major/patch}`
2. `vsce login YOUR-PUBLISH-NAME`
3. `vsce publish -p YOUR-PAT`

# Troubleshooting

### Issue: "command '...' not found" when using the extension in production
The extension doesn't work because when bundling, not all the required files were bundled. To do it properly and make it work use the following solution.
Solution: make sure all the files necessary for the bundle are not ignored in .vscodeignore file. `npm run webpack` and then `vsce publish`. Then, you can again ignore things, like node_modules because they are not needed anymore for the bundling.

### Issue: new changes to the extension are not there when trying to debug it
New changes are not there, because it all depends on where does `main` property in package.json point to.
Solution: make sure `main` property in package.json points to `./out`, not `./dist`