# 🎢 Seesaw Simulation

This interactive physics simulation, built with **Vanilla HTML, CSS, and JavaScript**, visually and practically demonstrates the principles of **torque calculations** and **rotational physics**.

---

## 💡 Overview

This simulation allows users to add random weights to the seesaw plank by clicking on it. The application instantly calculates the torque for each side based on the **weight (mass)** and the **distance from the center**, and dynamically adjusts the plank's rotation angle (tilt) to maintain balance.

---

## ✨ Key Features

* **Interactive Weight Placement**: Click anywhere on the plank to add a new weight.
* **Real-time Physics Calculations**: Torque and angle values are calculated **automatically and instantly**.
* **Visual Feedback**:
    * ⬇️ Placement preview before dropping the weight.
    * 📏 **Distance indicators** showing the placement position.
    * 💫 **Smooth animations** for weight drops and plank rotation.
* **📊 Statistics Dashboard**:
    * Total weight on the left and right sides.
    * Instantaneous torque values for each side.
    * Current tilt angle.
    * Preview of the next weight.
* **📜 Activity Log**: Tracks the side and distance information for the last 10 weight placements.
* **💾 State Persistence**: The simulation state is **automatically saved and restored** using the browser's `localStorage` mechanism.
* **🎨 Responsive Design**: A modern, clean user interface (UI) with color-coded statistics.

---

## 🛠️ How to Use

You can open and test the project using this link: [Seesaw Simulation](https://hakancankul.github.io/seesaw/HAKAN_CANKUL.html)

1.  **➕ Add Weight**: Move your mouse over the plank to see a preview, then click to drop the weight.
2.  **📈 View Statistics**: Check the dashboard to see total weights, torque values, and the current angle.
3.  **🔄 Reset**: Click the "Reset Seesaw" button to clear all weights and start fresh.
4.  **⏱️ View History**: Review recent placements in the Activity Log.

---

## 🔬 Physics and Calculations
The simulation applies physical principles using simplified formulas.

### Torque Calculation
Torque (Moment of Force) is calculated as:
```
Torque = Weight × Visual Distance
```

* **Weight**: Random mass (1–10 kg)
* **Visual Distance**: Normalized distance from the center: `distance / DISTANCE_SCALE`
* **DISTANCE_SCALE**: 10

### Angle Calculation
The plank's rotation angle is determined by the difference in torque:
```
TorqueDifference = RightTorque - LeftTorque
Angle = TorqueDifference / TORQUE_DIVISOR
```

* **TORQUE_DIVISOR**: **10** (Controls rotation sensitivity)
* **MAX_ANGLE**: **±30°** (Sets the maximum rotation limit)

---

## 🚧 Trade-off Limitations and Solution

When the board was tilted, especially at high angles, there were perspective-based deviations between the clicked position for a new object and its **actual horizontal placement**. This issue was resolved using **trigonometry**.

1.  **Angle Calculation**:
    * The `getCurrentRotation(plankEl)` function gets the plank's current rotation angle in degrees, which is then converted to radians:

    ```javascript
    const currentRotation = getCurrentRotation(plankEl);
    const radians = currentRotation * (Math.PI / 180); // Convert to radians
    ```

2.  **Horizontal Distance Correction**:
    * A scale factor is calculated using the **cosine** function, and the current distance is multiplied by this factor to find the true horizontal distance:

    $$\text{Actual Horizontal Distance} = \frac{\text{Screen Horizontal Distance}}{\cos\theta}$$

    ```javascript
    const scaleFactor = 1 / Math.cos(radians); // Calculate scale factor based on rotation
    distanceFromCenter = distanceFromCenter * scaleFactor; // Adjust distance based on rotation
    ```
    This correction achieved a result close to 99% accuracy.

3.  **Endpoint Clamping**:
    * For perfect results, the maximum distance from the center where an object can be placed is clamped to be 20 pixels inward from the ends of the plank:

    ```javascript
    const maxDistance = (plankEl.offsetWidth / 2) - 20;
    if (distanceFromCenter > maxDistance) distanceFromCenter = maxDistance;
    if (distanceFromCenter < -maxDistance) distanceFromCenter = -maxDistance;
    ```
