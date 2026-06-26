# Contributing to Kietu

## The one-minute version

1. Create `games/yourgame/` with a `Cargo.toml` and `src/lib.rs`
2. Implement the `Game` trait (3 methods)
3. Add it to `kietu-cli/Cargo.toml` and register it in `kietu-cli/src/main.rs`
4. `cargo build --release` — it appears in `kietu list`

See the [Adding a game](README.md#adding-a-game) section of the README for the full walkthrough with code.

---

## The Game trait

```rust
pub trait Game: Send + Sync {
    fn info(&self) -> &'static GameInfo;
    fn default_settings(&self) -> GameSettings { HashMap::new() }
    fn launch(&self, mode: PlayerMode, settings: GameSettings) -> Result<()>;
}
```

`info()` returns compile-time metadata (slug, name, description, supported modes).
`default_settings()` returns any configurable values (engine path, depth, etc.).
`launch()` opens a window and runs the game loop.

---

## EvE is the default

`kietu play <game>` defaults to `--mode eve`. This means when someone first runs your game they see the AI playing itself. Make sure this works and looks interesting — it's the first impression.

In EvE mode:
- No human input should be required (don't block waiting for keyboard/mouse)
- Both sides should move at a pace that's watchable (~300–500ms per move for turn-based games)
- Auto-restart when the game ends
- Puzzle games: auto-solve step by step, pause, then cycle to the next puzzle

---

## PlayerMode conventions

| Mode | Your game should... |
|------|---------------------|
| `EngineVsEngine` | AI plays both sides automatically. Auto-restart. No blocking on input. |
| `HumanVsEngine` | Human plays one side (usually the first mover), engine plays the other. |
| `HumanVsHuman` | Both sides controlled by keyboard/mouse. |
| `Solo` | Single-player. Human controls everything (arcade, puzzle). |

Only declare the modes your game actually supports in `GameInfo.modes`. The first mode listed is treated as the default.

---

## Using an external engine

```rust
use kietu_core::engine::EngineProcess;

let mut engine = EngineProcess::spawn("/opt/kietu-engines/myengine", &[])?;
engine.send("ucinewgame")?;                         // send a command
let line  = engine.read_line()?;                    // read one response line
let lines = engine.read_until("readyok")?;          // read until a token
let reply = engine.send_recv("isready")?;           // send + read one line
// engine is killed automatically when dropped
```

External engine binaries live in `/opt/kietu-engines/`. If your engine needs to be downloaded, add it to the `engine_list()` function in `kietu-cli/src/main.rs` so `kietu setup` installs it automatically.

---

## SDL2 rendering

Games use SDL2 directly — there's no mandatory rendering abstraction. The pattern every game follows:

```rust
let sdl   = sdl2::init()?;
let video = sdl.video()?;
let win   = video.window("Kietu -- MyGame", W, H).position_centered().build()?;
let mut canvas = win.into_canvas().present_vsync().build()?;
let mut events = sdl.event_pump()?;

'main: loop {
    for ev in events.poll_iter() {
        match ev {
            Event::Quit { .. } | Event::KeyDown { keycode: Some(Keycode::Escape), .. } => break 'main,
            Event::KeyDown { keycode: Some(Keycode::N), .. } => { /* new game */ }
            _ => {}
        }
    }
    // update state
    canvas.set_draw_color(Sdl::RGB(0,0,0)); canvas.clear();
    // draw
    canvas.present();
    std::thread::sleep(Duration::from_millis(16)); // ~60fps
}
```

`kietu-gui` has shared helpers (fonts, circle drawing) but you're not required to use them.

---

## Settings

If your game has configurable parameters, expose them through `default_settings()`:

```rust
fn default_settings(&self) -> GameSettings {
    let mut s = HashMap::new();
    s.insert("engine_path".into(), SettingValue::Str("/opt/kietu-engines/myengine".into()));
    s.insert("depth".into(),       SettingValue::Int(10));
    s.insert("sound".into(),       SettingValue::Bool(false));
    s
}
```

Read them back in `launch()`:

```rust
let path = match settings.get("engine_path") {
    Some(SettingValue::Str(p)) => p.clone(),
    _ => "/opt/kietu-engines/myengine".into(),
};
```

Users set them with `kietu config mygame depth 15`. They're saved to `~/.config/kietu/config.toml`.

---

## Checklist before opening a PR

- [ ] `cargo build --release` succeeds with no new warnings
- [ ] `kietu play <slug>` works in EvE mode (default)
- [ ] `kietu play <slug> --mode solo` or `--mode hve` works for the modes you declared
- [ ] `[N]` starts a new game, `[Esc]` quits
- [ ] If the game uses an external engine, it's added to `engine_list()` in `kietu-cli/src/main.rs`
- [ ] `GameInfo.description` fits on one line in `kietu list` (under ~60 chars before truncation)
