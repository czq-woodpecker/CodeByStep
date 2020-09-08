## 集成Sentry

### 介绍

 Sentry是一家开源公司，提供了一个可以帮助我们实时发现线上问题的监控平台应用。Sentry支持的平台非常多，比如JavaScript、ReactNative、Java等等。与Sentry类似的产品还有Firebase的Crashlytics功能。

### 原理

当我们的应用发生错误时，Sentry客户端（类库或SDK）会收集相关堆栈信息，并将其发送到Sentry的服务端进行分析和统计。

### 相关概念



### 与React集成



### 与ReactNative集成

#### Add the dependency

```
yarn add @sentry/react-native
```

#### Linking

```
yarn sentry-wizard -i reactNative -p ios android

cd ios
pod install
```





### 常见问题