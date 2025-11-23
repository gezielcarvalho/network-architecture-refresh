# Lucidchart Instructions: Net Refresh – Phase 00

Follow these steps to create the initial network design diagram in Lucidchart:

## 1. Create a New Document

- Title: **Net Refresh – Phase 00**

## 2. Add Components

### Network Segmentation Foundation

- Insert a large container labeled **App Stack**.
- Within the App Stack, create two smaller containers representing network segments:
  - **front_net** (left side)
  - **back_net** (right side)

### Services

- Add the **API** node spanning both networks (positioned at the boundary or with connections to both)
- Add an external shape labeled **Client** outside the App Stack container
- Draw an arrow from **Client** to **API**
  - Label the arrow: **HTTP 8080**

### Network Mockup Design

```
┌─────────────────────────────────────┐
│           App Stack                 │
│  ┌─────────────┐  ┌───────────────┐ │
│  │ front_net   │  │   back_net    │ │
│  │             │  │               │ │
│  │    API ◄────┼──┤   (prepared   │ │
│  │             │  │   for future) │ │
│  └─────────────┘  └───────────────┘ │
└─────────────────────────────────────┘
          ▲
     HTTP 8080
          │
    ┌─────────┐
    │ Client  │
    └─────────┘
```

## 3. Styling

- Use the following color scheme:
  - **Green**: Stateless components (API)
  - **Blue**: Stateful components
  - **Orange**: Infrastructure components
  - **Light Gray**: Network boundaries/containers
- Set the **API** node color to green
- Use dashed lines or different shading to distinguish network boundaries
- Add text labels for each network segment

## 4. Network Annotations

- Add callout notes:
  - **front_net**: "UI ↔ API (future phases)"
  - **back_net**: "API ↔ workers/queues/db/iseries/ftp (future phases)"
- This establishes the architectural foundation for all subsequent phases

## 5. Export

- When finished, export the diagram as a PNG file.
- Save the PNG in the `docs/lucid/` folder of this repository as `Net_Refresh_Phase_00.png`

---

**Why Include Network Segmentation in Phase 00?**

- Shows architectural intent from the beginning
- Demonstrates security-first thinking with network isolation
- Provides visual foundation for adding services in specific networks during future phases
- Makes the API's role as orchestrator/bridge between networks clear

Once the diagram is exported and saved, you can proceed to the next step in phase 00.
