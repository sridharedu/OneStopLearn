# Build Tools Combined Notes

## 001.Build_Tools-what-are-the-different-maven-scopes.md
✨ **Different Maven Dependency Scopes**
- → **compile:**
    - → Dependency is required during compilation of source code and test code.
    - → Will be bundled if creating a WAR or EAR file.
- → **test:**
    - → Dependency is required only for the compilation and execution of tests.
- → **provided:**
    - → Dependency is required for compilation and testing.
    - → Should *not* be included in the bundled WAR or EAR file.
    - → It is expected to be provided by the application server, web container, or environment where the application is deployed.
- → **import:**
    - → Makes sense only in the `pom.xml` of parent projects with packaging type `pom`.
    - → Usually used in combination with the `dependencyManagement` section to import dependencies from another POM.

## 002.Build_Tools-snapshots-vs-release.md
✨ **Maven Snapshot vs Release Versions**
- → **Snapshot Version (e.g., `1.0.0-SNAPSHOT`):**
    - → Indicates that the project is still under active development.
    - → If another project depends on a snapshot version, it should expect new code changes frequently (e.g., daily).
    - → Every time a commit happens, even with the same version number, new changes will be added to the snapshot.
- → **Release Version (e.g., `1.0.0`):**
    - → Indicates that the project is ready for release.
    - → There will be no `-SNAPSHOT` in the version string.
    - → No more code changes are expected for this particular version.
    - → Any project that depends on a release version will get the exact same copy, regardless of how many times they pull the library.

## 003.Build_Tools-how-to-control-dependencies.md
✨ **How to Control Dependencies in Maven**
- → **Multi-Module Project with Parent POM:**
    - → Create a multi-module project with a parent `pom.xml`.
    - → The parent `pom.xml`'s packaging type should be `pom` (not `jar` or `war`).
- → **`dependencyManagement` Section:**
    - → Define a `dependencyManagement` section in the parent `pom.xml`.
    - → List all required dependencies and their versions within this section.
- → **Child Projects:**
    - → In child projects, define the parent project in their `pom.xml`.
    - → When a child project needs a dependency, it uses the `<dependency>` block but *does not* specify the version.
    - → The version will be automatically derived from the parent's `dependencyManagement` section, ensuring consistency across all projects.
- → **Plugin Management:**
    - → Similarly, define a `pluginManagement` section in the parent `pom.xml` for plugins and their versions.
    - → Child projects simply use the plugin without specifying the version.

## 004.Build_Tools-how-to-override-a-transitive-dependency-version.md
✨ **How to Override a Transitive Dependency Version**
- → **Transitive Dependency:** When a project defines a dependency, and that dependency itself requires another library to function, the required library is a transitive dependency, which Maven automatically pulls.
- → **Overriding Version:** Yes, you can override the version of a transitive dependency.
- → To override, explicitly define the transitive dependency in your project's `pom.xml`.
- → Specify the exact version you want to use for that dependency.
- → By default, Maven pulls the version required by the direct dependency, but explicit definition allows you to enforce a specific version.
