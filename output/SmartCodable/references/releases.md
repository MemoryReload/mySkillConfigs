# Releases

Version history for this repository (14 releases).

## 6.0.0: V6.0.0
**Published:** 2025-12-18

协议名SmartCodable调整为SmartCodableX
```
struct User: SmartCodableX {
    var name: String = ""
}
```
解决打包xcframework，命名空间冲突问题 #121 .

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/6.0.0)

---

## 5.0.2: V5.0.2
**Published:** 2025-05-08

V5.0.2更新说明

【新增】支持继承！

【新增】支持继承！

【新增】支持继承！

```
class BaseModel: SmartCodable {
    var name: String = ""
    
    required init() { }
}

@SmartSubclass
class SubModel: BaseModel {
    var age: Int = 0
}
```

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/5.0.2)

---

## 4.4.0: V4.4.0
**Published:** 2025-05-06

V4.4.0更新说明 

【新增】提供属性包装器 `@SmartHexColor`，用来修饰UIColor.

【新增】提供属性包装器 `@SmartDate`，用来修饰Date类型。

【优化】优化CGFloat的实现代码。

【优化】提升解析性能。

【修复】某些场景下未使用初始化值的情况。

【修复】修复[Any] 类型 encode 时，transformer.toJSON不调用问题。

【删除】删除`SmartColor`类型，减少自创的类型。


[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.4.0)

---

## 4.3.8: 优化@SmartFlat的encode实现
**Published:** 2025-03-28



[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.3.8)

---

## 4.3.0: V4.3.0: 日志系统大升级
**Published:** 2024-11-25

1. 优化日志性能，减少遍历。
2. 优化属性日志的信息表达。
3. 支持日志的导出，如有需求可以上传服务器。
4. 优化日志格式：unkeyedContainer的解析格式化；格式的优化。

```
================================  [Smart Sentinel]  ================================
Array<SomeModel> 👈🏻 👀
   ╆━ Index 0
      ┆┄ a: Expected to decode 'Int' but found ‘String’ instead.
      ┆┄ b: Expected to decode 'Int' but found ’Array‘ instead.
      ┆┄ c: No value associated with key.
      ╆━ sub: SubModel
         ┆┄ sub_a: No value associated with key.
         ┆┄ sub_b: No value associated with key.
         ┆┄ sub_c: No value associated with key.
      ╆━ sub2s: [SubTwoModel]
         ╆━ Index 0
            ┆┄ sub2_a: No value associated with key.
            ┆┄ sub2_b: No value associated with key.
            ┆┄ sub2_c: No value associated with key.
         ╆━ Index 1
            ┆┄ sub2_a: Expected to decode 'Int' but found ’Array‘ instead.
   ╆━ Index 1
      ┆┄ a: No value associated with key.
      ┆┄ b: Expected to decode 'Int' but found ‘String’ instead.
      ┆┄ c: Expected to decode 'Int' but found ’Array‘ instead.
      ╆━ sub: SubModel
         ┆┄ sub_a: Expected to decode 'Int' but found ‘String’ instead.
      ╆━ sub2s: [SubTwoModel]
         ╆━ Index 0
            ┆┄ sub2_a: Expected to decode 'Int' but found ‘String’ instead.
         ╆━ Index 1
            ┆┄ sub2_a: Expected to decode 'Int' but found 'null' instead.
====================================================================================
```

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.3.0)

---

## 4.2.6: V4.2.6更新说明
**Published:** 2024-10-31

【优化】优化继承关系下的父类属性的解析。
【新增】SmartAny支持修饰Model类型。

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.2.6)

---

## 4.2.5: 优化KeyMap映射关系
**Published:** 2024-10-18



[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.2.5)

---

## 4.2.3: V4.2.3-BugFix  发布公告
**Published:** 2024-10-12

【BugFix】修改非基本数据类型的自定义解析的失效问题。
【BugFix】修复key的映射关系中，当前值为null的判断错误问题。
【新功能】提供FastTransformer快捷ValueTransformer
```
CodingKeys.name <--- FastTransformer<String, String>(fromJSON: { json in
    "abc"
},toJSON: { object in
    "123"
})
```

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.2.3)

---

## 4.2.0:  V4.1.12 发布公告
**Published:** 2024-09-27

 1. 【新功能】支持Combine，允许 @ Published修饰的属性解析。
 2. 【新功能】支持@igonreKey修饰的属性在encode时，不出现在json中（屏蔽这个属性key）
 3. 【新功能】支持encode时候的options，同decode的options使用。
 4. 【优化】Data类型在decode和encode时，只能使用base64解析.

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.2.0)

---

## 4.1.7: 4.1.7 - BugFix
**Published:** 2024-08-27

"Fixed the crash issue when entering compatibility logic due to parsing failure, and added handling for cases where the data is NaN or exceeds the Int type range."
修复了解析失败时进入兼容逻辑导致的crash问题，针对数据为NaN或超出Int类型长度的情况进行了处理。


[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.1.7)

---

## 4.1.3: 4.1.3 - SmartUpdater
**Published:** 2024-07-29

Optimize implementation of SmartUpdater.

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.1.3)

---

## 4.1.1: Optimize the precision of floating-point numbers
**Published:** 2024-07-05

When the json data is not 4.99 and the attribute is String, it is internally compatible and returns "4.99" instead of "4.899999999 ".

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.1.1)

---

## 4.0.0: new decoder, new encoder, new feature.
**Published:** 2024-06-13

V4.0.0 Release Notes
New Feature: Support for watchOS usage
New Feature: Support for visionOS usage
New Feature: Support for tvOS usage
New Feature: Custom encoder support, allowing for custom encoding, i.e., mappingForValue.
New Feature: Support for custom strategies for any type of Value, including Int, Bool, etc.
Optimization: Optimized decoder, synchronized with the master branch of Codable.
Optimization: Improved enum parsing, no longer requiring defaultCase.
Optimization: Enhanced README with detailed Chinese instructions.
Bugfix: Fixed a memory issue in concurrent logging.

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/4.0.0)

---

## 3.4.3: SmartAny使用优化
**Published:** 2024-05-15

包含SmartAny的model转json失败的优化

[View on GitHub](https://github.com/iAmMccc/SmartCodable/releases/tag/3.4.3)

---

