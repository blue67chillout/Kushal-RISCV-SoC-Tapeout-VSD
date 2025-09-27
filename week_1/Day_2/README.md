# Day 2: Timing Libraries, Synthesis Styles & Flip-Flop RTL Patterns  

This module introduces three key concepts in RTL-to-GDS flows:  

1. How to interpret and use a **.lib timing library** (example: `sky130_fd_sc_hd__tt_025C_1v80.lib`) from the SKY130 open-source PDK.  
2. The contrast between **hierarchical vs. flat synthesis strategies**.  
3. Writing **compact and efficient Verilog flip-flop descriptions** for synthesis.  

---

## Timing Libraries in ASIC Design  

### SKY130 PDK Essentials  
The **SkyWater 130nm PDK** (Process Design Kit) supplies device models and libraries necessary for IC implementation. Among these are timing libraries, which describe propagation delays, setup/hold characteristics, power usage, and process variations of standard cells.  

### Breaking Down `tt_025C_1v80`  
- **tt** → “Typical-Typical” process corner.  
- **025C** → Assumes device operation at **25 °C**.  
- **1v80** → Nominal supply voltage of **1.8 V**.  

This shorthand defines the **PVT (Process-Voltage-Temperature) conditions** under which the standard cell data was characterized.  

### Inspecting the .lib File  
Install a simple GUI editor:  
```bash
sudo apt install gedit
```

Open the library file:
```bash
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## Synthesis Styles: Hierarchical vs. Flat

### Hierarchical Synthesis
**Definition**: Keeps the original RTL module boundaries intact during synthesis.  
**Operation**: Tools (e.g., Yosys) analyze and synthesize each module separately.  

**Pros**:
- Faster compile time for large designs  
- Easier debugging since hierarchy is preserved  
- Modular design is simpler to integrate  

**Cons**:
- Optimizations are confined within each module  
- Reports may need extra setup  
