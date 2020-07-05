# difference

### 对比js数据修改前后差异，生成操作日志

```js
import difference from './difference'

// 数据修改前  
let before = {
  name: 'wang',
  sex: 1,
  skill: ['javascript', 'html', 'css'],
  asset: {
    house: 5,
    car: 6,
    dog: 1
  }
};

// 数据修改后  
let after = {
  name: 'wang',
  sex: 2,
  skill: ['javascript', 'html', 'vue', 'react'],
  asset: {
    house: 10,
    car: 6,
    cat: 2
  },
  job: null
};

const diff = difference(after, before);
```
输出结果为如下格式的数组：

```js
[{
  // 字段路径
  path: [],
  // 字段描述
  path_desc: [],
  // 操作共3种：ADD、REMOVE、UPDATE，即增、删、改
  action: "ADD",
  // 修改前的值
  modify_from: "",
  // 修改后的值
  modify_to: ""
}]
```

结果用表格展示如下：

| 操作 | 字段路径 | 修改前 | 修改后 |
| -- | -- | -- | -- |
| UPDATE | sex | 1 | 2 |
| UPDATE | skill/2 | css | vue |
| ADD | skill/3 |  | react |
| UPDATE | stock/0/apple | 100 | 500 |
| UPDATE | asset/house | 5 | 10 |
| ADD | asset/cat |  | 2 |
| ADD | job |  | null |
| REMOVE | asset/dog | 1 |  |

配合字典增强可读性

```js
// 字典
let dict = {
  name: {
    _label: '姓名'
  },
  age: {
    _label: '年龄'
  },
  sex: {
    _label: '性别',
    // 枚举数据
    _enum: {
      1: '男',
      2: '女'
    }
  },
  // 元素为基本数据类型的数组
  skill: {
    _label: '技能'
  },
  // 元素为对象的数组
  stock: {
    _label: '股票',
    apple: {
      _label: '苹果'
    },
    google: {
      _label: '谷歌'
    }
  },
  asset: {
    _label: '资产',
    house: {
      _label: '房子'
    },
    car: {
      _label: '豪车'
    },
    cat: {
      _label: '猫'
    },
    dog: {
      _label: '狗'
    }
  },
  job: {
    _label: '工作'
  }
}

const diff = difference(after, before, dict);
```
输出结果为数组，用表格展示如下：

| 操作 | 字段描述 | 修改前 | 修改后 |
| -- | -- | -- | -- |
| UPDATE | 性别 | 男 | 女 |
| UPDATE | 技能 | css | vue |
| ADD | 技能 |  | react |
| UPDATE | 资产/房子 | 5 | 10 |
| UPDATE | 股票/苹果 | 100 | 500 |
| ADD | 	资产/猫 |  | 2 |
| ADD | 工作 |  | null |
| REMOVE | 	资产/狗 | 1 |  |

