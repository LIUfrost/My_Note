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

## 十一、使用示例
```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include <errno.h>

#define MAX_LINE 1024
#define BUFFER_SIZE 4096

// 学生结构体示例
typedef struct {
    int id;
    char name[50];
    float score;
} Student;

// 打印错误信息
void print_error(const char* operation, const char* filename) {
    fprintf(stderr, "错误: %s 文件 '%s' 失败: %s\n", 
            operation, filename, strerror(errno));
}

// 1. 创建并写入文本文件（fprintf, fputc, fputs）
void write_text_file(const char* filename) {
    printf("\n=== 1. 写入文本文件 ===\n");
    
    FILE* fp = fopen(filename, "w");
    if (fp == NULL) {
        print_error("打开", filename);
        return;
    }
    
    // 使用 fprintf 写入格式化文本
    fprintf(fp, "这是文件操作示例程序\n");
    fprintf(fp, "日期: 2024-03-24\n\n");
    
    // 使用 fputs 写入字符串
    fputs("1. 使用 fputs 写入的一行\n", fp);
    fputs("2. 第二行文本\n", fp);
    
    // 使用 fputc 写入单个字符
    fputc('3', fp);
    fputc('.', fp);
    fputc(' ', fp);
    fputc('C', fp);
    fputc('h', fp);
    fputc('a', fp);
    fputc('r', fp);
    fputc('\n', fp);
    
    fclose(fp);
    printf("✓ 成功写入文本文件: %s\n", filename);
}

// 2. 读取文本文件（fscanf, fgets, fgetc）
void read_text_file(const char* filename) {
    printf("\n=== 2. 读取文本文件 ===\n");
    
    FILE* fp = fopen(filename, "r");
    if (fp == NULL) {
        print_error("打开", filename);
        return;
    }
    
    // 方法1: 使用 fgetc 逐个字符读取
    printf("--- 方法1: fgetc 逐个字符读取 ---\n");
    rewind(fp);  // 重置文件指针到开头
    int ch;
    while ((ch = fgetc(fp)) != EOF) {
        putchar(ch);
    }
    
    // 方法2: 使用 fgets 逐行读取
    printf("\n--- 方法2: fgets 逐行读取 ---\n");
    rewind(fp);
    char line[MAX_LINE];
    int line_num = 1;
    while (fgets(line, sizeof(line), fp) != NULL) {
        printf("%d: %s", line_num++, line);
    }
    
    // 方法3: 使用 fscanf 格式化读取
    printf("\n--- 方法3: fscanf 格式化读取 ---\n");
    rewind(fp);
    char buffer[MAX_LINE];
    while (fscanf(fp, " %[^\n]", buffer) == 1) {  // 读取一行
        printf("%s\n", buffer);
    }
    
    fclose(fp);
}

// 3. 追加写入文件（a模式）
void append_to_file(const char* filename) {
    printf("\n=== 3. 追加写入文件 ===\n");
    
    FILE* fp = fopen(filename, "a");
    if (fp == NULL) {
        print_error("打开", filename);
        return;
    }
    
    fprintf(fp, "\n--- 追加的内容 ---\n");
    fprintf(fp, "追加时间: %s\n", __TIME__);
    fprintf(fp, "追加日期: %s\n", __DATE__);
    
    fclose(fp);
    printf("✓ 成功追加内容到: %s\n", filename);
}

// 4. 二进制文件操作（fwrite, fread）
void binary_file_operation(const char* filename) {
    printf("\n=== 4. 二进制文件操作 ===\n");
    
    // 写入二进制数据
    Student students[] = {
        {1001, "张三", 85.5},
        {1002, "李四", 92.0},
        {1003, "王五", 78.5},
        {1004, "赵六", 88.0}
    };
    int count = sizeof(students) / sizeof(students[0]);
    
    FILE* fp = fopen(filename, "wb");
    if (fp == NULL) {
        print_error("打开", filename);
        return;
    }
    
    // 使用 fwrite 写入二进制数据
    size_t written = fwrite(students, sizeof(Student), count, fp);
    if (written != count) {
        printf("警告: 只写入了 %zu/%d 个学生\n", written, count);
    }
    fclose(fp);
    printf("✓ 成功写入 %d 个学生到二进制文件: %s\n", count, filename);
    
    // 读取二进制数据
    fp = fopen(filename, "rb");
    if (fp == NULL) {
        print_error("打开", filename);
        return;
    }
    
    Student read_students[10];
    // 使用 fread 读取二进制数据
    size_t read_count = fread(read_students, sizeof(Student), 10, fp);
    fclose(fp);
    
    printf("\n读取到的学生信息:\n");
    printf("ID\t姓名\t\t成绩\n");
    printf("--------------------------------\n");
    for (size_t i = 0; i < read_count; i++) {
        printf("%d\t%s\t\t%.1f\n", 
               read_students[i].id, 
               read_students[i].name, 
               read_students[i].score);
    }
}

// 5. 随机访问（fseek, ftell, rewind）
void random_access(const char* filename) {
    printf("\n=== 5. 随机访问文件 ===\n");
    
    FILE* fp = fopen(filename, "r");
    if (fp == NULL) {
        print_error("打开", filename);
        return;
    }
    
    // 获取当前文件位置
    long pos = ftell(fp);
    printf("初始位置: %ld\n", pos);
    
    // 移动文件指针到末尾
    fseek(fp, 0, SEEK_END);
    pos = ftell(fp);
    printf("文件末尾位置: %ld 字节\n", pos);
    printf("文件大小: %ld 字节\n", pos);
    
    // 移动文件指针到开头
    rewind(fp);
    pos = ftell(fp);
    printf("重置后位置: %ld\n", pos);
    
    // 读取前20个字符
    printf("前20个字符: ");
    for (int i = 0; i < 20; i++) {
        int ch = fgetc(fp);
        if (ch != EOF) putchar(ch);
        else break;
    }
    printf("\n");
    
    // 从偏移量50处读取
    fseek(fp, 50, SEEK_SET);
    printf("偏移量50处的内容: ");
    for (int i = 0; i < 20; i++) {
        int ch = fgetc(fp);
        if (ch != EOF) putchar(ch);
        else break;
    }
    printf("\n");
    
    fclose(fp);
}

// 6. 文件状态和属性（feof, ferror）
void file_status(const char* filename) {
    printf("\n=== 6. 文件状态检查 ===\n");
    
    FILE* fp = fopen(filename, "r");
    if (fp == NULL) {
        print_error("打开", filename);
        return;
    }
    
    // 读取直到文件末尾
    char buffer[MAX_LINE];
    int line_count = 0;
    while (fgets(buffer, sizeof(buffer), fp) != NULL) {
        line_count++;
    }
    
    // 检查是否到达文件末尾
    if (feof(fp)) {
        printf("✓ 已到达文件末尾，共读取 %d 行\n", line_count);
    }
    
    // 检查是否有错误
    if (ferror(fp)) {
        printf("✗ 文件读取过程中发生错误\n");
    } else {
        printf("✓ 文件读取正常，无错误\n");
    }
    
    // 清除错误标志
    clearerr(fp);
    fclose(fp);
}

// 7. 临时文件操作（tmpfile, tmpnam）
void temp_file_operation() {
    printf("\n=== 7. 临时文件操作 ===\n");
    
    // 创建临时文件（自动删除）
    FILE* temp = tmpfile();
    if (temp == NULL) {
        printf("创建临时文件失败\n");
        return;
    }
    
    fprintf(temp, "这是临时文件的内容\n");
    fprintf(temp, "程序结束后会自动删除\n");
    
    // 重置指针并读取
    rewind(temp);
    char buffer[MAX_LINE];
    printf("临时文件内容:\n");
    while (fgets(buffer, sizeof(buffer), temp) != NULL) {
        printf("  %s", buffer);
    }
    
    fclose(temp);  // 关闭后自动删除
    printf("临时文件已关闭并自动删除\n");
}

// 8. 文件重命名和删除（rename, remove）
void file_rename_delete() {
    printf("\n=== 8. 文件重命名和删除 ===\n");
    
    const char* original = "test_temp.txt";
    const char* renamed = "test_renamed.txt";
    
    // 创建测试文件
    FILE* fp = fopen(original, "w");
    if (fp != NULL) {
        fprintf(fp, "这是测试文件\n");
        fclose(fp);
        printf("✓ 创建测试文件: %s\n", original);
    }
    
    // 重命名文件
    if (rename(original, renamed) == 0) {
        printf("✓ 文件重命名成功: %s → %s\n", original, renamed);
    } else {
        print_error("重命名", original);
    }
    
    // 删除文件
    if (remove(renamed) == 0) {
        printf("✓ 文件删除成功: %s\n", renamed);
    } else {
        print_error("删除", renamed);
    }
}

// 9. 缓冲区操作（setbuf, setvbuf, fflush）
void buffer_operation() {
    printf("\n=== 9. 缓冲区操作 ===\n");
    
    const char* filename = "buffer_test.txt";
    FILE* fp = fopen(filename, "w");
    if (fp == NULL) {
        print_error("打开", filename);
        return;
    }
    
    // 设置全缓冲模式，缓冲区大小为 BUFFER_SIZE
    char buffer[BUFFER_SIZE];
    if (setvbuf(fp, buffer, _IOFBF, BUFFER_SIZE) == 0) {
        printf("✓ 设置全缓冲模式成功\n");
    }
    
    // 写入数据（可能在缓冲区中，未立即写入磁盘）
    for (int i = 0; i < 10; i++) {
        fprintf(fp, "第 %d 行数据\n", i + 1);
    }
    printf("数据已写入缓冲区，但可能未刷新到磁盘\n");
    
    // 强制刷新缓冲区
    if (fflush(fp) == 0) {
        printf("✓ 缓冲区已刷新，数据已写入磁盘\n");
    }
    
    fclose(fp);
    
    // 读取并显示文件内容
    fp = fopen(filename, "r");
    if (fp != NULL) {
        printf("\n文件内容:\n");
        char line[MAX_LINE];
        while (fgets(line, sizeof(line), fp) != NULL) {
            printf("  %s", line);
        }
        fclose(fp);
    }
    
    remove(filename);  // 清理测试文件
}

// 10. 文件指针复制（freopen）
void freopen_demo() {
    printf("\n=== 10. 文件指针重定向 ===\n");
    
    const char* filename = "redirect_test.txt";
    
    // 将 stdout 重定向到文件
    FILE* original_stdout = stdout;
    FILE* fp = freopen(filename, "w", stdout);
    if (fp == NULL) {
        print_error("重定向", filename);
        return;
    }
    
    // 这些 printf 会写入文件而不是屏幕
    printf("这行文字会写入文件\n");
    printf("stdout 已被重定向\n");
    printf("CSP 认证测试\n");
    
    // 恢复 stdout（关闭文件）
    fclose(fp);
    stdout = original_stdout;
    
    printf("✓ stdout 已恢复，这行显示在屏幕上\n");
    
    // 读取并显示文件内容
    fp = fopen(filename, "r");
    if (fp != NULL) {
        printf("\n文件内容:\n");
        char line[MAX_LINE];
        while (fgets(line, sizeof(line), fp) != NULL) {
            printf("  %s", line);
        }
        fclose(fp);
        remove(filename);
    }
}

int main() {
    printf("========================================\n");
    printf("C语言文件操作综合示例\n");
    printf("========================================\n");
    
    const char* text_file = "test_text.txt";
    const char* binary_file = "test_binary.dat";
    
    // 执行各种文件操作
    write_text_file(text_file);
    read_text_file(text_file);
    append_to_file(text_file);
    binary_file_operation(binary_file);
    random_access(text_file);
    file_status(text_file);
    temp_file_operation();
    file_rename_delete();
    buffer_operation();
    freopen_demo();
    
    // 清理测试文件
    printf("\n=== 清理测试文件 ===\n");
    remove(text_file);
    remove(binary_file);
    printf("✓ 已删除测试文件\n");
    
    printf("\n========================================\n");
    printf("程序执行完成！\n");
    printf("========================================\n");
    
    return 0;
}
```
