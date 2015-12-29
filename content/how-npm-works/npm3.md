<!--
title: 03 - npm v3
featured: true
-->

# npm v3 Dependency Resolution

## Example

Imagine we have a module, A. A requires B.

![A depends on B](/images/npm3deps1.png)

Now, let's create an application that requires module A.

On `npm install`, npm v3 will install both module A and its
dependency, module B, inside the `/node_modules` directory, flat.

In npm v2 this would have happened in a nested way.

![npm2 vs 3](/images/npm3deps2.png)

Now, let's say we want to require another module, C. C requires B,
but at another version than A.

![new module dep, C](/images/npm3deps3.png)

However, since B v1.0 is already a top-level dep, we cannot install
B v2.0 as a top level dependency. npm v3 handles this by defaulting
to npm v2 behavior and nesting the new, different, module B version
dependency under the module that requires it -- in this case, module C.

![nested dep](/images/npm3deps4.png)

**However, please note at this time since there is not a direct dependency on B in your package.json, a build server (doing a clean install) or doing a fresh install (e.g. someone building your source for the first time) may not end up with B v1.0 in the root consistently.** At this time there is no defined rule (highest version or lowest version) to ensure what will go in the root of node_modules so if you end up depending on it the safest approach would be to add it as a direct dependency (if you know you'll end up with more than one version).
