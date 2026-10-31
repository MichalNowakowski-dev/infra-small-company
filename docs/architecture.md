```mermaid
graph TD
  NET((Internet / NAT)) --> FW[fw01 pfSense]
  FW --- SRV[SERVERS 10.10.10.0/24]
  FW --- OFF[OFFICE 10.10.20.0/24]
  FW --- MGT[MGMT 10.10.99.0/24]
  MGT --- CTRL[ctrl01]
```
