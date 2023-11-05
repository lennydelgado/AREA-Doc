# Installation

To begin with, in order to use the project, you need to clone it using the following command:

```sh
git clone git@github.com:EpitechPromo2026/B-CPP-500-MAR-5-1-rtype-theo.liennard.git
```

Once you have obtained the project, navigate to it to install the dependencies and build the project.

The installation of dependencies varies significantly depending on the operating system used.

Before proceeding, you need to install **conan**, which is an *open-source* tool that facilitates the installation of external C/C++ libraries.

To install it, visit [https://conan.io/downloads](https://conan.io/downloads)

Install **conan** according to your operating system or the method you prefer to use.

=== "Automatic Installation"
    === "For Windows"
        To install the project you can use the following command at the root of the project:

        ```sh
        ./windowCompile.bat
        ```

        !!! info "Note"
            if the build folder already exists, use the following command to rebuild the project:

            ```sh
            ./windowCompile.bat all
            ```

        !!! warning
            You may encounter a conan error during the execution of this command. If this is the case, you must execute the following command:

            ```sh
            conan config home
            ```

            then add the following lines to the global.conf file in folder that was returned by the previous command:

            ```sh
            tools.system.package_manager:mode=install
            tools.system.package_manager:sudo=True
            ```

        !!! warning
            If you encounter other errors, try to do a manual installation.

    === "For Linux and MacOS"
        To install the project you can use the following command at the root of the project:

        ```sh
        ./unixCompile.sh
        ```

        !!! info "Note"
            if the build folder already exists, use the following command to rebuild the project:

            ```sh
            ./unixCompile.sh all
            ```

        !!! warning
            You may encounter a conan error during the execution of this command. If this is the case, you must execute the following command:

            ```sh
            conan config home
            ```

            then add the following lines to the global.conf file in folder that was returned by the previous command:

            ```sh
            tools.system.package_manager:mode=install
            tools.system.package_manager:sudo=True
            ```

        !!! warning
            If you encounter other errors, try to do a manual installation.

=== "Manual Installation"
    ## Dependency Installation

    To verify its installation, you can now execute the following command, which allows **conan** to detect the version of the compiler you are using and several other necessary pieces of information for its operation.

    !!! info inline end "Note"
        This command must be executed in the project directory, not elsewhere.

    ```sh
    conan profile detect --force
    ```

    Once this is done, you need to prepare the files to install the dependencies.

    ```sh
    conan install . --output-folder=build --build=missing
    ```

    !!! warning
        You may encounter an error during the execution of this command. If this is the case, you must execute the following command:

        ```sh
        conan config home
        ```

        then add the following lines to the global.conf file in folder that was returned by the previous command:

        ```sh
        tools.system.package_manager:mode=install
        tools.system.package_manager:sudo=True
        ```

    === "For Linux and MacOS"

        To complete the installation of dependencies, execute the following commands. First, proceed to generate the project files.

        ```sh
        cmake -S . -B ./build --preset conan-release
        ```

        Then, you can initiate the project compilation with the following command:

        ```sh
        cmake --build build
        ```

    === "For Linux and MacOS (alt)"

        To complete the installation of dependencies, execute the following commands. First, navigate to the build directory.

        ```sh
        cd build
        ```

        Then, proceed to install the dependencies.

        ```sh
        cmake .. -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Release
        ```

        Now, you can initiate the project compilation with the following command:

        !!! info inline end "Note"
            To execute this command, you must be in the build directory.

        ```sh
        cmake --build .
        ```

    === "For Windows"

        To complete the installation of dependencies, execute the following commands. First, proceed to generate the project files.

        ```bat
        cmake -S . -B ./build --preset conan-default
        ```

        Then, you can initiate the project compilation with the following command:

        ```bat
        cmake --build build --config Release
        ```

    === "For Windows (alt)"

        To complete the installation of dependencies, execute the following commands. First, navigate to the build directory.

        ```sh
        cd build
        ```

        Then, proceed to install the dependencies.

        ```sh
        # assuming Visual Studio 15 2017 is your VS version and that it matches your default profile
        cmake .. -G "Visual Studio 15 2017" -DCMAKE_TOOLCHAIN_FILE=./conan_toolchain.cmake
        ```

        Now, you can initiate the project compilation with the following command:

        !!! info inline end "Note"
            To execute this command, you must be in the build directory.

        ```sh
        cmake --build . --config Release
        ```
