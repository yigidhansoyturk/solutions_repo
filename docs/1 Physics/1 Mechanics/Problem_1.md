# Problem 1
import numpy as np
import matplotlib.pyplot as plt

# Constants
g = 9.81  # acceleration due to gravity (m/s^2)

# Function to calculate the range
def calculate_range(v0, theta_0):
    # Convert angle to radians
    theta_0_rad = np.radians(theta_0)
    # Range formula
    return (v0**2 * np.sin(2*theta_0_rad)) / g

# Initial velocity
v0 = 20  # m/s

# Angles from 0 to 90 degrees
angles = np.linspace(0, 90, 100)
ranges = [calculate_range(v0, angle) for angle in angles]

# Plotting the range as a function of the angle
plt.figure(figsize=(8, 6))
plt.plot(angles, ranges, label=f'Initial velocity = {v0} m/s')
plt.title('Projectile Range vs Angle of Projection')
plt.xlabel('Launch Angle (degrees)')
plt.ylabel('Range (meters)')
plt.grid(True)
plt.legend()
plt.show()