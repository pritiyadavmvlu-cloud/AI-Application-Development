#Practical 1
Design a Simple Rational Agent by defining PEAS for Vacuum Cleaner Environment and Autonomous  Taxi. Classify environments (fully/partially observable, deterministic/stochastic, episodic/sequential). Implement a  simple table-driven agent in Python.  

a.For Vacuum Cleaner Environment:- 
CODE:- 
table = { 
 ('A', 'Dirty'): 'Suck', 
 ('A', 'Clean'): 'Move ', 
 ('B', 'Dirty'): 'Suck', 
 ('B', 'Clean'): 'Move', 
 ('C', 'Dirty'): 'Suck', 
 ('C', 'Clean'): 'Move', 
 ('D', 'Dirty'): 'Suck', 
 ('D', 'Clean'): 'Move' 
} 
sequence_table = { 
 (('A', 'Clean'),('B', 'Clean'),('C', 'Clean'),('D', 'Clean')): 'NoOp', 
 (('A', 'Dirty'),('B', 'Clean'),('C', 'Clean'),('D', 'Clean')): 'Suck A', 
 (('A', 'Clean'),('B', 'Dirty'),('C', 'Clean'),('D', 'Clean')): 'Suck B', 
 (('A', 'Clean'),('B', 'Clean'),('C', 'Dirty'),('D', 'Clean')): 'Suck C', 
 (('A', 'Clean'),('B', 'Clean'),('C', 'Clean'),('D', 'Dirty')): 'Suck D' 
} 
percepts = [] 
def table_driven_agent(percept): 
 percepts.append(percept)
 # Check if the complete percept sequence matches 
 if tuple(percepts) in sequence_table: 
 return sequence_table[tuple(percepts)] 
 return table.get(percept, "NoOp") 
print("4-Room Vacuum Cleaner Table-Driven Agent") 
n = int(input("Enter number of percepts (4): ")) 
for i in range(n): 
 print(f"\nPercept {i+1}") 
 location = input("Enter Location (A/B/C/D): ").upper() 
 status = input("Enter Status (Clean/Dirty): ").capitalize() 
 percept = (location, status) 
 action = table_driven_agent(percept) 
 print("Percept History :", percepts) 
 print("Agent Action :", action) 



b.For Autonomous Taxi:- 
CODE:- 
table = {  
('Green', 'No', 'No', 'No', 'No'): 'Move Forward',  
('Green', 'Yes', 'No', 'No', 'No'): 'Stop',  
('Red', 'No', 'No', 'No', 'No'): 'Stop',  
('Yellow', 'No', 'No', 'No', 'No'): 'Slow Down',  
('Green', 'No', 'Yes', 'No', 'No'): 'Pick Up Passenger',  
('Red', 'Yes', 'No', 'No', 'No'): 'Stop',  
('Yellow', 'Yes', 'No', 'No', 'No'): 'Stop',  
('Green', 'No', 'No', 'Yes', 'No'): 'Give Way to Emergency Vehicle',  
('Green', 'No', 'No', 'No', 'Yes'): 'Go to Fuel Station'  
}  
percepts = []  
def taxi_agent(percept):  
percepts.append(percept)  
action = table.get(percept, "Wait")  
return action  
print("Autonomous Taxi Table-Driven Agent") 
n = int(input("Enter number of percepts: "))  
for i in range(n):  
print(f"\nPercept {i+1}")  
signal = input("Traffic Signal (Green/Yellow/Red): ").capitalize()  
obstacle = input("Obstacle Present? (Yes/No): ").capitalize() 
passenger = input("Passenger Request? (Yes/No): ").capitalize()  
emergency = input("Emergency Vehicle Nearby? (Yes/No): ").capitalize()  
fuel = input("Low Fuel? (Yes/No): ").capitalize()  
percept = (signal, obstacle, passenger, emergency, fuel)  
action = taxi_agent(percept)  
print("Current Percept:", percept)  
print("Agent Action:", action)  
print("Priti Yadav T053")



c.Table-Driven Agent program for a Smart Fan/Light System. It turns the fan/light ON or  OFF depending on:  
• Whether someone is in the room.  
• Whether the person has left the room.  
• Whether natural light or natural air is available  
CODE:- 
table = { 
 # (Person in Room, Left Room, Natural Light/Air) 
 ('Yes', 'No', 'No'): 'Turn ON Fan/Light', 
 ('Yes', 'No', 'Yes'): 'Turn OFF Fan/Light', 
 ('No', 'Yes', 'No'): 'Turn OFF Fan/Light', 
 ('No', 'Yes', 'Yes'): 'Turn OFF Fan/Light', 
 ('No', 'No', 'No'): 'Keep OFF', 
 ('No', 'No', 'Yes'): 'Keep OFF', 
 ('Yes', 'Yes', 'No'): 'Turn OFF Fan/Light', 
 ('Yes', 'Yes', 'Yes'): 'Turn OFF Fan/Light'
} 
percepts = [] 
def smart_home_agent(percept): 
 percepts.append(percept) 
 action = table.get(percept, "No Action") 
 return action 
print("Smart Fan/Light Table-Driven Agent") 
n = int(input("Enter number of percepts: ")) 
for i in range(n): 
 print(f"\nPercept {i+1}") 
 person = input("Is a person in the room? (Yes/No): ").capitalize()  left_room = input("Has the person left the room? (Yes/No): ").capitalize()  natural = input("Is natural light/air available? (Yes/No): ").capitalize()  percept = (person, left_room, natural) 
 action = smart_home_agent(percept) 
 print("Current Percept:", percept) 
 print("Agent Action:", action) 
print("Priti Yadav T054")  
-------------------------------------------------------------------------------------------------------------------------------------------------------
#Practical2
2.Problem Solving by Searching (Uninformed Search)
a.Given an initial configuration of the 8-puzzle and a goal configuration, write a Python program to find the shortest sequence of moves to reach the goal state using Breadth-First Search (BFS).
CODE:-
# library 
from collections import deque
# Function to display puzzle state
def print_state(state):
    for i in range(0, 9, 3):
        print(state[i], state[i + 1], state[i + 2])
    print()
# Function to find possible next states
def get_neighbors(state):
    neighbors = []
    # Find position of blank tile
    blank_index = state.index(0)
    # Possible moves: Up, Down, Left, Right
    moves = {
        "Up": -3,
        "Down": 3,
        "Left": -1,
        "Right": 1}
    for move, position_change in moves.items():
        new_index = blank_index + position_change
        # Check valid moves
        if move == "Up" and blank_index < 3:
            continue
        if move == "Down" and blank_index > 5:
            continue
        if move == "Left" and blank_index % 3 == 0:
            continue
        if move == "Right" and blank_index % 3 == 2:
            continue
        # Create new state by swapping blank tile
        new_state = list(state)
        new_state[blank_index], new_state[new_index] = (
            new_state[new_index],
            new_state[blank_index])
        neighbors.append((tuple(new_state), move))
    return neighbors
# BFS function
def bfs(initial_state, goal_state):
    queue = deque()
    # Each queue item contains: current_state, path
    queue.append((initial_state, []))
    visited = set()
    visited.add(initial_state)
    while queue:
        current_state, path = queue.popleft()
        # Check if goal is reached
        if current_state == goal_state:
            return path
        # Generate next possible states
        for neighbor, move in get_neighbors(current_state):
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [(move, neighbor)]))
    return None
# Main Program
initial_state = (
    1, 2, 3,
    4, 0, 5,
    6, 7, 8)
goal_state = (
    1, 2, 3,
    4, 5, 6,
    7, 8, 0 )
print("Initial State:")
print_state(initial_state)
print("Goal State:")
print_state(goal_state)
solution = bfs(initial_state, goal_state)
if solution:
    print("Shortest sequence of moves:")
    current_step = 1
    for move, state in solution:
        print("Step", current_step, "- Move:", move)
        print_state(state)
        current_step += 1
    print("Goal reached successfully.")
    print("Total number of moves:", len(solution))
else:
    print("No solution found.")
print("Priti Yadav T054")


b. Given two water jugs of 4 litres and 3 litres capacity, write a Python program to obtain exactly 2 litres in one jug using Depth-First Search (DFS). [Vary capacity of jugs]
CODE:-
# Water Jug Problem using DFS
j1 = int(input("Enter capacity of Jug 1: "))
j2 = int(input("Enter capacity of Jug 2: "))
goal = int(input("Enter target amount: "))
visited = set()
def dfs(x, y, path):
    # Goal condition
    if x == goal or y == goal:
        path.append((x, y))
        print("\nGoal Achieved Successfully!")
        print("Solution Path:")
        for state in path:
            print(state)
        return True
    if (x, y) in visited:
        return False
    visited.add((x, y))
    path.append((x, y))
    next_states = [
        (j1, y),          # Fill Jug1
        (x, j2),          # Fill Jug2
        (0, y),           # Empty Jug1
        (x, 0),          # Empty Jug2
        # Pour Jug1 -> Jug2(
            x - min(x, j2 - y),
            y + min(x, j2 - y)),
        # Pour Jug2 -> Jug1 (
            x + min(y, j1 - x),
            y - min(y, j1 - x) )]
    for state in next_states:
        if dfs(state[0], state[1], path.copy()):
            return True
    return False
# Initial state
if not dfs(0, 0, []):
    print("No solution found")
print("Priti Yadav T054")


c.Given a weighted graph representing cities and distances between them, write a Python program to find the least-cost path from Arad to Bucharest using Uniform Cost Search (UCS). [Provide any other weighted graph for applying UCS]
CODE:-
import heapq
graph = {
    'Mumbai': [('Pune', 150), ('Hyderabad', 710)],
    'Pune': [('Mumbai', 150), ('Bangalore', 840)],
    'Hyderabad': [('Mumbai', 710), ('Bangalore', 570)],
    'Bangalore': []
}
def ucs(start, goal):
    queue = [(0, start, [start])]
    visited = []
    while queue:
        cost, city, path = heapq.heappop(queue)
        if city == goal:
            return cost, path
        if city not in visited:
            visited.append(city)
            for neighbor, distance in graph[city]:
                heapq.heappush(
                    queue,
                    (cost + distance, neighbor, path + [neighbor])
                )
cost, path = ucs("Mumbai", "Bangalore")
print("Least-cost path:")
print("Path:", " -> ".join(path))
print("Total Distance:", cost, "km")
print("Priti Yadav T054")

-------------------------------------------------------------------------------------------------------------------------------------------------------
Practical 3
3.Problem Solving by Searching (Informed Search)
a. Given an initial configuration of the 8-puzzle and a goal configuration, write a Python program to find the solution using Greedy Best-First Search with the Manhattan Distance heuristic.
CODE:-
import heapq
# Display puzzle state
def print_state(state):
    for i in range(0, 9, 3):
        print(state[i], state[i+1], state[i+2])
    print()
# Manhattan Distance Heuristic
def heuristic(state, goal):
    distance = 0
    for num in range(1, 9):
        current = state.index(num)
        target = goal.index(num)
        x1, y1 = current // 3, current % 3
        x2, y2 = target // 3, target % 3
        distance += abs(x1 - x2) + abs(y1 - y2)
    return distance
# Generate next states
def get_neighbors(state):
    neighbors = []
    blank = state.index(0)
    moves = [-3, 3, -1, 1]
    for move in moves:
        new = blank + move
        if 0 <= new < 9:
            if blank % 3 == 0 and move == -1:
                continue
            if blank % 3 == 2 and move == 1:
                continue
            temp = list(state)
            temp[blank], temp[new] = temp[new], temp[blank]
            neighbors.append(tuple(temp))
    return neighbors
# Greedy Best First Search
def greedy(initial, goal):
    queue = []
    heapq.heappush(queue, (heuristic(initial, goal), initial, []))
    visited = set()
    while queue:
        h, state, path = heapq.heappop(queue)
        if state == goal:
            return path + [state]
        if state not in visited:
            visited.add(state)
            for next_state in get_neighbors(state):
                heapq.heappush(
                    queue,
                    (heuristic(next_state, goal), next_state, path + [state]))
    return None
# Initial and Goal State
initial = (1, 2, 3,
           4, 0, 6,
           7, 5, 8)
goal = (1, 2, 3,
        4, 5, 6,
        7, 8, 0)
print("Initial State:")
print_state(initial)
print("Goal State:")
print_state(goal)
solution = greedy(initial, goal)
print("Solution:")
for step, state in enumerate(solution):
    print("Step", step)
    print_state(state)
print("Goal reached successfully.")
print("Priti Yadav T054")


     
b. Given an initial configuration of the 8-puzzle and a goal configuration, write a Python program to find the shortest path using A* search with the Manhattan Distance heuristic.
CODE:-
import heapq
# Display puzzle state
def print_state(state):
    for i in range(0, 9, 3):
        print(state[i], state[i+1], state[i+2])
    print()
# Manhattan Distance Heuristic
def heuristic(state, goal):
    distance = 0
    for num in range(1, 9):
        current = state.index(num)
        target = goal.index(num)
        x1, y1 = current // 3, current % 3
        x2, y2 = target // 3, target % 3
        distance += abs(x1 - x2) + abs(y1 - y2)
    return distance
# Generate next states
def get_neighbors(state):
    neighbors = []
    blank = state.index(0)
    moves = [-3, 3, -1, 1]
    for move in moves:
        new = blank + move
        if 0 <= new < 9:
            if blank % 3 == 0 and move == -1:
                continue
            if blank % 3 == 2 and move == 1:
                continue
            temp = list(state)
            temp[blank], temp[new] = temp[new], temp[blank]
            neighbors.append(tuple(temp))
    return neighbors
# A* Search
def astar(initial, goal):
    queue = []
    heapq.heappush(queue, (heuristic(initial, goal), 0, initial, []))
    visited = set()
    while queue:
        f, g, state, path = heapq.heappop(queue)
        if state == goal:
            return path + [state]
        if state not in visited:
            visited.add(state)
            for next_state in get_neighbors(state):
                new_g = g + 1
                new_f = new_g + heuristic(next_state, goal)
                heapq.heappush(
                    queue,
                    (new_f, new_g, next_state, path + [state]))
    return None
# Initial and Goal State
initial = (1, 2, 3,
           4, 0, 6,
           7, 5, 8)
goal = (1, 2, 3,
        4, 5, 6,
        7, 8, 0)
print("Initial State:")
print_state(initial)
print("Goal State:")
print_state(goal)
solution = astar(initial, goal)
print("Shortest Path using A* Search:")
step = 1
for state in solution[1:]:
    print("Step", step)
    print_state(state)
    step += 1
print("Goal reached successfully.")
print("Total number of moves:", len(solution) - 1)
print("Priti Yadav T054")
  
 
c. Given a weighted graph representing cities and distances between them, write a Python program to find the shortest path from Arad to Bucharest using A* search with a heuristic function (straight-line distance to destination).
CODE:-
import heapq
# Graph: city and distance to connected cities
graph = {
    'Arad': [('Zerind',75), ('Sibiu',140), ('Timisoara',118)],
    'Zerind': [('Arad',75), ('Oradea',71)],
    'Oradea': [('Zerind',71), ('Sibiu',151)],
    'Sibiu': [('Arad',140), ('Oradea',151), ('Fagaras',99), ('Rimnicu Vilcea',80)],
    'Timisoara': [('Arad',118), ('Lugoj',111)],
    'Lugoj': [('Timisoara',111), ('Mehadia',70)],
    'Mehadia': [('Lugoj',70), ('Drobeta',75)],
    'Drobeta': [('Mehadia',75), ('Craiova',120)],
    'Craiova': [('Drobeta',120), ('Rimnicu Vilcea',146), ('Pitesti',138)],
    'Rimnicu Vilcea': [('Sibiu',80), ('Craiova',146), ('Pitesti',97)],
    'Fagaras': [('Sibiu',99), ('Bucharest',211)],
    'Pitesti': [('Rimnicu Vilcea',97), ('Craiova',138), ('Bucharest',101)],
    'Bucharest': []
}
# Heuristic: straight-line distance to Bucharest
heuristic = {
    'Arad': 366,
    'Zerind': 374,
    'Oradea': 380,
    'Sibiu': 253,
    'Timisoara': 329,
    'Lugoj': 244,
    'Mehadia': 241,
    'Drobeta': 242,
    'Craiova': 160,
    'Rimnicu Vilcea': 193,
    'Fagaras': 176,
    'Pitesti': 100,
    'Bucharest': 0
}
def astar(start, goal):
    queue = []
    # queue contains: f_cost, g_cost, city, path
    heapq.heappush(queue, (heuristic[start], 0, start, [start]))
    visited = set()
    while queue:
        f_cost, g_cost, city, path = heapq.heappop(queue)
        if city == goal:
            return g_cost, path
        if city not in visited:
            visited.add(city)
            for neighbor, distance in graph[city]:
                new_g = g_cost + distance
                new_f = new_g + heuristic[neighbor]
                heapq.heappush(queue,
                    (new_f, new_g, neighbor, path + [neighbor]))
    return None, []
# Run the algorithm
cost, path = astar('Arad', 'Bucharest')
print("Shortest path using A* Search:")
print("Path:", " -> ".join(path))
print("Total Cost:", cost)
print("Priti Yadav T054")
