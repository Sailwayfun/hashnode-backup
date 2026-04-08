---
title: "Morris Traversal"
datePublished: 2026-04-08T00:56:15.206Z
cuid: cmnpc7qzp00zb1qqe1qdn7aue
slug: morris-traversal
cover: https://cdn.hashnode.com/uploads/covers/67fb74a3ee9bbbf179cc6353/14b592d8-366a-4b7e-b96b-7db32b63a421.jpg

---

最近在練習二元樹 (Binary Trees) 相關的演算法，遇到需要驗證一棵樹是否符合**二元搜尋樹** (Binary Search Tree) 的需求，首先得懂甚麼是 Binary Search Tree:

### Binary Search Tree

> A **valid BST** is defined as follows:
> 
> *   The left subtree of a node contains only nodes with keys **strictly less than** the node's key.
>     
> *   The right subtree of a node contains only nodes with keys **strictly greater than** the node's key.
>     
> *   Both the left and right subtrees must also be binary search trees.
>     

根據這個定義，Binary Search Tree 不會有兩個數值相等的節點，同時，每個節點往左看都會比它的數值小，往右看都會比它的數值大。

### Naive Solution

一開始直覺的想法是，就利用深度優先搜尋 (dfs) 檢查每個節點的左節點是否小於自己和右節點是否大於自己：

```javascript
function isValidBST(root) {
  if (!root) return true;

  // 只檢查「直接子節點」
  if (root.left && root.left.val >= root.val) return false;
  if (root.right && root.right.val <= root.val) return false;

  return isValidBST(root.left) && isValidBST(root.right);
}
```

但只檢查一層是不夠的，因為有可能右邊的子樹仍會有小於當下節點數字，但遍歷到右節點時，我們就丟失了比較的基準，例如:

```plaintext
      10
     /  \
    5   15
       /  \
      6   20
```

這棵樹在第一層是合法的，但 6 卻出現在 10 的右子樹，照上面的邏輯這棵樹會回傳 true，就是判斷錯誤的例子。

### Better Mindset

看到 NeetCode 的提示有提到可以用區間的概念來想。每個節點的數字有最大和最小值，我們先初始化一組最大最小值，往左走時將最大值更新為當下節點的值，反之往右走時，則更新最小值。

```javascript
function isValidBST(root) {
  function check(node, min, max) {
    if (!node) return true;
    if (node.val >= max || node.val <= min) return false;
    return check(node.left, min, node.val) && check(node.right, node.val, max);
  }
  return check(root, -Infinity, Infinity);
}
```

這邊有個小技巧是用無限大和負無限大初始化邊界，就可以避免遺漏檢查範圍。這種解法要遍歷所有的節點，且因為用遞迴會需要空間儲存回傳每一層的答案，時間複雜度和空間複雜度都是 *O(n)*

### Creative Solution: Morris Traversal

後來看到一種特殊的遍歷方式，可以把空間複雜度壓縮到 O(1)。一般我們不使用遞迴來做 DFS 的話，就會用 stack 搭配 while loop，雖然可以避免因為遞迴層次過深造成 stack overflow，但空間複雜度還是 *O(n)。*

Morris Traversal 的想法是，當我往左走到沒路的地方，要回到根節點一定得依賴 stack 嗎? 不，一個特別的想法是: 我先找左子樹最右邊的節點，這個節點一般稱為前驅節點 (in Order Predecessor)，因為依照 左 -> 根-> 右順序遍歷時，它出現的位置會在當前節點的前面。如果一開始左邊就沒路，就會往右走，如果左邊有路，右邊已經到了前驅節點，就建立一個傳送門接回根結點，並一邊往右走。走到發現已經繞回根節點後，再把傳送門拆掉，並開始往左邊走。

這種演算法的好處是，不用額外的空間來儲存左邊已經探訪過的節點，只要用右邊的傳送門讓左邊可以順順的走完，也因此有效降低空間複雜度。

```javascript
function isValidBST(root) {
  let prevVal = -Infinity;
  let curr = root;
  let isValid = true; //use global variable to avoid breaking tree structure when early return

  //Morris Traversal: find predecessor before traversal

  while (curr) {
    if (!curr.left) {
      if (prevVal >= curr.val) isValid = false;
      prevVal = curr.val;
      curr = curr.right;
    } else {
      let pre = curr.left;
      while (pre.right && pre.right !== curr) {
        pre = pre.right;
      }

      if (!pre.right) {
        //first round, create cycle from predecessor
        pre.right = curr;
        curr = curr.left;
      } else {
        //second round, break the cycle
        pre.right = null;

        if (prevVal >= curr.val) isValid = false;
        prevVal = curr.val;
        curr = curr.right;
      }
    }
  }

  return isValid;
}
```

### Sum Up

Morris Traversal 透過暫時破壞樹的結構，節省原本需要額外儲存訪問過節點的 stack 空間開銷，是一種創意的解法，但通常這種破壞結構的解法，都會希望可以在完成演算後能保有樹的原本結構，例如上面的算法其實可以 early return，但這樣樹的結構就壞掉。在需要時間恢復樹的前提下，雖然所有的節點都會被探訪兩次，時間複雜度一樣是 O (n)，也算是非常高效了。