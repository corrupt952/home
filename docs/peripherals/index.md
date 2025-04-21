# Peripheral Device Management

## Home Office

This desk setup handles both remote work and personal activities.  
Work and personal peripherals are kept separate to avoid interference.

For network separation details, see [Network Devices](/docs/networks/index.md).

```mermaid
---
title: Audio Input
---
graph LR
    PC[Main PC]
    B1[Company B PC]
    AI[Audio Interface]
    MIC[Microphone]
    WE[Wireless Earbuds]

    %% Audio Input Connections
    MIC -- "XLR" --> AI
    AI -- "USB" --> PC
    WE -. "Bluetooth" .-> B1
```

```mermaid
---
title: Audio Output
---
graph LR
    PC[Main PC]
    B1[Company B PC]
    AI[Audio Interface]
    SPK[Soundbar]
    HP[Headphones]
    WE[Wireless Earbuds]

    %% Audio Output Connections
    PC -- "USB" --> AI
    AI -- "3.5mm" --> SPK
    AI -- "3.5mm" --> HP
    B1 -. "Bluetooth" .-> WE
```

```mermaid
---
title: Video Output
---
graph LR
    PC[Main PC]
    A1[Company A PC 1]
    A2[Company A PC 2]
    B1[Company B PC]
    D1[Display 1]
    D2[Display 2]
    HUB[Thunderbolt Hub]

    %% Display Connections
    PC -- "HDMI/DP" --> D1
    PC -- "HDMI/DP" --> D2
    HUB -- "HDMI/DP" --> D1
    HUB -- "HDMI/DP" --> D2
    A1 -- "TB4" --> HUB
    A2 -. "Switch TB4" .-> HUB
    B1 -. "Switch TB4" .-> HUB
```

### Home Office Devices

- Computers
  - Private Main PC (for gaming and heavy processing)
  - Mac mini (Company A)
  - Windows Laptop PC (with GPU, for Company A game development and debugging)
  - MacBook Pro (Company B)
- Audio Equipment
  - Audio Interface/Mixer (AG06 Mk2)
  - Microphone (Shure, input to AG06)
  - Soundbar (output from AG06)
  - Headphones (output from AG06)
  - Wireless Earbuds (for calls on Company B Mac)
- Video Equipment
  - WQHD Displays x2
- Hubs
  - Thunderbolt 4 Hub (TS4, for switching video outputs between Company B and Company A)

## Mobile Setup

This setup covers co-working spaces and office visits.  
It's designed to be portable while maintaining essential work functionality.

```mermaid
graph LR
```

### Mobile Setup Devices

-
