---
title: Merge conflict in package-lock.json
---

We probably have all been here

```bash
$ git pull origin develop
remote: Counting objects: 14, done.
remote: Total 14 (delta 6), reused 6 (delta 6), pack-reused 8
Unpacking objects: 100% (14/14), done.
From github.com:adriaanvanrossum/feature/something-really-awesome
 * branch            develop    -> FETCH_HEAD
   41a55e3..575f291  develop    -> origin/develop
Auto-merging package.json
Auto-merging package-lock.json
CONFLICT (add/add): Merge conflict in package-lock.json
Automatic merge failed; fix conflicts and then commit the result.
```

To fix this error run these commands

```bash
rm package-lock.json
npm install
git add package-lock.json
git commit
```

Or as a oneliner so it does not continue when one command fails

```bash
rm package-lock.json && npm install && git add package-lock.json && git commit
```

Do note that this _might_ update your package versions, which defeats the purpose of why the `package-lock.json` exists. It's recommended to manually edit the `package.json` file, and run `npm install [--package-lock-only]` again, as per the [docs](https://docs.npmjs.com/files/package-locks#resolving-lockfile-conflicts).

