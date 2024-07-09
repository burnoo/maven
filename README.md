# burnoo's Maven repository ☺️
I've created this repository for delayed or outdated Kotlin projects. I'm starting it as a personal public Maven (for private projects where I need them), but I'm going to take requests from community for other libraries (see [Requests](#Requests) section).

It's hosted on Azure and can be browsed [here](https://dev.azure.com/burnoo/maven/_artifacts/feed/public).

## Setup
To add this repository to your project, include this maven repository in your `settings.gradle.kts` file:

```kotlin
dependencyResolutionManagement {
    repositories {
        maven(url = "https://pkgs.dev.azure.com/burnoo/maven/_packaging/public/maven/v1") {
            content {
                includeVersionByRegex(".*", ".*", ".*-beap[0-9]+")
            }
        }
    }
}
```

## Published libraries
<details>
<summary><b><code>com.russhwolf:multiplatform-settings:1.2.0-beap1</code></b></summary>

### My fork
https://github.com/burnoo/multiplatform-settings

### Installation
In your module's dependencies:
```kotlin
commonMain {
    dependencies {
        implementation("com.russhwolf:multiplatform-settings:1.2.0-beap1")
        implementation("com.russhwolf:multiplatform-settings-coroutines:1.2.0-beap1")
        // etc.
    }
}
```
### Changes
- Merging branch `1.2` from original repository: https://github.com/burnoo/multiplatform-settings/pull/1
- Updating Gradle configuration and GitHub actions to support this Maven: https://github.com/burnoo/multiplatform-settings/pull/2

### Release details
- Published from https://github.com/burnoo/multiplatform-settings/tree/v1.2.0-beap1
- Publish GitHub action: https://github.com/burnoo/multiplatform-settings/actions/runs/9700339405 
</details>

<details>
<summary><b><code>dev.icerock.moko:geo:0.6.1-beap1</code></b></summary>

### My fork
https://github.com/burnoo/moko-geo

### Installation
In your module's dependencies:
```kotlin
commonMain {
    dependencies {
        implementation("dev.icerock.moko:geo:0.6.1-beap1")
    }
}
```
### Changes
* Add support for moko permissions 0.18.0 by @burnoo in https://github.com/burnoo/moko-geo/pull/4
* Update project dependencies versions to the newest (stack: JVM 17 / Kotlin 1.9.24 / Gradle 8.8) (full list [here](https://github.com/burnoo/moko-geo/compare/0.6.0...burnoo:moko-geo:burnoo-maven?expand=1#diff-697f70cdd88ba88fe77eebda60c7e143f6ad1286bca75017421e93ad84fb87df))

**Full Changelog**: https://github.com/burnoo/moko-geo/compare/0.6.0...release/0.6.1-beap1

### Release details
- Published from https://github.com/burnoo/moko-geo/tree/release/0.6.1-beap1
- Publish GitHub action: https://github.com/burnoo/moko-geo/actions/runs/9819227106
</details>

## What is `-beapN`
The `-beapN` suffix stands for burnoo's Early Access Preview. This naming convention, together with `includeVersionByRegex` in the Maven content block, prevents burnoo's Maven repository from overriding official versions and allows me to provide updates for libraries. Additionally, it means that when the creator of the library releases the proper version, it can be automatically picked up and updated by dependency bots or IDEs.

#### Update examples in IDEA:
<details>
  <summary>Screens</summary>
  
  ![official-update](https://github.com/burnoo/maven/assets/17478192/d4bbdc5d-7215-44e7-a16d-4b63876e6c69)
  ![beap-update](https://github.com/burnoo/maven/assets/17478192/840aa510-d848-4178-83c4-596bec36ca3a)
</details>

## Requests
I am open to taking requests from the community. If there is a library that is important to you, please tag me in a pull request or an issue. A few rules:
1. If it's a JVM library, try [JitPack](https://jitpack.io/) first.
2. I won't host any changes that I haven't reviewed first, so I need to be able to do a code review first (this means it should be written in Kotlin and shouldn't do any black magic 🪄).
3. Library should not be too huge, as I have only 2GB storage for now

## Security
To ensure the security of the libraries you are downloading, you can follow these steps:

### Checking SHA-256 checksum of the artifact 
1. Clone my fork of the library.
2. Find the commit with the version tag and check the code, commit list, and compare it with the original library.
3. Use the `publishToMavenLocal` Gradle task to generate artifacts and SHA sums.
4. Download the artifact from Maven.
5. Calculate the SHA-256 for the downloaded artifact:
 ```sh
shasum -a 256 path/to/local/artifact.jar
```
6. Compare the SHA-256 with the SHA-256 from the local Maven.

### Restrict burnoo's Maven versions
You can restrict your project to only use specific versions of the libraries by editing `includeVersionByRegex` to be more strict. For example:
```kotlin
includeVersionByRegex("com.russhwolf", "multiplatform-settings.*", "1.2.0-beap1")
```

### Azure artifacts
Azure Artifacts do not allow replacing versions in the repository, so it is impossible to change the artifact content. Once a library is verified, it should be immutable. More info [here](https://learn.microsoft.com/en-us/azure/devops/artifacts/artifacts-key-concepts?view=azure-devops#immutability).
