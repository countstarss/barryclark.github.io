# Mongoose中，`lean()`方法


在Mongoose中，`lean()`方法用于优化查询结果的性能。具体来说，`lean()`方法返回一个普通的JavaScript对象，而不是Mongoose文档对象。这种优化可以显著提高查询性能，特别是在读取大量数据时。

### 详细解释

当你进行Mongoose查询时，默认情况下返回的是Mongoose文档对象，这些对象带有许多Mongoose特有的方法和功能，如保存（`save()`）、验证（`validate()`）等。这些功能在某些情况下可能是多余的，尤其是当你只需要读取数据而不需要修改时。

使用`lean()`方法可以避免这些额外的开销，使查询结果更轻量化。

### 使用场景

1. **只读查询**：如果你只需要读取数据而不进行任何修改，使用`lean()`可以提高查询效率。
2. **高性能需求**：当需要高性能读取大量数据时，`lean()`可以显著减少内存占用和处理时间。

### 使用示例

假设你有一个用户模型，并且你想查询所有用户的数据：

```javascript
const User = require('./models/user');

// 使用 lean() 进行只读查询
User.find().lean().exec((err, users) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(users); // 返回的是普通 JavaScript 对象数组
});
```

### 详细示例

假设你有一个用户模型，并且你想查询所有用户的数据：

#### 模型定义

```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  age: Number
});

const User = mongoose.model('User', userSchema);

module.exports = User;
```

#### 使用 `lean()`

```javascript
const User = require('./models/user');

async function getUsers() {
  try {
    const users = await User.find().lean();
    console.log(users);
  } catch (err) {
    console.error(err);
  }
}

getUsers();
```

### 没有使用 `lean()` 的区别

如果不使用`lean()`，查询结果是Mongoose文档对象：

```javascript
const User = require('./models/user');

async function getUsers() {
  try {
    const users = await User.find();
    console.log(users);
  } catch (err) {
    console.error(err);
  }
}

getUsers();
```

在这种情况下，`users`数组中的每个元素都是一个Mongoose文档对象，包含Mongoose的各种方法和功能。

### 总结

- **`lean()`的作用**：返回普通JavaScript对象而不是Mongoose文档对象，优化查询性能。
- **使用场景**：只读查询、高性能需求。
- **示例**：在查询中使用`lean()`方法可以显著提高查询的速度和效率。

通过使用`lean()`，你可以在不需要Mongoose文档对象的场景下提高查询效率，使你的应用更加高效。