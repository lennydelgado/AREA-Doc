# Lua Manager

## What is the Lua Manager?

The Lua Manager is a module of the Game Engine that allows you to use Lua scripts in your game.

## Methods

### GameEngine::luaLoadFile

```cpp
void luaLoadFile(const std::string &path);
```

This function loads a Lua script from a file.

### GameEngine::luaAddCppFunction

```cpp
void luaAddCppFunction(const std::string &fromFile, const std::string &name, lua_CFunction func);
```

This function add a C++ function to the Lua state of a script so you can call it from the script.

The parameter `fromFile` is the path of the script you want to add the function to.

The parameter `name` is the name of the function in the script.

The function `func` must have the following signature:

```cpp
int func(lua_State *L);
```

The int returned by the function is the number of values returned by the function.

### GameEngine::luaCallFunction

```cpp
template <typename... Args>
void luaCallFunction(const std::string &fromFile, const std::string &name, int nbReturnValue, Args... args);
```

This function calls a function from a script.

The parameter `fromFile` is the path of the script you want to call the function from.

The parameter `name` is the name of the function in the script.

The parameter `nbReturnValue` is the number of values returned by the function.

The parameters `args` are the arguments passed to the function.

### GameEngine::luaGetReturnValue

```cpp
template <typename... Args>
void luaGetReturnValue(const std::string &fromFile, Args&... args);
```

This function gets the return values of the last function called.

The parameter `fromFile` is the path of the script you want to get the return values from.

The parameters `args` are the variables that will be set with the return values.

### GameEngine::luaGetArguement

```cpp
template <typename... Args>
static void luaGetArguement(lua_State *L, Args&... args);
```

This function gets the arguments passed to a function.

It is used in the C++ functions added to the Lua state to get the arguments passed to the function in the script.

The parameter `L` is the Lua state required in the function signature.

The parameters `args` are the variables that will be set with the arguments.

### GameEngine::luaSendResult

```cpp
template <typename... Args>
static void luaSendResult(lua_State *luaState, Args&... args)
```

This function sends the return values of a C++ function to the Lua state.

It is used in the C++ functions added to the Lua state to send the return values of the function to the script.

The parameter `luaState` is the Lua state required in the function signature.

The parameters `args` are the variables that will be sent as return values.

### GameEngine::luaPrintStack

```cpp
void luaPrintStack(const std::string &fromFile);
```

This function prints the Lua stack of a script.
