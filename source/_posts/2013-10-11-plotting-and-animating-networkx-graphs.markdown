---
layout: post
title: "Plotting and Animating NetworkX graphs"
date: 2013-10-11 23:04
comments: true
categories: NetworkX, Matplotlib, Animation, Plotting
---
Let's start with a very simple color changing animation in which we will draw 
a graph whose nodes will change color. Here in each iteration we are drawing 
a new graph over the previous ones with different node colors. This is a 
very bad approach but let's just start with this. I will write about better 
ways to do it in the next post.

I will be using `networkX` for drawing the graphs and `matplotlib` for 
animation.
```python
import networkx as nx
import matplotlib.animation as animation
import matplotlib.pyplot as plt
import random

# Graph initialization
G = nx.Graph()
G.add_nodes_from([1, 2, 3, 4, 5, 6, 7, 8, 9])
G.add_edges_from([(1,2), (3,4), (2,5), (4,5), (6,7), (8,9), (4,7), (1,7), (3,5), (2,7), (5,8), (2,9), (5,7)])

# Animation funciton
define animate(i):
    colors = ['r', 'b', 'g', 'y', 'w', 'm']
    nx.draw_circular(G, node_color=[random.choice(colors) for j in range(9)]

nx.draw_circular(G)
fig = plt.gcf()

# Animator call
anim = animation.FuncAnimation(fig, animate, frames=20, interval=20, blit=True)
```

Let's step through and see what's happening. In the first four lines we are 
importing `networkX`, `matplotlib.animation`, `matplotlib.pyplot` and `random` 
modules. 

In the next few lines we create a graph using networkX:
```python
G = nx.Graph()                                                             
G.add_nodes_from([1, 2, 3, 4, 5, 6, 7, 8, 9])
G.add_edges_from([(1,2), (3,4), (2,5), (4,5), (6,7), (8,9), (4,7), (1,7), (3,5), (2,7), (5,8), (2,9), (5,7)])
```
First we initialize an empty graph `G`. Then we add 9 nodes and 13 edges to it.

This next piece is the animation function which takes a single parameter `i`
 which is the frame number of the animation.
```python
define animate(i): 
    colors = ['r', 'b', 'g', 'y', 'w', 'm']                                
    nx.draw_circular(G, node_color=[random.choice(colors) for j in range(9)]   
```
Here the `colors` list is a list of colors from which we will be randomly 
picking up colors for our nodes.
`nx.draw_circular` draws the graph keeping the nodes in a circular pattern.

```python
anim = animation.FuncAnimation(fig, animate, frames=20, interval=20, blit=True)
```
`animation.FuncAnimation` repeatedly calls the animate fucntion incrementing 
`i` in each iteration.
`frames` define the number of times animate function is called, 
`interval` is the inverval between each call.
`blit=True` defines to draw only those parts which have changed.
