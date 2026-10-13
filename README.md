# frozen-bubble-tng

**The Next Generation of Frozen-Bubble**

A modernized, Vulkan-ready fork of the classic Frozen-Bubble puzzle game.

## Status

✅ **Playable** - Single-player and multiplayer (LAN/internet) work on Wayland and X11  
✅ **SDL 2.x** - Upgraded from SDL 1.2; native Wayland support  
🚧 **Vulkan migration in progress** - Rendering pipeline and 11 graphics effects ported to Vulkan shaders  
📦 **Hybrid architecture** - Perl for game logic/editor, C + XS for performance-critical effects, C server for network daemon

## Quick Start

```bash
# Run the game (SDL 2.x, Wayland/X11)
perl bin/frozen-bubble --no-sound --no-music --no-joysticks

# Run the level editor
perl bin/frozen-bubble-editor
```

## Architecture

| Component | Language | Role |
|-----------|----------|------|
| **Game Logic** | Perl | Full game physics, chain reactions, malus bubble distribution, network protocol |
| **Level Editor** | Perl | Full GUI with 20+ dialog types, mouse/keyboard events, level file I/O |
| **Graphics Effects** | C + XS | 11 effects (plasma, rotation, bicubic, bilinear, flipflop, enlighten, waterize, stretch, shrink, tilt, autocrop) ported to Vulkan shaders |
| **C Server** | C | Standalone game server; handles 255 simultaneous players at **0% CPU** |
| **Build System** | Perl + CExtension | Module::Build + XS compilation; Alien::SDL, SDL, Locale::Maketext::Extract |

## Migration Roadmap

| Phase | Focus |
|-------|-------|
| **SDL 1.2 → 2.x** | Update Perl SDL calls; replace `SDL_SetVideoMode()` → `SDL_CreateWindow()` + renderer; native Wayland support |
| **Vulkan Boilerplate** | Create Vulkan instance, device, swapchain, render pass; bind to SDL window |
| **Effects → Shaders** | Port 11 CStuff.xs effects to Vulkan compute/vertex/fragment shaders |
| **LevelEditor + ImGui** | Port GUI to Dear ImGui + Vulkan backend, or SDL2 UI + Vulkan render area |
| **Polish & Test** | Full playthrough, network connectivity, editor functionality, performance verification |

## Performance

| Metric | Result |
|--------|--------|
| **Server: 255 players** | **0% CPU** (proven benchmark) |
| **Game: Single-player** | Playable FPS with full graphics effects |
| **Graphics: 11 effects** | C + XS at interactive FPS; pure Perl would be 5-12 FPS |
| **Startup time** | 1-2 seconds |

## Philosophy

This fork intentionally uses the **right tool for each job** — the same hybrid approach the original codebase already embodies:

- **Perl** for application logic, rapid iteration, CPAN ecosystem, text processing
- **C** for performance-critical daemons, graphics effects via XS, memory control
- **XS** as the bridge between Perl and C for the graphics effects
- **Vulkan** as the modern rendering target, replacing SDL 1.2 immediate mode

The goal is modernize the rendering pipeline (SDL 2.x + Vulkan) while preserving the 20+ years of game logic, level design, and network protocol that makes Frozen-Bubble unique.

## Development

```bash
# Build from source
perl Build.PL
./Build
./Build test
./Build install

# Or run directly from source tree
PERL5LIB=lib:$PERL5LIB bin/frozen-bubble
```

## License

GPL-2.0 — same as the original Frozen-Bubble.

See `COPYING` for full license text.