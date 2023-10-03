<!-- Game Engine Documentation -->

# Game Engine

## Structure

Old Diagram
``` mermaid
stateDiagram-v2
    direction LR
    state Game {
        direction LR
        [*] --> GameEngine
        state GameEngine {
            direction LR
            [*] --> Registry
            [*] --> Network
            state Registry {
                direction LR
                Entity --> System: is passed to
                state Entity {
                    [Components...]
                }
                state System {
                    void(Registry&,sparseArray<>...)
                }
            }

            state Network {
                []
            }
        }
    }
```

Real Diagram
``` mermaid
stateDiagram-v2
    state Game {
        direction LR
        state GameLogic {
            direction LR
            player
            ennemy
            ...
        }
        state GameEngine {
            state ECS {
                state Entity {
                    direction LR
                    [Components...]
                }
                Entity --> System
            }
            state Network {
                direction LR
                NotDone
            }
            state Event {
                state TODO {
                    direction LR
                    onKeyPress
                    onKeyReleased
                    isKeyPressed
                }
            }
            state Render {
                direction LR
                state SFML {
                    direction LR
                    2D
                    3D
                }
            }
        }
    }
```