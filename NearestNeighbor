#include <iostream>
#include <fstream>
#include <sstream>
#include <vector>
#include <ctime>
#include <cmath>
#include <climits>
#include <chrono>
#include <algorithm>
#include <iterator>   



class Node {
public:
    int id;
    double x, y; // Assume nodes have coordinates for simplicity

    Node(int id, double x, double y) : id(id), x(x), y(y) {}

    double distance(const Node& other) const {
        //Euclidean Distance Formula 
        return sqrt((x - other.x) * (x - other.x) + (y - other.y) * (y - other.y));
    }
    bool operator==(const Node& other) const {
        return id == other.id && x == other.x && y == other.y;
    }
};

void nearestNeighbor(std::string filename) {
    // Load nodes from the file and create a vector of unvisited nodes
    int totalDistance= 0; //Initalize TotalDistance
    std::vector<Node> unvisitedNodes;
   
    std::ifstream file(filename);

    if (!file.is_open()) { //Parsing
        std::cerr << "Error opening file: " << filename << std::endl;
        return;
    }

    std::string line;
    bool nodeCoordSectionStarted = false;

    while (std::getline(file, line)) {
        if (line.find("NODE_COORD_SECTION") != std::string::npos) {
            nodeCoordSectionStarted = true;
            continue;
        }

        if (nodeCoordSectionStarted && line.find("EOF") == std::string::npos) {
            std::istringstream iss(line);
            int nodeId;
            double x, y;
            iss >> nodeId >> x >> y;
            unvisitedNodes.emplace_back(nodeId, x, y);
        }
    }

    // Vector to store the order in which nodes are visited
    std::vector<Node> visitedNodes;

    auto start_time = std::chrono::high_resolution_clock::now(); //Start the Time

    // Start from the first node
    Node currentnode = unvisitedNodes[0];
    visitedNodes.push_back(currentnode);
    unvisitedNodes.erase(unvisitedNodes.begin()); // Remove the first node from unvisited nodes

    while (!unvisitedNodes.empty()) {
        // Find the nearest neighbor to the current node
        double minDistance = std::numeric_limits<double>::max();
        Node nearestNeighbor = unvisitedNodes[0]; // Initialize with the first unvisited node

    for (const Node& neighbor : unvisitedNodes) {
        double d = currentnode.distance(neighbor);
        if (d < minDistance) {
        minDistance = d;
        nearestNeighbor = neighbor;
        
        }
    }
        // Move to the nearest neighbor, add mindistance to total distance
        totalDistance += minDistance;
        currentnode = nearestNeighbor;
        visitedNodes.push_back(currentnode); //pushback city into the visited vector
        unvisitedNodes.erase(std::remove(unvisitedNodes.begin(), unvisitedNodes.end(), currentnode), unvisitedNodes.end()); //remove the city from the unvisited vector of nodes

    }

    // Add the distance from the last node visited back to the starting node
    visitedNodes.push_back(visitedNodes.front());

    auto end_time = std::chrono::high_resolution_clock::now();
    int duration = std::chrono::duration_cast<std::chrono::milliseconds>(end_time - start_time).count();
    // Output the order of visited nodes
    for (const Node& node : visitedNodes) {
        std::cout << node.id << " ";
    }
 
    //Print Distance of tour and Time Taken in ms
    std::cout << "\nTotal Distance: " << totalDistance;
    std::cout << "\nTime in ms: " << duration << std::endl;    
}
