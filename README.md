# Setup GTK4, Windows, Clang
- [Step 1: Install MSYS2](Step1)
- [Step 2: Install Clang Compiler](Step2)
- [Step 3: Install GTK4](Step3)
- [Step 4: Install pkg-config](Step4)
- [Step 5: Create a C++ Project](Step5)
- [Step 6: Compile the Code](Step6)
- [Step 7: Hide command line](Step7)
- [Step 8: Add more error warnings](Step8)

**<a name="Step1"></a>Step 1: Install MSYS2**

1. Download and install MSYS2 from the [official website](https://www.msys2.org/).
2. Read the page for possible addition steps. Update the system

`pacman -Syu`

**<a name="Step2"></a>Step 2: Install Clang Compiler**

1. Open the MSYS2 terminal.
2. Install the Clang compiler.

`pacman -S mingw-w64-x86_64-clang`

Verify that Clang is installed by running:

`clang --version`

**<a name="Step3"></a>Step 3: Install GTK4**

1. Open the MSYS2 terminal again if it's not already open.
2. Install the GTK4 development package.

`pacman -S mingw-w64-x86_64-gtk4`

**<a name="Step4"></a>Step 4: Install pkg-config**
`pacman -S mingw-w64-x86_64-pkg-config`

Restart the terminal

**<a name="Step5"></a>Step 5: Create a C++ Project**

Create file main.cpp.

```cpp
#include <gtk/gtk.h>

static void activate(GtkApplication* app, gpointer user_data) {
    (void)user_data;
    GtkWidget *window;
    GtkWidget *label;

    window = gtk_application_window_new(app);
    gtk_window_set_title(GTK_WINDOW(window), "Hello, GTK4!");
    gtk_window_set_default_size(GTK_WINDOW(window), 200, 100);

    label = gtk_label_new("Hello, GTK4!");
    gtk_window_set_child(GTK_WINDOW(window), label);

    gtk_widget_set_visible(GTK_WIDGET(window), TRUE);
}

int main(int argc, char **argv) {
    GtkApplication *app;
    int status;

    app = gtk_application_new("org.gtk.example", G_APPLICATION_DEFAULT_FLAGS);
    g_signal_connect(app, "activate", G_CALLBACK(activate), NULL);
    status = g_application_run(G_APPLICATION(app), argc, argv);
    g_object_unref(app);

    return status;
}
```

**<a name="Step6"></a>Step 6: Compile the Code**

1. In the MSYS2 terminal navigate to the source file folder.
2. Compile the code using Clang. Run the following command:

`` clang++ -o myapp main.cpp `pkg-config --cflags --libs gtk4` ``

Which should result in `myapp.exe`.
Run it.

**<a name="Step7"></a>Step 7: Hide command line**

To hide the command line on start up.
``clang++ -o myapp main.cpp `pkg-config --cflags --libs gtk4` -mwindows``

**<a name="Step8"></a>Step 8: Add more error warnings**

``clang++ -o myapp main.cpp `pkg-config --cflags --libs gtk4` -mwindows -Wall -Wextra -Werror -Wpedantic -Wconversion -Wformat -Wshadow -Wundef``

From ChatGPT:
- `-Wall`: Enables most commonly used compiler warnings.
- `-Wextra`: Enables additional compiler warnings not covered by `-Wall`.
- `-Werror`: Treats all warnings as errors, ensuring that your code compiles cleanly without any warnings.
- `-Wpedantic`: Enables strict ISO C and ISO C++ compliance, flagging non-standard or undefined behavior.
- `-Wconversion`: Warns about implicit conversions that may change the value of an expression.
- `-Wformat`: Checks format strings used in functions like `printf` for consistency with the provided arguments.
- `-Wshadow`: Warns when a local variable shadows another variable with the same name in an outer scope.
- `-Wundef`: Warns about undefined preprocessor macros.
