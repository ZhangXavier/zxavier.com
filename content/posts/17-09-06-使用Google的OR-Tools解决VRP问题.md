---
title: "使用 Google 的 OR-Tools 解决 VRP 问题"
date: 2017-09-06T08:07:23+08:00
categories: [代码之道]
tags: [google,OR-Tools,数学建模]
draft: false
---

前一段时间在做数学建模，发现了一个谷歌开发的解决优化问题的工具包，可以说是目前市面上 VRP 功能最强的开源算法包了，支持解决约束，线性规划，最佳路径，图论的一些问题，网上关于它的中文资料比较少，这里便给大家介绍下它的使用。（最近事情比较多，写的比较简单，大家可以先去官网自行学习，等过了这一段时间，我会详细的再写下）

## OR-Tools 官网

[OR-Tools 官网](https://developers.google.com/optimization/)

### 版本安装 (Python)

OR-Tools 是全平台的工具，支持的操作系统包括 Windows，Mac OS X，Linux，支持的语言包括 C++，C#，Java 和 Python。我这里是在 Windows 上使用 Python 来进行使用的。

## VRP 问题

下面是 Goolge 给出的解决 VRP 问题的 Python 示例代码：

``` python
# coding:utf-8
import math
from numpy import *
from ortools.constraint_solver import pywrapcp
from ortools.constraint_solver import routing_enums_pb2


def distance(x1, y1, x2, y2):
    # Manhattan distance(曼哈顿距离)
    dist = abs(x1 - x2) + abs(y1 - y2)
    return dist


class CreateDistanceCallback(object):
    """Create callback to calculate distances between points(创建回调计算点之间的距离)."""

    def __init__(self, locations):
        """Initialize distance array(距离数组初始化)."""
        size = len(locations)
        self.matrix = {}

        for from_node in xrange(size):
            self.matrix[from_node] = {}
            for to_node in xrange(size):
                x1 = locations[from_node][0]
                y1 = locations[from_node][1]
                x2 = locations[to_node][0]
                y2 = locations[to_node][1]
                self.matrix[from_node][to_node] = distance(x1, y1, x2, y2)

    def Distance(self, from_node, to_node):
        return self.matrix[from_node][to_node]


# Demand callback(需求回调)
class CreateDemandCallback(object):
    """Create callback to get demands at each location(创建回调以获取每个位置的需求)."""

    def __init__(self, demands):
        self.matrix = demands

    def Demand(self, from_node, to_node):
        return self.matrix[from_node]


def main():
    # Create the data(创建数据).
    data = create_data_array()
    locations = data[0]
    demands = data[1]
    num_locations = len(locations)
    depot = 0  # The depot is the start and end point of each route(仓库是每一条路线的起点和终点).
    num_vehicles = 7

    # Create routing model(创建路由模型).
    if num_locations > 0:
        routing = pywrapcp.RoutingModel(num_locations, num_vehicles, depot)
        search_parameters = pywrapcp.RoutingModel.DefaultSearchParameters()

        # Setting first solution heuristic: the
        # method for finding a first solution to the problem(寻找问题的第一解的方法).
        search_parameters.first_solution_strategy = (
            routing_enums_pb2.FirstSolutionStrategy.PATH_CHEAPEST_ARC)

        # The 'PATH_CHEAPEST_ARC' method does the following:
        # Starting from a route "start" node, connect it to the node which produces the
        # cheapest route segment, then extend the route by iterating on the last
        # node added to the route.
        # 从路由“开始”节点开始，将其连接到生成最廉价路由段的节点，然后通过添加到路由的最后一个节点来扩展路由.

        # Put a callback to the distance function here. The callback takes two
        # arguments (the from and to node indices) and returns the distance between
        # these nodes.
        # 将回调函数放在这里的距离函数。回调函数接受两个参数（从和节点指标）并返回这些节点之间的距离

        dist_between_locations = CreateDistanceCallback(locations)
        dist_callback = dist_between_locations.Distance
        routing.SetArcCostEvaluatorOfAllVehicles(dist_callback)

        # Put a callback to the demands.
        # 对需求进行回调
        demands_at_locations = CreateDemandCallback(demands)
        demands_callback = demands_at_locations.Demand

        # Add a dimension for demand.
        # 为需求添加一个维度
        slack_max = 0
        vehicle_capacity = 2500
        fix_start_cumul_to_zero = True
        demand = "Demand"
        routing.AddDimension(demands_callback, slack_max, vehicle_capacity,
                             fix_start_cumul_to_zero, demand)

        # Solve, displays a solution if any(解决，显示解决方案，如果有的话).
        assignment = routing.SolveWithParameters(search_parameters)
        if assignment:
            # Display solution(显示解决方案).
            # Solution cost(解决方案的成本).
            print "Total distance of all routes: " + str(assignment.ObjectiveValue()) + "\n"

            for vehicle_nbr in range(num_vehicles):
                index = routing.Start(vehicle_nbr)
                index_next = assignment.Value(routing.NextVar(index))
                route = ''
                route_dist = 0
                route_demand = 0

                while not routing.IsEnd(index_next):
                    node_index = routing.IndexToNode(index)
                    node_index_next = routing.IndexToNode(index_next)
                    route += str(node_index) + " -> "
                    # Add the distance to the next node(将距离添加到下一个节点).
                    route_dist += dist_callback(node_index, node_index_next)
                    # Add demand(增加需求).
                    route_demand += demands[node_index_next]
                    index = index_next
                    index_next = assignment.Value(routing.NextVar(index))

                node_index = routing.IndexToNode(index)
                node_index_next = routing.IndexToNode(index_next)
                route += str(node_index) + " -> " + str(node_index_next)
                route_dist += dist_callback(node_index, node_index_next)
                print "Route for vehicle (车辆路线) " + str(vehicle_nbr + 1) + ":\n\n" + route + "\n"
                print "Distance of route (路线距离) " + str(vehicle_nbr + 1) + ": " + str(route_dist)
                print "Demand met by vehicle (车辆运送货物量) " + str(vehicle_nbr + 1) + ": " + str(route_demand) + "\n"
        else:
            print 'No solution found.'
    else:
        print 'Specify an instance greater than 0.'


def create_data_array():
    locations = [[82, 76], [96, 44], [50, 5], [49, 8], [13, 7], [29, 89], [58, 30], [84, 39],
             [14, 24], [12, 39], [3, 82], [5, 10], [98, 52], [84, 25], [61, 59], [1, 65],
             [88, 51], [91, 2], [19, 32], [93, 3], [50, 93], [98, 14], [5, 42], [42, 9],
             [61, 62], [9, 97], [80, 55], [57, 69], [23, 15], [20, 70], [85, 60], [98, 5]]
    demands = [0, 19, 21, 6, 19, 7, 12, 16, 6, 16, 8, 14, 21, 16, 3, 22, 18,
           19, 1, 24, 8, 12, 4, 8, 24, 24, 2, 20, 15, 2, 14, 9]
    data = [locations, demands]

    return data


if __name__ == '__main__':
    main()
```
