# Spotless bug reproduction repository

This repo is a [SSCCE](https://sscce.org) of a potential bug with the Spotless formatting tool for Gradle.

To reproduce the issue:

1. Clone the repository.
2. Ensure [App.java](app/src/main/java/org/example/App.java) does _not_ end with a newline.
3. Format the file using the IDE hook. [format-app-file](format-app-file) is a helper script that does this for you.
4. Ensure the task output contains `IS DIRTY` and that the file now ends with a newline.
5. Without running `./gradlew clean`, re-run `./format-app-file`.
6. Observe that the file does not end with a newline, and that the task output does not show `IS DIRTY`.

Notes:

- Running `./gradlew clean` will reset the Gradle state to a point where the IDE hook will work again, but only on its first invocation.
- Running `./gradlew spotlessApply` _without_ the IDE hook seems to always work, regardless of the state of the cache. This will not bust the cache though.
