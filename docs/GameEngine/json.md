# Json Manager

## What is the Json Manager?

The Json Manager is a module of the Game Engine that allows you to load and use Json files in your game.

## Methods

### GameEngine::jsonLoadFile

```cpp
void jsonLoadFile(const std::string &path);
```

This function loads a Json file.

### GameEngine::jsonGetFile

```cpp
nlohmann::json jsonGetFile(const std::string &path);
```

This function returns the Json file loaded with `jsonLoadFile`.

The parameter `path` is the path to the Json file you want to get.

### GameEngine::jsonGetValue

```cpp
template <typename T, class... Args>
T jsonGetValue(const std::string &path, Args... args)

template <typename T, class... Args>
T jsonGetValue(nlohmann::json &json, Args... args)
```

This function returns the value of a Json file.

The parameter `path` is the path to the value you want to get.

The parameters `args` are the keys to the value you want to get, it can be a string or an int.
