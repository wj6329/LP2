def Graph(graph, stNode):
    shortestDist = {}
    shortestDist[stNode] = 0
    preNode = {}
    visited = set()
    currNode = stNode
    
    while currNode is not None:
        visited.add(currNode)
        
        for neighbour in graph[currNode]:
            dist = shortestDist.get(currNode, float('inf')) + 1
           
            if dist < shortestDist.get(neighbour, float('inf')):
                shortestDist[neighbour] = dist
                preNode[neighbour] = currNode
        
        currNode = None
        minDist = float('inf')
        
        for node in graph:
            if node not in visited and shortestDist.get(node, float('inf')) < minDist:
                minDist = shortestDist[node]
                currNode = node
        
    return shortestDist, preNode


graph = {
    '5' : ['3', '7'],
    '3' : ['2', '4'],
    '7' : ['8'],
    '2' : [],
    '4' : ['8'],
    '8' : []
}

# graph = {
#     '5' : [('3', 9), ('7', 3)],
#     '3' : [('2', 5), ('4', 2)],
#     '7' : [('8', 4)],
#     '2' : [],
#     '4' : [('8', 2)],
#     '8' : []
# }

stNode = '5'
shortestDist, preNode = Graph(graph, stNode)

for node, dist in shortestDist.items():
    print('Shortest distance from', stNode, 'to', node, 'is', dist)
    

targetNode = '8'
path = []

while targetNode != stNode:
    path.insert(0, targetNode)
    targetNode = preNode[targetNode]

path.insert(0, stNode)

print(f"\n\nShortest path from {stNode} to {8}: {' -> '.join(path)}")