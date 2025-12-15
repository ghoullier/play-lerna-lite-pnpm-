# lerna-lite-pnpm-catalog-issue

> [!NOTE]
> This repository is a test case for the [lerna-lite](https://github.com/lerna/lerna-lite) package.

## Patch

```patch
--- a/dist/utils/collect-updates/lib/make-diff-predicate.js
+++ b/dist/utils/collect-updates/lib/make-diff-predicate.js
@@ -102,14 +102,14 @@ export function diffWorkspaceCatalog(prevTag, npmClient) {
             const yamlFilename = getClientConfigFilename(npmClient);
             const yamlPath = join(cwd, yamlFilename);
             if (existsSync(yamlPath)) {
-                const prevYamlStr = execSync(`git show ${prevTag}:${yamlFilename}`);
+                const prevYamlStr = execSync('git', ['show', `${prevTag}:${yamlFilename}`]);
                 const currYamlStr = readFileSync(yamlPath, 'utf8');
                 prevConfig = extractCatalogConfigFromYaml(npmClient, prevYamlStr);
                 currConfig = extractCatalogConfigFromYaml(npmClient, currYamlStr);
             }
         }
         else if (npmClient === 'bun' && existsSync(rootPkgPath)) {
-            const prevJsonStr = execSync(`git show ${prevTag}:package.json`);
+            const prevJsonStr = execSync('git', ['show', `${prevTag}:package.json`]);
             const currJsonStr = readFileSync(rootPkgPath, 'utf8');
             prevConfig = extractCatalogConfigFromPkg(prevJsonStr);
             currConfig = extractCatalogConfigFromPkg(currJsonStr);

```

## Issue

`execSync` from `execa` fails when called with a command with args instead of the process to spawn.
