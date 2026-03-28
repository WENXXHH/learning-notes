# 1.gradle下载缓慢/报错

    修改gadle/wrapper/gradle-wrapper.properties文件，找到有gradle下载地址的代码，替换国内镜像网站，
    可以加快下载速度
    如： https://mirrors.aliyun.com/macports/distfiles/gradle/

# 2.maven依赖下载报错
    在setting.gradle.kts里面配置Maven仓库

    修改成以下代码
    ————————————————————————————————————————————————————————————————————————————————————————
    pluginManagement {
        repositories {
            maven {
                url = uri("https://maven.aliyun.com/repository/google")
                content {
                    includeGroupByRegex("com\\.android.*")
                    includeGroupByRegex("com\\.google.*")
                    includeGroupByRegex("androidx.*")
                }
            }
            maven {
                url = uri("https://maven.aliyun.com/repository/central")
            }
            gradlePluginPortal()
        }
    }
    plugins {
        id("org.gradle.toolchains.foojay-resolver-convention") version "1.0.0"
    }
    dependencyResolutionManagement {
        repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
        repositories {
            maven {
                url = uri("https://maven.aliyun.com/repository/google")
            }
            maven {
                url = uri("https://maven.aliyun.com/repository/central")
            }
            mavenCentral() // 保留备用，实际不会使用
        }
    }
    
    rootProject.name = "Test2" //名字可以修改
    include(":app")
    ————————————————————————————————————————————————————————————————————————————————————————
    或
    ————————————————————————————————————————————————————————————————————————————————————————
    pluginManagement {
        repositories {
            // 1. 移除 content 配置（Gradle 9.x 不支持）
            maven { url = uri("https://maven.aliyun.com/repository/google") }
            maven { url = uri("https://maven.aliyun.com/repository/central") }
            gradlePluginPortal()
        }
    }
    
    plugins {
        id("org.gradle.toolchains.foojay-resolver-convention") version "1.0.0"
    }
    
    dependencyResolutionManagement {
        repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
        repositories {
            maven { url = uri("https://maven.aliyun.com/repository/google") }
            maven { url = uri("https://maven.aliyun.com/repository/central") }
            mavenCentral()
        }
    }
    
    rootProject.name = "Test2" //名字可以修改
    include(":app")
    ————————————————————————————————————————————————————————————————————————————————————————

# 3，