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

### The recommended way

It can create a few issues if you use the above method. You basically remove the enhandcements that the lock file brings.

> Yes it can have bad side effects, maybe not very often but for example you can have in `package.json` `"moduleX": "^1.0.0"` and you used to have `"moduleX": "1.0.0"` in `package-lock.json`.
> By deleting `package-lock.json` and running `npm install` you could be updating to version `1.0.999` of `moduleX` without knowing about it and maybe they have created a bug or done a backwards breaking change (not following semantic versioning).

It can change the versions you have currently installed. Usually this is not a big problem.

If you care about this, use the following way.

 - Fix conflicts in `package.json`
 - Run `npm install`

Source: [stackoverflow.com/.../deleting-package-lock-json-to-resolve-conflicts-quickly](https://stackoverflow.com/questions/54124033/deleting-package-lock-json-to-resolve-conflicts-quickly/54127283).

Thanks to [@qwen-3108 (comment)](https://github.com/adriaanvanrossum/blog.adriaan.io/issues/22#issuecomment-997697156) for pointing this out in the comments.
