
# DiffPlug [Blowdryer](https://github.com/diffplug/blowdryer) scripts

[![License Apache 2.0](https://img.shields.io/badge/license-apache--2.0-brightgreen.svg)](https://tldrlegal.com/license/apache-license-2.0-(apache-2.0))
[![Changelog](https://img.shields.io/badge/keepachangelog-yes-brightgreen.svg)](#changelog)
[![CircleCI](https://circleci.com/gh/diffplug/blowdryer-diffplug.svg?style=shield)](https://circleci.com/gh/diffplug/blowdryer-diffplug)

## Users

| user                                                                     | updated on  | to version |
| :----------------------------------------------------------------------- | :---------- | :--------- |
| [blowdryer](https://github.com/diffplug/blowdryer)                       | 2021-12-16  | `5.1.3`    |
| [durian-globals](https://github.com/diffplug/durian-globals)             | 2020-06-05  | `3.2.2`    |
| [durian-rx](https://github.com/diffplug/durian-rx)                       | 2020-01-12  | `3.1.0`    |
| [durian-swt](https://github.com/diffplug/durian-swt)                     | 2021-12-16  | `5.1.3`    |
| [goomph](https://github.com/diffplug/goomph)                             | 2020-12-16  | `5.1.3`    |
| [libxdiff](https://github.com/diffplug/libxdiff)                         | 2021-02-18  | `3.3.0`    |
| [image-grinder](https://github.com/diffplug/image-grinder)               | 2020-12-16  | `5.1.3`    |
| [spotless-changelog](https://github.com/diffplug/spotless-changelog)     | 2021-12-16  | `5.1.3`    |
| *internal projects*      |  |  |
| [buildcloset](http://gitlab.diffplug.local/diffplug/buildcloset/)        | 2020-01-12  | `3.2.5`    |
| website                                                                  | 2021-05-28  | `5.0.2`    |

## Available scripts (without `.gradle` extension)

- **spotless/freshmark** - applies to `*.gradle` and `*.md`
  - if `com.diffplug.spotless-changelog` is applied in this or the parent project, then `versionLast` will be set in freshmark
- **spotless/java** - applies to `*.gradle` and java sourcesets
  - `干.proj('license', 'supported: [apache, confidential, lgpl-2.1]')`
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
  - `干.proj('license', 'supported: [apache, confidential, lgpl-2.1]')`
- **base/javadoc-agg** - aggregates javadoc from subprojects
  - `干.proj('javadoc_agg', 'space-delimited list of projects containing javadoc to be aggregated')`
  - (also a subset of **base/maven**)
- **base/sonatype** - publishes to mavenCentral
  - requires `id 'io.github.gradle-nexus.publish-plugin' version '1.0.0'`
  - unless `enable_publishing` is set in `gradle.properties`, this will be skipped
  - `干.proj('nexus_user', 'username for nexus/mavencentral')`
  - `干.proj('nexus_pass', 'password for nexus/mavencentral')`
  - `干.proj('gpg_key64',  'gpg --export-secret-keys --armor <key-id>')`
  - `干.proj('gpg_passphrase', 'passphrase for unlocking gpg_key')`
  - (also a subset of **base/maven**)
- **base/osgi** - adds OSGi metadata to the jar
  - requires `id 'com.diffplug.osgi.bndmanifest' version '3.22.0'` (or later)
  - `干.proj('osgi_export', '-exportcontents bnd directive')`
  - `干.proj('osgi_symbolic_name', 'Bundle-SymbolicName')`
  - (also a subset of **base/maven**)
- **base/autovalue** - adds Google AutoValue 1.7
  - requires `id 'com.diffplug.eclipse.apt' version '3.22.0'` (or later)
  - `干.proj('VER_AUTOVALUE', 'the autovalue version the user wants')`
- **swt/svg-images** - renders SVG images from `src/svg` into `src/main/resources/svg-rendered` at 1x and 2x DPI (in-line with SWT high-DPI support)
  - recommend adding `svg-rendered/` to `.gitignore`
  - requires `id 'com.diffplug.image-grinder' version '2.1.2'` (or later)

# Changelog

## [Unreleased]

## [6.0.0] - 2022-12-30
### Fixed
- **BREAKING** New minimum requirements `gradle:7.6` and `com.gradle.plugin-publish:1.1.0`, adapted to those changes.
- **BREAKING** Remove support for `org.jdrupes.mdoclet`.

## [5.2.1] - 2022-04-01
### Fixed
- `base/kotlin` now specifies `freeCompilerArgs = ["-Xjvm-default=all"]` so that Kotlin interface method implementations can be used from Java.

## [5.2.0] - 2022-02-15
### Added
- `base/kotlin.gradle`
### Fixed
- Make the eclipse plugin optional for `gradle-plugin.gradle`

## [5.1.3] - 2021-12-16
### Fixed
- Update sonatype URL to [`https://s01.oss.sonatype.org/`](https://s01.oss.sonatype.org/) since we migrated for better performance.

## [5.1.2] - 2021-11-10
### Fixed
- Fix sonatype publishing for multi-module projects.

## [5.1.1] - 2021-10-21
### Fixed
- `gpg64` from sonatype is now read using `干.proj` as intended.

## [5.1.0] - 2021-09-18
### Added
- Builds that do not have Sonatype publish plugin applied now assume that they are publishing to GitLab.

## [5.0.6] - 2021-07-18
### Fixed
- Bugfix for previous.

## [5.0.5] - 2021-07-18 [YANKED]
### Fixed
- Now encodes gpg key using Base64 which actually works on CircleCI.

## [5.0.4] - 2021-07-18 [YANKED]
### Fixed
- We were sometimes deploying gradle plugins without also deploying to maven, now fixed.
- We yanked all earlier versions of 5.0.x to force an update because this is a fairly pernicious error.

## [5.0.3] - 2021-07-17 [YANKED]
### Fixed
- `sourcesJar` no longer throws warnings on Gradle 7.1
- Only use `org.jdrupes.mdoclet` on Java 8 and below

## [5.0.2] - 2021-05-30 [YANKED]
### Fixed
- no more Gradle 7.0 warnings abour `generateMetadataFileForPluginMavenPublication` having undeclared dependencies.

## [5.0.1] - 2021-05-29 [YANKED]
### Fixed
- no more errors on machines where `enable_publishing` hasn't been set

## [5.0.0] - 2021-05-28 [YANKED]
### Added
- **BREAKING** `base/autovalue` now requires a `VER_AUTOVALUE` parameter

## [4.0.3] - 2021-05-11
### Fixed
- yet more sonatype fixes

## [4.0.2] - 2021-04-10
### Fixed
- more sonatype fixes

## [4.0.1] - 2021-04-09
### Fixed
- fix sonatype deployment

## [4.0.0] - 2021-04-09
### Added
- `base/sonatype` has been added (because **BREAKING** `base/bintray` has been removed)

## [3.3.0] - 2021-02-19
### Added
- Added `lgpl-2.1` as a supported license.

## [3.2.5] - 2020-10-14
### Changed
- Adopt the new `spotless` plugin coordinate at `com.diffplug.spotless`
- Bump `VER_AUTOVALUE` from `1.7.3` to `1.7.4`.

## [3.2.4] - 2020-06-16
### Changed
- We now do formatting relative to `origin/main`.
- Added missing `(C)` in our apache copyright header.
- Bump `VER_AUTOVALUE` from `1.7` to `1.7.3`.

## [3.2.3] - 2020-06-06
### Fixed
- `spotlessApply -PspotlessSetLicenseHeaderYearsFromGitHistory=true` now updates all files (as intended) rather than only the changed ones.

## [3.2.2] - 2020-06-06
### Fixed
- `org.jdrupes.mdoclet` was failing to resolve when used with `javadoc-agg.gradle`

## [3.2.1] - 2020-06-05
### Changed
- We now require Spotless `4.3+`, because we do formatting relative to `origin/master`.
  * If you run `spotlessApply -PspotlessSetLicenseHeaderYearsFromGitHistory=true` then every license header will be updated appropriately

## [3.2.0] - 2020-04-07
### Added
- `base/autovalue` for applying Google AutoValue
- `swt/svg-images` renders SVG images with SWT high-DPI support
### Fixed
- Added an error message to prevent accidental misuse of the `git_url` parameter
### Changed
- Better short names for `license=confidential`
- `javadoc-markdown.gradle` now requires that `id 'org.jdrupes.mdoclet' version '1.0.9'` (or later) is on the classpath
- Migrate from `com.diffplug.gradle.x` to `com.diffplug.x` for goomph

## [3.1.0] - 2020-01-12
### Added
- Support `confidential` as a value for `license`.

## [3.0.0] - 2020-01-11
### Added
- Inserted commented-out `干.mustRunAfter`.  No behavior change, but we'll try to keep these up-to-date in case [diffplug/blowdryer#8](https://github.com/diffplug/blowdryer/issues/8) gets implemented. ([a9c3489](https://github.com/diffplug/blowdryer-diffplug/commit/a9c34895a00c4a7ee2e76db3545aa6a12bd4effa))
- Every javadoc task now has `javadocView` task, which will render and open the javadoc in a browser.
  - **BREAKING** Requires that at least one of the [`goomph`](https://github.com/diffplug/goomph) plugins is on the classpath.
- Every javadoc task now uses [jdrupes-mdoclet](https://github.com/mnlipp/jdrupes-mdoclet).

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
