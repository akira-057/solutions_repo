# Problem 1

Investigating the Range as a Function of the Angle of Projection

📌 1. Theoretical Foundation

Projectile motion follows Newton's laws and is governed by kinematic equations. The motion can be broken down into:

✅ Horizontal Motion: Constant velocity motion since there are no horizontal forces (ignoring air resistance).
✅ Vertical Motion: Accelerated motion under gravity.

We consider an object launched from ground level with an initial velocity  at an angle  from the horizontal.

🔹 Equations of Motion

The initial velocity components are:




Using the kinematic equations:




where:

 is gravitational acceleration ( m/s² on Earth).

 is time.

The time to reach the highest point:



The total time of flight (since time to go up = time to come down):



📌 2. Analysis of the Range

The range  is the total horizontal distance traveled by the projectile before landing. It is found by using:



Substituting  and :



Using the identity , we simplify:



🔹 Key Observations:

🔹 The range depends on , meaning the maximum range occurs at .
🔹 For any angle , there is a complementary angle  that gives the same range.
🔹 Increasing  increases the range proportionally to .

📌 3. Practical Applications

📍 Sports: Understanding how to kick or throw a ball for maximum distance.
📍 Engineering: Designing missile or projectile trajectories.
📍 Astrophysics: Studying planetary motion without air resistance.

📌 4. Implementation in Python

A simple Python script to visualize the relationship between range and angle:

import numpy as np
import matplotlib.pyplot as plt

g = 9.81  # Acceleration due to gravity
v0 = 20   # Initial velocity (m/s)
angles = np.linspace(0, 90, 100)  # Angles from 0 to 90 degrees
radii = (v0**2 * np.sin(np.radians(2 * angles))) / g  # Compute range

plt.figure(figsize=(8, 5))
plt.plot(angles, radii, label='Range', color='b', linewidth=2)
plt.axvline(x=45, color='r', linestyle='--', label='Max Range (45°)')
plt.xlabel('Angle of Projection (degrees)', fontsize=12)
plt.ylabel('Range (m)', fontsize=12)
plt.title('🔹 Range vs Angle of Projection 🔹', fontsize=14, fontweight='bold')
plt.legend()
plt.grid(True, linestyle='--', alpha=0.6)
plt.show()

📌 5. Limitations and Future Considerations

⚠️ Air Resistance: Real-world motion is affected by drag, reducing range.
⚠️ Uneven Terrain: The formula assumes a flat landing surface.
⚠️ Non-Uniform Gravity: On different planets,  changes.

