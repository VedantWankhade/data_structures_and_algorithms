# Proof by Induction
* Induction and recursion are similiar.
* Recursion is the mathematical induction in action.
* In both, we have general and boundary conditions, with the general condition breaking the problem into smaller and smaller pieces. The initial or boundary condition terminates the recursion.
* Induction is used to prove incremental or recursive algorithms.

## Real Life Example
Suppose, we line up a large group of people, and hand each of them a piece of paper and a pen. The first person in line (on the far left) will write a number on their paper, and each person will look at their neighbor on the left and copy what their neighbor wrote on the paper. If we glance at what the first person wrote, and his paper says `27`, then intuitively we know that every person has `27` written on their paper (assuming that every person followed the rule faithfully). If you were to explain why you know that is true, you might say something along the lines of "Well, the first person wrote `27` on their paper, and every other person copied the person on their left." That is just your two conditions! We can see how if you took either of them away, we can't say anything about some arbitrary person in the line.

We can make this more complicated by changing the "rule". If instead the rule is that each person adds `1` to what the person on their left had, then intuitively you can see that a person in position `i` in the line will have `whatever the first person had + (i-1)`.

## Example 
**Problem:** Prove the correctness of the following recursive algorithm for incrementing natural numbers, that is, `y -> y + 1`

```
Increment(y)
    if (y = 0) then return 1
    else if (y mod 2) = 1 then return Increment(floor(y/2))
         else return y + 1
```

**Solution:** 

**Basis Case**: Basis case is of `y = 0`, which is correctly handled as it returns `1` and `0 + 1 = 1`.

**Inductive Hypothesis:** Now assume that the algorithm is correct for some number `n - 1`, that is for `y = n - 1` we get correct result. And because this algorithm is recursive, we can safely assume that it works correctly for all the natural values from `1` to `n - 1` i.e for `y <= n - 1`. Now we must proove that it also works for `y = n`.

The case for even number is handled correctly because `y + 1` is explicitly returned.

For odd numbers (i.e the case where `y mod 2 = 1`), the answer depends on what `Increment(floor(y/2))` returns. Here we use our inductive hypothesis.

Now for odd `y`, we can assume `y = 2m + 1` for some natural integer `m`.

Therefore,
```
Increment(y) = 
2 * Increment(Floor(y/2)) = 2 * Increment(Floor((2m + 1)/2))
                          = 2 * Increment(Floor(m + 1/2))
                          = 2 * Increment(m)
                          = 2 * (m + 1)
                          = 2m + 2
Increment(y)              = y + 1
```
**Conclusion:** As for even and odd integer `y`, `Increment(y)` results in `y + 1`, and for basis case of `y = 0` gives correct answer `1`, it is proved that the algorithm works correctly for any natural integer.

# Modelling the Problem using Combinatorial Objects
Real world problems include real objects. For example, suppose we want to write an algorithm for car parking system, this problem include real cars, but we cannot include real cars in our algorithm, we include something that represents a car. In most cases algorithms have to work on a collection of objects, so we have to generalise the collection of objects in one of the following: 
* **Permutations** are arrangements, or orderings, of items. For example, { 1, 4, 3, 2 } and { 4, 3, 2, 1 } are two distinct permutations of the same set of four integers. Permutations are likely the object in question whenever our problem seeks an "arrangement", "tour", "ordering", or "sequence".
* **Subsets** represent selections from a set of items. For example, { 1, 3, 4 } and { 2 } are two distinct subsets of the first four integers. Order does not matter in subsets the way it does with permutations, so the subsets { 1, 3, 4 } and { 4, 3, 1 } would be considered identical. They are likely the object in question whenever your problem seeks a "cluster", "collection", "committee", "group", "packaging", or "selection".
* **Trees** represent hierarchical relationships between items. Trees are likely the object in question whenever your problem seeks a "hierarchy", "dominance relationship", "ancestor/descendant relationship", or "taxonomy".
* **Graphs** represent relationships between arbitrary pairs of objects. For example, a network of roads as a graph, where the vertices are cities and the edges are roads connecting pairs of cities. Graphs are likely the object in question whenever you seek a "network", "circuit", "web", or "relationship".
* **Points** define locations in some geometric space. For example, the locations of McDonald’s restaurants can be described by points on a map/plane. Points are likely the object in question whenever your problems work on "sites", "positions", "data records", or "locations".
* **Polygons** define regions in some geometric spaces. For example, the borders of a country can be described by a polygon on a map/plane. Polygons and polyhedra are likely the object in question whenever you are working on "shapes", "regions", "configurations", or "boundaries".
* **Strings** represent sequences of characters, or patterns. For example, the names of students in a class can be represented by strings. Strings are likely the object in question whenever you are dealing with "text", "characters", "patterns", or "labels".

**The structures given above are recursive structures, for example, if we remove the first element from a permutation, we get a permutation of remaining elements. Learning to think recursive about these structures is learning to break a big problem into small instances of same kind.**

**Modeling your application in terms of well-defined structures and algorithms is the most important single step towards a solution.**


# Interview Questions
1. **Problem:** Write a function to perform integer division without using either the `/` or `*` operators.

    **Solution:** The key observation is that the quotient of a division is just the number of times that we can subtract the divisor from the dividend without making it negative.

    Here we are increasing the value of divisor using **left shift by 1** (equivalent to **multiply by 2**) until it is greater than or equal to dividend. For example, consider `dividend = 15` and `divisor = 5`, we are increasing divisor `5` using `left shift by 1` until it can be safely substracted from dividend `15`. In this case `5 << 1 = 10`,
    can safely be substracted from `15`. That means we are shifting one time, but since we are "shifting", we also have to shift the number of "shifting" which becomes `2`. Now we substract `10` from `15`, `15 - 10 = 5`, this is the new dividend. Now we apply same method to divide new dividend `5` by the divisor `5`, which gives `1`. Now we add this result to previous result.

    **Psuedocode:**
    ```
    DIVIDE_USING_SHIFT_OPERATOR(dividend, divisor) 
        1. Handle base cases like divide by 0, and divide by 1 etc
        2. Set quotient = 1 and temp = divisor
        3. Repeat while (temp << 1) <= dividend 
                a. temp = temp << 1
                b. quotient = quotient << 1
        4. quotient = quotient + DIVIDE_USING_SHIFT_OPERATOR((dividend - temp), divisor);
        5. Return quotient;
    ```

    **Programm Link:** https://github.com/VedantWankhade/coding-problems/blob/master/division_without_mul_div_operator.cpp

2. **Problem:** There are twenty-five horses. At most, five horses can race together at a time. You must determine the fastest, second fastest, and third fastest horses. Find the minimum number of races in which this can be done.

**Answer:** 7 races

**Solution:** We have 25 horses, lets arrange them in a 5x5 matrix. Where each row will represent a race.

                       1       2       3       4       5
    Race 1 ->    1   [1,1]   [1,2]   [1,3]   [1,4]   [1,5]
    Race 2 ->    2   [2,1]   [2,2]   [2,3]   [2,4]   [2,5]
    Race 3 ->    3   [3,1]   [3,2]   [3,3]   [3,4]   [3,5]
    Race 4 ->    4   [4,1]   [4,2]   [4,3]   [4,4]   [4,5]
    Race 5 ->    5   [5,1]   [5,2]   [5,3]   [5,4]   [5,5]

After these 5 races, we have 5 horses which are the `fastest in there respective row (race)`. If we race these 5 horses in Race 6, the winning horse will be the `fastest among the 25 horses`. Now the `second fastest horse among the 25 horses` could be the `second fastest horse in the race 6` or `second fastest horse in the race 1`. So we have to race these two horses.
The `third fastest horse among 25` could be the `third fastest horse in the race 6` or the `second fastes horse in race 2` or the `third fastest horse in Race 1`. So we have to race these three horses too. And we can race these five ramaining horse in a single race Race 7. 

For example, suppose in first 5 races, the fastest horse are the 5th horses, second fastest are the 4th horses and third fastest are 3rd in row. 

For instance, the fastest horse in Race 1 is [1,5], the second fastest is [1,4] and the third fastest is [1,3].

If we race the fastest horses from each row ([1,5], [2,5], [3,5], [4,5], [5,5]) in Race 6, the winning horse will be the fastest among all 25 horses. 

Race 6 -> [1,5], [2,5], [3,5], [4,5], [5,5]

Let's say the winning horse in Race 6 is [1,5]. So [1,5] is the fastes horse among 25 horses.

                       1       2       3       4       5
    Race 1 ->    1   [1,1]   [1,2]   [1,3]   [1,4]   [1,5] 
    Race 2 ->    2   [2,1]   [2,2]   [2,3]   [2,4]   [2,5]
    Race 3 ->    3   [3,1]   [3,2]   [3,3]   [3,4]   [3,5]
    Race 4 ->    4   [4,1]   [4,2]   [4,3]   [4,4]   [4,5]
    Race 5 ->    5   [5,1]   [5,2]   [5,3]   [5,4]   [5,5]

                                                        ^
                                                        |
                                                      Race 6


Now the `second fastest horse among all 25` could be any runner up horse to [1,5]. The runner up horses to [1,5] are [1,4] (second fastest horse from Race 1) and [2,5] (second fastest horse in Race 6).
So we have to race these two horses.
And the `third fastest among all 25` could be any runner up horse to [1,5] and [2,5] (second fastes in Race 6) and second runner up to [1,5]. So we have to race these horses too.

We can race these remaining horses in Race 7. 

Race 7 -> [1,4], [2,5], [1,3], [2,4], [3,5]

The `fastest in Race 7` will be the `second fastest among 25` and the `second fastest in Race 7` will be the `third fastest among 25`.
