---
layout: default
title: Home
---

# 1|ABSTRACT

This study applied two complementary AI - based approaches to improve marine oil split response. First model produced physics - based synthetic data using advection - diffusion formula, and predicted diffusion route and ecological effect (Do decrease, toxic exposure, recovery level). Second model compared random arrangement and AI - based hotspot strategy using predicted pollution levels. Though each model is realized independently, prediction model predict diffusion dynamics exactly and hotspot strategy increases removal effectiveness about 2 times to show potential for future integration.

# 2|INTRODUCTION

## 1 scientific background, scientific knowledge

### Physics (Fluid Dynamics)

Moving of marine oil spill is a complex action that is caused by advection by ocean currents and diffusion by turbulence. This research calculated temporal and spatial variation of marine oil spill while materializing Advection - Diffusion Equation to imitate this unregular moving.

### Chemistry (Chemical Weathering)

We modeled a biochemical oxygen demand mechanism consuming underwater oxygen rapidly in the process that disassembles the oil. Especially, we quantified Hypoxia by blocking the inflow of atmospheric oxygen (reaeration) due to oil slick.

### Biology (Ecotoxicology)

We applied toxic model based on lethal concentration (LC50) to expect survival of plankton that is the basic of the marine food chain. Also, We produced resilience of the ecosystem by simulating the process of population returning to normal levels after removing contamination using a logistic growth model.

### AI (ConvLSTM)

We launched the ConvLSTM (Convolutional LSTM) model to integrate these 3 scientific principles. This predicts the spread of marine oil split and ecological impacts in the complex marine ecosystem, learning physical fluid movement like spatial information and ecological/chemical changes like temporal information.

## 2 Research necessity and motivation

### Research necessity and motivation

Lesson of Taean accident (social- economical effects) : The 2007 Taean Hebei spirit oil spill accident was a social - economical crisis that caused enormous economic damages (approximately 330 million USD​​) to local fisheries more than a simple environmental disaster. After the surface oil cleanup efforts ended, residual oil existed for a few years damaging seriously to marine ecosystems and local living.

### Limitation of Existing Technology

Limitation of existing technology (invisible damage) : Conventional oil control systems only focus on surface oil removal. However, It has limitations that cannot detect or respond to invisible ecological damage due to dissolved toxins and oxygen depletion (Hypoxia) in the water rapidly.

### Research Purpose (Paradigm Shift)

Research purpose (Paradigm shift) : To overcome these limitations, this research suggests proactive control solutions that prioritized ecological recovery through AI. The final goal is protecting the local community’s right to economic livelihood and achieving ‘ UN Sustainable Development Goal 14 (SDG 14, Life Below water) to shorten the recovery time a lot.

## 3 Research Question - main question

> “ How can an AI system that integrates physical moving, chemical weathering, and biological recovery models optimize the effectiveness of marine disaster response?”

# 2 Material&Method

## Model A : Focused on Physics–Ecology Integrated Prediction Model

### A.Data construction: Physics-based + Evidence-based

To resolve the problem of lacking real-world accident data, we constructed Physics-based synthetic dataset through extensive literature-based review.

- Physical Dynamics: We conducted Advection-Diffusion Equation-based oil spill spread simulation which integrated real-world ocean current data (CMEMS).
- Bio-Chemical Parameters: By setting up the environmental parameter based on literature, we secured ecosystem reality of the model.
- Biodegradation Rate (Kbio): 0.05 ~ 0,07 day-1(Prince et al., 2013).
- Toxicity Threshold (LC50): 30.0 mg/L (Newsted, 1987; Almeda et al., 2013).
- Oxygen Demand: 3.0 g/g (Stoichiometric analysis)

So, Not just simple imaginary data, We use real marine data (NetCDF file) provided by CMEMS (Europe marine monitoring service). We convert this data to PyTorch tensor through the code(convert_data.py) and set the resolution using bilinear interpolation and construct time series learning data using the sliding window method.

### B. model structure: Spatiotemporal Prediction

Model : We designed ConLSTM (Convolutional LSTM) structure that combines CNN and LSTM,

- Through CNN, spatial distribution is achieved,
- Through LSTM, temporal dynamics are learned simultaneously

  Training: We trained the model using over 80,000 hybrid scenarios frames, and optimized the model to predict spread pathways in a complicated ocean current condition.
  Therefore, We construct the ConvLSTM network directly. To learn oil distribution (space) and ocean current flow (time) at the same time, we choose 2-stacked structures. The kernel size was 3x3, and it was designed to extract a complex vector field by setting 32 hidden channels. Learning was performed for 100 Epoch using Adam optimizer and MSE loss function.

### C. Ecological Modeling: Reaction - Diffusion PDE

We designed Reaction - Diffusion PDE to quantify the ecological recovery process.

![images](assets/images/Reaction-Diffusion-PDE.png)

The reaction term R(C) models microorganism decomposition, reaeration dampening, and the toxic mortality, allowing for calculation of the process of formation and destruction of Dead Zone.
Based on data that was expected by AI, We calculated PDE numerically. At this time, scientific constants such as LC50 (toxicity) and Kbio (biodegradation rate) were implemented in the code (biology_ops.py) based on the paper to increase reliability."

### Conceptual Prototype Design

System Architecture: Feedback Loop Model

_This system designed as Feedback Loop Model: [Detection & prediction -> strategic decision making -> ecological recovery]_

Step 1. Spatiotemporal Prediction

- Input: satellite-based initial oil spill map + live environment / ocean resources (wind speed & ocean current)
- Process: ConvLSTM predicts oil spill pathways and concentration change over the next 5~10 days.

Step 2. Selective Cleanup Strategy

- Protocol: Considering biological sensitivity, create Priority Map based on contamination prediction map
- Decision: Limited cleanup resources (ships, oil boom, etc.) are priorly deployed to Hotspots, maximizing efficiency of ecological damage mitigation.

Step 3. Ecological Recovery Simulation

- Assessment: Based on the concentration of residual contamination, numerically simulate the dissolved oxygen (DO) and Planktonic Biomass.
  Output: Visualize Ecological Recovery Index (ERI) and estimated recovery time (days) to support policy & control decision making.
