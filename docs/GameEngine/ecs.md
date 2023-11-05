# ECS (Entity Component System)

## What is ECS?

ECS is a design pattern that is used to make games. It is a way to organize your code in a way that makes it easier to add new features to your game. It is also a way to make your code more efficient.

## Methods

### Component

A component is a piece of data that can be attached to an entity. A component can be anything from a position to a velocity to a sprite. A component can also be a group of components.

#### GameEngine::ecsRegisterComponent

```cpp
template <typename Component>
void ecsRegisterComponent();
```

This function registers a component to the game engine. This means that the game engine will be able to use this component.

#### GameEngine::ecsGetComponents

```cpp
template <typename Component>
SparseArray<Component> &ecsGetComponents();
```

This function returns a reference to the component list of a certain type.

### Entity

An entity is an object that has a unique ID and a list of components. An entity can be anything from a player to a bullet to a wall. An entity can also be a group of entities.

#### GameEngine::ecsSpawnEntity

``` cpp
Entity ecsSpawnEntity();
```

This function creates a new entity and returns its ID.

#### GameEngine::ecsEntityFromIndex

```cpp
Entity ecsEntityFromIndex(size_t index);
```

Since the constructor of the Entity class is private, you can't create an entity directly. Instead, you have to use this function to get an entity from its index.

All entities are implicitly convertable to their index, so you can use this function to get an entity from its index.

#### GameEngine::ecsKillEntity

```cpp
void ecsKillEntity(Entity entity);
```

This function kills an entity. This means that it will be removed from the game engine and all of its components will be removed from the game engine.

#### GameEngine::ecsKillAllEntities

```cpp
void ecsKillAllEntities();
```

This function kills all entities. This means that they will be removed from the game engine and all of their components will be removed from the game engine.

Useful for when you want to restart the game or you when you leave a menu to go to the game and you want to remove all the entities from the menu.

#### GameEngine::ecsAddComponent

```cpp
template <typename Component>
typename SparseArray<Component>::reference_type ecsAddComponent(Entity entity, Component component);
```

This function adds a component to an entity. It returns a reference to the component that was added.

#### GameEngine::ecsEmplaceComponent

```cpp
template <typename Component, typename... Args>
typename SparseArray<Component>::reference_type ecsEmplaceComponent(Entity entity, Args &&... args);
```

This function emplaces a component to an entity. It returns a reference to the component that was added.

#### GameEngine::ecsRemoveComponent

```cpp
template <typename Component>
void ecsRemoveComponent(Entity entity);
```

This function removes a component from an entity.

### System

A system is a function that takes a list of components and does something with them. A system can be anything from a movement system to a rendering system to a collision system.

#### GameEngine::ecsAddSystem

```cpp
template <class... Components, typename Function>
std::function<void()> ecsAddSystem(Function const &f)

template <class... Components, typename Function>
std::function<void()> ecsAddSystem(Function &&f)
```

This function adds a system to the game engine. It returns a function that you can call to remove the system from the game engine.

The function that you pass to this function must take a `GameEngine &` as a parameter, a list of components you want to use and return void.

```cpp
template <class... Components, typename... ExtraParams, typename Function>
std::function<void()> ecsAddSystem(Function &&f, ExtraParams &&...p)
```

This function adds a system to the game engine. It returns a function that you can call to remove the system from the game engine.

The function that you pass to this function must take a `GameEngine &` as a parameter, a list of components you want to use, some extra parameters if you need them and return void.

#### GameEngine::ecsClearSystems

```cpp
void ecsClearSystems();
```

This function removes all systems from the game engine.

#### GameEngine::ecsRunSystems

```cpp
void ecsRunSystems();
```

This function runs all systems added to the game engine in the order they were added.

#### GameEngine::ecsRunSingleSystem

```cpp
template <class... Components, typename Function>
void ecsRunSingleSystem(Function &&f)
```

This function runs a system once. It takes a function that takes a `GameEngine &` as a parameter, a list of components you want to use and returns void.

```cpp
template <class... Components, typename... ExtraParams, typename Function>
void ecsRunSingleSystem(Function &&f, ExtraParams &&...p)
```

This function runs a system once. It takes a function that takes a `GameEngine &` as a parameter, a list of components you want to use, some extra parameters if you need them and returns void.
