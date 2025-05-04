# Problem 1

# **Electromagnetism**  
## **Problem 1 — Simulation of the Lorentz Force**

---

## 🌟 Motivation

The **Lorentz force** governs how a charged particle moves through **electric** and **magnetic fields**. Understanding this phenomenon is essential in a wide range of scientific and engineering applications:

- ⚛ **Particle Accelerators** — Guide and speed up particles using magnetic fields.  
- 🧪 **Mass Spectrometry** — Identify chemical substances by separating ions by mass and charge.  
- 🔥 **Plasma Confinement** — Trap hot plasma in devices for nuclear fusion (e.g., Tokamaks).  
- 🌌 **Space Physics** — Explain how charged particles (like solar wind) interact with magnetic fields in space.

---

## ⚡ Electromagnetism Problem: Lorentz Force

## Given:
- **Charge:** $q = 1\ \text{C}$
- **Mass:** $m = 1\ \text{g} = 0.001\ \text{kg}$

---

## 1. Theory: Lorentz Force

The Lorentz force is defined as:

$$
\vec{F} = q(\vec{E} + \vec{v} \times \vec{B})
$$

Where:

- $\vec{F}$ — force acting on the charge,
- $q$ — charge,
- $\vec{E}$ — electric field vector,
- $\vec{B}$ — magnetic field vector,
- $\vec{v}$ — velocity of the particle.

![alt text](image-12.png)

[Visit My colab](https://colab.research.google.com/drive/1Bv1xWJ2zIVPVlxB-y_DUu2DTKRkTseQx)

``` python
# КРУГОВАЯ ТРАЕКТОРИЯ
import numpy as np
from scipy.integrate import solve_ivp
import matplotlib.pyplot as plt

# Параметры
q = 1  # Кл
m = 0.001  # кг
qm = q / m

# Уравнения Лоренца
def lorentz_rhs(t, y):
    vx, vy, vz = y[3:]
    B = np.array([0, 0, 1])  # Магнитное поле вдоль z
    v = np.array([vx, vy, vz])
    dvdt = qm * np.cross(v, B)
    return [vx, vy, vz, *dvdt]

# Начальные условия: положение и скорость
y0 = [0, 0, 0, 1, 0, 0]  # v вдоль x, B вдоль z

# Временной интервал
t_span = (0, 0.05)
t_eval = np.linspace(*t_span, 1000)

# Решение ОДУ
sol = solve_ivp(lorentz_rhs, t_span, y0, t_eval=t_eval)

# График
plt.figure(figsize=(6,6))
plt.plot(sol.y[0], sol.y[1])
plt.xlabel('x')
plt.ylabel('y')
plt.title('Circular trajectory (v ⟂ B)')
plt.axis('equal')
plt.grid(True)
plt.show()
```

---

## 2. Equation of Motion

From Newton's second law:

$$
m \frac{d\vec{v}}{dt} = q(\vec{E} + \vec{v} \times \vec{B})
$$

Dividing both sides by mass $m$:

$$
\frac{d\vec{v}}{dt} = \frac{q}{m}(\vec{E} + \vec{v} \times \vec{B})
$$

![alt text](image-13.png)

[Visit My Colab](https://colab.research.google.com/drive/1yGPCk_v0ouBSo2W5vaFOOQtDYtBwbxed)

``` python
# СПИРАЛЬНАЯ ТРАЕКТОРИЯ ВДОЛЬ Z
import numpy as np
from scipy.integrate import solve_ivp
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

q = 1  # Кл
m = 0.001  # кг
qm = q / m

def lorentz_rhs(t, y):
    vx, vy, vz = y[3:]
    B = np.array([0, 0, 1])  # B вдоль z
    v = np.array([vx, vy, vz])
    dvdt = qm * np.cross(v, B)
    return [vx, vy, vz, *dvdt]

y0 = [0, 0, 0, 1, 0, 1]  # v по x и по z

t_span = (0, 0.1)
t_eval = np.linspace(*t_span, 1000)

sol = solve_ivp(lorentz_rhs, t_span, y0, t_eval=t_eval)

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.plot(sol.y[0], sol.y[1], sol.y[2])
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
ax.set_title('Spiral along the axis z')
plt.show()
```

---

## 3. Python Simulation (Scenarios)

We implement three scenarios to observe the resulting particle trajectories:

### a. Circular Trajectory

**Conditions:**
- $\vec{E} = 0$
- $\vec{B} = (0, 0, B)$ (along the $z$-axis)
- Initial velocity: $\vec{v}_0 = (v, 0, 0)$

**Expected result:** Circular motion in the $xy$-plane.

---

### b. Spiral Along the Z-axis

**Conditions:**
- $\vec{E} = 0$
- $\vec{B} = (0, 0, B)$
- Initial velocity: $\vec{v}_0 = (v, 0, v_z)$

**Expected result:** Helical (spiral) motion along the $z$-axis.

---

### c. Interesting Drift Trajectory

**Conditions:**
- $\vec{E} \ne 0$, $\vec{B} \ne 0$
- Example: $\vec{E} = (0, E, 0)$, $\vec{B} = (0, 0, B)$
- Initial velocity: $\vec{v}_0 = (v_x, 0, 0)$

**Expected result:** Complex drift motion due to both electric and magnetic fields (E×B drift).

![alt text](image-14.png)

[Visit My Colab](https://colab.research.google.com/drive/11LVY-oECIBTi-wBZgVslF2bd77V0VSkG)

``` python
# СЛОЖНАЯ ТРАЕКТОРИЯ В НАКЛОННОМ ПОЛЕ
import numpy as np
from scipy.integrate import solve_ivp
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

q = 1
m = 0.001
qm = q / m

def lorentz_rhs(t, y):
    vx, vy, vz = y[3:]
    B = np.array([0.5, 0, 1])  # Наклонное поле
    v = np.array([vx, vy, vz])
    dvdt = qm * np.cross(v, B)
    return [vx, vy, vz, *dvdt]

y0 = [0, 0, 0, 1, 0, 0.5]  # скорость по x и z

t_span = (0, 0.1)
t_eval = np.linspace(*t_span, 1000)

sol = solve_ivp(lorentz_rhs, t_span, y0, t_eval=t_eval)

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
ax.plot(sol.y[0], sol.y[1], sol.y[2])
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
ax.set_title('Complex 3D trajectory (inclined B)')
plt.show()
```
