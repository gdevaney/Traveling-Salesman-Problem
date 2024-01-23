# Traveling-Salesman-Problem
Researching heuristics to improve both optimality and algorithm efficiency in route-planning in the infamous Traveling Salesman Problem. 

## Introduction/Background
While enrolled in StanfordOnline's CSX0003 to prepare for graduate school, I ran across the Traveling Salesman Problem (TSP). TSP asks the question "Given a list of cities and the distances between each pair of cities, what is the shortest possible route that visits each city exactly once and returns to the origin city?" [**[1]**](#ref1) This is an NP-Hard problem, meaning as you increase the number of cities in the route, the time required to compute the optimal solution becomes exponentially large. Because of the nature of the problem, many have sought to use greedy approaches (connecting cities closest to one another first) to find near optimal solution in polynomial time. However, these still result in O(n^2log(n)) runtimes, and are generally far from optimal solutions. Therefore, the motivation for this project was to introduce a heuristic that could improve both complexity and optimality from the traditional greedy approach. 

As I began framing the problem through simple examples with few cities, I noticed that outlying cities played a key role in a potential solution. I found that by connecting outlying cities to their nearest neighbors first, I could eliminate the need to traverse long distances at the conclusion of the algorithm. 

## Methods
### Preprocessing
I pulled in data from https://www.math.uwaterloo.ca/tsp/world/countries.html [**[2]**](#ref2) and converted to .txt files
During preprocessing, I first converted the .txt files into numpy arrays and calculated distances to and from every city based on coordinates. Next, I introduced an outlier heuristic by ordering cities based on aggregate distance to all other cities. 

### Algorithms
I used two algorithms to develop TSP routes:
- greedy_tsp
- greedier_tsp

#### greedy_tsp
greedy_tsp prioritizes outlying cities, connecting the most outlying city to its nearest neighbor, and continuing to move inward on the graph while building out the loop. Because no city can be visited twice, I knew that I would need to keep track of a frontier and explored space, so I developed sets that would move cities from the frontier (if connected to only 1 city) to the explored space (once connected to 2 other cities). I also needed to keep track of cluster frontier points to ensure that subloops didn't form prematurely. I used an array to assign and update opposing ends of the subpath accordingly and added filters to prevent subloops from forming. As I added cities to the route, I updated total path distance and returned the final route length when all cities were connected to 2 other points.

#### greedier_tsp
greedier_tsp used the same logic as greedy_tsp. However, greedier_tsp connects outlying cities to their nearest 2 neighbors, if possible. This produced a faster algorithm, and in some cases, created a more optimal route than the previous method. I made sure to verify cities weren't on the frontier before creating multiple edges to neighbors. 

## Observed Results

My two algorithms executed in a worst case O(n^2) time and 

#### Efficiency Results (seconds)
| | Djibouti | Zimbabwe | Rwanda | Oman | Japan | Greece | Finland | Vietnam | Sweden |
| --------- | --------- | --------- | --------- | --------- | --------- | --------- | --------- | --------- | --------- |
| non_heuristic | 0.00090 | 0.34777 | 1.03953| 1.57584 | 38.10914 | 38.39735 | 44.48990 | 205.35490 | 250.07005 |
| greedy_tsp | 0.00029 | 0.04110 | 0.10661 | 0.14263 | 4.06400 | 4.02461 | 5.10876 | 21.33420 | 30.14881 |
| greedier_tsp | 0.00017 | 0.03674 | 0.09397 | 0.13360 | 3.87369 | 3.80739 | 4.84779 | 19.25207 | 27.64599 |

#### Optimality Results (miles)
| | Djibouti | Zimbabwe | Rwanda | Oman | Japan | Greece | Finland | Vietnam | Sweden |
| --------- | --------- | --------- | --------- | --------- | --------- | --------- | --------- | --------- | --------- |
| non-heuristic | 8127.34 | 113506.10 | 31890.96 | 115149.40 | 615220.28 | 387147.15 | 649199.32 | 713474.41 | 1069442.97 |
| greedy_tsp | 7129.15 | 108575.48 | 30379.59 | 101848.37 | 577324.78 | 347948.47 | 595549.38 | 54108.38 | 983166.15 |
| greedier_tsp | 7720.55 | 112538.10 | 36891.70 | 108858.38 | 576393.98 | 344367.66 | 603773.87 | 640159.01 | 983897.47 |
| optimal | 6656 | 95345 | 26051 | 86891 | 491924 | 300899 | 520527 | 569288 | 855597 |

Per the results, my algorithms produce significantly better efficiency and approximately 10% more optimal routes as traditional greedy algorithms. 

## Folders
#### Countries
- Original data consisting of .txt files containing coordinates for cities within each country

## Files
#### Greedy_TSP
- Introduces heuristics, advanced algorithms, and displays performance improvements in visual manner

## References:
<a id="ref1"></a> [1] “Travelling Salesman Problem.” Wikipedia, Wikimedia Foundation, 27 Dec. 2023, en.wikipedia.org/wiki/Travelling_salesman_problem. 

<a id="ref2"></a> [2] National Traveling Salesman Problems, University of Waterloo, www.math.uwaterloo.ca/tsp/world/countries.html.
