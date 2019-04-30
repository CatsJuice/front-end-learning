# FOR OF

**遍历数组**

数组如下：
```
const fruits = ['Apple', 'Banna', 'Orange', 'Mango'];
```

ES6之前：
```
    // es6之前：

    // 1. for循环
    for (let i=0; i<fruits.length; i++) {
        console.log(fruits[i]);
    }

    // 2. forEach
    fruits.forEach(fruit => {
        // 无法 contine | break
        console.log(fruit);
    })

    // 3. for in
    for(let index in fruits) {
        console.log(fruits[index]);
    }
```

ES6中：
```
// es6
    for(let fruit of fruits) {
        console.log(fruit);
    }
```