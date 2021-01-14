额外的属性检查：

- 变量的interface类型检查、对象字面量的interface类型检查是不一样的，前者存在interface描述的属性即可、允许更多属性存在，而后者仅允许包含interface描述的属性，所以名之为额外的属性检查；

```typescript
interface LabeledValue {
  label: string;
}

function printLabel(labeledObj: LabeledValue) {
  console.log(labeledObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);

printLabel({ size: 10, label: "Size 10 Object" });
// Object literal may only specify known properties, and 'size' does not exist in type 'LabeledValue'.
```

