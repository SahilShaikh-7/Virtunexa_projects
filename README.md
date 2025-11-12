https://sites.google.com/view/pradip-patil/subjects


2.2.	Write a program to implement Huffman Encoding using a greedy strategy.


🎯 Objective

To generate Huffman Codes (variable-length binary codes) for given characters based on their frequency.

➡️ Characters with higher frequency get shorter codes.
➡️ Characters with lower frequency get longer codes.

This minimizes the average number of bits needed to represent data — a core idea in data compression (used in ZIP, JPEG, MP3, etc.).

🧱 1. Import and Class Definition
import heapq


heapq provides a min-heap (priority queue).

It always pops the smallest frequency node first — perfect for the greedy strategy of Huffman coding.

🧩 Class: node
class node:
    def __init__(self, freq, symbol, left=None, right=None):
        self.freq = freq        # Frequency of the character
        self.symbol = symbol    # Character itself
        self.left = left        # Left child
        self.right = right      # Right child
        self.huff = ''          # To store '0' or '1' direction


Each node object represents a node in the Huffman Tree.
It can be:

A leaf node (contains a character)

An internal node (sum of two smaller frequencies)

👇 This method is used to compare nodes in heap:
def __lt__(self, nxt):
    return self.freq < nxt.freq


So that the heap knows how to compare two nodes — by their frequency.

🧮 2. Printing Huffman Codes
def printNodes(node, val=''):
    newVal = val + str(node.huff)

    if(node.left):
        printNodes(node.left, newVal)
    if(node.right):
        printNodes(node.right, newVal)

    if(not node.left and not node.right):
        print(f"{node.symbol} -> {newVal}")

🔍 What it does:

This function traverses the tree recursively:

Appends '0' or '1' as it moves left or right.

When it reaches a leaf node (a character), it prints its Huffman code.

📊 3. Input Data
chars = ['a', 'b', 'c', 'd', 'e', 'f']
freq = [5, 9, 12, 13, 16, 45]

Character	Frequency
a	5
b	9
c	12
d	13
e	16
f	45
🏗️ 4. Creating Initial Nodes
nodes = []
for x in range(len(chars)):
    heapq.heappush(nodes, node(freq[x], chars[x]))


This creates individual leaf nodes for all characters and pushes them into the min-heap.

🔄 5. Building the Huffman Tree
while len(nodes) > 1:
    left = heapq.heappop(nodes)
    right = heapq.heappop(nodes)
    left.huff = 0
    right.huff = 1
    newNode = node(left.freq+right.freq, left.symbol+right.symbol, left, right)
    heapq.heappush(nodes, newNode)

Step Explanation:

Pop 2 smallest nodes (least frequent symbols).

Assign directions:

Left child → 0

Right child → 1

Create new parent node

Frequency = sum of the two child frequencies

Push this parent node back into the heap.

Repeat until only one node remains — the root of the Huffman Tree.

🌳 6. Display Huffman Codes
printNodes(nodes[0])


nodes[0] is the root of the final Huffman Tree.

The function prints each character with its generated code.

🧩 7. Sample Output
f -> 0
c -> 100
d -> 101
a -> 1100
b -> 1101
e -> 111


(Output may vary slightly depending on implementation)

⚙️ 8. Complexity Analysis
Type	Complexity	Reason
Time	O(n log n)	Heap operations for combining n symbols
Space	O(n)	Tree nodes stored in memory
🧠 9. Short Summary (for viva)
Concept	Description
Algorithm Type	Greedy
Goal	Compress data by assigning variable-length binary codes
Strategy	Combine two smallest frequencies at each step
Data Structure	Min-Heap (Priority Queue)
Output	Huffman codes (shorter for frequent symbols)


















🧩 Huffman Encoding (Greedy Technique)
1. Introduction

Huffman Encoding is a lossless data compression algorithm developed by David A. Huffman in 1952.
It is based on the Greedy strategy and is used to reduce the total number of bits needed to represent a message without losing any information.

The basic idea is to assign shorter binary codes to more frequent characters and longer codes to less frequent characters, thus minimizing the overall size of the encoded data.

2. Principle Behind Huffman Coding

The algorithm builds a binary tree (called a Huffman Tree) where:

Each leaf node represents a character and its frequency.

The path from the root to the leaf represents the binary code for that character.

Moving left adds a 0, and moving right adds a 1.

Characters that occur more frequently are placed closer to the root, resulting in shorter codes, while infrequent ones go deeper in the tree.

3. Steps in Huffman Encoding Algorithm

Input:
A set of characters with their corresponding frequencies.

Create Leaf Nodes:
Each character and its frequency are used to create a leaf node.

Build a Min-Heap:
Insert all nodes into a min-heap (priority queue) based on frequency.

Combine Nodes:

Remove two nodes with the smallest frequencies.

Create a new internal node with these two as children.

The new node’s frequency = sum of its children’s frequencies.

Push the new node back into the heap.

Repeat until only one node remains — the root of the Huffman Tree.

Assign Codes:
Traverse the tree:

Left edge → 0

Right edge → 1
The code formed along the path from root to leaf is the Huffman Code of that character.

Output:
The set of binary Huffman codes for all characters.

4. Example
Character	Frequency
a	5
b	9
c	12
d	13
e	16
f	45

Step 1: Combine smallest frequencies repeatedly → Build tree
Step 2: Assign 0/1 codes from root to leaves

Resulting Huffman Codes (example):

Character	Code
f	0
c	100
d	101
a	1100
b	1101
e	111

Encoded string will use fewer bits than fixed-length coding.

5. Example Calculation of Average Code Length

Let frequency be total occurrences out of 100.

If each symbol used 3 bits (fixed-length for 6 symbols), total bits = 6 × 3 = 18 bits.
Using Huffman coding, total bits = (5×4 + 9×4 + 12×3 + 13×3 + 16×3 + 45×1) = much smaller.
Hence, compression achieved.

6. Pseudocode
Huffman(C):
1. n = number of characters
2. Create n leaf nodes for each character and insert into min-heap Q
3. while size(Q) > 1:
      left = ExtractMin(Q)
      right = ExtractMin(Q)
      z = new node()
      z.freq = left.freq + right.freq
      z.left = left
      z.right = right
      Insert(Q, z)
4. return ExtractMin(Q)  // root of Huffman tree

7. Time and Space Complexity
Measure	Complexity	Explanation
Time	O(n log n)	Each heap operation (insert/extract) takes log n time
Space	O(n)	For storing nodes of Huffman Tree
8. Advantages

Produces optimal prefix codes (no code is a prefix of another).

Achieves maximum compression for given frequencies.

Used in real-world compression formats like ZIP, JPEG, MP3, GZIP.

9. Disadvantages

Requires the frequency table to be known or sent with the encoded data.

May not be efficient if symbol frequencies are nearly equal.

Tree construction requires extra memory for large symbol sets.

10. Applications

File compression tools (WinZIP, 7-Zip)

Multimedia encoding (JPEG, MP3)

Network data transmission

Text and image compression in computer systems

11. Summary
Point	Description
Algorithm Type	Greedy
Structure Used	Binary Tree, Min-Heap
Goal	Minimize average code length
Output	Variable-length, prefix-free binary codes
Best Case	When frequencies vary widely
Worst Case	When all frequencies are equal

✅ In short:

Huffman Encoding is a greedy, optimal method of compressing data by assigning variable-length binary codes based on symbol frequencies, minimizing total bits required while ensuring lossless decoding.
