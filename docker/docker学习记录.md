# docker学习笔记

## 1.ENTRYPOINT 与CMD
ENTRYPOINT与CMD都是在**容器**启动之后的执行命令。区别在于，cmd在执行docker run 的时候，可以被run 命令container name后面的命令替换，在dockerfile 里面配置的cmd的内容将作为参数进行执行，ENTRYPOINT可以解决这个问题。