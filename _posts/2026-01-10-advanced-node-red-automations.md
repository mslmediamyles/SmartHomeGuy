---
layout: post
title: "Advanced Automations with Node-RED"
date: 2026-01-10
category: Automation
description: "Master complex automation flows using Node-RED's visual programming interface for your smart home."
---

Node-RED is a powerful visual programming tool perfect for creating complex smart home automations without writing extensive code. Let's explore some advanced techniques.

## Why Node-RED?

While Home Assistant's built-in automations are great for simple tasks, Node-RED excels at:

- Complex conditional logic
- Multi-step workflows
- API integrations
- Data transformation
- Debugging automation flows

## Installation

If you're running Home Assistant OS or Supervised, install Node-RED from the Add-on Store. Otherwise, install it standalone:

```bash
# Install Node-RED globally
npm install -g --unsafe-perm node-red

# Install Home Assistant nodes
cd ~/.node-red
npm install node-red-contrib-home-assistant-websocket

# Start Node-RED
node-red
```

Access the editor at `http://your-ip:1880`

## Flow 1: Smart Morning Routine

This flow creates a morning routine that adapts to your schedule:

### Flow Logic

1. Trigger at 6:00 AM on weekdays
2. Check if anyone is home
3. Gradually increase bedroom lights
4. Start coffee maker
5. Read weather and adjust thermostat
6. Send notification with day's schedule

### Node-RED Configuration

Here's the function node for the light gradient:

```javascript
// Gradual light increase over 20 minutes
const steps = 20;
const interval = 60000; // 1 minute
let currentStep = 0;

const fadeIn = setInterval(() => {
    currentStep++;
    const brightness = Math.floor((currentStep / steps) * 255);
    
    msg.payload = {
        entity_id: "light.bedroom",
        brightness: brightness,
        color_temp: 370 - (currentStep * 5) // Warmer to cooler
    };
    
    node.send(msg);
    
    if (currentStep >= steps) {
        clearInterval(fadeIn);
    }
}, interval);
```

Weather-based climate adjustment:

```javascript
// Get weather data
const weatherEntity = global.get('homeassistant.homeAssistant.states["weather.home"]');
const temperature = weatherEntity.attributes.temperature;
const condition = weatherEntity.state;

// Calculate target temperature
let targetTemp = 21; // Default

if (temperature < 0) {
    targetTemp = 23; // Colder outside, warmer inside
} else if (temperature > 25) {
    targetTemp = 19; // Hot outside, cooler inside
}

// Adjust for weather conditions
if (condition === 'rainy' || condition === 'snowy') {
    targetTemp += 1; // Extra cozy
}

msg.payload = {
    entity_id: "climate.main_thermostat",
    temperature: targetTemp
};

return msg;
```

## Flow 2: Advanced Presence Detection

Combine multiple sensors for accurate presence detection:

```javascript
// Multi-factor presence detection
const factors = {
    phoneLocation: global.get('homeassistant.homeAssistant.states["device_tracker.phone"]').state,
    wifiConnected: global.get('homeassistant.homeAssistant.states["device_tracker.phone_wifi"]').state,
    doorSensor: global.get('homeassistant.homeAssistant.states["binary_sensor.front_door"]').state,
    motionDetected: global.get('homeassistant.homeAssistant.states["binary_sensor.living_room_motion"]').state,
    lastSeen: global.get('lastSeenTimestamp') || 0
};

// Scoring system
let presenceScore = 0;

if (factors.phoneLocation === 'home') presenceScore += 40;
if (factors.wifiConnected === 'home') presenceScore += 30;
if (factors.doorSensor === 'on') presenceScore += 15;
if (factors.motionDetected === 'on') presenceScore += 15;

// Time-based decay
const timeSinceLastSeen = Date.now() - factors.lastSeen;
if (timeSinceLastSeen < 300000) { // 5 minutes
    presenceScore += 20;
}

// Determine state
const isHome = presenceScore >= 50;

msg.payload = {
    state: isHome ? 'home' : 'away',
    confidence: presenceScore,
    factors: factors
};

// Update last seen if home
if (isHome) {
    global.set('lastSeenTimestamp', Date.now());
}

return msg;
```

## Flow 3: Energy Management

Monitor and optimize energy usage:

```javascript
// Energy usage analyzer
const currentUsage = global.get('homeassistant.homeAssistant.states["sensor.total_power"]').state;
const solarProduction = global.get('homeassistant.homeAssistant.states["sensor.solar_power"]').state;
const batteryLevel = global.get('homeassistant.homeAssistant.states["sensor.battery_soc"]').state;

// Calculate net energy
const netEnergy = parseFloat(solarProduction) - parseFloat(currentUsage);

// Decision logic
let action = {
    type: 'none',
    devices: []
};

if (netEnergy > 1000 && batteryLevel < 90) {
    // Excess solar, charge battery
    action.type = 'charge_battery';
    action.devices.push('battery.main');
    
} else if (netEnergy > 500) {
    // Some excess, run non-critical loads
    action.type = 'run_deferrable';
    action.devices = [
        'switch.water_heater',
        'switch.pool_pump',
        'switch.ev_charger'
    ];
    
} else if (currentUsage > 3000 && solarProduction < 500) {
    // High grid usage, reduce load
    action.type = 'reduce_load';
    action.devices = [
        'climate.main_hvac',
        'water_heater.main'
    ];
}

msg.payload = action;
return msg;
```

## Flow 4: Notification Hub

Create intelligent notifications that avoid spam:

```javascript
// Smart notification manager
const notificationQueue = context.get('queue') || [];
const lastNotification = context.get('lastSent') || 0;
const now = Date.now();

// Add new notification to queue
const newNotification = {
    title: msg.payload.title,
    message: msg.payload.message,
    priority: msg.payload.priority || 'normal',
    timestamp: now
};

notificationQueue.push(newNotification);

// Sort by priority (high > normal > low)
const priorityOrder = { high: 3, normal: 2, low: 1 };
notificationQueue.sort((a, b) => 
    priorityOrder[b.priority] - priorityOrder[a.priority]
);

// Determine if we should send now
let shouldSend = false;
const timeSinceLastNotification = now - lastNotification;

if (newNotification.priority === 'high') {
    shouldSend = true; // Always send high priority
} else if (timeSinceLastNotification > 300000) { // 5 minutes
    shouldSend = true; // Enough time has passed
} else if (notificationQueue.length >= 5) {
    shouldSend = true; // Too many queued, batch send
}

if (shouldSend) {
    // Prepare batch notification
    let batchMessage = '';
    
    if (notificationQueue.length === 1) {
        msg.payload = notificationQueue[0];
    } else {
        batchMessage = `${notificationQueue.length} notifications:\n\n`;
        notificationQueue.forEach((notif, idx) => {
            batchMessage += `${idx + 1}. ${notif.message}\n`;
        });
        
        msg.payload = {
            title: "Smart Home Updates",
            message: batchMessage,
            priority: 'normal'
        };
    }
    
    // Clear queue and update timestamp
    context.set('queue', []);
    context.set('lastSent', now);
    
    return msg;
} else {
    // Don't send yet
    context.set('queue', notificationQueue);
    return null;
}
```

## Debugging Tips

### Enable Debug Nodes

Add debug nodes to see data flow:

```javascript
// In any function node, add logging
node.warn("Current value: " + JSON.stringify(msg.payload));
```

### Use Status Nodes

Show flow state visually:

```javascript
// Set node status
node.status({
    fill: "green",
    shape: "dot",
    text: "Active: " + msg.payload.state
});
```

### Context Debugging

```javascript
// View all context data
node.warn("Flow context: " + JSON.stringify(flow.keys()));
node.warn("Global context: " + JSON.stringify(global.keys()));
```

## Best Practices

1. **Use Subflows**: Create reusable components
2. **Comment Your Flows**: Add descriptions to complex nodes
3. **Error Handling**: Always include catch nodes
4. **Rate Limiting**: Prevent API spam with delay nodes
5. **Test Incrementally**: Build and test one node at a time

## Integration with Home Assistant

Configure Home Assistant to trigger Node-RED flows:

```yaml
# configuration.yaml
automation:
  - alias: "Trigger Node-RED Flow"
    trigger:
      platform: state
      entity_id: binary_sensor.front_door
      to: 'on'
    action:
      service: rest_command.trigger_nodered
      data:
        flow_id: "morning_routine"
        
rest_command:
  trigger_nodered:
    url: "http://localhost:1880/trigger/{{ flow_id }}"
    method: POST
```

## Conclusion

Node-RED's visual programming interface makes complex automations accessible while maintaining flexibility. Start with simple flows and gradually increase complexity as you become comfortable with the environment.

Check out my [Node-RED Flow Collection](https://github.com/yourusername/nodered-flows) for more examples!
