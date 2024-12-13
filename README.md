# Spotless bug reproduction repository

This repo is a [SSCCE](https://sscce.org) of a potential bug with the Spotless formatting tool for Gradle.

### Current behavior:
If I run `spotlessApply -PspotlessIdeHook=/path/to/my/file` more than once, undoing the formatting changes after the first invocation, subsequent invocations won't apply changes to the file.

### Expected behavior:
Spotless should be consistent and idempotent, so I should be able to run it as many times as I want and get the same output each time.

***

To reproduce the issue:

1. Clone the repository.
2. Ensure [App.java](app/src/main/java/org/example/App.java) does _not_ end with a newline.
3. Format the file using the IDE hook. [format-app-file](format-app-file) is a helper script that does this for you.
4. Ensure the task output contains `IS DIRTY` and that the file now ends with a newline.
5. Remove the newline.
6. Without running `./gradlew clean`, re-run `./format-app-file`.
7. Observe that the file does not end with a newline, and that the task output does not show `IS DIRTY`.

Notes:

- Running `./gradlew clean` will reset the Gradle state to a point where the IDE hook will work again, but only on its first invocation.
- Running `./gradlew spotlessApply` _without_ the IDE hook seems to always work, regardless of the state of the cache. This will not bust the cache though.
