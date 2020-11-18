
## AST 相关资料
```
https://developer.mozilla.org/en-US/docs/Mozilla/Projects/SpiderMonkey/Parser_API

https://astexplorer.net/

https://segmentfault.com/a/1190000016231512
```

```
babel valueToNode 、matchesPattern

module.exports = function({ types: babelTypes }) {
  return {
    name: "node-env-replacer",
    visitor: {
      // 成员表达式
      MemberExpression(path, state) {
        // 如果 object 对应的节点匹配了模式 "process.env"
        if (path.get("object").matchesPattern("process.env")) {
            //=================上面这句标红=====================


          // 这里返回结果为字符串字面量类型的节点
          const key = path.toComputedKey();
          if ( babelTypes.isStringLiteral(key) ) {

            // path.replaceWith( newNode ) 用来替换当前节点
            // babelTypes.valueToNode( value ) 用来创建节点，如果value是字符串，则返回字符串字面量类型的节点

            path.replaceWith(babelTypes.valueToNode(process.env[key.value]));
            //=================上面这句标红=====================
            
          }
        }
      }
    }
  }
```

## AST 语法树
```
节点对象
Node 含有type和loc(sourceLocation|null) 

sourceLocation, 含有source(代码内容), start和end(Position)

Position 含有line和column属性

Program 除了type, 只有一个body属性


函数
Function 含有id,params,defaults,rest,body,generator,expression, 应该还有async, isArrow



语句

Statement 任意语句

Emptystatement 空语句 ;

BlockStatement  语句块{}

ExpressionStatement 表达式语句

IfStatement 具有test属性

LabelStatement

BreakStatement

ContinueStatement

withStatement 含有object和body主体

switchStatement 含有判别式discriminant和cases

returnStatement

throwStatement

tryStatement 含有block块[BlockStatement]和handles[catchClause], 以及finalizer(BlockStatement)

whileStatement 含有test和body

doWhileStatement 含有test和body

forStatement 含有init和test,update三要素,以及body

forInStatement 含有left,right以及body

debuggerStatement


声明
FunctionDeclaration, 含有{id,prarams,defaults,rest,body,genertor,expression}

varDeclaration 含有declarations[VariableDeclarator...]和kind

VariableDeclarator 含有id和init


表达式
Expression

Thisexpression

arrayExpression 含有elements[...expresssion]

objectExpression 含有properties[{key,value,kind}],  key分为literal字面量和identifier标识符, kind分为init,get,set

FunctionExpression 同FunctionDeclaration, 只是id可以省略

sequenceExpression 序列表达式, 即逗号风格的各个表达式

unaryExpression 一元操作 含有操作符operator,是否前置prefix,操作目标argument

binaryExpression 二元运算 含有operator, left, right

assignmentExpression 赋值表达式  含有operator(AssignmentOperator),left,right

updateExpression 含有operator, prefix, argument  ++或--

logicalExpression 逻辑运算符表达式 含有operator,left,right

conditionExpression 条件运算符表达式  即三目运算符, 含有test和alternate交替,替换项,consequent结果项

newExpression  含有callee和arguments, 也就是构造函数和参数

callExpression 含有callee和arguments, 也就是被调用的函数和参数

memberExpression 含有object,property和computed属性, 也就是obj.a 还是 obj[key], 后者是computed

yieldExpression 只有一个argument属性

ComprehensionExpression?  推导表达式 含有body,blocks[ComprehensionBlock]和filter 
用数组推导举例:
 [expression for ( variable in object ) if ( condition )]
 let evensquares = [x*x for (x in range(0,10)) if (x % 2 === 0)]
 等价于:
let evensquares = [];
for(x in range(0,10)) {
    if(x % 2 === 0)
        evensquares.push(x*x);
}

GeneratorExpression?   迭代表达式  含有属性同上
举例:
let h = (f(x) for(x in g));
等价于:
function map(i, g) {
    for(let x in g) yield f(x);
}

GraphExpression   愚蠢的#1#

LetExpression 含有head和body



模式
pattern 属于变量定义和形参定义, 分别是对象解构和数组解构

objectPattern  含有properties[{key,value}], key是被解构对象内的属性, value是Pattern,递归
user = {name:'amy', job:{title:'ceo'} }
var { name:varN, job:{title:varT} } = user
varN  varT  // 得到amy和ceo

arrayPattern 含有elements[Pattern], 递归



子句
SwitchCase 含有test和consequent

CatchClause 含有param和guard,body

ComprehensionBlock 推导,  含有left(Pattern),right 和 each属性, 这个是高级特性,最新chrome72尚未支持 , 借鉴Python



杂项

Identifier  标识符 含有一个name属性

Literal 字面量

UnaryOperator  一元操作符

BinaryOperator 二元运算符

LogicalOperator 逻辑运算符

AssignmentOperator 赋值操作符

UpdateOperator 自增自减运算符

```







