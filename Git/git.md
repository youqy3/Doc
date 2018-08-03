## git blame file
  - git blame file 可以查看具体某个文件的提交历史
  - git blame -L <start>,<end> file 用于指定查看的起始行
  - 显示格式为：
    - commit ID | 代码提交者 | 提交时间 | 代码位于文件中的行数 | 实际代码

## git show commitID
  - git show commitID可以查看对应commit提交的具体信息
  - git show可以和git blame配合使用