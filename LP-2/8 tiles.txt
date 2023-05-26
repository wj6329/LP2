def print_board(elements):
    for i in range(9):
        if i % 3 == 0:
            print()
        if elements[i] == -1:
            print("_", end=" ")
        else:
            print(elements[i], end=" ")
    print()

def solvable(start):
    inv = 0
    for i in range(9):
        if start[i] <= 1:
            continue
        for j in range(i + 1, 9):
            if start[j] == -1:
                continue
            if start[i] > start[j]:
                inv += 1
    return inv % 2 == 0

def heuristic(start, goal):
    h = 0
    for i in range(9):
        for j in range(9):
            if start[i] == goal[j] and start[i] != -1:
                h += abs(j - i) // 3 + abs(j - i) % 3
    return h

def move_tile(start, goal):
    empty_at = start.index(-1)
    row = empty_at // 3
    col = empty_at % 3
    directions = [(0, -1), (0, 1), (1, 0), (-1, 0)]  # Left, Right, Down, Up
    min_heuristic = float('inf')
    best_move = None

    for dx, dy in directions:
        new_col = col + dx
        new_row = row + dy
        if 0 <= new_col < 3 and 0 <= new_row < 3:
            new_empty_at = new_row * 3 + new_col
            start[new_empty_at], start[empty_at] = start[empty_at], start[new_empty_at]
            h = heuristic(start, goal)
            if h < min_heuristic:
                min_heuristic = h
                best_move = (dx, dy)
            start[new_empty_at], start[empty_at] = start[empty_at], start[new_empty_at]

    if best_move is not None:
        dx, dy = best_move
        new_col = col + dx
        new_row = row + dy
        new_empty_at = new_row * 3 + new_col
        start[new_empty_at], start[empty_at] = start[empty_at], start[new_empty_at]

def solve_eight_puzzle(start, goal):
    if not solvable(start):
        print("Not possible to solve")
        return

    print_board(start)
    while start != goal:
        move_tile(start, goal)
        print_board(start)

start = [1, 2, 3, -1, 4, 6, 7, 5, 8]
goal = [1, 2, 3, 4, 5, 6, 7, 8, -1]

solve_eight_puzzle(start, goal)