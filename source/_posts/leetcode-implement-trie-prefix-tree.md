---
title: leetcode-implement-trie-prefix-tree
date: 2019-08-14 01:11:46
categories:
- Programming
- Algorithm
tags:
- algorithm
- data structure
- tree
- python
---

## 题目介绍
原题链接：[https://leetcode.com/problems/implement-trie-prefix-tree/](https://leetcode.com/problems/implement-trie-prefix-tree/)

### 208. Implement Trie (Prefix Tree)

- difficulty：Medium


Implement a trie with `insert`, `search`, and `startsWith` methods.

**Example:**

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

**Note:**

- You may assume that all inputs are consist of lowercase letters `a-z`.
- All inputs are guaranteed to be non-empty strings.

## 思路

> 在[计算机科学](https://zh.wikipedia.org/wiki/计算机科学)中，**trie**，又称**前缀树**或**字典树**，是一种有序[树](https://zh.wikipedia.org/wiki/树_(数据结构))，用于保存[关联数组](https://zh.wikipedia.org/wiki/关联数组)，其中的键通常是[字符串](https://zh.wikipedia.org/wiki/字符串)。与[二叉查找树](https://zh.wikipedia.org/wiki/二叉查找树)不同，键不是直接保存在节点中，而是由节点在树中的位置决定。一个节点的所有子孙都有相同的[前缀](https://zh.wikipedia.org/wiki/前缀)，也就是这个节点对应的字符串，而根节点对应[空字符串](https://zh.wikipedia.org/wiki/空字符串)。一般情况下，不是所有的节点都有对应的值，只有叶子节点和部分内部节点所对应的键才有相关的值。

- 首先定义一个节点TrieNode，有两个元素，一个是children，代表孩子，总长度是字母表的个数，另一个是isEnd，表示是否是单词结尾（叶子）
- Trie树有一个根节点root
- 不同的叶子可能有公共的前缀

### 插入

- 键值为字符串，长度为m
- 遍历键值中的每个字符，当此位置的孩子为空时插入节点，不为空时，当前节点指向这个孩子，重复操作直到最后一个字符，将当前节点的isEnd标记为True
- 时间复杂度：$O(m)$
- 空间复杂度：$O(m)$

### 查找

- 键值为字符串，长度为m
- 从根节点开始，检查是否有指向键值第一个字符的链接
- 如果有，移动到该节点，继续查找下一个字符
- 如果没有
  - 还有键的字符剩余，返回False
  - 没有键的字符剩余，如果节点isEnd是True，则返回True，否则要查找到键值只是其他单词的前缀，返回False
- 时间复杂度：$O(m)$
- 空间复杂度：$O(1)$

### 查找前缀

- 与查找相似，不同之处在与，遍历完键值的字符时，不管节点的isEnd是否为True，都返回True
- 键值为字符串，长度为m
- 从根节点开始，检查是否有指向键值第一个字符的链接
- 如果有，移动到该节点，继续查找下一个字符
- 如果没有
  - 还有键的字符剩余，返回False
  - 没有键的字符剩余，则返回True
- 时间复杂度：$O(m)$
- 空间复杂度：$O(1)$

## 代码实现

### python2，使用节点，beats 19.77%

```python
class TrieNode(object):
    def __init__(self):
        self.children = [None]*26
        self.isEnd = False

class Trie(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode()

    def insert(self, word):
        """
        Inserts a word into the trie.
        :type word: str
        :rtype: None
        """
        cur = self.root
        for char in word:
            if cur.children[ord(char) - ord('a')] is None:
                cur.children[ord(char) - ord('a')] = TrieNode()
            cur = cur.children[ord(char) - ord('a')]
        cur.isEnd = True

    def search(self, word):
        """
        Returns if the word is in the trie.
        :type word: str
        :rtype: bool
        """
        cur = self.root
        for char in word:
            if cur.children[ord(char) - ord('a')]:
                cur = cur.children[ord(char) - ord('a')]
            else:
                return False
        return cur is not None and cur.isEnd

    def startsWith(self, prefix):
        """
        Returns if there is any word in the trie that starts with the given prefix.
        :type prefix: str
        :rtype: bool
        """
        cur = self.root
        for char in prefix:
            if cur.children[ord(char) - ord('a')]:
                cur = cur.children[ord(char) - ord('a')]
            else:
                return False
        return cur is not None


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

### python3，不使用节点，直接使用字典实现，速度更快，beats 98.67%

```python
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.trie = {}

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        trie = self.trie
        for ch in word:
            if ch not in trie:
                trie[ch] = {}
            trie = trie[ch]
        trie['end'] = True
        
    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        trie = self.trie
        for ch in word:
            if ch in trie:
                trie = trie[ch]
            else:
                return False
        return 'end' in trie

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        trie = self.trie
        for ch in prefix:
            if ch in trie:
                trie = trie[ch]
            else:
                return False
        return True


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```
