# PikaDevBoard Assembly Guide

## Before You Start

### Required Tools
- Soldering iron (temperature controlled, 300-350°C)
- Solder (0.6-0.8mm, 60/40 or lead-free)
- Flux (rosin-based)
- Tweezers (for SMD components)
- Multimeter
- Magnifying glass or microscope (recommended)

### Skills Required
- Basic soldering experience
- Ability to solder 06ц03 SMD components
- Understanding of component polarity

### Safety
- Work in well-ventilated area
- Use proper ESD precautions
- Keep workspace clean and organized

## Assembly Steps

### Step 1: PCB Inspection
1. Inspect PCB for defects or damage
2. Check solder mask and silkscreen quality
3. Verify all holes are properly drilled
4. Clean PCB with isopropyl alcohol if needed

### Step 2: Component Preparation
1. Organize components by type and value
2. Check all components against BOM
3. Verify component orientations using placement diagram
4. Pre-tin difficult pads if necessary

### Step 3: SMD Component Assembly

#### Order of Assembly
1. **Passive Components First** (resistors, capacitors)
2. **Active Components** (regulator, LEDs)
3. **Connectors and Switches**
4. **Final Inspection and Testing**

#### Detailed Assembly

**3a. Resistors (R1-R4)**
```
- R1, R2: 10kΩ (pullup resistors)
- R3, R4: 330Ω (LED current limiting)
- No polarity concern
- Use tweezers to place, tack one end, align, solder other end
```

**3b. Capacitors (C1-C6)**
```
- C1, C2: 10μF (power supply filtering)
- C3, C4: 100nF (decoupling)
- C5, C6: 22pF (crystal load - if using external crystal)
- Ceramic caps have no polarity
- Place carefully to avoid tombstoning
```

**3c. Voltage Regulator (U2)**
```
- AMS1117-3.3 in SOT-223 package
- Check datasheet for pin orientation
- Pin 1: GND, Pin 2: OUT, Pin 3: IN
- Use adequate flux and avoid bridging
```

**3d. LEDs (LED1, LED2)**
```
- LED1: Power indicator (Green)
- LED2: User LED (Red)
- IMPORTANT: Check polarity!
- Cathode (shorter lead) marked on silkscreen
- Test with multimeter if unsure
```

### Step 4: Through-Hole Components

**4a. Switches (SW1, SW2)**
```
- SW1: Reset switch
- SW2: User switch
- Push through from top, solder from bottom
- Ensure switches sit flat against PCB
```

**4b. Headers and Connectors**
```
- J1: USB-C connector (if using)
- J2: ST-Link programming header (2x2)
- J3, J4: GPIO headers (customize as needed)
- J5: Power jack (optional)

Installation:
1. Insert from top side
2. Use tape to hold in place while soldering
3. Solder one pin, check alignment
4. Solder remaining pins
```

### Step 5: Microcontroller Socket/Headers

**Option A: Socket Installation**
```
- Use DIP socket or pin headers for removable MCU
- Ensures MCU can be replaced or upgraded
- Align carefully with pin 1 marking
```

**Option B: Direct Soldering**
```
- Solder MCU directly to board
- More permanent but lower profile
- Use proper ESD precautions
- Double-check orientation before soldering
```

### Step 6: Testing and Validation

#### Initial Power Test
1. **Visual Inspection**
   - Check for solder bridges
   - Verify component orientations
   - Look for cold solder joints

2. **Continuity Test**
   - Test power and ground connections
   - Verify no shorts between VCC and GND
   - Check programming header connections

3. **Power-Up Test**
   - Connect power (USB or external)
   - Check 3.3V output from regulator
   - Verify power LED operation

#### Functional Testing
1. **Programming Interface Test**
   - Connect ST-Link V2
   - Attempt to detect MCU with STM32CubeIDE
   - Upload simple blink program

2. **GPIO Test**
   - Test user LED control
   - Verify button inputs
   - Check header pin continuity

## Common Issues and Solutions

### Soldering Issues

**Solder Bridges**
- Use solder wick to remove excess solder
- Apply flux and use clean iron tip
- Work slowly with proper temperature

**Cold Joints**
- Reheat joint with proper temperature
- Ensure both pad and component leg are heated
- Use fresh solder if joint looks dull

**Component Tombstoning**
- Heat both ends of component simultaneously
- Use less solder and more flux
- Pre-tin pads if necessary

### Electrical Issues

**No Power Output**
- Check input voltage at regulator
- Verify regulator orientation
- Test for shorts in power supply

**Programming Fails**
- Verify ST-Link connections
- Check MCU power supply (3.3V)
- Ensure proper ground connection
- Try different programming speed

## Quality Control Checklist

Before considering assembly complete:

- [ ] All components installed per BOM
- [ ] No visible solder bridges or defects
- [ ] Power supply outputs correct voltage
- [ ] Programming interface functional
- [ ] User LEDs and buttons work
- [ ] All header pins have continuity
- [ ] Board passes visual inspection

## Documentation

Take photos of:
- Completed board (top and bottom)
- Any modifications or issues encountered
- Test results and measurements

## Support

If you encounter issues during assembly:
1. Consult this guide and component datasheets
2. Check the troubleshooting section in main docs
3. Post questions in GitHub issues with photos
4. Join the community discussion forum

## Files Reference

- `assembly_diagram.pdf` - Component placement reference
- `step_by_step_photos/` - Visual assembly guide
- `test_procedures.md` - Detailed testing instructions