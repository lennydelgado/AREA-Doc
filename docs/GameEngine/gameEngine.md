<!-- Game Engine Documentation -->

# Game Engine

## Introduction

<!-- Introduction pour le game engine en anglais -->
The Game Engine is the main part of the project, it is the part that manages the game loop, the entities, the systems, the events, the rendering and the network.

## Goal

<!-- Objectif du game engine pour le projet en anglais -->
The goal of the game engine is to provide a simple and efficient way to create a game with the ECS architecture it provides and to be able to use it in a networked environment.

## Architectural View

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
            direction LR
            state Core {
                state ECS {
                    state Entity {
                        direction LR
                        [Components...]
                    }
                    Entity --> System
                }
                state Network {
                    direction LR
                    ...
                }
                state Event {
                    direction LR
                    KeyboardEvent
                    MouseEvent
                    WindowEvent
                }
                state Render {
                    direction LR
                    state SFML {
                        direction LR
                        2D
                    }
                }
                state SpatialAudio {
                    direction LR
                    state SFMLAudio {
                        direction LR
                        Audio2D
                        Audio3D
                    }
                }
            }
            state Module {
                state TimeManager {
                    direction LR
                    FunctionWithTimeOut
                    FunctionOnTick
                }
                state LuaManager {
                    direction LR
                    LoadLuaScripts
                    AddC++FunctionToLua
                    RunLuaFunction
                }
                state JsonManager {
                    direction LR
                    LoadJsonFile
                    GetInformationFromJson
                }
                state AssetManager {
                    direction LR
                    AddAsset
                    GetAsset
                }
            }
        }
    }
```

<!--
## Main systems

### Rendering

Expliquez le système de rendu et comment il fonctionne.

### Event

Expliquez le système d'événement et comment il fonctionne.

### ECS

Expliquez le système ECS et comment il fonctionne.

### Network

Expliquez le système de réseau et comment il fonctionne.
-->

## First steps

To use the game engine you need to have the following dependencies:

- [Conan](https://conan.io/) - Package Manager
- [CMake](https://cmake.org/) - Compiler

Follow the installation instructions on the [Installation page](https://lennydelgado.github.io/R-Type-Doc/) and you should have the game engine compiled as a library in the `build/GameEngine` folder.

## Tutorial

In this section we will explain how to use the game engine to create a simple game.

### Include required files

To use the game engine you need to include the following files:

``` cpp
#include "gameEngine.hpp"
```

### Create your own components

The game engine is based on the ECS architecture, so you need to create your own components to use it or use the ones already created.
For the sake of the example we will create our own components.

To create a component you just need to create a struct with the properties you want to have in your component.

``` cpp
struct Position {
    float x;
    float y;
};

struct Velocity {
    float x;
    float y;
};

struct Sprite {
    gameEngine::Sprite sprite;

    Sprite() {
        sprite.setTexture(gameEngine::Texture());
    }

    setPosition(float x, float y) {
        sprite.setPosition(x, y);
    }
};
```

### Create your own systems

The game engine is based on the ECS architecture, so you need to create your own systems to use it or use the ones already created.
For the sake of the example we will create our own systems.

To create a system you just need to create a function that takes a `GameEngine &` as a parameter, a list of components you want to use, some extra parameters if you need them and return void.

``` cpp
void move(GameEngine &engine, SparseArray<Position> &positions, SparseArray<Velocity> &velocities) {
    for (auto &pos : positions) {
        if (velocities.has(pos.first)) {
            pos.second.x += velocities[pos.first].x;
            pos.second.y += velocities[pos.first].y;
        }
    }
}

void render(GameEngine &engine, SparseArray<Position> &positions, SparseArray<Sprite> &sprites, gameEngine::RenderWindow &window) {
    for (auto &pos : positions) {
        if (sprites.has(pos.first)) {
            sprites[pos.first].setPosition(pos.second.x, pos.second.y);
            window.draw(sprites[pos.first].sprite);
        }
    }
}
```

### Create your own game

To create your own game you first need to register your components and systems to the game engine.

``` cpp
GameEngine engine;
gameEngine::RenderWindow window(gameEngine::VideoMode(1280, 720), "GameEngine window");

engine.ecsRegisterComponent<Position>();
engine.ecsRegisterComponent<Velocity>();

engine.ecsAddSystem<Position, Velocity>(move);
engine.ecsAddSystem<Position, Sprite>(render, window);
```

Then you need to create your entities and add them to the game engine.

``` cpp
Entity entity = engine.ecsSpawnEntity();

Position pos{0, 0};
engine.ecsAddComponent(entity, pos);

Velocity vel{1, 1};
engine.ecsAddComponent(entity, vel);
```

Now that we have setup our entity we can start the game loop.

``` cpp
while (window.isOpen()) {
    gameEngine::Event event;
    while (window.pollEvent(event)) {
        if (event.type == gameEngine::Event::Closed) {
            window.close();
        }
    }

    window.clear();

    engine.ecsRunSystems();

    window.display();
}
```

But maybe you want to execute some code based on a tick rate, you can do that by using the `setInterval` method.

``` cpp
engine.timeSetInterval([]() {
    std::cout << "Hello World every 1s" << std::endl;
}, 1000); // 1000ms = 1s
```

Or maybe you want to execute some code only once but after a certain amount of time, you can do that by using the `setTimeout` method.

``` cpp
engine.timeSetTimeout([]() {
    std::cout << "Hello World after 10s" << std::endl;
}, 10000); // 10000ms = 10s
```

### Full example

``` cpp
#include "gameEngine.hpp"

struct Position {
    float x;
    float y;
};

struct Velocity {
    float x;
    float y;
};

struct Sprite {
    gameEngine::Sprite sprite;

    Sprite() {
        sprite.setTexture(gameEngine::Texture());
    }

    setPosition(float x, float y) {
        sprite.setPosition(x, y);
    }
};

void move(GameEngine &engine, SparseArray<Position> &positions, SparseArray<Velocity> &velocities) {
    for (auto &pos : positions) {
        if (velocities.has(pos.first)) {
            pos.second.x += velocities[pos.first].x;
            pos.second.y += velocities[pos.first].y;
        }
    }
}

void render(GameEngine &engine, SparseArray<Position> &positions, SparseArray<Sprite> &sprites, gameEngine::RenderWindow &window) {
    for (auto &pos : positions) {
        if (sprites.has(pos.first)) {
            sprites[pos.first].setPosition(pos.second.x, pos.second.y);
            window.draw(sprites[pos.first].sprite);
        }
    }
}

int main() {
    GameEngine engine;
    gameEngine::RenderWindow window(gameEngine::VideoMode(1280, 720), "GameEngine window");

    engine.ecsRegisterComponent<Position>();
    engine.ecsRegisterComponent<Velocity>();

    engine.ecsAddSystem<Position, Velocity>(move);
    engine.ecsAddSystem<Position, Sprite>(render, window);

    Entity entity = engine.ecsSpawnEntity();

    Position pos{0, 0};
    engine.ecsAddComponent(entity, pos);

    Velocity vel{1, 1};
    engine.ecsAddComponent(entity, vel);

    engine.timeSetInterval([]() {
        std::cout << "Hello World every 1s" << std::endl;
    }, 1000); // 1000ms = 1s
    engine.timeStartTick();

    while (window.isOpen()) {
        gameEngine::Event event;
        while (window.pollEvent(event)) {
            if (event.type == gameEngine::Event::Closed) {
                window.close();
            }

            if (event.type == gameEngine::Event::KeyPressed) {
                if (event.key.code == gameEngine::Keyboard::Escape) {
                    engine.timeSetTimeout([]() {
                        std::cout << "Hello World after 10s" << std::endl;
                    }, 10000); // 10000ms = 10s
                }
            }
        }

        window.clear();

        engine.ecsRunSystems();

        window.display();
    }

    return 0;
}
```

## Methods

To see all the methods available in the game engine, please refer to the corresponding documentation page depending on the module you want to use.

- [ECS](https://lennydelgado.github.io/R-Type-Doc/GameEngine/ecs/)
- [Network](https://lennydelgado.github.io/R-Type-Doc/GameEngine/network/)
- [Time](https://lennydelgado.github.io/R-Type-Doc/GameEngine/time/)
- [Lua](https://lennydelgado.github.io/R-Type-Doc/GameEngine/lua/)
- [Json](https://lennydelgado.github.io/R-Type-Doc/GameEngine/json/)
- [Asset](https://lennydelgado.github.io/R-Type-Doc/GameEngine/asset/)

!!! info "Note"
    If you know how to use the method but don't remember it's exact name, all maethod are named with the following convention: `module` + `MethodName`
    So for example if you want to use the `ecsSpawnEntity` method you know that it is in the `ecs` module, so you can just type `ecs` and the auto-completion will show you all the methods available in the `ecs` module.

## Authors/Contacts

- [Kenan Blasius](https://github.com/Kenan-Blasius)

## Conclusion

The game engine is the main part of the project, it is the part that manages the entities, the systems, the events and the rendering.
