# TYU_Mid-Term

import sys
from typing import List

NO_PATH = sys.maxsize

def floyd_recursive(distance: List[List[int]], n: int, k: int, i: int, j: int) -> int:
    """
    Recursive implementation of Floyd Warshall algorithm
    """
    if k >= n:  # base case
        return distance[i][j]
    else:
        # calculate distance by choosing or not choosing intermediate node
        dist1 = floyd_recursive(distance, n, k+1, i, j)
        dist2 = floyd_recursive(distance, n, k+1, i, k) + floyd_recursive(distance, n, k+1, k, j)
        return min(dist1, dist2)

def floyd_recursive_main(distance: List[List[int]]) -> List[List[int]]:
    """
    Main function for recursive implementation of Floyd Warshall algorithm
    """
    n = len(distance)
    for i in range(n):
        for j in range(n):
            if i == j:
                distance[i][j] = 0
            elif distance[i][j] == NO_PATH:
                distance[i][j] = sys.maxsize
    for k in range(n):
        for i in range(n):
            for j in range(n):
                distance[i][j] = floyd_recursive(distance, n, k, i, j)
    return distance

# Unit tests
def test_floyd_recursive():
    graph1 = [[0, 5, NO_PATH, 10],
              [NO_PATH, 0, 3, NO_PATH],
              [NO_PATH, NO_PATH, 0, 1],
              [NO_PATH, NO_PATH, NO_PATH, 0]]
    assert floyd_recursive_main(graph1) == [[0, 5, 8, 9],
                                            [NO_PATH, 0, 3, 4],
                                            [NO_PATH, NO_PATH, 0, 1],
                                            [NO_PATH, NO_PATH, NO_PATH, 0]]
    graph2 = [[0, 1, 2],
              [NO_PATH, 0, NO_PATH],
              [NO_PATH, 4, 0]]
    assert floyd_recursive_main(graph2) == [[0, 1, 2],
                                            [NO_PATH, 0, NO_PATH],
                                            [NO_PATH, 4, 0]]
    graph3 = [[0, 2, 3, NO_PATH],
              [NO_PATH, 0, 7, 1],
              [6, NO_PATH, 0, 4],
              [NO_PATH, NO_PATH, 2, 0]]
    assert floyd_recursive_main(graph3) == [[0, 2, 3, 4],
                                            [7, 0, 5, 1],
                                            [6, 8, 0, 4],
                                            [8, 10, 2, 0]]

# Performance tests
import timeit

graph4 = [[0]*100 for _ in range(100)]
for i in range(100):
    for j in range(100):
        if i != j:
            graph4[i][j] = 1
start_time = timeit.default_timer()
floyd_recursive_main(graph4)
end_time = timeit.default_timer()
print(f"Recursive Floyd Warshall algorithm for 100 nodes took
