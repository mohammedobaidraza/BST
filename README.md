# Persistent Binary Search Tree.

A Python implementation of a persistent binary search tree that maintains the history of all operations, allowing access to previous versions of the tree.

## Overview

This persistent binary search tree (BST) implementation uses the "fat node" approach to maintain the history of all changes while providing efficient access to any previous version of the tree. Each operation creates a new version rather than modifying the existing structure, making it ideal for applications requiring historical data access or undo functionality.

## Features

- **Version Control**: Maintain a complete history of all tree modifications
- **Immutability**: Previous versions remain unchanged when new operations are performed
- **Historical Access**: Retrieve any previous state of the tree by version number
- **Standard BST Operations**: Supports insert, delete, and search operations
- **Efficient**: Minimizes memory usage by sharing unchanged portions of the tree across versions

## How It Works

The implementation uses two main classes:

1. **FatBSTNode**: Maintains versioned properties (value, left child, right child) for each node
2. **PersistentBST**: Orchestrates operations and version management for the entire tree

Each modification operation:
- Creates a new version number
- Clones only the nodes along the path that changes
- Shares unchanged subtrees with previous versions
- Stores the new root in a version history

## Usage

```python
# Create a new persistent BST
tree = PersistentBST()

# Insert elements (each creating a new version)
tree.insert(50)
tree.insert(30)
tree.insert(70)

# Print the current version of the tree
print("Current tree:")
tree.print_tree()

# Delete a node
tree.delete(30)
print("After deletion:")
tree.print_tree()

# Access an older version
print("Tree at version 2:")
tree.print_tree(2)

# Search for values in different versions
print("Is 30 in current version?", tree.search(30))
print("Is 30 in version 2?", tree.search(30, 2))
```

## Applications

Persistent data structures like this BST are useful in:

- **Version control systems**: Maintaining different states of data
- **Functional programming**: Providing immutable data structures
- **Undo/redo functionality**: Reverting to previous states
- **Concurrent access**: Allowing multiple readers without locking
- **Time travel debugging**: Inspecting historical states

## Implementation Details

### FatBSTNode Class

The `FatBSTNode` class stores values and child references as version-value pairs:

- `value_versions`: List of (version, value) tuples
- `left_versions`: List of (version, left_node) tuples
- `right_versions`: List of (version, right_node) tuples

Methods:
- `get_value(version)`: Get the node's value at a specific version
- `get_left(version)`: Get the left child at a specific version
- `get_right(version)`: Get the right child at a specific version
- `set_value(version, value)`: Set the value at a new version
- `set_left(version, node)`: Set the left child at a new version
- `set_right(version, node)`: Set the right child at a new version
- `clone_for_version(version)`: Create a new node with the same properties at a new version

### PersistentBST Class

The `PersistentBST` class manages the tree structure and versioning:

- `version`: Current version number
- `root_versions`: List of (version, root_node) tuples

Methods:
- `insert(value)`: Insert a value, creating a new version
- `delete(value)`: Delete a value, creating a new version
- `search(value, version)`: Search for a value in a specific version
- `get_root(version)`: Get the root node at a specific version
- `print_tree(version)`: Print the tree at a specific version

## Time and Space Complexity

- **Time Complexity**: O(log n) for search, O(log n) for insert and delete operations
- **Space Complexity**: O(log n) additional space per operation for path copying

## Installation

```bash
# Clone the repository
git clone https://github.com/mohammedobaidraza/bst.git

# Navigate to the project directory
cd persistent-bst
```

## Example

```python
from persistent_bst import PersistentBST

# Create a tree with initial values
tree = PersistentBST()
tree.insert(50)
tree.insert(30)
tree.insert(70)
tree.insert(20)
tree.insert(40)

# View tree at version 3
print(tree.in_order_traversal(3))  # [20, 30, 40, 50, 70]

# Make modifications
tree.delete(30)  # Creates version 6
tree.insert(35)  # Creates version 7

# Compare different versions
print(tree.in_order_traversal(3))  # [20, 30, 40, 50, 70] (unchanged)
print(tree.in_order_traversal(6))  # [20, 40, 50, 70] (after deletion)
print(tree.in_order_traversal(7))  # [20, 35, 40, 50, 70] (latest)
```

## License

[MIT License](LICENSE)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request
