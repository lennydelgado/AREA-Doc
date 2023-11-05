# Time Manager

## What is the Time Manager?

The Time Manager is a module of the Game Engine that allows you to manage time in your game. It allows you to create timers and to execute functions on a regular basis.

## Methods

### GameEngine::timeSetTimeout

```cpp
void timeSetTimeout(std::function<void()> f, time_t timeout);
```

This function executes the function `f` after `timeout` milliseconds.

Hint: Use lambda functions to pass parameters to your function.

### GameEngine::timeSetInterval

```cpp
std::function<void()> timeSetInterval(std::function<void()> f, time_t tickTime);
```

This function executes the function `f` every `tickTime` milliseconds.
It returns a function that you can call to remove the function from the time manager, stopping it from being executed.

Hint: Use lambda functions to pass parameters to your function.

### GameEngine::timeStartTick

```cpp
void timeStartTick();
```

This function starts the tick of the time manager. All intervals will start executing.

### GameEngine::timeStopTick

```cpp
void timeStopTick();
```

This function stops the tick of the time manager. All intervals will stop executing.
