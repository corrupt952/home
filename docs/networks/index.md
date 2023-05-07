# Network devices

## Architecture

I have set up a home network using VLANs to separate personal and business subnets.  

```mermaid
graph LR

%% Definitions
WAN
subgraph Home
    ONU
    Router
    subgraph Personal["Personal Subnet(192.168.x.0/24)"]
        Smartphone
        Desktop
    end
    subgraph Business["Business Subnet(192.168.y.0/24)"]
        Laptop
        BSmartphone["Business phone"]
    end
end

%% Relations
WAN --> ONU
ONU --> Router
%% Personal
Router -->|"🏠 Personal Traffic"| Personal
%% Business
Router -->|"🏢 Business Trafic"| Business
```

### Communication between subnets

Although not shown in the figure, communication is not allowed between each subnet.

```mermaid
graph TB

%% Definitions
subgraph Personal["Personal Subnet(192.168.x.0/24)"]
    Smartphone
    Desktop
end
subgraph Business["Business Subnet(192.168.y.0/24)"]
    Laptop
    BSmartphone["Business phone"]
end

%% Relations
Personal x--x|"❌ Unable to commmunicate"| Business
```

### Wireless router

The wireless router uses tag VLANs so that SSIDs 1 and 2 are assigned to the Personal subnet and SSID3 is assigned to the Business subnet.

```mermaid
graph LR

%% Definitions
subgraph Personal["Personal Subnet(192.168.x.0/24)"]
    SSID1(("SSID1: 2.5Hz"))
    SSID2(("SSID2: 5GHz"))
end
subgraph Business["Business Subnet(192.168.y.0/24)"]
    SSID3(("SSID3: 5GHz"))
end

%% Relations
Personal ~~~ Business
```

## Setup

[Set up network devices](/docs/networks/setup.md)
