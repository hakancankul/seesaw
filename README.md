# Seesaw Simulation

An interactive physics-based seesaw (tahterevalli) simulation built with vanilla HTML, CSS, and JavaScript. This application demonstrates torque calculations and rotational physics in a visual, interactive way.

## Overview

This simulation allows users to add weights to a seesaw plank by clicking on it. The application calculates torque for each side based on weight and distance from the center, then dynamically adjusts the plank's rotation angle to maintain balance.

## Features

- **Interactive Weight Placement**: Click anywhere on the plank to add weights
- **Real-time Physics Calculations**: Automatic torque and angle calculations
- **Visual Feedback**: 
  - Preview weight placement before dropping
  - Distance indicators showing placement position
  - Smooth animations for weight drops and plank rotation
- **Statistics Dashboard**: 
  - Left and right side weight totals
  - Torque values for each side
  - Current tilt angle
  - Next weight preview
- **Activity Log**: Tracks the last 10 weight placements with side and distance information
- **State Persistence**: Automatically saves and restores simulation state using localStorage
- **Responsive Design**: Modern, clean UI with color-coded statistics

## How to Use

1. **Add Weight**: Move your mouse over the plank to see a preview, then click to drop the weight
2. **View Statistics**: Check the dashboard at the top to see weight totals, torque values, and current angle
3. **Reset**: Click the "Reset Seesaw" button to clear all weights and start fresh
4. **View History**: Scroll through the activity log to see recent weight placements

## Physics & Calculations

### Torque Calculation

The simulation uses a simplified torque formula:

```
Torque = Weight × Visual Distance
```

Where:
- **Weight**: The mass value (1-10 kg, randomly generated)
- **Visual Distance**: Normalized distance from center (`distance / DISTANCE_SCALE`)
- **DISTANCE_SCALE**: 10 (converts pixels to physical units)

### Angle Calculation

The plank's rotation angle is determined by the torque difference:

```
torqueDifference = rightTorque - leftTorque
angle = torqueDifference / TORQUE_DIVISOR
```

Where:
- **TORQUE_DIVISOR**: 10 (controls rotation sensitivity)
- **MAX_ANGLE**: ±30° (maximum rotation limit)

### Trade-off Limitations

When the board was tilted, especially at high angles, there were deviations from the position I clicked when adding a new object due to perspective. I thought about this for a while and came up with a trigonometric solution. Firstly, getCurrentRotation() fonksiyonu ile dönüş açısını derece cinsinden aldım ve trigonometrik hesaplamalar için radyan cinsine dönüştürdüm

```javascript
const currentRotation = getCurrentRotation(plankEl);
const radians = currentRotation * (Math.PI / 180); // Convert to radians
```
I calculated the scale factor and multiplied it by the current distance value to find the actual horizontal distance. (I got a result close to 99%)

```javascript
//Actural Horizontal Distance = (Screen Horizontal Distance) / cosθ
const scaleFactor = 1 / Math.cos(radians); // Calculate scale factor based on rotation
distanceFromCenter = distanceFromCenter * scaleFactor; // Adjust distance based on rotation
```

Actually, it was mostly correct, but I still clamped the distance we could place the plank object from the ends to make it 100%.
```javascript
const maxDistance = (plankEl.offsetWidth / 2) - 20;
if (distanceFromCenter > maxDistance) distanceFromCenter = maxDistance;
if (distanceFromCenter < -maxDistance) distanceFromCenter = -maxDistance;
```



