[[Basics]]

1. Enumeration topics: [[#Combinations]], [[#Ordered Numbe Sequence]], [[#Subsequence]], [[#Subsets]], [[#Permutation]], [[#Board]]
	- In these topics, we keep creating nodes in a tree structure. Since we will only explore each node once, we do not care about how to process the node after we visit it. 
	- Mark nodes as the current path and edges as the element we use
1. Graph topics: [[#Pathfinding]], [[#Topological Sort]]
	- These topics fit the topics of DFS better. We're actually exploring in a graph, and we may reach one node several times, so we have to consider how process those nodes. Do we every need to explore this node again or not? We use modified versions of DFS to solve the problem. 
- If this element is not in the loop, when do we continue and when do we return?
	- If we still want to check later elements in the same level, we continue to skip this element. We do it when later elements may still be valid. e.g. [[491. Non-decreasing Subsequences]]
	- If we don't want to check later elements in the same level, which means all elements after is impossible to be valid, we return to stop checking the whole level. e.g. [[93. Restore IP Addresses]]
# Combinations

## No Duplicate

[[77. Combinations]]

[[216. Combination Sum III]]

[[39. Combination Sum]]

## Remove duplicate

[[40. Combination Sum II]]

# Ordered Numbe Sequence

- Use product rule

[[17. Letter Combinations of a Phone Number]]



  

# Subsequence 

[[131. Palindrome Partitioning]]

[[93. Restore IP Addresses]]

  

# Subsets

## No Duplicates

[[78. Subsets]]

## Remove duplicate

[[90. Subsets II]]

[[491. Non-decreasing Subsequences]]

[[282. Expression Add Operators]]

  

# Permutation

## No Duplicate

[[46. Permutations]]

[[22. Generate Parentheses]]
## Remove duplicate

[[47. Permutations II]]

# Board

[[51. N-Queens]]

[[37. Sudoku Solver]]

# Pathfinding

[[79. Word Search]]

[[200. Number of Islands]]

[[212. Word Search II]]

[[489. Robot Room Cleaner]]
# Topological Sort

[[210. Course Schedule II]]