
# DiffPlug [Blowdryer](https://github.com/diffplug/blowdryer) scripts

[![License Apache 2.0](https://img.shields.io/badge/license-apache--2.0-brightgreen.svg)](https://tldrlegal.com/license/apache-license-2.0-(apache-2.0))
[![Changelog](https://img.shields.io/badge/keepachangelog-yes-brightgreen.svg)](#changelog)
[![](https://jitci.com/gh/diffplug/blowdryer-diffplug/svg)](https://jitci.com/gh/diffplug/blowdryer-diffplug)

## Available scripts (without `.gradle` extension)

- **spotless/freshmark** - applies to `*.gradle` and `*.md`
  - if `com.diffplug.spotless-changelog` is applied in this or the parent project, then `versionLast` will be set in freshmark
- **spotless/java** - applies to `*.gradle` and java sourcesets
  - `干.proj('license', 'supported: apache')`
- **base/java8** - sets up java 8 with UTF-8, clean eclipse projects, and mavenCentral
- **base/changelog** - pulls version information from a changelog in either the same project or the parent project
- **base/gradle-plugin** - sets up gradle plugin metadata and plugin portal publishing, fixes eclipse to hook gradle
  - requires `id 'com.gradle.plugin-publish' version '0.10.1'`
  - `干.proj('git_url', 'the git url with no protocol, e.g.: github.com/diffplug/blowdryer')`
  - `干.proj('plugin_tags', 'space-delimited list of tags for the gradle plugin portal')`
  - `干.proj('plugin_list', 'space-delimited list of plugin names')`
    - `干.proj("plugin_${plugin}_id", "for ${plugin}: apply plugin: 'id'")`
    - `干.proj("plugin_${plugin}_impl", "for ${plugin}: implementationClass")`
    - `干.proj("plugin_${plugin}_name", "for ${plugin}: name for the plugin portal")`
    - `干.proj("plugin_${plugin}_desc", "for ${plugin}: description for the plugin portal")`
    - optional: `"plugin_${plugin}_tags" : space-delimited list of tags to override plugin_tags only for ${plugin}`
- **base/maven** - sets up maven-publish and javadoc
  - `干.proj('git_url', 'the git url with no protocol, e.g.: github.com/diffplug/blowdryer')`
  - `干.proj('maven_group', 'the maven group, recommend com.diffplug')`
  - `干.proj('maven_artifact', 'the maven artifactId')`
  - `干.proj('maven_name', 'human-friendly name')`
  - `干.proj('maven_desc', 'human-friendly description')`
  - `干.proj('javadoc_links', "space-delimited links, if you add '/package-list' to the urls you should get a package list")`
  - `干.proj('license', 'supported: apache')`
- **base/javadoc-agg** - aggregates javadoc from subprojects
  - `干.proj('javadoc_agg', 'space-delimited list of projects containing javadoc to be aggregated')`
  - (also a subset of **base/maven**)
- **base/javadoc-markdown** - allows markdown in javadoc using [jdrupes-mdoclet](https://github.com/mnlipp/jdrupes-mdoclet)
- **base/bintray** - publishes to bintray and mavenCentral
  - requires `id 'com.jfrog.bintray' version '1.8.4'`
  - unless `enable_publishing` is set in `gradle.properties`, this will be skipped
  - `干.proj('bintray_user', 'username for bintray')`
  - `干.proj('bintray_pass', 'password for bintray')`
  - `干.proj('nexus_user', 'username for nexus/mavencentral')`
  - `干.proj('nexus_pass', 'password for nexus/mavencentral')`
  - (also a subset of **base/maven**)
- **base/osgi** - adds OSGi metadata to the jar
  - requires `id 'com.diffplug.gradle.osgi.bndmanifest' version '3.18.1'`
  - `干.proj('osgi_export', '-exportcontents bnd directive')`
  - `干.proj('osgi_symbolic_name', 'Bundle-SymbolicName')`
  - (also a subset of **base/maven**)

## Users

| user                                                                  | updated on  | to version                                                    |
| :-------------------------------------------------------------------- | :---------- | :------------------------------------------------------------ |
| [blowdryer](https://github.com/diffplug/blowdryer)                    | 2020-01-09  | [2.0.0](https://github.com/diffplug/blowdryer-diffplug#200---2020-01-02) |
| [spotless-changelog](https://github.com/diffplug/spotless-changelog)  | 2020-01-01  | [2.0.0](https://github.com/diffplug/blowdryer-diffplug#200---2020-01-02) |
| [durian-globals](https://github.com/diffplug/durian-globals)          | 2020-01-01  | [2.0.0](https://github.com/diffplug/blowdryer-diffplug#200---2020-01-02) |

# Changelog

## [Unreleased]
### Added
- Inserted commented-out `干.mustRunAfter`.  No behavior change, but we'll try to keep these up-to-date in case [diffplug/blowdryer#8](https://github.com/diffplug/blowdryer/issues/8) gets implemented. ([a9c3489](https://github.com/diffplug/blowdryer-diffplug/commit/a9c34895a00c4a7ee2e76db3545aa6a12bd4effa))
- Added `base/javadoc-markdown`
- Every javadoc task now has `javadocView` task, which will render and open the javadoc in a browser.
  - **BREAKING** Requires that at least one of the [`goomph`](https://github.com/diffplug/goomph) plugins is on the classpath.
### Changed
- Javadoc now no longer includes any group in the top header, since there's often not enough space.

## [2.0.0] - 2020-01-02
### Added
- `base/changelog`, which pulls version information from a changelog in either the same project or the parent project
  - Also hooked into `spotless/freshmark`, to pass the `versionNext` and `versionLast` properties
- `base/bintray`, for pushing to bintray and mavenCentral
- `base/osgi`, for OSGi metadata
- `base/javadoc-agg`, for aggregating javadoc from subprojects into one central artifact
- support for per-plugin tags in `base/gradle-plugin`.
- `spotless/freshmark` now sets `versionLast` property if there is a `spotless-changelog` plugin applied somewhere
  - **BREAKING** the spotless plugin must be declared in `settings.gradle` or `buildSrc`.  That's probably a good pattern in general.
### Changed
- The spotless license header is now `https`, and we also force the year to 2020 since we're about to release a bunch of new code in 2020.
### Fixed
- Set javadoc to use `UTF-8` encoding.
- If you apply `spotless/java` to the root project, it will now fix `buildSrc/*.gradle` in addition to the `*.gradle` which it already fixed.

## [1.0.0] - 2019-12-10
First release.

<!-- END CHANGELOG -->

# Acknowledgements

- Thanks to [Vladimir Sitnikov](https://github.com/vlsi) for his [mdoclet workaround](https://github.com/autostyle/autostyle/blob/f201199bc327887225db38ae34261bfb4a861527/buildSrc/src/main/kotlin/mdoclet.gradle.kts).
- Thanks to [Lars Grefer](https://github.com/larsgrefer) and [Dennis Fricke](https://github.com/Frisch12) for
  - [`JavadocUtf8Plugin`](https://github.com/freefair/gradle-plugins/blob/6d6f5ff6036e7da1c329075a02c6452c0bb669be/maven-plugin/src/main/java/io/freefair/gradle/plugins/maven/javadoc/JavadocUtf8Plugin.java), licensed under the [MIT License](https://github.com/freefair/gradle-plugins/blob/6d6f5ff6036e7da1c329075a02c6452c0bb669be/LICENSE), used [here](https://github.com/diffplug/blowdryer-diffplug/blob/9fcb5e22b9ba2cc1c0884c4fb60b4080687b2435/src/main/resources/base/javadoc-agg.gradle#L38-L41).
  - [`JavadocJarPlugin`](https://github.com/freefair/gradle-plugins/blob/6d6f5ff6036e7da1c329075a02c6452c0bb669be/maven-plugin/src/main/java/io/freefair/gradle/plugins/maven/javadoc/JavadocJarPlugin.java), licensed under the [MIT License](https://github.com/freefair/gradle-plugins/blob/6d6f5ff6036e7da1c329075a02c6452c0bb669be/LICENSE), used [here](https://github.com/diffplug/blowdryer-diffplug/blob/9fcb5e22b9ba2cc1c0884c4fb60b4080687b2435/src/main/resources/base/javadoc-agg.gradle#L52-L60).
- Thanks [Netflix Build Language Plugins (nebula)](https://github.com/nebula-plugins) for [`NebulaAggregateJavadocPlugin`](https://github.com/nebula-plugins/gradle-aggregate-javadocs-plugin/blob/8deca214e2ec2463b7d23c46d17b33edf60a2360/src/main/groovy/nebula/plugin/javadoc/NebulaAggregateJavadocPlugin.groovy), licensed under the [`Apache 2.0 license`](https://github.com/nebula-plugins/gradle-aggregate-javadocs-plugin/blob/8deca214e2ec2463b7d23c46d17b33edf60a2360/LICENSE), used [here](https://github.com/diffplug/blowdryer-diffplug/blob/9fcb5e22b9ba2cc1c0884c4fb60b4080687b2435/src/main/resources/base/javadoc-agg.gradle#L22-L28).
