# A Closer Look at Real-World Patches

## Research Questions

1. Do patches impact some specific statement types?
2. Are there code elements in statements that are prone to be faulty?
3. Which expression types are most impacted by patches?
4. Which parts of buggy expressions are prone to be buggy?

## Method

1. 根据提交信息（commit message）提取缺陷修复提交；
2. 使用抽象语法树（AST）解析文件；
3. 使用AST差分工具GumTree提取缺陷修复动作；

## Conclusion

1. Top-5 Buggy Statement：ExpressionStatement, VariableDeclarationStatement, IfStatement, FieldDeclaration, ReturnStatement.
2. Updates account for about a half of repair actions.
3. Top-5 Buggy Expression: MethodInvocation, SimpleName, InfixExpression, Assignment, ClassInstanceCreation.