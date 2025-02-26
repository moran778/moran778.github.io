## 17. linux打补丁操作

------

### 1. 单个文件操作

#### 1.1 生成补丁- `diff` 命令

使用 `diff` 命令生成补丁文件。假设有两个文件 `file1.txt` 和 `file2.txt`，使用以下命令生成补丁文件 `patch.diff`：

```bash
diff -u file1.txt file2.txt > patch.diff
```

`-u` 参数表示输出统一格式的差异。

#### 1.2 打补丁 `patch` 命令

将生成的 `patch.diff` 补丁应用到原始文件 `file1.txt` 上：

```bash
patch < patch.diff
```

这会修改 `file1.txt` 使其内容与 `file2.txt` 相同。

#### 1.3 完整示例

##### 1.3.1 原始文件

```bash
echo "Hello, World!" > file1.txt
```

##### 1.3.2 修改文件

```bash
echo "Hello, Linux World!" > file2.txt
```

##### 1.3.3 生成补丁

```bash
diff -u file1.txt file2.txt > patch.diff
```

##### 1.3.4 使用 `patch` 命令打补丁

确保 `file1.txt` 是未修改版本，可以备份或删除 `file1.txt` 后重新创建：

```bash
patch < patch.diff
```

##### 1.3.5 反向修复：卸载补丁，还原版本

```bash
patch -R < patch.diff
```

`-R` 用于反向修复，即卸载补丁，恢复到原始版本。

------

### 2. 目录操作

#### 2.1 生成补丁- `diff` 命令

对两个目录 `dir1` 和 `dir2` 生成补丁：

```bash
diff -urN dir1 dir2 > changes.patch
```

`-u` 表示统一格式输出，`-r` 表示递归比较子目录，`-N` 表示处理不存在的文件。

#### 2.2 打补丁 `patch` 命令

将 `changes.patch` 补丁应用到原始目录：

```bash
patch -p1 < changes.patch
```

`-p1` 表示从补丁中的路径中剥离一个目录层级。

#### 2.3 完整示例

##### 2.3.1 创建目录和文件

```bash
mkdir dir1 dir2
echo "Hello, World!" > dir1/file1.txt
echo "This is file2." > dir1/file2.txt
mkdir dir1/subdir
echo "Inside subdir." > dir1/subdir/file3.txt

echo "Hello, Linux World!" > dir2/file1.txt
echo "This is file2 updated." > dir2/file2.txt
mkdir dir2/subdir
echo "New file in subdir." > dir2/subdir/file4.txt
```

##### 2.3.2 生成补丁

```bash
diff -urN dir1 dir2 > changes.patch
```

##### 2.3.3 使用 `patch` 命令打补丁

进入 `dir1` 目录，应用补丁：

```bash
cd dir1
patch -p1 < ../changes.patch
```

##### 2.3.4 反向修复：卸载补丁，还原版本

```bash
cd dir1
patch -p1 -R < ../changes.patch
```

------

### 3. `diff` 命令

#### 3.1 `diff` 命令常用选项

- `-u`：输出统一内容的头部信息。
- `-r`：递归比较目录。
- `-a`：将所有文件视为文本文件。
- `-N`：处理空文件。

#### 3.2 `diff` 命令三种格式

##### 3.2.1 正常格式

输出差异的行，并用符号表示差异：

```bash
diff file1 file2
```

##### 3.2.2 上下文格式（Context Diff）

通过 `-c` 参数显示上下文，帮助理解差异：

```bash
diff -c file1 file2
```

##### 3.2.3 统一格式（Unified Diff）

通过 `-u` 参数启用统一格式，适用于版本控制系统：

```bash
diff -u file1 file2
```

##### 3.2.4 三种格式的区别

| 格式       | 特点               | 示例                      |
| ---------- | ------------------ | ------------------------- |
| 正常格式   | 显示差异行         | `5c5,6`                   |
| 上下文格式 | 显示差异上下文     | `***` 和 `---` 包裹上下文 |
| 统一格式   | 简洁且适合版本控制 | `@@` 包裹差异范围         |

------

### 4. `patch` 命令

#### 常用选项

- `-R` (reverse)：反向修复，撤销补丁。

- `-E`：删除修复后为空的文件。

- `-b`：备份每个原始文件。

- `-p` <剥离层级> 或 `--strip=<剥离层级>`：指定路径剥离的层级。

- `--verbo`文章目录

  1. **单个文件操作**
     - 1.1 生成补丁- `diff` 命令
     - 1.2 打补丁 `patch` 命令
     - 1.3 完整示例
       - 1.3.1 原始文件
       - 1.3.2 修改文件
       - 1.3.3 生成补丁
       - 1.3.4 使用 `patch` 命令打补丁
       - 1.3.5 反向修复：卸载补丁，还原版本

  2. **目录操作**
     - 2.1 生成补丁- `diff` 命令
     - 2.2 打补丁 `patch` 命令
     - 2.3 完整示例
       - 2.3.1 创建目录和文件
       - 2.3.2 生成补丁
       - 2.3.3 使用 `patch` 命令打补丁
       - 2.3.4 反向修复：卸载补丁，还原版本

  3. **`diff` 命令**
     - 3.1 `diff` 命令常用选项
     - 3.2 `diff` 命令三种格式
       - 3.2.1 正常格式
       - 3.2.2 上下文格式（Context Diff）
       - 3.2.3 统一格式（Unified Diff）
       - 3.2.4 三种格式的区别

  4. **`patch` 命令**
     - 常用选项
       - `-R` (reverse) 反向修复
       - `-E` 删除空文件
       - `-b` 备份每个原始文件
       - `-p` <剥离层级> 或 `--strip=<剥离层级>` 设置剥离路径层级
       - `--verbose` 显示详细执行过程

  ---

  ### 1. 单个文件操作

  #### 1.1 生成补丁- `diff` 命令

  使用 `diff` 命令生成补丁文件。假设有两个文件 `file1.txt` 和 `file2.txt`，使用以下命令生成补丁文件 `patch.diff`：

  ```bash
  diff -u file1.txt file2.txt > patch.diff
  ```

  `-u` 参数表示输出统一格式的差异。

  #### 1.2 打补丁 `patch` 命令

  将生成的 `patch.diff` 补丁应用到原始文件 `file1.txt` 上：

  ```bash
  patch < patch.diff
  ```

  这会修改 `file1.txt` 使其内容与 `file2.txt` 相同。

  #### 1.3 完整示例

  ##### 1.3.1 原始文件

  ```bash
  echo "Hello, World!" > file1.txt
  ```

  ##### 1.3.2 修改文件

  ```bash
  echo "Hello, Linux World!" > file2.txt
  ```

  ##### 1.3.3 生成补丁

  ```bash
  diff -u file1.txt file2.txt > patch.diff
  ```

  ##### 1.3.4 使用 `patch` 命令打补丁

  确保 `file1.txt` 是未修改版本，可以备份或删除 `file1.txt` 后重新创建：

  ```bash
  patch < patch.diff
  ```

  ##### 1.3.5 反向修复：卸载补丁，还原版本

  ```bash
  patch -R < patch.diff
  ```

  `-R` 用于反向修复，即卸载补丁，恢复到原始版本。

  ---

  ### 2. 目录操作

  #### 2.1 生成补丁- `diff` 命令

  对两个目录 `dir1` 和 `dir2` 生成补丁：

  ```bash
  diff -urN dir1 dir2 > changes.patch
  ```

  `-u` 表示统一格式输出，`-r` 表示递归比较子目录，`-N` 表示处理不存在的文件。

  #### 2.2 打补丁 `patch` 命令

  将 `changes.patch` 补丁应用到原始目录：

  ```bash
  patch -p1 < changes.patch
  ```

  `-p1` 表示从补丁中的路径中剥离一个目录层级。

  #### 2.3 完整示例

  ##### 2.3.1 创建目录和文件

  ```bash
  mkdir dir1 dir2
  echo "Hello, World!" > dir1/file1.txt
  echo "This is file2." > dir1/file2.txt
  mkdir dir1/subdir
  echo "Inside subdir." > dir1/subdir/file3.txt
  
  echo "Hello, Linux World!" > dir2/file1.txt
  echo "This is file2 updated." > dir2/file2.txt
  mkdir dir2/subdir
  echo "New file in subdir." > dir2/subdir/file4.txt
  ```

  ##### 2.3.2 生成补丁

  ```bash
  diff -urN dir1 dir2 > changes.patch
  ```

  ##### 2.3.3 使用 `patch` 命令打补丁

  进入 `dir1` 目录，应用补丁：

  ```bash
  cd dir1
  patch -p1 < ../changes.patch
  ```

  ##### 2.3.4 反向修复：卸载补丁，还原版本

  ```bash
  cd dir1
  patch -p1 -R < ../changes.patch
  ```

  ---

  ### 3. `diff` 命令

  #### 3.1 `diff` 命令常用选项

  - `-u`：输出统一内容的头部信息。
  - `-r`：递归比较目录。
  - `-a`：将所有文件视为文本文件。
  - `-N`：处理空文件。

  #### 3.2 `diff` 命令三种格式

  ##### 3.2.1 正常格式

  输出差异的行，并用符号表示差异：

  ```bash
  diff file1 file2
  ```

  ##### 3.2.2 上下文格式（Context Diff）

  通过 `-c` 参数显示上下文，帮助理解差异：

  ```bash
  diff -c file1 file2
  ```

  ##### 3.2.3 统一格式（Unified Diff）

  通过 `-u` 参数启用统一格式，适用于版本控制系统：

  ```bash
  diff -u file1 file2
  ```

  ##### 3.2.4 三种格式的区别

  | 格式       | 特点               | 示例                      |
  | ---------- | ------------------ | ------------------------- |
  | 正常格式   | 显示差异行         | `5c5,6`                   |
  | 上下文格式 | 显示差异上下文     | `***` 和 `---` 包裹上下文 |
  | 统一格式   | 简洁且适合版本控制 | `@@` 包裹差异范围         |

  ---

  ### 4. `patch` 命令

  #### 常用选项

  - `-R` (reverse)：反向修复，撤销补丁。
  - `-E`：删除修复后为空的文件。
  - `-b`：备份每个原始文件。
  - `-p` <剥离层级> 或 `--strip=<剥离层级>`：指定路径剥离的层级。
  - `--verbose`：显示详细执行过程。

- `se`：显示详细执行过程。