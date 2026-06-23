---
name: binary-search-demo
description: "二分查找（折半查找）演示：编写、运行、可视化查找过程，Python + C++ 双版本"
version: 1.0.0
author: Hermes Agent
platforms: [windows, linux, macos]
---

# 二分查找演示

当用户要求演示二分查找（二分法、折半查找）时，按以下步骤操作。

## 算法原理

二分查找是在**有序数组**中查找目标值的算法：
1. 取数组中间元素与目标值比较
2. 相等 → 找到
3. 目标值小于中间 → 在左半部分继续查找
4. 目标值大于中间 → 在右半部分继续查找
5. 重复直到找到或区间为空

**时间复杂度：** O(log n)

## 步骤

### 1. 创建 Python 脚本

写入桌面（Windows）或当前目录，包含：
- 二分查找函数（带过程可视化）
- 随机生成 N 个整数并排序
- 查找目标并展示每一步的查找区间

```python
import random

def binary_search(arr, target):
    """二分查找，返回索引和查找过程"""
    left, right = 0, len(arr) - 1
    steps = []
    
    while left <= right:
        mid = (left + right) // 2
        steps.append((left, mid, right, arr[left:right+1]))
        
        if arr[mid] == target:
            return mid, steps
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1, steps


def visualize_search(arr, target, result, steps):
    """可视化二分查找过程"""
    print(f"有序数组：{arr}")
    print(f"目标值：{target}")
    print(f"\n查找过程：")
    
    for i, (left, mid, right, sub) in enumerate(steps):
        marker = [" "] * len(arr)
        marker[mid] = "↑"
        print(f"  第{i+1}步: left={left}, right={right}, mid={mid}(arr[{mid}]={arr[mid]})")
        print(f"          {arr}")
        print(f"          {''.join(marker)}")
        if arr[mid] == target:
            print(f"          ✅ 找到！")
        elif arr[mid] < target:
            print(f"          → {arr[mid]} < {target}，向右查找")
        else:
            print(f"          → {arr[mid]} > {target}，向左查找")
    
    if result != -1:
        print(f"\n✅ 目标 {target} 找到，索引位置：{result}")
    else:
        print(f"\n❌ 目标 {target} 不存在于数组中")


if __name__ == "__main__":
    # 生成10个随机整数
    random.seed()
    n = 10
    data = random.sample(range(1, 100), n)
    data.sort()
    
    # 随机选择一个存在的目标
    target = random.choice(data)
    
    result, steps = binary_search(data, target)
    visualize_search(data, target, result, steps)
    
    # 也测试一个不存在的值
    print("\n" + "="*50)
    print("测试不存在的值：")
    target2 = 999
    result2, steps2 = binary_search(data, target2)
    visualize_search(data, target2, result2, steps2)
```

### 2. 运行并验证

```bash
python binary_search.py
```

确认输出展示每一步的查找区间变化。

### 3. 输出结果

给用户展示：
- 查找过程的分步可视化
- 找到/未找到的两种情况
- 时间复杂度分析

## 注意事项

- 二分查找要求数组**必须是有序的**
- 生成随机数后先排序再查找
- 代码放在桌面（Windows）方便用户查看
