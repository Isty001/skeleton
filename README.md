# skeleton
Some very basic project skeletons.

#### Libraries

```bash
cp -R ./skeleton/lib ~/projects/my_fanyc_lib
cd ~/projects/my_fanyc_lib
make project-init # to remove unnecessary .gitkeeps
``` 

The usual tasks are available: `make` to build, `make install` to install, and `make test` to run the test executable.
Build task will create `libfoo.so.(MAJOR).(MINOR).(PATCH)` with `libfoo.so.(MAJOR)` SONAME. 

The install already has a basic versioning, something like this:

```bash
/usr/local/lib/libfoo.so -> /usr/local/lib/libfoo.so.(MAJOR)
/usr/local/lib/libfoo.so.(MAJOR) -> /usr/local/lib/libfoo.so.(MAJOR).(MINOR).(PATCH)
/usr/local/lib/libfoo.so.(MAJOR).(MINOR).(PATCH)
```

Keep in mind, the `LD_LIBRARY_PATH` env variable might need to contain the path (`/usr/local/lib`)
 
