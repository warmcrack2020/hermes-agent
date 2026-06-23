---
name: bubble-sort-demo
description: "排序算法跨语言演示：编写、编译/运行、可视化排序过程（Python + C++），含 Wandbox 在线编译技巧"
version: 2.2.0
author: Hermes Agent
platforms: [windows, linux, macos]
related_skills: [online-cpp-compilation]
---

# 冒泡排序演示

当用户要求编写冒泡排序或演示排序算法时，按以下步骤操作。

支持 **Python**（直接运行）和 **C++**（本地编译或 Wandbox 在线编译）。

---

## 步骤

### 1. 创建 Python 脚本

写入 `HERMES-DOC` 目录（桌面下）或用户指定的目录，包含：
- 带 `swapped` 标志的优化冒泡排序
- 每轮排序过程的可视化输出
- 提前结束检测（本轮无交换即有序）

```python
def bubble_sort(arr):
    """冒泡排序（优化版）"""
    n = len(arr)
    for i in range(n - 1):
        swapped = False
        print(f"\n第 {i+1} 轮排序：")
        for j in range(n - 1 - i):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
                print(f"  交换 → {arr}")
        if not swapped:
            print("  本轮无交换，提前结束！")
            break
    return arr


if __name__ == "__main__":
    data = [9, 5, 3, 7, 2, 8]
    print(f"排序前：{data}")
    sorted_data = bubble_sort(data)
    print(f"\n排序后：{sorted_data}")
```

### 2. 运行并验证（Python）

```bash
python bubble_sort.py
```

### 3. 可选：C++ 版本

当用户要求 C++ 版本时，创建以下模板：

```cpp
#include <iostream>
#include <vector>
using namespace std;

void bubble_sort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        cout << "\n第 " << i + 1 << " 轮排序：\n";
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
                cout << "  交换 → ";
                for (int x : arr) cout << x << " ";
                cout << "\n";
            }
        }
        if (!swapped) {
            cout << "  本轮无交换，提前结束！\n";
            break;
        }
    }
}

int main() {
    vector<int> data = {5, 3, 7, 1, 2, 9, 6, 3};
    cout << "排序前：";
    for (int x : data) cout << x << " ";
    cout << "\n";
    bubble_sort(data);
    cout << "\n排序后：";
    for (int x : data) cout << x << " ";
    cout << "\n";
    return 0;
}
```

#### 3a. 有本地编译器（g++/clang++）

```bash
g++ -O2 -std=c++17 bubble_sort.cpp -o bubble_sort
./bubble_sort
```

#### 3b. 无本地编译器 — 用 Wandbox 在线 API

```bash
# 转义源码为 JSON + 发送编译请求
CODE=$(cat bubble_sort.cpp | python -c "import sys,json; print(json.dumps(sys.stdin.read()))")
curl -sL -X POST "https://wandbox.org/api/compile.json" \
  -H "Content-Type: application/json" \
  -d "{\"compiler\":\"gcc-head\",\"code\":$CODE,\"options\":\"-O2 -std=c++17\",\"save\":false}"
```

返回 JSON 中的 `program_output` 字段即运行结果，`status: "0"` 表示成功。

### 4. 输出结果

给用户展示：
- 排序前后的对比
- 每轮排序的交换过程
- 提前结束优化的效果说明
- 相同数据 Python 和 C++ 输出一致性验证（可选）

---

## 安装 & 使用

### 安装此技能

```bash
# 方式1：从 GitHub 直接安装
hermes skills install \
  "https://raw.githubusercontent.com/warmcrack2020/hermes-agent/master/skills/software-development/bubble-sort-demo/SKILL.md"

# 方式2：添加技能源后安装
hermes skills tap add warmcrack2020/hermes-agent
hermes skills install bubble-sort-demo
```

### 加载使用

```bash
# 启动时加载
hermes -s bubble-sort-demo

# 聊天中加载
/skill bubble-sort-demo
```

加载后，说"演示冒泡排序"或"来个C++版排序"，我就会按此技能流程执行。

## 注意事项

- Windows 路径用 `/c/Users/...` 格式（git-bash）或 `C:/...`
- 默认数据 `[9, 5, 3, 7, 2, 8]`
- 所有生成的文件保存到 `C:/Users/Administrator/Desktop/HERMES-DOC/`，**不要直接放桌面**
- 无本地 C++ 编译器时，用 Wandbox API 在线编译运行（设 `--max-time 60`）
- 用 `python -c "import sys,json; print(json.dumps(sys.stdin.read()))"` 将多行源码安全转义为 JSON 字符串
- 优先直接创建目标文件，而非先生成中间脚本再执行（用户偏好）
