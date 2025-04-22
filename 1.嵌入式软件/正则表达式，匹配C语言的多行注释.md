1.使用“伪正则表达式”：

{START({NOT_WORRYING}*{WORRYING}+{NOT_WORRYING_NOR_FINAL})*{NOT_WORRYING}*{WORRYING}+{FINAL}

 

2.对上面的表达式经行说明：

{START}                                                             /\* 

({NOT_WORRYING}*{WORRYING}+{NOT_WORRYING_NOR_FINAL})*      ([^*]*\*+[^*/])* 

{NOT_WORRYING}*                                                  [^*]*

{WORRYING}+{FINAL}                                                \*+/

 

3.使用案例：

3.1下是一个包含随机注释的C语言文件示例：

/*** this is c comment ** /

 **/

 

int blah(struct myobj **p)

{

  return (*p)->f(p);

}

 

/* /* /*

 \* other c comment

 */

3.2在JavaScript中处理文件

const comments = /\/\*([^*]*\*+[^*/])*[^*]*\*+\//s.exec(c)[0];

3.3运行结果返回值
 '/*** this is c comment ** /\n **/'

 

 

3.4可以将上面的正则表达式配合sed命令使用。

如果我们想将源文件中所有的注释替换成一个空行，只需运行以下命令：

sed -z -E "s#/\*([^*]*\*+[^*/])*[^*]*\*+/##g" a.c

 

3.5运行结果

int blah(struct myobj **p)

{

  return (*p)->f(p);

}