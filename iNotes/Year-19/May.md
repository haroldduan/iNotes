<!--
@author: harold.duan
@date: 19-05-01
@memo: Notes logging
-->

# git command memo

撤销上一次的commit
``` git
$ git reset --soft HEAD^
```

> HEAD^的意思是上一个版本，也可以写成HEAD~1  
> 如果你进行了2次commit，想都撤回，可以使用HEAD~2  
>   
> 参数：  
>   
> --mixed   
> 不删除工作空间改动代码，撤销commit，并且撤销git add . 操作  
> 这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。  
>   
> --soft  
> 不删除工作空间改动代码，撤销commit，不撤销git add .   
>   
> --hard  
> 删除工作空间改动代码，撤销commit，撤销git add .   
> 注意完成这个操作后，就恢复到了上一次的commit状态。  
>   
> 如果commit注释写错了，只是想改一下注释，只需要：  
> git commit --amend