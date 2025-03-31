# Problem 1
import numpy as np
import matplotlib.pyplot as plt

# Constants
g = 9.81  # Gravitational acceleration in m/s^2

# Function to calculate the range based on initial velocity and launch angle
def calculate_range(v0, theta_0_deg):
    # Convert angle from degrees to radians
    theta_0_rad = np.radians(theta_0_deg)
    
    # Range formula: R = (v0^2 * sin(2*theta)) / g
    R = (v0**2 * np.sin(2 * theta_0_rad)) / g
    return R

# Initial velocity (m/s)
v0 = 20  # Example initial velocity, you can change it

# Launch angles from 0 to 90 degrees
angles = np.linspace(0, 90, 100)  # 100 angles from 0 to 90 degrees

# Calculate the range for each angle
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
