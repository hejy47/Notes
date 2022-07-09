# TBar论文阅读及工具剖析

## 错误定位

由于是借用工具实现，为在该工具中实现，而是直接提供定位结果

1. perfect localization：提供已知的错误语句位置——BugPositions.txt
2. normal localization：使用GZoltar框架自动执行每个错误程序的测试用例得到——SuspiciousCodePositions文件夹

## 模板选择

1. parseSuspiciousCode解析可疑代码，得到可疑代码结点列表；
2. 对于结点列表的每个结点，解析其上下文信息distinctContexInfo；
3. 根据其收集到的上下文信息，匹配模板并修复——fixWithMatchedFixTemplates
   1. isMethodDeclaration
      1. isCastExpression-ClassCastChecker()
      2. isClassInstanceCreation-MethodInvocationMutator()
      3. isIfStatement/isDoStatement/isWhileStatement
         1. isInfixExpression-OperatorMutator(0)
         2. ConditionalExpressionMutator(2)
      4. isConditionalExpression-ConditionalExpressionMutator(0)
      5. isCatchClause/isVariableDeclarationStatement-DataTypeReplacer()
      6. isInfixExpression
         1. -ICASTIdivCastToDouble()
         2. !operator-OperatorMutator(0)
         3. ConditionalExpressionMutator(1)
         4. OperatorMutator(4)
      7. isBooleanLiteral/isNumberLiteral/isCharacterLiteral/isStringLiteral-LiteralExpressionMutator()
      8. isMethodInvocation/isConstructorInvocation/isSuperConstructorInvocation
         1. !methodChanged-MethodInvocationMutator()
         2. isMethodInvocation
            1. NPEqualsShouldHandleNullArgument()
            2. RangeChecker(false)
      9. isAssignment-OperatorMutator(2)
      10. isInstanceofExpression-OperatorMutator(5)
      11. isArrayAccess-RangeChecker(true)
      12. isReturnStatement
          1. "boolean".equalsIgnoreCase(returnType)-ConditionalExpressionMutator(2)
          2. ReturnStatementMutator(returnType)
      13. isSimpleName/isQualifiedName 
          1. VariableReplacer()
          2. !nullChecked-NullPointerChecker()
      14. 以上情况没有生成补丁或补丁验证未通过
          1. !nullChecked-NullPointerChecker
          2. StatementMover
          3. StatementRemover
          4. StatementInserter
   2. checker中没有对应的方法声明-StatementRemover()

## 补丁生成

更加匹配到的fix template，生成并验证补丁——generateAndValidatePatches

## 补丁验证

根据生成的补丁候补列表，将列表中的每个补丁插入到错误代码的位置(addPatchCodeToFile)中并进行测试——testGeneratedPatches

1. 重新编译补丁版本
2. 读取之前未通过的测试用例，再进行测试
   1. errorTestAfterFix == 0 || failedTestsAfterFix.isEmpty()：Succeeded to fix the bug
   2. minErrorTestAfterFix == 0 || errorTestAfterFix <= minErrorTestAfterFix：Partially Succeeded to fix the bug
