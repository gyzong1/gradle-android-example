创建三个 maven remote 仓库分别指向:
google()
[https://dl.google.com/dl/android/maven2/（Android](https://dl.google.com/dl/android/maven2/（Android) / Google 相关依赖，如 AndroidX、部分 Play 服务等）
mavenCentral()
[https://repo1.maven.org/maven2/（Maven](https://repo1.maven.org/maven2/（Maven) Central，大量开源库）
gradlePluginPortal()
[https://plugins.gradle.org/m2/（Gradle](https://plugins.gradle.org/m2/（Gradle) 插件门户，用于解析 plugins { id ... version ... } 等插件）



安装 SDK



sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"

yes | sdkmanager --licenses# gradle-android-example
# gradle-android-example
