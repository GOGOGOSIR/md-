# 使用github， push时出去设置用户名密码操作

```
git config --global credential.helper store
```

这一步会在用户目录下的.gitconfig文件最后添加:

现在push你的代码 (`git push`), 这时会让你输入用户名密码, 这一步输入的用户名密码会被记住, 下次再push代码时就不用输入用户名密码啦!

