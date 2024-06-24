# 快速开始
<br>
<Badge type="tip" text="^1.9.0" /><br>

## 1. 根目录build.gradle 引入插件
::: code-group

```kotlin [build.gradle.kts]
buildscript {
    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
    dependencies {
        // 按需引入
        classpath(libs.codeGen.java)
        classpath(libs.codeGen.kotlin)
    }

}
```
```groovy [build.gradle]
buildscript {
    repositories {
        google()
        mavenCentral()
        gradlePluginPortal()
    }
    dependencies {
        // 按需引入
        classpath(libs.codeGen.java)
        classpath(libs.codeGen.kotlin)
    }

}
```
:::
## 2. app/build.gradle 配置插件
::: code-group

```kotlin [app/build.gradle.kts]
plugins {
    id("com.android.application")
    id("com.android.library")
    id("com.configweaver")
}
```
```groovy [app/build.gradle]
plugins {
    id 'com.android.application'
    id 'com.android.library'
    id 'com.configweaver'
}
```
:::
## 使用 json 生成配置

### 添加配置文件 生成 Kotlin
在 app 目录下创建 config 目录，添加如下配置文件
::: code-group
```json [app/config/main/network_config.json]
{
    "base_url": "https://www.xxx.com",
    "print_log": true,
    "timeout": {
        "read": 1000,
        "write": 1000,
        "connect": 1000
    }
}
```
```json [app/config/main/feature_config.json]
{
 "enable_log": true,
 // 是否显示支付页面
 "show_pay_page": true,
}
```
::: 
在重新 build 后会在 app/build/generated/source/configWeaver 目录下生成如下 Kotlin 文件

::: code-group
```kotlin [NetworkConfig.kt]
public object NetworkConfig {
  /**
   * key:base_url value:https://www.xxx.com
   */
  public const val baseUrl: String = "https://www.xxx.com"

  /**
   * key:print_log value:true
   */
  public const val printLog: Boolean = true

  /**
   * key:timeout - value:{"read":1000,"write":1000,"connect":1000}
   */
  public object Timeout {
    /**
     * key:read value:1000
     */
    public const val read: Int = 1000

    /**
     * key:write value:1000
     */
    public const val write: Int = 1000

    /**
     * key:connect value:1000
     */
    public const val connect: Int = 1000
  }
}
```
```kotlin [FeatureConfig.kt]
public object FeatureConfig {
  /**
   * key:enable_log value:true
   */
  public const val enableLog: Boolean = true

  /**
   * key:show_pay_page value:true
   */
  public const val showPayPage: Boolean = true
}
```
:::

<details>
  <summary>查看代码2</summary>

``` vue
<template>
  <el-button type="primary">主要按钮</el-button>
  <el-button type="success">绿色按钮</el-button>
  <el-button type="info">灰色按钮</el-button>
  <el-button type="warning">黄色按钮</el-button>
  <el-button type="danger">红色按钮</el-button>
</template>
```

</details>

## 4. 多变体 BuildType 支持
在上方 network_config.json 配置网络请求使用的 host
如果我们希望在 debug 包中使用测试环境 

在 app/config/debug/中创建network_config.json 添加如下配置
```json{2}
{
    //覆盖 app/config/main/network_config.json 中的base_url属性
    "base_url": "https://www.xxx-test.com",
}
```
选择编译 debug 包 将会生成如下 NetworkConfig.kt 代码
```kotlin{3,5}
public object NetworkConfig {
  /**
   * key:base_url value:https://www.xxx-test.com
   */
  public const val baseUrl: String = "https://www.xxx-test.com"// [!code focus]

  /**
   * key:print_log value:true
   */
  public const val printLog: Boolean = true

  /**
   * key:timeout - value:{"read":1000,"write":1000,"connect":1000}
   */
  public object Timeout {
    /**
     * key:read value:1000
     */
    public const val read: Int = 1000

    /**
     * key:write value:1000
     */
    public const val write: Int = 1000

    /**
     * key:connect value:1000
     */
    public const val connect: Int = 1000
  }
}
```
可以看到 最终生成的配置会将 **main**、**debug** 两个文件夹内的同名 json文件进行合并





