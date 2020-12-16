# Undirected Graph

* **Dijstra**
![](https://github.com/shamy1st/data-structure/blob/main/images/dijstra-1.png)
![](https://github.com/shamy1st/data-structure/blob/main/images/dijstra-2.png)

        public class WeightedGraph {
            private class Node {
                private String label;
                private List<Edge> edges;

                public Node(String label) {
                    this.label = label;
                    this.edges = new ArrayList<>();
                }

                public void addEdge(Node to, int weight) {
                    edges.add(new Edge(this, to, weight));
                }

                public List<Edge> getEdges() {
                    return edges;
                }

                @Override
                public String toString() {
                    return label;
                }
            }

            private class NodeEntry {
                private Node node;
                private int priority;

                public NodeEntry(Node node, int priority) {
                    this.node = node;
                    this.priority = priority;
                }
            }

            private class Edge {
                private Node from;
                private Node to;
                private int weight;

                public Edge(Node from, Node to, int weight) {
                    this.from = from;
                    this.to = to;
                    this.weight = weight;
                }

                @Override
                public String toString() {
                    return from + "->" + to;
                }
            }

            private Map<String, Node> nodes;

            public WeightedGraph() {
                this.nodes = new HashMap<>();
            }

            public void addNode(String label) {
                nodes.putIfAbsent(label, new Node(label));
            }

            public void addEdge(String from, String to, int weight) {
                if(!nodes.containsKey(from) || !nodes.containsKey(to))
                    throw new IllegalStateException();

                Node fromNode = nodes.get(from);
                Node toNode = nodes.get(to);

                fromNode.addEdge(toNode, weight);
                toNode.addEdge(fromNode, weight);
            }

            public void print() {
                for(Node node : nodes.values())
                    System.out.println(node + " is connected to " + node.getEdges());
            }

            public Path getShortestPath(String from, String to) {
                Node fromNode = nodes.get(from);
                Node toNode = nodes.get(to);

                Map<Node, Integer> distances = new HashMap<>();
                for(Node node : nodes.values())
                    distances.put(node, Integer.MAX_VALUE);
                distances.replace(fromNode, 0);

                Map<Node, Node> previousNodes = new HashMap<>();

                Set<Node> visited = new HashSet<>();

                PriorityQueue<NodeEntry> queue = new PriorityQueue<>(
                        Comparator.comparingInt(entry -> entry.priority)
                );
                queue.add(new NodeEntry(fromNode, 0));

                while(!queue.isEmpty()){
                    Node current = queue.remove().node;
                    visited.add(current);

                    for(Edge edge : current.getEdges()){
                        if(visited.contains(edge.to))
                            continue;

                        int newDistance = distances.get(current) + edge.weight;
                        if(newDistance < distances.get(edge.to)) {
                            distances.replace(edge.to, newDistance);
                            previousNodes.put(edge.to, current);
                            queue.add(new NodeEntry(edge.to, newDistance));
                        }
                    }
                }

                return buildPath(toNode, previousNodes);
            }

            private Path buildPath(Node toNode, Map<Node, Node> previousNodes) {
                Stack<Node> stack = new Stack<>();
                stack.push(toNode);
                Node previous = previousNodes.get(toNode);
                while(previous != null) {
                    stack.push(previous);
                    previous = previousNodes.get(previous);
                }

                Path path = new Path();
                while(!stack.isEmpty())
                    path.add(stack.pop().label);
                return path;
            }
        }

* **Cycle Detection**

* **Minimum Spanning Tree**
![](https://github.com/shamy1st/data-structure/blob/main/images/minimum-spanning-tree-1.png)
![](https://github.com/shamy1st/data-structure/blob/main/images/minimum-spanning-tree-2.png)

  * **Prim's Algorithm**
  ![](https://github.com/shamy1st/data-structure/blob/main/images/prim-algorithm-1.png)
  ![](https://github.com/shamy1st/data-structure/blob/main/images/prim-algorithm-2.png)
