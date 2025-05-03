# Problem 1
# 🧠 Equivalent Resistance Using Graph Theory

## 📌 Introduction

Calculating equivalent resistance is a fundamental task in electrical engineering. For complex circuits, it’s easier to analyze them using graph theory, where:
- **Nodes** represent connection points.
- **Edges** represent resistors with weights equal to resistance values.

Graph representation enables:
- Automated calculations.
- Handling of nested configurations.
- Clear visualizations.

---

## 🔗 Series Connection

Resistors are connected **end-to-end**. Current is the same through all; voltages add up.

**Formula:**

$$
R_{eq} = R_1 + R_2 + \dots + R_n
$$

**Visualization in Python (Colab compatible):**

```python
import networkx as nx
import matplotlib.pyplot as plt

G = nx.Graph()
G.add_edge("1", "2", label="R1")
G.add_edge("2", "3", label="R2")
G.add_edge("3", "4", label="R3")
pos = {"1": (0,0), "2": (1,0), "3": (2,0), "4": (3,0)}

nx.draw_networkx_nodes(G, pos, node_color='lightblue', node_size=500)
nx.draw_networkx_labels(G, pos)
edge_labels = {(u, v): d['label'] for u, v, d in G.edges(data=True)}
nx.draw_networkx_edges(G, pos)
nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)
plt.axis('off')
plt.show()
```
## 🪄 Parallel Connection

Resistors are connected between the same two nodes. Voltage is the same across them; currents add up.

**Formula:**

$$
\frac{1}{R_{\text{eq}}} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n}
$$

``` python
import networkx as nx
import matplotlib.pyplot as plt

G = nx.MultiGraph()
G.add_edge("A", "B", label="R1")
G.add_edge("A", "B", label="R2")
pos = {"A": (0,0), "B": (2,0)}

nx.draw_networkx_nodes(G, pos, node_color='lightblue', node_size=500)
nx.draw_networkx_labels(G, pos)
nx.draw_networkx_edges(G, pos, edgelist=[("A", "B")], connectionstyle="arc3,rad=0.2")
nx.draw_networkx_edges(G, pos, edgelist=[("A", "B")], connectionstyle="arc3,rad=-0.2")
plt.text(1, 0.2, "R1", horizontalalignment='center')
plt.text(1, -0.2, "R2", horizontalalignment='center')
plt.axis('off')
plt.show()
```

## 🔗 Example 1: Simple Series

**Given:**

$R_1 = 2\,\Omega$, $R_2 = 3\,\Omega$, $R_3 = 5\,\Omega$

**Solution:**

$$
R_{\text{eq}} = 2 + 3 + 5 = 10\,\Omega
$$

**Visualization:**

``` python
import networkx as nx
import matplotlib.pyplot as plt

G = nx.Graph()
G.add_edge("A", "B", label="2Ω")
G.add_edge("B", "C", label="3Ω")
G.add_edge("C", "D", label="5Ω")
pos = {"A": (0,0), "B": (1,0), "C": (2,0), "D": (3,0)}

nx.draw_networkx_nodes(G, pos, node_color='lightblue', node_size=500)
nx.draw_networkx_labels(G, pos)
edge_labels = {(u, v): d['label'] for u, v, d in G.edges(data=True)}
nx.draw_networkx_edges(G, pos)
nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)
plt.axis('off')
plt.show()
```

## ⚡ Example 2: Simple Parallel

**Given:**

$R_1 = 4\,\Omega$, $R_2 = 6\,\Omega$

**Solution:**

$$
\frac{1}{R_{\text{eq}}} = \frac{1}{4} + \frac{1}{6} = \frac{5}{12} \Rightarrow R_{\text{eq}} = 2.4\,\Omega
$$

**Visualization:**

``` python
import networkx as nx
import matplotlib.pyplot as plt

G = nx.MultiGraph()
G.add_edge("A", "B", label="4Ω")
G.add_edge("A", "B", label="6Ω")
pos = {"A": (0,0), "B": (2,0)}

nx.draw_networkx_nodes(G, pos, node_color='lightblue', node_size=500)
nx.draw_networkx_labels(G, pos)
nx.draw_networkx_edges(G, pos, edgelist=[("A", "B")], connectionstyle="arc3,rad=0.2")
nx.draw_networkx_edges(G, pos, edgelist=[("A", "B")], connectionstyle="arc3,rad=-0.2")
plt.text(1, 0.2, "4Ω", horizontalalignment='center')
plt.text(1, -0.2, "6Ω", horizontalalignment='center')
plt.axis('off')
plt.show()
```

## 🔀 Example 3: Nested Configuration

**Structure:**

$R_1$ and $R_2$ are in series.  
Their combination is in parallel with $R_3$.

**Given:**

$R_1 = 5\,\Omega$, $R_2 = 5\,\Omega$, $R_3 = 10\,\Omega$

**Solution:**

**Series:**

$$
R_{12} = R_1 + R_2 = 5 + 5 = 10\,\Omega
$$

**Parallel:**

$$
\frac{1}{R_{\text{eq}}} = \frac{1}{R_{12}} + \frac{1}{R_3} = \frac{1}{10} + \frac{1}{10} = \frac{1}{5} \Rightarrow R_{\text{eq}} = 5\,\Omega
$$

**Visualization:**

``` python
import networkx as nx
import matplotlib.pyplot as plt

G = nx.Graph()
G.add_edge("1", "2", label="R1")
G.add_edge("2", "3", label="R2")
G.add_edge("1", "3", label="R3")
pos = {"1": (0,0), "2": (1,1), "3": (1,-1)}

nx.draw_networkx_nodes(G, pos, node_color='lightblue', node_size=500)
nx.draw_networkx_labels(G, pos)
edge_labels = {(u, v): d['label'] for u, v, d in G.edges(data=True)}
nx.draw_networkx_edges(G, pos)
nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)
plt.axis('off')
plt.show()
```

##  Circuit Reduction Algorithm (Pseudocode)

**1. Build a graph where:**  
- **Nodes** = connection points  
- **Edges** = resistors with resistance values

**2. While the graph is not simplified:**  
- Look for degree-2 nodes → **combine series resistors**:  
  $R = R_1 + R_2$  
- Look for multiple edges between two nodes → **combine in parallel**:  
  $$
  \frac{1}{R} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots
  $$

**3. Repeat until only one edge remains → equivalent resistance.**

``` python
import networkx as nx
import matplotlib.pyplot as plt

def draw_step1():
    G = nx.Graph()
    G.add_edge("A", "B", label="R1")
    G.add_edge("B", "C", label="R2")
    G.add_edge("A", "C", label="R3")  # Параллельно R1+R2

    pos = {"A": (0, 0), "B": (1, 1), "C": (2, 0)}
    edge_labels = nx.get_edge_attributes(G, 'label')

    plt.figure(figsize=(4, 4))  # Уменьшение размера графика
    nx.draw(G, pos, with_labels=True, node_color='lightblue', node_size=500, font_weight='bold')
    nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)
    plt.title("Step 1: Initial Circuit")
    plt.axis('off')
    plt.show()

def draw_step2():
    G2 = nx.MultiGraph()
    G2.add_edge("A", "C", label="R12 (R1+R2)")
    G2.add_edge("A", "C", label="R3")

    pos = {"A": (0, 0), "C": (2, 0)}
    plt.figure(figsize=(4, 4))  # Уменьшение размера графика
    nx.draw_networkx_nodes(G2, pos, node_color='lightblue', node_size=500)
    nx.draw_networkx_labels(G2, pos, font_weight='bold')
    # Рисуем две дуги для параллельных резисторов
    nx.draw_networkx_edges(G2, pos, edgelist=[("A", "C")], connectionstyle="arc3,rad=0.2", width=2)
    nx.draw_networkx_edges(G2, pos, edgelist=[("A", "C")], connectionstyle="arc3,rad=-0.2", width=2)
    plt.text(1, 0.3, "R12", horizontalalignment='center', fontsize=10)
    plt.text(1, -0.3, "R3", horizontalalignment='center', fontsize=10)
    plt.title("Step 2: Series Combined → Parallel")
    plt.axis('off')
    plt.show()

def draw_step3():
    G3 = nx.Graph()
    G3.add_edge("A", "C", label="R_eq")

    pos = {"A": (0, 0), "C": (2, 0)}
    edge_labels = nx.get_edge_attributes(G3, 'label')

    plt.figure(figsize=(4, 4))  # Уменьшение размера графика
    nx.draw(G3, pos, with_labels=True, node_color='lightblue', node_size=500, font_weight='bold')
    nx.draw_networkx_edge_labels(G3, pos, edge_labels=edge_labels)
    plt.title("Step 3: Final Equivalent Resistance")
    plt.axis('off')
    plt.show()

# Вызов всех трёх шагов с отступами
draw_step1()
plt.subplots_adjust(hspace=0.4)  # Добавляем пространство между графиками
draw_step2()
plt.subplots_adjust(hspace=0.4)  # Добавляем пространство между графиками
draw_step3()
```
---

## 🧠 Conclusion

Graph theory allows **automated and visual circuit analysis**.

Complex networks can be **reduced step-by-step** to one equivalent value.

Ideal for **simulations**, **optimization tasks**, and **learning purposes**.




