# burnoo's Maven repository
I've created this repository for delayed and abandoned Kotlin projects. I'm starting it as a personal public Maven (for private projects where I need them), but I'm going to take requests from community for other libraries (see [Requests](#Requests) section).

It's hosted on Azure and can be browsed [here](https://dev.azure.com/burnoo/maven/_artifacts/feed/public).

## Setup
To add this repository to your project, include the following URL in your `settings.gradle.kts` file:

```kotlin
dependencyResolutionManagement {
    repositories {
        maven {
            url = uri("https://pkgs.dev.azure.com/burnoo/maven/_packaging/public/maven/v1")
            content {
                includeVersionByRegex(".*", ".*", ".*-beap[0-9]+")
            }
        }
    }
}
```

## Published libraries
(TODO: Information about published libraries will be filled in later)

## What is `-beapN`
The `-beapN` suffix stands for burnoo's Early Access Preview. This naming convention, together with `includeVersionByRegex` in the Maven block, prevents my Maven repository from overriding official versions and allows me to provide updates for libraries. Additionally, it means that when the creator of the library releases the proper version, it can be automatically picked up and updated by dependency bots or IDEs.

#### Update examples in IDEA:
<details>
  <summary>Screens</summary>
  
  ![official-update](https://github.com/burnoo/maven/assets/17478192/d4bbdc5d-7215-44e7-a16d-4b63876e6c69)
  ![beap-update](https://github.com/burnoo/maven/assets/17478192/840aa510-d848-4178-83c4-596bec36ca3a)
</details>

## Requests
I am open to taking requests from the community. If there is a library that is important to you, please tag me in a pull request or issue. A few rules:
1. If it's a JVM library, try [JitPack](https://jitpack.io/) first.
2. I won't host any changes that I haven't reviewed first, so I need to be able to do a code review first (this means it should be written in Kotlin and shouldn't do any black magic 🪄).
3. Should not be too huge, as I have only 2GB storage for now

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
