# GanttChart

# ✈️ Gantt Chart Backend Infrastructure

This project showcases the backend architecture and UI integration for an airline **Gate Assignment and Gantt Visualization System**. It was built to optimize airport gate utilization and aircraft movement efficiency.

## 🧠 Key Features

### ✅ Backend Infrastructure

- **Gate Environment Initialization**:
  - Reads datasets such as PhysicalGate, StaffedGate, GateSetup, WorkDurationArrival/Departure, etc.
  - Uses smart fallback behavior when certain aircraft or subfleet information is missing

- **Gate Model Construction**:
  - Builds a tree structure of gate availability per airport and date
  - Incorporates physical gates, staffed gates, and remote parking options

- **Assignment Solver Logic**:
  - Assigns schedule turns to available gates per airport/date
  - Includes towing logic for RON and midday scenarios
  - Generates final assignment output for reference and visualization

---

### 🎨 Gantt Chart UI

- Reuses customized Route View controls instead of Telerik RadGanttView
- Handles rendering and layout challenges for overlapping gate schedules
- Plans future improvements like rendering freeze-on-idle and SharpDX refresh

---

## 🗺️ Architecture Diagram
                        ┌────────────────────┐
                        │   Input Datasets   │
                        │────────────────────│
                        │ - PhysicalGate     │
                        │ - StaffedGate      │
                        │ - GateSetup        │
                        │ - WorkDuration*    │
                        │ - RemoteParking    │
                        └────────┬───────────┘
                                 │
                                 ▼
                    ┌────────────────────────────┐
                    │ Gate Environment Builder   │
                    │────────────────────────────│
                    │ Builds availability trees  │
                    │ per airport & date         │
                    └────────┬───────────┬───────┘
                             │           │
              ┌─────────────▼──┐      ┌──▼──────────────┐
              │ Gate Model Tree│      │ Towing Logic    │
              │ (Physical,     │      │ (RON, Midday)   │
              │ Staffed, RP)   │      │ Towing strategy │
              └────────┬───────┘      └────┬────────────┘
                       │                   │
                       ▼                   ▼
                ┌────────────────────────────────────┐
                │ Schedule Turn Assignment Engine    │
                │────────────────────────────────────│
                │ - Creates assignment nodes         │
                │ - Resolves conflicts               │
                │ - Handles unassigned turns         │
                └─────────────────┬──────────────────┘
                                  │
                                  ▼
                   ┌────────────────────────────┐
                   │ Final Solution Generator   │
                   │────────────────────────────│
                   │ Outputs assignments for     │
                   │ visualization & analytics   │
                   └────────────┬───────────────┘
                                │
                                ▼
               ┌──────────────────────────────────┐
               │     Gantt Chart UI Integration   │
               │  (via RouteView-based controls)  │
               └──────────────────────────────────┘

