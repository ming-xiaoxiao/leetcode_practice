[toc]

# C库函数合集

## 1.string相关：

1. **strcpy**    原型`char *strcpy( char *restrict dest, const char *restrict src );`

   作用：将src拷贝到dest中，包括结尾的\0

2. **strncpy**   原型`char *strncpy( char *restrict dest, const char *restrict src, size_t count );`

   作用：将count长度的src字符拷贝到dest

```c
	char *src = "Take the test.";
//  src[0] = 'M' ; // this would be undefined behavior
    char dst[strlen(src) + 1]; // +1 to accomodate for the null terminator
    strncpy(dst, src, 6);
    dst[0] = 'M'; // OK
    printf("src = %s\ndst = %s\n", src, dst);
```

3. **strcat**和**strncat**       原型`char *strcat( char *restrict dest, const char *restrict src );`

   ​                                        `char *strncat( char *restrict dest, const char *restrict src, size_t count );`

   作用：连接两个字符串

```c
	char str[8] = "asd";
	char str1[] = "fgh";
	strncat(str, str1,3);		//此处要注意str是否越界
	printf("%s", str);
```

4. **strcmp**    **strncmp**    原型`int strcmp( const char *lhs, const char *rhs );`

   ​										`int strncmp( const char *lhs, const char *rhs, size_t count );`

   作用：比较两个字符串大小/比较前count个字符串大小

5. **strchr**   **strrchr**      原型`char *strchr( const char *str, int ch );`

   ​									 `char *strrchr( const char *str, int ch );`

   作用：找第一个/最后一个字符出现的位置并返回指向该字符的指针

   ```c
   	char str[] = "asdasd";
   	char *s = strchr(str, 'a');
   	s = strchr(s+1, 'a');	//find第二个a
   	printf("%s\n", s);
   ```

   ```c
       char szSomeFileName[] = "foo/bar/foobar.txt";
       char *pLastSlash = strrchr(szSomeFileName, '/');
       char *pszBaseName = pLastSlash ? pLastSlash + 1 : szSomeFileName;
       printf("Base Name: %s", pszBaseName);		//Base Name: foobar.txt
   ```

6. **strstr**      原型`char *strstr( const char* str, const char* substr );`

   作用：类似于strchr，找的对象变成了字符串

## 2. 排序：

1. **qsort**      原型`void qsort( void *ptr, size_t count, size_t size, int (*comp)(const void *, const void *) );`

   1. 参数1：pointer to the array to sort
   2. 参数2：number of element in the array
   3. 参数3：size of each element in the array in bytes
   4. 参数4：comparison function which returns a negative integer value if the first argument is *less* than the second

   ```c
   int cmp(const void*a,const void *b)	//增序
   {
       return *(int*)a-*(int*)b;
   }
   int main()
   {
   	int *array;
   	int n;
   	scanf("%d",&n);
       //动态分配内存
   	array=(int*)malloc(n*sizeof(int));
   	for(int i = 0; i<n; i++) {
   		scanf("%d",(array+i));
   	}
   	qsort(array, n, sizeof(int),cmp);
   	for(int i=0;i<n;i++) {
   		printf("%d\t",array[i]);
   	}
   	return 0;
   }
   ```

2. 字符串字典排序：

```c
int cmp(const void * a, const void *b) //qsort库要求参数const
{
	//return strcmp(*(char **)a, *(char **)b);
	return strcmp((char *)a,(char *)b) ; 
}
int main()
{
	char s[3][55] = { "im","imi","in" };  //字符串数组排序
	qsort(s, 3, 55, cmp); //一共三组，每组大小取最长字符串大小，这里取数组定义的醉哒哒
	for (int i = 0; i<3; i++)
		printf("%s\n", s[i]);
	return 0;
}
```

