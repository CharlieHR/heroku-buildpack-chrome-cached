# heroku-buildpack-chrome-cached

Installs Chrome for Testing and the libraries it needs, for Heroku CI, and caches the extracted result between builds.

Based on [heroku/heroku-buildpack-chrome-for-testing](https://github.com/heroku/heroku-buildpack-chrome-for-testing), with these changes:

- **Tracks the latest stable Chrome by default.** Each build makes one small request to find the current STABLE version. When Google releases a new one, the next build misses the cache and does a full install, then caches the new version. If the lookup fails, the build uses the last cached version. To pin a version instead, set `CHROME_FOR_TESTING_VERSION` (e.g. `156.0.8078.80`).
- **Full cache.** The unpacked Chrome and libraries are stored in `CACHE_DIR`, keyed on stack, version and package list. A cache hit is two `cp -a` calls with no network and no apt.
- **No Chromedriver.** Cuprite/Ferrum talk to Chrome directly over the DevTools protocol.
- **One apt resolution** for all packages instead of one per package.
- **`ldd` check.** The build fails early if Chrome can't find a shared library.

There's no configuration: add the buildpack to `app.json` and it works.
