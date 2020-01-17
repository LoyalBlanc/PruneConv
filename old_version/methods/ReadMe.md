# Methods v0.4

## 使用方法
0. 声明调用并初始化剪枝工具  
    ```
    import method.abstract_method as pruning_methods
    
    pruning_methods.PruningTool(
        input_channel=1,                    # 网络的输入通道数
        method="minimum_weight",            # 设定剪枝方法
        pruning_rate=0.1,                   # 设定目标剪枝率
        ) -> PruningTool                    # 返回修剪工具
    ```
1. 一次式剪枝
    根据给定的剪枝率对网络进行直接修剪：
    ```
    pruning_tool.one_shot_pruning(
        network,                            # 目标网络
        ) -> an.AbstractNetwork             # 返回修剪后的网络
    ```
    一次成功的修剪预期输出：
    >   Start one-shot pruning.
        FLOPs before pruning is 600064, target is 540057.  
        Pruning Channel 26 in Layer .layer1.c, .layer4.c,  (Score 0.3877)  
        Pruning Channel 6 in Layer .layer1.c, .layer4.c,  (Score 0.3910)  
        ...  
        Pruning Channel 6 in Layer .layer1.c, .layer4.c,  (Score 0.6221)  
        Successfully prune the network, the FLOPs now is 538624
        
<del>  

2. 迭代式剪枝
    根据给定的剪枝率对网络进行修剪的同时进行fine-tuning：
    ```
    pruning_tool.iterative_pruning(
        network,                            # 目标网络
        dataset,                            # Fine-tuning使用的数据集
        criterion,                          # Fine-tuning使用的损失函数 
        lr=1e-3,                            # 学习率 默认使用Adam优化参数
        epoch_interval=1,                   # 两轮修剪之间所需的训练轮数               
        ) -> an.AbstractNetwork             # 返回修剪后的网络
    ```
    一次成功的修剪预期输出：
    >   Start iterative pruning.  
        FLOPs before pruning is 600064, target is 540057.  
        Pruning Channel 6 in Layer .layer1.c, .layer4.c,  (Score 0.3507)  
        Epoch 1, FLOPs: 593920, Step Loss: 178.8125  
        Epoch 2, FLOPs: 593920, Step Loss: 153.6338  
        Pruning Channel 25 in Layer .layer1.c, .layer4.c,  (Score 0.3836)    
        ...  
        
        

3. 自动剪枝
    在给定的轮次内按Loss值尽可能地修剪网络：
    ```
    pruning_tool.automatic_pruning(
        network,                            # 目标网络
        dataset,                            # Fine-tuning使用的数据集
        criterion,                          # Fine-tuning使用的损失函数 
        lr=1e-3,                            # 学习率 默认使用Adam优化参数
        epochs=10,                          # 最大训练轮数
        ) -> an.AbstractNetwork             # 返回Fine-tuning过程中Loss最低的网络和参数
    ```
    一次成功的修剪预期输出：
    >   123  
        123

</del>



*更多内容可参考demo.py*