# 初探mount

<h2  style="font-size: 18px; display: inline-block; padding-left: 10px; border-left: 5px solid rgb(145, 109, 213); font-weight:bold;">更新流程</h2>

更新的目的：

- 生成wip fiberNode树
- 标记flags

更新流程的步骤：

- 递： beginWork
- 归： completeWork

<h3 style="text-align:center;  font-size: 20px;font-weight: bold;"><span style="border-bottom: 2px solid rgb(145, 109,213);">beginWork</span></h3>

对比B的current fiberNode与B的reactElement,生成B对应的wip fiberNode

- placement

```md
插入： a -> ab 移动： abc -> bca
```

- childDeltetion

```md
删除 ul>li*3 -> ul>li*1
```



- propsUpdate

```md
<img title="wip"/> -> <img title="pending"/>
```

__实现与Host相关节点的beginWork__

首先，为开发环境增加`__DEV__`标识，方便Dev包打印更多信息

```bash
pnpm i -d -w @rollup/plugin-replace
```



HostRoot的beginWork工作流程

1. 计算状态的最新值
2. 创造子fiberNode

HostComponent的beginWork工作流程

1. 创造子fiberNode

HostText没有beginWork工作流（因为它没有子节点）

<h2  style="font-size: 18px; display: inline-block; padding-left: 10px; border-left: 5px solid rgb(145, 109, 213); font-weight:bold;">beginWork的性能优化策略</h2>
