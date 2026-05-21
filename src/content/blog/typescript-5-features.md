---
title: "TypeScript 5.0 新特性全面解析"
description: "深入解读 TypeScript 5.0 带来的重要更新：装饰器、const 类型参数、枚举改进等，以及如何在实际项目中应用。"
pubDate: 2026-05-18
tags: [TypeScript, 编程语言, 前端]
category: 编程语言
---

## TypeScript 5.0 的重要更新

TypeScript 5.0 带来了多项重要改进，让我们逐一分析。

## 1. 装饰器（Decorators）

TypeScript 5.0 正式支持了 ECMAScript 装饰器标准：

```typescript
function logged(originalMethod: any, context: ClassMethodDecoratorContext) {
  const methodName = String(context.name);
  function replacementMethod(this: any, ...args: any[]) {
    console.log(`LOG: Entering method '${methodName}'.`);
    const result = originalMethod.call(this, ...args);
    console.log(`LOG: Exiting method '${methodName}'.`);
    return result;
  }
  return replacementMethod;
}

class Person {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  
  @logged
  greet() {
    console.log(`Hello, my name is ${this.name}.`);
  }
}
```

### 装饰器类型

- **类装饰器**：修改或增强类定义
- **方法装饰器**：包装方法调用
- **属性装饰器**：拦截属性访问
- **访问器装饰器**：控制 getter/setter

## 2. const 类型参数

新增 `const` 修饰符，让类型推断更精确：

```typescript
// 以前：类型推断为 string[]
function createRoute<const T extends readonly string[]>(...segments: T) {
  return segments;
}

// 现在：T 保留了字面量类型
const route = createRoute("home", "dashboard", "settings");
// 类型为 readonly ["home", "dashboard", "settings"]
```

## 3. 枚举改进

所有枚举现在都是联合枚举：

```typescript
enum Color {
  Red,    // 0
  Green,  // 1
  Blue,   // 2
}

// 安全的枚举赋值
const c: Color = Color.Red; // ✅
const d: Color = 0;         // ✅ 现在也合法
```

## 4. 模块解析改进

`--moduleResolution bundler` 模式更适合现代打包工具：

```json
{
  "compilerOptions": {
    "moduleResolution": "bundler"
  }
}
```

## 实际应用建议

1. **渐进式升级**：先在非核心模块试用装饰器
2. **const 泛型**：适用于路由定义、配置常量等场景
3. **模块解析**：新项目直接用 `bundler` 模式

> TypeScript 5.0 是一个里程碑版本，建议所有项目尽快升级。
