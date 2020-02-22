# Optimal Solution
An optimal solution is a feasible solution where the objective function reaches its maximum (or minimum) value - for example, the most profit or the least one cost. A globally optimal solution is one where there are no other feasible solutions with better objective function values.

# Greedy Algorithms
An algorithm is designed to achieve optimum solution for a given problem. In greedy algorithm approach, decisions are made from given solution domain. As being greedy, the closest solution that seems to provide an optimum solution is chosen.
Greedy algorithms try to find a localized optimum solution, which may eventually lead to globally optimized solutions. However, generally greedy algorithms do not provide globally optimized solutions.

The greedy algorithm, as the name suggest, always makes the choice that seems to be the best at that moment. This means that it makes a locally-optimal choice in the hope that this choice will lead to a globally-optimal solution.

## Counting Coins
This problem is to count to a desired value by choosing the least possible coins and the greedy approach forces the algorithm to pick the largest possible coin. If we are provided coins of Rs. 1, 2, 5 and 10 and we are asked to count Rs.18 then the greedy procedure will be -
1. Select one Rs.10 coin, the remaining count is 8
2. Then select one Rs.5 coin, the remaining count is 3
3. Then select one Rs.2 coin, them remaining count is 1
4. Then select one Rs.1 coin solve the problem.

Though, it seems to be working fine, for this count we need to pick only 4 coins. But if we slightly change the problem then the same approach may not be able to produce the same optimum result.

For the currency system, where we have coins of 1, 7, 10 value, counting coins for value 18 will be absolutely optimum but for count like 15, it may use more coin than necessary. For example, the greedy approach will use 10 + 1 + 1 + 1 + 1 + 1, total 6 coins. Whereas the same problem could be solved by using only 3 coins (7 + 7 + 1)


