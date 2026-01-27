---
tags:
  - C
url: https://github.com/DaveGamble/cJSON.git
---
![cJSON常用库函数|500x400](C:\2.STUDY\NOTE\My_Note\1.编程类\C语言\图片\cJSON库.png)
### 1.使用流程实例
```C
0. 打开 snippets.json 文件，读成一个很长的字符串（用 fread 或其他方式）
1. 把字符串解析成 cJSON 结构
	root = cJSON_Parse(文件内容);
2. 如果 root == NULL，说明 JSON 格式错了，要报错

3. 判断 root 是不是数组
   if (!cJSON_IsArray(root)) { 不是我们想要的格式 }

4. 遍历所有片段（比如实现 fcode list）
   cJSON *snippet = NULL;
   cJSON_ArrayForEach(snippet, root) {
       cJSON *name = cJSON_GetObjectItem(snippet, "name");
       cJSON *lang = cJSON_GetObjectItem(snippet, "lang");
       if (name && lang) {
			printf("%s (%s)\n", name->valuestring, lang-
			>valuestring);
       }
   }

5. 用户要新增一个片段（fcode add）
   cJSON *new_snippet = cJSON_CreateObject();
   cJSON_AddStringToObject(new_snippet, "name", "new-one");
   cJSON_AddStringToObject(new_snippet, "lang", "c");
   cJSON_AddStringToObject(new_snippet, "desc", "这是一个例子");
   cJSON *code = cJSON_CreateString("printf(\"hello\");");
   cJSON_AddItemToObject(new_snippet, "code", code);

   // 加到根数组里
   cJSON_AddItemToArray(root, new_snippet);

6. 保存回去
   char *json_str = cJSON_Print(root);          // 带格式的漂亮打印
   // 或者 cJSON_PrintUnformatted(root);        // 压缩成一行，文件小一点
   写入文件...

7. 程序退出前（或不再需要时）
   cJSON_Delete(root);
   free(json_str);   // Print 函数返回的字符串也要 free
```
