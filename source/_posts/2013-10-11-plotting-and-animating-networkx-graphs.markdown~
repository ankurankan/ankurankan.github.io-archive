---
layout: post
title: "Plotting and Animating NetworkX graphs"
date: 2013-10-11 23:04
comments: true
categories: NetworkX, Matplotlib, Animation, Plotting
---
### Basic Animation
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

### Modifying neworkX graph using Matplotlib
Let's dive deeper into how does networkX internally draws graphs. 
Taking a quick look of all the children of the plot.
```python
fig = plt.gcf()
axes = plt.gca()
axes.get_children()
```
We get the following output:
```python
[<matplotlib.axis.XAxis at 0x226ebd0>,
 <matplotlib.axis.YAxis at 0x36fa2d0>,
 <matplotlib.text.Text at 0x373fad0>,
 <matplotlib.text.Text at 0x373f410>,
 <matplotlib.text.Text at 0x373fc50>,
 <matplotlib.text.Text at 0x373fcd0>,
 <matplotlib.text.Text at 0x373fd50>,
 <matplotlib.text.Text at 0x373fdd0>,
 <matplotlib.text.Text at 0x373fe50>,
 <matplotlib.text.Text at 0x373fed0>,
 <matplotlib.text.Text at 0x373ff90>,
 <matplotlib.collections.PathCollection at 0x373dc10>,
 <matplotlib.collections.LineCollection at 0x373f3d0>,
 <matplotlib.text.Text at 0x36fcd50>,
 <matplotlib.patches.Rectangle at 0x36fc890>,
 <matplotlib.spines.Spine at 0x36f2b10>,
 <matplotlib.spines.Spine at 0x36f2810>,
 <matplotlib.spines.Spine at 0x36f2990>,
 <matplotlib.spines.Spine at 0x36f2650>]
```
Here we see there are 9 `matplotlib.text.Text` objects which are the labels
on the nodes. Let's see the properties of the object.
```python
In [15]: text_object = axes.get_children()[2]
In [16]: text.get_text()
Out[16]: '1'

In [17]: text_object.get_size()
Out[17]: 12.0

In [18]: text_object.get_color()
Out[18]: 'k'
```
Here we see a bunch of properties of the object. For the complete list we can
do `text_object.properties()`.

Next object we see is the `matplotlib.collections.PathCollection` object which
is the collection object of all the nodes. Now again we can see some properties
and modify them.
```python
nodes = axes.get_children()[11]
In [25]: nodes.get_pickradius()
Out[25]: 5.0

In [26]: nodes.get_label()
Out[26]: '_collection0'

In [27]: nodes.get_facecolor()
Out[27]: array([[ 1.,  0.,  0.,  1.]])

In [28]: nodes.get_edgecolors()
Out[28]: array([[ 0.,  0.,  0.,  1.]])
```
Now with the `offsets` property we can change the position of the nodes.
```python
In [32]: offsets = nodes.get_offsets()

In [33]: offsets
Out[33]: 
array([[ 1.        ,  0.31732594],
       [ 0.60075321,  0.28045667],
       [ 0.04382142,  0.98124087],
       [ 0.38063134,  0.99245427],
       [ 0.27586167,  0.61542286],
       [ 0.9961299 ,  0.91637658],
       [ 0.70916575,  0.66473314],
       [ 0.        ,  0.23580501],
       [ 0.28996686,  0.        ]])

In [35]: offsets[0][0] = 0.5

In [37]: offsets[0][1] = 0.5

In [38]: plt.draw()
```
Now we get this output:

[img]

We see here that only the node has moved not the label and the edges are 
also at there original position. We will now move the edges to the new node
position.
networkX uses `matplotlib.scatter` to draw nodes and then draws the edges.
And as we know matplotlib.scatter creates a node collection object and edges 
colleciton object. We can see the properties of node_collection object as

