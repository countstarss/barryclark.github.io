在 Git 中，一个仓库可以包含另一个仓库，这种结构通常被称为 **submodule（子模块）**。子模块允许你在一个 Git 仓库中包含另一个 Git 仓库，并且它们各自的历史和变更记录是独立的。子模块的变更不会直接影响主仓库，反之亦然。

### 使用子模块

#### 添加子模块

你可以通过以下命令将一个外部 Git 仓库添加为子模块：

```sh
git submodule add <repository_url> <path>
```

例如，将一个外部仓库添加到当前仓库的 `libs/somelib` 目录中：

```sh
git submodule add https://github.com/someuser/somelib.git libs/somelib
```

#### 初始化和更新子模块

克隆包含子模块的仓库时，初始克隆不会自动包含子模块。你需要显式初始化和更新子模块：

```sh
git submodule init
git submodule update
```

或者通过一条命令完成：

```sh
git clone --recurse-submodules <repository_url>
```

#### 提交子模块的变更

在子模块中进行更改后，需要在主仓库中记录这些更改：

1. 进入子模块目录并提交更改：

```sh
cd libs/somelib
git add .
git commit -m "Update in submodule"
```

2. 返回主仓库目录并提交子模块引用的更改：

```sh
cd ../..
git add libs/somelib
git commit -m "Update submodule reference"
```

### 管理子模块

#### 查看子模块状态

你可以使用以下命令查看子模块的状态：

```sh
git submodule status
```

#### 更新子模块

如果子模块的远程仓库有更新，可以使用以下命令来拉取更新：

```sh
git submodule update --remote
```

### 子模块的优点和缺点

#### 优点

1. **模块化管理**：子模块允许你将项目分解为独立的模块，每个模块可以单独管理。
2. **独立版本控制**：子模块有自己的版本历史和控制，与主仓库独立。

#### 缺点

1. **复杂性增加**：管理子模块需要额外的命令和步骤，增加了使用的复杂性。
2. **同步问题**：在更新子模块时，需要确保主仓库和子模块之间的引用同步。

### 结论

子模块是一种有效的方法，可以在一个 Git 仓库中包含另一个 Git 仓库而不互相影响。然而，需要注意管理和同步子模块的复杂性。如果你频繁需要共享代码或依赖，子模块是一个合适的选择。否则，你可能需要考虑其他方案，如包管理器或单一仓库（monorepo）策略。

### 实例操作

以下是一个完整的操作示例：

1. **主仓库操作**

```sh
mkdir main-repo
cd main-repo
git init
echo "Main repository" > README.md
git add README.md
git commit -m "Initial commit"
```

2. **添加子模块**

```sh
git submodule add https://github.com/someuser/somelib.git libs/somelib
git commit -m "Add submodule"
```

3. **克隆包含子模块的仓库**

```sh
git clone --recurse-submodules <repository_url>
```

通过这些步骤，你可以成功地在一个 Git 仓库中包含另一个 Git 仓库，并且它们不会互相影响。