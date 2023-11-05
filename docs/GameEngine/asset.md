# Asset Manager

## What is the Asset Manager?

The Asset Manager is a class that is used to load and store assets. It is used to store asset in memory. It is also used to get assets from memory. Using it ensure that assets are only loaded once if correctly used.

## Methods

### GameEngine::assetAddAsset

```cpp
template <typename T>
void assetAddAsset(const std::string &name, T &asset);
```

This function adds an asset to the asset manager. It takes a name and an asset. The name is used to get the asset later.

### GameEngine::assetGetAsset

```cpp
template <typename T>
T &assetGetAsset(const std::string &name);
```

This function gets an asset from the asset manager. It takes a name and returns the asset.

### GameEngine::assetRemoveAsset

```cpp
template<typename T>
T &assetRemoveAsset(const std::string &name);
```

This function removes an asset from the asset manager. It takes a name and returns the asset. It return a reference to the asset so you can manage how it is deleted.

### GameEngine::assetExists

```cpp
bool assetExists(const std::string &name);
```

This function checks if an asset exists in the asset manager. It takes a name and returns true if the asset exists and false if it doesn't.

### GameEngine::assetClear

```cpp
void assetClear();
```

This function removes all assets from the asset manager. This will not delete the assets. It will only remove them from the asset manager. Their default destructor will be called.
