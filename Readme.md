# PTK indicators



# P.T.K - VWAP Indicator Analysis



## Overview
Indicator for VWAP based trading system with multiple bands, previous value area tracking, and a simple/sequence based alerting system for different market conditions.

## Core Components

### 1. VWAP Configuration
- **Anchor Periods**: Session, Week, Month, Quarter, Year, Decade, Century, Earnings, Dividends, Splits
- **Source**: Configurable price source (default: HLC3)
- **Calculation Modes**: Standard Deviation or Percentage-based bands
- **Hide Option**: Can hide VWAP on daily or higher timeframes

### 2. Band System

#### Developing Value Area (DVA) Bands
- **Value Area**: Primary VWAP bands (±1.0 multiplier default)
- **Buffer Bands**: Secondary bands (±0.8 multiplier default)
- **Mid Bands**: Inner bands (±0.5 multiplier default)
- **Standard Deviation Bands**: 
  - ±1 bands (1.5 multiplier default)
  - ±2 bands (2.0 multiplier default)
  - ±3 bands (3.0 multiplier default)

#### Display Options
- **Band Mode**: Choose between line bands or filled areas
- **Individual Control**: Toggle each band type on/off
- **Color Coding**: Different colors for each band level

### 3. Previous Value Area (PVA)
- Tracks and displays the previous period's VWAP and value area
- Shows previous DVAH (Developing Value Area High) and DVAL (Developing Value Area Low)
- Includes previous buffer and mid-level bands
- Updates when a new period begins based on the selected anchor

## Alerting System

Multi-tier alerting system with both simple and sequence-based alerts.

### Simple Alerts (Single Condition)

#### DVA Simple Alerts
1. **EXEC_FADE_LOW**: Price touches/breaks below DVAL
2. **EXEC_FADE_HIGH**: Price touches/breaks above DVAH  
3. **EXEC_TOUCH_VWAP**: Price crosses the VWAP line

#### PVA Simple Alerts
1. **PVA_LOW_TEST**: Price tests the previous value area low
2. **PVA_HIGH_TEST**: Price tests the previous value area high
3. **PVA_VWAP_TEST**: Price crosses the previous VWAP

### Sequence-Based Alerts (Multi-Step Conditions)

#### DVA Complex Alerts
1. **FAILED_AUCTION_LOWER** (3 steps):
   - Step 1: Price closes below DVAL
   - Step 2: Price closes above buffer low
   - Step 3: Price drops back below buffer low

2. **FAILED_AUCTION_HIGHER** (2 steps):
   - Step 1: Price closes below buffer high
   - Step 2: Price rises above DVAH

3. **IMBALANCED_UP** (2 steps):
   - Step 1: Price closes above DVAH
   - Step 2: Price pulls back to touch DVAH

4. **IMBALANCED_DOWN** (2 steps):
   - Step 1: Price closes below DVAL
   - Step 2: Price pulls back to touch DVAL

#### PVA Complex Alerts
1. **PVA_BPB** (Breakout Pullback - 2 steps):
   - Step 1: Price closes above previous DVAH
   - Step 2: Price pulls back below previous DVAH

### Alert Features

#### Optimistic Alerting
- **Buffer Zone**: Adds a small buffer to alert levels to avoid being "front-run"
- **Calculation**: Uses half the distance between DVAH and buffer high as buffer
- **Direction-Based**: Adds buffer for longs, subtracts for shorts

#### Visual Indicators
- **Alert Triggers**: Optional visual markers when alerts fire
- **Background Colors**: Debug mode with colored backgrounds for sequence steps
- **Cross Markers**: Visual indicators on band touch points

## Technical Implementation

### Alert State Management
- Uses custom types (`CustomAlertState`, `SequenceAlertState`) to track multi-step conditions
- Maintains arrays of alert definitions and states
- Resets sequence counters after alerts trigger

### Helper Functions
- **Price Level Checks**: Functions to determine if price is in/above/below value areas
- **Crossing Detection**: Identifies when price crosses key levels
- **Dynamic Level Adjustment**: Modifies alert levels based on optimistic setting

### Color Scheme
- **DVA Colors**: Blue tones for developing value area
- **PVA Colors**: Beige/yellow tones for previous value area  
- **Band Colors**: Pink and cyan for positive/negative bands
- **Transparency**: Graduated transparency for visual hierarchy

## Use Cases

### Rotational Trading
- Fade moves at value area extremes
- Test and rejection setups at key levels

### Directional Trading  
- Failed auction patterns for continuation trades
- Imbalanced moves with pullback entries

### Previous Value Analysis
- Support/resistance from prior periods
- Breakout and pullback strategies

## Configuration Options

### Display Settings
- Hide/show individual components
- Choose between line and fill displays
- Adjust colors and transparency

### Alert Settings
- Enable/disable optimistic alerting
- Show/hide visual trigger markers
- Individual alert on/off controls

