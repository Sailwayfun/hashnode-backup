---
title: "對稱二元樹 (Symmetric Tree)"
datePublished: 2026-05-04T15:08:54.020Z
cuid: cmorc4epu00fy2ad356ljg45t
slug: symmetric-tree

---

## 核心概念

要判斷一棵樹是否對稱，本質上是檢查其左子樹與右子樹是否互為鏡像 (Mirror)。

### 鏡像的定義：

1.  **根節點值相等**：left.val === right.val
    
2.  **外側對稱**：左子樹的左邊與右子樹的右邊相等。
    
3.  **內側對稱**：左子樹的右邊與右子樹的左邊相等。
    

* * *

## 解法一：迭代法 (Iterative BFS)

使用隊列 (Queue) 進行成對比較。

### 實作要點

*   將需要比較的對應節點「成對」放入隊列。
    
*   每次取出兩個節點進行對比。
    
*   **穩健性（robustness）優點**：使用堆積 (Heap) 記憶體，避免極深樹結構導致的棧溢出。
    

JavaScript

```plaintext
var isSymmetric = function (root) {
    if (!root) return true;
    const queue = [root.left, root.right];

    while (queue.length > 0) {
        const t1 = queue.shift();
        const t2 = queue.shift();

        // 穩健性檢查：處理 null 與數值
        if (!t1 && !t2) continue;
        if (!t1 || !t2 || t1.val !== t2.val) return false;

        // 成對推入鏡像位置節點
        queue.push(t1.left, t2.right); // 外側
        queue.push(t1.right, t2.left); // 內側
    }
    return true;
};
```

* * *

## 解法二：遞迴法 (Recursive DFS)

利用函數呼叫棧進行深度優先遍歷。

### 實作要點

*   定義一個輔助函數 dfs(left, right)。
    
*   **穩健性（robustness）考量**：在呼叫 dfs(root.left, root.right) 前需確保 root 不為 null。
    

JavaScript

```javascript
var isSymmetric = function (root) {
    if (!root) return true; 

    function dfs(left, right) {
        // 基準條件 (Base Cases)
        if (!left && !right) return true;
        if (!left || !right || left.val !== right.val) return false;

        // 遞迴檢查鏡像子樹
        return dfs(left.left, right.right) && dfs(left.right, right.left);
    }

    return dfs(root.left, root.right);
};
```

* * *

## 穩健性 (Robustness) 總結比較

<table style="min-width: 75px;"><colgroup><col style="min-width: 25px;"><col style="min-width: 25px;"><col style="min-width: 25px;"></colgroup><tbody><tr><td colspan="1" rowspan="1"><p><strong>特性</strong></p></td><td colspan="1" rowspan="1"><p><strong>迭代法 (Queue)</strong></p></td><td colspan="1" rowspan="1"><p><strong>遞迴法 (DFS)</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>記憶體限制</strong></p></td><td colspan="1" rowspan="1"><p>較穩健，受限於堆積空間</p></td><td colspan="1" rowspan="1"><p>較危險，易受限於執行棧大小 (Stack Overflow)</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>程式碼簡潔度</strong></p></td><td colspan="1" rowspan="1"><p>邏輯較多，需手動管理隊列</p></td><td colspan="1" rowspan="1"><p>極度簡潔，符合樹的遞迴特性</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>空值處理</strong></p></td><td colspan="1" rowspan="1"><p>需注意 shift() 後的 null 判斷</p></td><td colspan="1" rowspan="1"><p>需注意初始 root 為 null 的讀取錯誤</p></td></tr></tbody></table>