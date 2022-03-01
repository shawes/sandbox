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

### Complexity Issues

There are no easy solutions to the travelling salesman / vehicle routing problems. The find the exact solution all possible combinations of routes and costs need to be calculated.

### Considered Solutions

Simulated annealing becomes a useful tool.
Adaptive Large Neighbourhood Search (ALNS) seems to be an industry standard approach to solving time dependent VRP problems (Ropke & Pisinger, 2006).

ALNS could be used on existing routes (sub-optimisation of local routes relevant to new order) to determine new minimum route cost.

Issue? ALNS is potentially too slow to meet the stochastic demands of short-delivery windows, e.g. time-dependent products.

>Interestingly, the literature seems to say that giving defined delivery windows can reduce cost. One benefit is in reducing failed deliveries, a huge cost overhead in repeated deliveries (Ozarik et al. 2021)

From the literature, I prefer the theoretical ESI algorithm proposed by Zeng et al. 2019.

### VRP Definitions

One thing confusing in the literature are the interchangable words used to describe the same thing - not many standards it seems

**Order** A tuple of 3 values (o, d, c). (o)rigin, (d)estination, and (c)ost.

The origin and destination are just where the package is picked up and the packaged is delivered too.
^In Singapore, can postcodes could be used instead of GPS? Are they unique to buildings?
The cost is the weight of the delivery route. Multiple measures could be incorporated distance, traffic, tolls etc.
Then associated with each order must be a time (t_o).

**Driver** Tuple of 2 values (loc, cap). Location and capacity.

There is a set of couriers, C, all assigned n orders. (Although couriers are elastic so can add more?)

**Route** List of the *sequential* orders assigned to a driver.


### VRP Constraints

- Each route must be practicable
  - Orders must be picked up by a driver before they can be delivered
  - Orders need to be delivered in time windows
  - Driver must have capacity for the packages
  - All orders need to be delivered
- Each route must be completed

### Insertion algorithm

The general idea is to assign adaptive travel budgets to drivers and then break the routes for each driver up into segments. With the idea a courier completes as many deliveries close to them when their travel budget is small and helps other drivers when the travel budget is large.

    LRP-ESI-algorithm(orders O, drivers D, travel_budget_parameter *p*):
      Orders_unassigned (new orders that are not assigned)
      Orders_assigned (set of planned routes for each driver)
      foreach travel_budget_parameter (i=p, i<max; i=i^2)
        foreach driver in D:
            k = find_orders_within_budget(driver, Orders_unassigned)
            if k > 0 && still routes left
                update the route of the driver
                remove assigned orders from Orders_assigned
            if Orders_unassigned is empty -> break
        return drivers with updated routes

Use a hierarchically separated tree to determine the orders that fit within the travel budget. The idea is to select the unassigned orders that are closest to the location of the driver.

ESI = (E)mbed the route into a HST, (S)ort the tree to find the most optimal route with least cost using the new orders. (I)nsert the new orders into the new route. Their algorithm for this is fast, which is required because it is executed so many times.