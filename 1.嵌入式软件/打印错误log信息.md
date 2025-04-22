// 定义一个__aeabi_assert函数，当expr为假时，打印出错误信息，包括文件名file和行号line，并调用abort()函数终止程序

 

 

void __aeabi_assert(const char *expr, const char *file, int line) {

  fprintf(stderr, "*** assertion failed: %s, file %s, line %d\n", expr, file, line);

  abort();

}