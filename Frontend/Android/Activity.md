# Activity

## Activity的4种启动模式

Activity的启动模式可以通过AndroidManifest中的Activity的android：launchMode属性设置。分为以下4种：

* standard：标准模式，默认值。每次启动Activity，都去创建这个Activity的实例。
* singleTop：Task栈顶复用模式。如果Task栈顶有正要启动的Activity实例，则直接使用该实例，而不是去重新实例化
* singleTask：Task栈内复用模式。
* singleInstance：全局单例模式。无论在哪个栈里有该Activity的实例都直接复用。

