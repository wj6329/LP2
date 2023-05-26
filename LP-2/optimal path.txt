def AStar(startNode, stopNode):
    
    openSet = set([startNode])
    closeSet = set()
    dist = {}  # store distance from start node
    neighbour = {}  # Store adjacency nodes of all nodes
    
    dist[startNode] = 0
    neighbour[startNode] = None
    
    while len(openSet) > 0:
        currNode = None
        
        for v in openSet:
            if currNode is None or dist[v] + Heuristics(v) < dist[currNode] + Heuristics(currNode):
                currNode = v
        
        if currNode == stopNode or currNode not in graph:
            break
        else:
            for m, weight in GetNeighbour(currNode):
                if m not in openSet and m not in closeSet:
                    openSet.add(m)
                    neighbour[m] = currNode
                    dist[m] = dist[currNode] + weight
                else:
                    if dist[m] > dist[currNode] + weight:
                        dist[m] = dist[currNode] + weight
                        neighbour[m] = currNode
                        
                        if m in closeSet:
                            closeSet.remove(m)
                            openSet.add(m)
        
        openSet.remove(currNode)
        closeSet.add(currNode)
    
    if currNode is None:
        print('Path does not exist')
        return None
    
    if currNode == stopNode:
        path = []
        while neighbour[currNode] is not None:
            path.append(currNode)
            currNode = neighbour[currNode]
        path.append(startNode)
        path.reverse()
        print('Path Found: {}'.format(path))
        return path
    
    print('Path does not exist')
    return None

def GetNeighbour(currNode):
    if currNode in graph:
        return graph[currNode]
    else:
        return None

def Heuristics(n):
    hDist = {
        'A': 11,
        'B': 6,
        'C': 99,
        'D': 1,
        'E': 7,
        'G': 0,
    }
    return hDist[n]

graph = {
    'A': [('B', 2), ('E', 3)],
    'B': [('C', 1), ('G', 9)],
    'C': None,
    'E': [('D', 6)],
    'D': [('G', 1)]
}

AStar('A', 'G')