# Project Overview – concise version  

## 📌 Restrictions & Mandatory Stack  
- **Unity 6**  
- **Clean Architecture** – four folders (each an Assembly Definition)  
  - `Domain/` – pure models (`IBuildingModel`, `IEntity`)  
  - `Application/` – use‑cases, business logic  
  - `Presentation/` – UI Toolkit views + presenters  
  - `Infrastructure/` – Input System, DI (VContainer), MessagePipe, adapters, persistence  

- **UI Toolkit** – declarative layout.  
- **DI:** VContainer.  
- **Event bus:** MessagePipe.  
- **Reactive streams:** UniRx (R3).  
- **Async / timers:** UniTask (`UniTask.Delay(TimeSpan)`) + R3 `Timer`.  
- **Input System** – WASD, right‑drag/zoom, hotkeys (`1/2/3`, `R`, `Del`).  
- **Editor helpers:** TriInspector where needed.  
- **No ECS:** use plain `MonoBehaviour` / `ScriptableObject`.  

*Code style guidelines omitted – follow team convention.*

---

## 🚀 Functional Requirements

| Feature | Key details |
|---|---|
| **Map & Grid** | Flat scene with a cell grid; cursor highlights cells—green if placeable, red otherwise. |
| **Building Catalog** | Icons / buttons in UI Toolkit panel for selecting building prefabs. |
| **Placement / Move / Delete** | User clicks/uses hotkeys → Presenter sends DTO to MessagePipe → Application Use Case handles the action. No direct View interaction. |
| **Upgrades** | Minimum 2 upgrade levels; cost is checked against Gold. Insufficient gold triggers UI tooltip via event. |
| **Resources & Income** | Gold currency, passive income generated every N seconds (or each tick) using `UniTask.Delay + R3.Timer`. Implemented without extra allocations. |
| **Camera Controls** | WASD + right‑drag to pan; mouse wheel zoom. Hotkeys: `1/2/3` → select prefab, `R` rotate, `Del` delete. All via Input System. |
| **Persistence** | Save/load state (positions, types, levels) as JSON in PlayerPrefs or a file. Auto‑save every X seconds (`UniTask`). Manual buttons in UI for “Save/Load”. |
| **UI Toolkit Panels** | • Building selector (icons/buttons) <br>• Resource & notification area <br>• Selected building info panel (level, Upgrade / Move / Delete buttons) |
| **Event Bus** | MessagePipe events: `BuildingBuilt`, `Deleted`, `Upgraded`, `InsufficientGold`, `Saved/Loaded`. Use Cases receive DTO messages. |

---

## 🎯 Architecture Note  

- **All business logic resides in the **Application** layer**.  
- The **View** only renders state; it never performs operations.  
- The **Presenter** translates user actions into DTOs and publishes them on MessagePipe, which triggers the appropriate Application use‑case.  

This separation guarantees testable domain code, clear responsibilities, and a clean, maintainable project structure.
