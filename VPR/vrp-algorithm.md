# Vehicle Routing Problem Algorithm

## Problem Given

In the context of Last-Mile-Deliveries, optimization of non-only minimising total distance travelled but also no. of orders undertaken per vehicle in a given time window is critical to the profitability of operations.

Formulate a general VRP algorithm (you may make reference to external docs) to address on-demand orders being inserted into the existing pipeline of routes, name the following:

1. What are the parameters of your algorithm
2. What do you believe are the appropriate constraints
3. How specifically would you facilitate the new orders' insertion into existing routes

Knowledge:

1. Number of drivers is elastic
2. Map representation of SG is provided

## Solution

### Deliverables

1. Most cost effective routes
2. Maximise deliveries per time unit (e.g. hence minimise the waiting time for the customer)

### Drivers

The cost of last-mile-deliveries is surprisingly high. There is a huge cost in the driver delivery phase (i.e. the last mile) and I've read figures estimating 25-50% of the delivery cost can be the LMD. Which seems wild?? In addition, customers want free or subsidised same day shipping - which means costs are borne by the supplier. Therefore, when optimisation of routes, y (and fixed costs) can be of considerable benefit to the supplier.

### Problems

There are no easy solutions to the travelling salesman / vehicle routing problems. The find the exact solution all possible combinations of routes and costs need to be calculated.

Simulated annealing becomes a useful tool.
Adaptive Large Neighbourhood Search (ALNS) seems to be an industry standard approach to solving time dependent VRP problems (Ropke & Pisinger, 2006). Interestingly, the literature seems to say that giving defined delivery windows can reduce cost. One benefit is in reducing failed deliveries, a huge cost overhead in repeated deliveries.


Set of customers, C
Set of drivers, K (vehicle capacity is a concern?)
Cost is time (+ fuel?)
Each customer must be visited
List of depot(s)


Insertion -> use an ALNS method of local routes to determine the min cost 

Each postcode in SG is a delivery point.

How can machine learning help?
Learn from the time taken to travel from historical routes when calculating the cost