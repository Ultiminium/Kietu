# Kietu

A command-line launcher for 38 arcade, strategy, and puzzle games — each backed by a superhuman AI engine. The engine plays **for** you by default (EvE mode), so you watch the best possible play. Switch to solo or human-vs-engine when you want to play yourself.

```
kietu list               # browse all 38 games
kietu play chess         # engine vs engine (default)
kietu play chess --mode hve   # you play white, Stockfish plays black
kietu play tetris        # Cold Clear AI plays Tetris
kietu play slither-io    # 24 AI snakes in an arena
kietu setup              # install all engines
```

---

## Installation

### Prerequisites

SDL2 (rendering) and a font:

```bash
# Debian / Ubuntu
sudo apt-get install libsdl2-2.0-0 libsdl2-ttf-2.0-0 fonts-dejavu-core

# Fedora
sudo dnf install SDL2 SDL2_ttf dejavu-sans-fonts

# Arch
sudo pacman -S sdl2 sdl2_ttf ttf-dejavu

# macOS
brew install sdl2 sdl2_ttf
```

### Download a release binary

Grab `kietu` from the [Releases](../../releases) page, make it executable, and run:

```bash
chmod +x kietu
./kietu list
```

Or use the bundled tarball which includes SDL2 libs and a wrapper script so it works without a system SDL2:

```bash
tar xzf kietu-linux.tar.gz
./kietu-run.sh list
```

### Build from source

Requires Rust 1.75+.

```bash
git clone https://github.com/yourname/kietu
cd kietu
cargo build --release
./target/release/kietu list
```

### Install engines

Most games have a built-in AI. Games marked `*` in the list use an external engine binary (Stockfish, KataGo, etc.). Install them all at once:

```bash
kietu setup
```

The wizard detects your OS and CPU, downloads the right variant of each engine into `/opt/kietu-engines/`, and installs SDL2 if missing. You can also install engines manually — see [Engines](#engines) below.

---

## Playing

```
kietu list                          list all games with descriptions
kietu play <slug>                   launch (default: EvE — engine watches itself)
kietu play <slug> --mode eve        engine vs engine (default)
kietu play <slug> --mode hve        human vs engine
kietu play <slug> --mode hvh        human vs human
kietu play <slug> --mode solo       single-player / puzzle mode
kietu info <slug>                   show engine info and available modes
kietu setup                         download and install all engines
```

Every game window responds to `[Esc]` to quit and `[N]` to start a new game.

### Modes explained

| Mode | What happens |
|------|-------------|
| `eve` | Engine plays both sides. You watch. **This is the default.** |
| `hve` | You play one side, the engine plays the other. |
| `hvh` | Two humans share the keyboard/mouse. |
| `solo` | Single-player puzzle or arcade mode — you control everything. |

Not every mode is available for every game. `kietu info <slug>` shows which modes a game supports.

---

## Games

| Slug | Name | Engine | Category |
|------|------|--------|----------|
| `chess` | Chess | Stockfish 17 (~3600 ELO) | Strategy |
| `go` | Go | KataGo | Strategy |
| `shogi` | Shogi | YaneuraOu | Strategy |
| `xiangqi` | Xiangqi | Pikafish | Strategy |
| `checkers` | Checkers | Custom engine | Strategy |
| `othello` | Othello | Edax | Strategy |
| `connect4` | Connect Four | Pascal Pons perfect solver | Strategy |
| `backgammon` | Backgammon | GNU Backgammon | Strategy |
| `mancala` | Mancala | Minimax depth-14 | Strategy |
| `nine-mens-morris` | Nine Men's Morris | Minimax | Strategy |
| `hex` | Hex | MoHex 2.0 | Strategy |
| `havannah` | Havannah | Castro MCTS | Strategy |
| `amazons` | Game of Amazons | MCTS | Strategy |
| `lines-of-action` | Lines of Action | Connectivity AI | Strategy |
| `gomoku` | Gomoku | Rapfi NNUE | Tic-Tac-Toe |
| `tictactoe` | Tic-Tac-Toe | Perfect minimax | Tic-Tac-Toe |
| `ultimate-ttt` | Ultimate Tic-Tac-Toe | MCTS | Tic-Tac-Toe |
| `tetris` | Tetris | Cold Clear (FFI) | Puzzle |
| `2048` | 2048 | Expectimax | Puzzle |
| `sudoku` | Sudoku | Dancing Links | Puzzle |
| `minesweeper` | Minesweeper | CSP solver | Puzzle |
| `fifteen-puzzle` | 15 Puzzle | A* solver | Puzzle |
| `nonogram` | Nonogram | Constraint propagation | Puzzle |
| `kakuro` | Kakuro | Backtracking | Puzzle |
| `nurikabe` | Nurikabe | Backtracking | Puzzle |
| `slitherlink` | Slitherlink | Backtracking | Puzzle |
| `lights-out` | Lights Out | GF(2) linear algebra | Puzzle |
| `sokoban` | Sokoban | Push solver | Puzzle |
| `pacman` | Pac-Man | BFS ghost + pac AI | Arcade |
| `tetris` | Tetris | Cold Clear | Arcade |
| `snake` | Snake | BFS pathfinding | Arcade |
| `slither-io` | Slither.io | 24 strategy AIs + boost | Arcade |
| `flappy-bird` | Flappy Bird | Predictive physics AI | Arcade |
| `solitaire` | Klondike Solitaire | Greedy + auto-foundation | Card |
| `freecell` | FreeCell | Auto-foundation | Card |
| `mahjong-solitaire` | Mahjong Solitaire | Hint solver | Card |
| `yahtzee` | Yahtzee | Optimal DP AI | Card |
| `garut` | Garut | Custom engine | Custom |

---

## Engines

External engines live in `/opt/kietu-engines/`. `kietu setup` installs them automatically. To install manually:

| Engine | Game | Download |
|--------|------|----------|
| Stockfish | Chess | [stockfishchess.org](https://stockfishchess.org/download/) |
| KataGo | Go | [github.com/lightvector/KataGo](https://github.com/lightvector/KataGo/releases) |
| YaneuraOu | Shogi | [github.com/yaneurao/YaneuraOu](https://github.com/yaneurao/YaneuraOu/releases) |
| Pikafish | Xiangqi | [github.com/official-pikafish/Pikafish](https://github.com/official-pikafish/Pikafish/releases) |
| Rapfi | Gomoku | [github.com/dhbloo/rapfi](https://github.com/dhbloo/rapfi/releases) |
| Edax | Othello | [github.com/abulmo/edax-reversi](https://github.com/abulmo/edax-reversi/releases) |
| MoHex | Hex | [github.com/cgao3/benzene-vanilla-cmake](https://github.com/cgao3/benzene-vanilla-cmake) |
| GNU Backgammon | Backgammon | `sudo apt-get install gnubg` |
| Cold Clear | Tetris | [github.com/MinusKelvin/cold-clear](https://github.com/MinusKelvin/cold-clear) — place `libcold_clear.so` in `/opt/kietu-engines/` |

**Stockfish CPU variants:** Stockfish ships multiple builds. `kietu setup` reads `/proc/cpuinfo` and picks the right one (AVX2 for modern CPUs, SSE4.1+popcnt for older, universal baseline for everything else). If you get "Exec format error", run `kietu setup` to replace the binary with the correct variant.

---

## Adding a game

Adding a game is intentionally simple. The whole framework is a single `Game` trait.

### 1. Create the crate

```bash
mkdir games/mygame
```

`games/mygame/Cargo.toml`:
```toml
[package]
name    = "mygame"
version = "0.1.0"
edition = "2021"

[dependencies]
kietu-core = { workspace = true }
anyhow     = { workspace = true }
sdl2       = { version = "0.37", features = ["ttf"] }
```

### 2. Implement the Game trait

`games/mygame/src/lib.rs`:
```rust
use anyhow::Result;
use kietu_core::traits::*;
use std::collections::HashMap;

pub struct MyGame;
impl MyGame { pub fn new() -> Self { Self } }
impl Default for MyGame { fn default() -> Self { Self::new() } }

static INFO: GameInfo = GameInfo {
    slug:              "mygame",
    name:              "My Game",
    description:       "One-line description shown in kietu list",
    category:          GameCategory::StrategyAbstract,  // or PuzzleSolo, ArcadeAction, etc.
    modes:             &[PlayerMode::EngineVsEngine, PlayerMode::Solo],
    superhuman_engine: false,
};

impl Game for MyGame {
    fn info(&self) -> &'static GameInfo { &INFO }
    fn default_settings(&self) -> GameSettings { HashMap::new() }

    fn launch(&self, mode: PlayerMode, _settings: GameSettings) -> Result<()> {
        let eve = matches!(mode, PlayerMode::EngineVsEngine);
        // open an SDL2 window, run your game loop
        // in EvE: engine drives everything, no human input needed
        // in Solo/HvE: handle keyboard/mouse events
        Ok(())
    }
}
```

**EvE convention:** In `EngineVsEngine` mode the engine should play automatically with no user input required. The game runs as a demo — both sides played by AI, auto-restarting when done. Human input is disabled. This is the default mode, so it's what people see first.

### 3. Register it

In `kietu-cli/Cargo.toml`, add:
```toml
mygame = { path = "../games/mygame" }
```

In `kietu-cli/src/main.rs`, inside `build_registry()`:
```rust
reg.register(Box::new(mygame::MyGame::new()));
```

That's it. `cargo build --release` and your game appears in `kietu list`.

### Adding configurable settings

```rust
fn default_settings(&self) -> GameSettings {
    let mut s = HashMap::new();
    s.insert("depth".into(), SettingValue::Int(8));
    s.insert("engine_path".into(), SettingValue::Str("/opt/kietu-engines/myengine".into()));
    s
}
```

Users can override these with `kietu config mygame depth 12`. Settings are persisted to `~/.config/kietu/config.toml`.

### Using an external engine

```rust
use kietu_core::engine::EngineProcess;

// In launch():
let mut engine = EngineProcess::spawn("/opt/kietu-engines/myengine", &[])?;
engine.send("newgame")?;
let response = engine.read_line()?;
// or: let lines = engine.read_until("ready")?;
// or: let reply = engine.send_recv("go")?;
// Engine is killed automatically when dropped
```

`EngineProcess` handles stdin/stdout piping, line buffering, and cleanup. It works with any engine that communicates over stdio — UCI, GTP, USI, or a custom protocol.

---

## Project structure

```
kietu/
├── kietu-cli/          # CLI: list, play, info, config, setup commands
├── kietu-core/         # Game trait, Registry, EngineProcess, KietuConfig
├── kietu-gui/          # Shared SDL2 helpers (optional — games can use SDL2 directly)
└── games/
    ├── chess/          # One directory per game
    ├── go/
    ├── tetris/
    └── ...             # 38 total
```

Each game is a standalone Rust crate. It depends only on `kietu-core` (for the trait) and whatever rendering/engine libs it needs. Games don't depend on each other.

---

## Contributing

Pull requests welcome. A few guidelines:

**New games** — implement `Game`, follow the EvE convention, keep it to one crate in `games/`. If you want to use an existing open-source engine, add it to the `kietu setup` engine list in `kietu-cli/src/main.rs`.

**Bug fixes** — check the issue tracker. Common areas: engine coordinate translation (checkers, xiangqi), ghost AI (pacman), and collision detection (slither-io).

**New categories** — add a variant to `GameCategory` in `kietu-core/src/traits.rs` and a section header in `cmd_list()` in `kietu-cli/src/main.rs`.

**Engines** — if you know of a better free engine for any existing game, open a PR updating the engine path in that game's `default_settings()` and the download URL in `cmd_setup()`.

The fastest way to add a game: paste this README and the `Game` trait definition into an AI assistant, describe the game you want, and ask it to write the crate. The output drops straight in.

---

## License

Custom

## Disclaimer

This code is made entirely by a generative AI. I do not claim to have made any of this code.
