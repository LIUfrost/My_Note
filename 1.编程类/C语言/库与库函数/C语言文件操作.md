## 一、文件类型与基本概念
### 1. 文件指针类型
`FILE *fp;`
- `FILE`：标准库中用于描述文件的结构体类型
- `FILE *`：文件指针，用于后续所有文件操作
## 二、文件的打开与关闭

### 1. 打开文件：`fopen`
`FILE *fopen(const char *filename, const char *mode);`

**功能：**
- 打开指定文件，并返回文件指针
- 打开失败返回 `NULL`
**常用模式（mode）：**

| 模式     | 含义                 |
| ------ | ------------------ |
| `"r"`  | 只读打开（文件必须存在）       |
| `"w"`  | 只写打开（不存在则创建，存在则清空） |
| `"a"`  | 追加写（不存在则创建）        |
| `"r+"` | 读写打开（文件必须存在）       |
| `"w+"` | 读写打开（清空或创建）        |
| `"a+"` | 读写追加               |

### 2. 关闭文件：`fclose`
`int fclose(FILE *stream);`

**功能：**
- 关闭文件，释放资源
- 成功返回 `0`，失败返回 `EOF`
## 三、字符级文件操作

### 1. 读字符：`fgetc`
`int fgetc(FILE *stream);`

**功能：**
- 从文件中读取一个字符
- 返回读取到的字符（以 `int` 返回）
- 到达文件末尾或出错返回 `EOF`
### 2. 写字符：`fputc`
`int fputc(int c, FILE *stream);`

**功能：**
- 向文件写入一个字符
- 成功返回写入的字符，失败返回 `EOF`
## 四、字符串级文件操作

### 1. 读字符串：`fgets`
`char *fgets(char *str, int n, FILE *stream);`

**功能：**
- 从文件中读取一行（最多 `n-1` 个字符）
- 遇到换行或 EOF 结束
- 成功返回 `str`，失败返回 `NULL`

---

### 2. 写字符串：`fputs`
`int fputs(const char *str, FILE *stream);`

**功能：**
- 将字符串写入文件（不自动添加换行符）
- 成功返回非负值，失败返回 `EOF`

---

## 五、格式化文件输入输出

### 1. 格式化读：`fscanf`
`int fscanf(FILE *stream, const char *format, ...);`

**功能：**
- 按格式从文件中读取数据
- 返回成功读取并赋值的项数
- 出错或 EOF 返回 `EOF`

---

### 2. 格式化写：`fprintf`

`int fprintf(FILE *stream, const char *format, ...);`
**功能：**
- 按格式向文件写入数据
- 返回成功写入的字符数
- 出错返回负值

---

## 六、二进制文件读写

### 1. 块读：`fread`

`size_t fread(void *ptr, size_t size, size_t count, FILE *stream);`
**功能：**
- 从文件中读取 `count` 个大小为 `size` 的数据块
- 返回实际读取的块数
---

### 2. 块写：`fwrite`
`size_t fwrite(const void *ptr, size_t size, size_t count, FILE *stream);`

**功能：**
- 向文件写入 `count` 个大小为 `size` 的数据块
- 返回实际写入的块数
---

## 七、文件位置指示器操作

### 1. 移动文件指针：`fseek`
`int fseek(FILE *stream, long offset, int origin);`

**功能：**
- 设置文件当前位置
**origin 参数：**

| 常量         | 含义   |
| ---------- | ---- |
| `SEEK_SET` | 文件开头 |
| `SEEK_CUR` | 当前位置 |
| `SEEK_END` | 文件末尾 |

---

### 2. 获取当前位置：`ftell`
`long ftell(FILE *stream);`

**功能：**
- 返回当前文件指针相对于文件开头的偏移量
---
### 3. 重置文件指针：`rewind`
`void rewind(FILE *stream);`

**功能：**
- 将文件指针移到文件开头
- 清除错误标志和 EOF 标志
---

## 八、文件状态与错误检测

### 1. 判断是否到文件末尾：`feof`
`int feof(FILE *stream);`

**功能：**
- 如果已到文件末尾，返回非 0 值
---

### 2. 判断文件错误：`ferror`
`int ferror(FILE *stream);`

**功能：**
- 检测文件流是否发生错误
---

### 3. 清除错误状态：`clearerr`
`void clearerr(FILE *stream);`

---

## 九、文件缓冲控制

### 刷新缓冲区：`fflush`
`int fflush(FILE *stream);`

**功能：**
- 将输出缓冲区的数据立即写入文件
- 成功返回 `0`，失败返回 `EOF`
---

## 十、文件的删除与重命名

### 1. 删除文件：`remove`
`int remove(const char *filename);`

---

### 2. 重命名文件：`rename`
`int rename(const char *oldname, const char *newname);`