import heapq

def MST(graph):
    root = next(iter(graph))
    visited = set([root])
    mst = []
    heap = []
    
    for neighbour, weight in graph[root]:
        heapq.heappush(heap, (weight, root, neighbour))
        
    while heap:
        weight, src, dest = heapq.heappop(heap)
        
        if dest not in visited:
            visited.add(dest)
            mst.append((src, dest, weight))
            
            for neighbour, weight in graph[dest]:
                if neighbour not in visited:
                    heapq.heappush(heap, (weight, dest, neighbour))
    return mst
        
        

graph = {
    '5' : [('3', 9), ('7', 3)],
    '3' : [('2', 5), ('4', 2)],
    '7' : [('8', 4)],
    '2' : [],
    '4' : [('8', 2)],
    '8' : []
}

MST(graph)