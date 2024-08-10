
## 类型注解
类型约束:约束了变量赋值的类型

字面量类型
```ts
type direction = 'up' | 'down' | 'left' | 'right'
```



类型别名



接口继承


```ts
interface Point2D {x:numer, y:number}
interface Point3D extends Point2D {z: number}
```

元组
包含元素,以及特定索引对应的类型


类型断言

类型宽泛
```ts
const linkEl = document.getElementById('link') as HTMLAnchorElement
```
```js title="devtool console"
console.dir($0)
__proto__: HTMLAnchorElement
```

## 类型

### 接口的继承


### 接口的实现

### 可见性修饰符
public  在类和子类和实例对象中都可见
protected 仅在类中和子类中可见，实例对象不可见
private 仅在类中可见，实例和子类中不可见


## 只读修饰符


## 类型兼容性

结构化类型系统 duck type

标明类型系统


## 泛型
类型变量
determine at call time, same at this

```ts
join(separator?:string): string
slice(state?:nubmer, end?:number): T[]
```

### 泛型工具

## 索引签名

## 映射类型

```ts
type Partial<T> = {
  [p in keyof T]?: T[p]
}

```

索引查询类型
