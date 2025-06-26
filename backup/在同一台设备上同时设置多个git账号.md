# 0 介绍

经常有这样的情况，需要在同一台设备上同时用多个git账号来工作，今天就尝试来解决这个问题。
git支持 `https` 和 `ssh` 协议，要解决上面的需求一般就选择 `ssh` 协议比较好。

# 1 配置文件

ssh有一个配置文件，默认是 `~/.ssh/config`，里面配置了各个 `ssh` 账号的信息， 包括 Host， 密钥等。
例如：
```shell
Host *   
  AddKeysToAgent yes   
  UseKeychain yes   
  IdentityFile ~/.ssh/id_ed25519
```

# 2 新增一个配置

```shell
ssh-keygen -t ed25519 -C "demo@gmail.com"
```

然后就可以在 `~/.ssh/` 下面看到新增了一个私钥和一个公钥，然后将公钥上传到 `github` 的 `Profile -> SSH and GPG keys -> New SSH key`

# 3 修改 ssh config文件

```shell
Host demo   
  HostName github.com   
  AddKeysToAgent yes   
  UseKeychain yes   
  IdentityFile ~/.ssh/id_demo_ed25519
```


# 4 将私钥加入ssh

```shell
ssh-add ~/.ssh/id_demo_ed25519
```


# 5 Clone

加入我们对应的项目， 例如在 `github` 上的remote地址为：
```shell
git@github.com:xxx/yyy.git
```

那么在配置之后我们在clone的时候需要改为
```shell
git clone git@demo:xxx/yyy.git
```




# * 引用
1. https://www.ssh.com/academy/ssh/config
2. https://www.cnblogs.com/cangqinglang/p/12462272.html