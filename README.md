<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 
<h3>Name:       </h3>
<h3>Register Number:           </h3>
<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>

``````
// A* Search Algorithm
1.  Initialize the open list
2.  Initialize the closed list
    put the starting node on the open 
    list (you can leave its f at zero)

3.  while the open list is not empty
    a) find the node with the least f on 
       the open list, call it "q"

    b) pop q off the open list
  
    c) generate q's 8 successors and set their 
       parents to q
   
    d) for each successor
        i) if successor is the goal, stop search
        
        ii) else, compute both g and h for successor
          successor.g = q.g + distance between 
                              successor and q
          successor.h = distance from goal to 
          successor (This can be done using many 
          ways, we will discuss three heuristics- 
          Manhattan, Diagonal and Euclidean 
          Heuristics)
          
          successor.f = successor.g + successor.h

        iii) if a node with the same position as 
            successor is in the OPEN list which has a 
           lower f than successor, skip this successor

        iV) if a node with the same position as 
            successor  is in the CLOSED list which has
            a lower f than successor, skip this successor
            otherwise, add  the node to the open list
     end (for loop)
  
    e) push q on the closed list
    end (while loop)

``````
<h3>Program:</h3>

```python
import heapq

class Node:
    def __init__(self, row, col, cost, heuristic):
        self.row = row
        self.col = col
        self.cost = cost
        self.heuristic = heuristic
        self.total_cost = cost + heuristic

    def __lt__(self, other):
        return self.total_cost < other.total_cost

def is_valid(row, col, grid):
    return 0 <= row < len(grid) and 0 <= col < len(grid[0]) and grid[row][col] != -1

def calculate_heuristic(row, col, goal):
    return abs(row - goal[0]) + abs(col - goal[1])

def astar(grid, start, goal):
    rows, cols = len(grid), len(grid[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]

    open_set = [Node(start[0], start[1], 0, calculate_heuristic(start[0], start[1], goal))]
    closed_set = set()

    while open_set:
        current_node = heapq.heappop(open_set)

        if (current_node.row, current_node.col) == goal:
            # Reconstruct path
            path = [(current_node.row, current_node.col)]
            while current_node.parent:
                current_node = current_node.parent
                path.append((current_node.row, current_node.col))
            return path[::-1]

        closed_set.add((current_node.row, current_node.col))

        for dr, dc in directions:
            new_row, new_col = current_node.row + dr, current_node.col + dc

            if is_valid(new_row, new_col, grid) and (new_row, new_col) not in closed_set:
                new_cost = current_node.cost + grid[new_row][new_col]
                new_heuristic = calculate_heuristic(new_row, new_col, goal)
                new_node = Node(new_row, new_col, new_cost, new_heuristic)
                new_node.parent = current_node

                if new_node not in open_set:
                    heapq.heappush(open_set, new_node)

``` 
<h2>Sample Graph I</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/b1377c3f-011a-4c0f-a843-516842ae056a)

<hr>
<h2>Sample Input</h2>
<hr>
10 14 <br>
A B 6 <br>
A F 3 <br>
B D 2 <br>
B C 3 <br>
C D 1 <br>
C E 5 <br>
D E 8 <br>
E I 5 <br>
E J 5 <br>
F G 1 <br>
G I 3 <br>
I J 3 <br>
F H 7 <br>
I H 2 <br>
A 10 <br>
B 8 <br>
C 5 <br>
D 7 <br>
E 3 <br>
F 6 <br>
G 5 <br>
H 3 <br>
I 1 <br>
J 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'F', 'G', 'I', 'J']


<hr>
<h2>Sample Graph II</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/acbb09cb-ed39-48e5-a59b-2f8d61b978a3)


<hr>
<h2>Sample Input</h2>
<hr>
6 6 <br>
A B 2 <br>
B C 1 <br>
A E 3 <br>
B G 9 <br>
E D 6 <br>
D G 1 <br>
A 11 <br>
B 6 <br>
C 99 <br>
E 7 <br>
D 1 <br>
G 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'E', 'D', 'G']
