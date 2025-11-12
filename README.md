https://sites.google.com/view/pradip-patil/subjects


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
