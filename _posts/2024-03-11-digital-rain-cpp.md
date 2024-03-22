---
layout: post
title: A Project in Modern C++
tags: cpp coding project
categories: demo
---
## Introduction
Have you ever wondered how the iconic digital rain from the Matrix series could be brought to life in a modern C++ console application? Let's dive into the journey of creating a digital rain effect that pays homage to one of the most memorable sequences in cinematic history. The project utilises the use of threads and mutexs in order to produce the digital rain effect.

## This is a Heading

This is a paragraph. Add an empty line to start a new paragraph.

Font can be *Italic* or **Bold**.

Code can be highlighted with 'backticks'.

Hyperlinks look like this [GitHub Help](https://help.github.com/).

A bullet list:

- vectors
- algorithms
- iterators

You can add an image that has been uploaded to the repository in a /docs/assets/images folder.


<img src="https://raw.githubusercontent.com/PatrickHession02/digital-rain-cpp/main/docs/assets/images/Screenshot 2024-03-22 110117.png" width="400" height="400">
<img src="https://raw.githubusercontent.com/PatrickHession02/digital-rain-cpp/main/docs/assets/images/Screenshot 2024-03-22 111744.png" width="400" height="400">


## Design and Test


## Algorithim
The provided code not only creates an engaging "Matrix Rain" animation but also incorporates algorithmic concepts to achieve its captivating effects. While the primary goal is to deliver a visually appealing experience, the code ingeniously implements algorithms to generate random characters, animate raindrops, and manage concurrency. These algorithmic processes, though tailored for the animation's aesthetics, showcase the versatility and practicality of algorithmic thinking in real-world applications. Let's delve into these algorithms further to appreciate their ingenuity and contribution to the overall functionality and charm of the program.

- **Random Character Generation Algorithm:**
  - The `getRandomChar()` function generates a random character from the ASCII range to simulate the appearance of characters falling like raindrops. This involves the algorithmic process of selecting a random character from a given range.

- **Raindrop Animation Algorithm:**
  - The `simulateRaindrop()` function simulates the animation of a raindrop falling through the matrix. It involves updating the matrix with characters representing the raindrop and then clearing them to simulate movement. This algorithmic process includes iterating over rows of the matrix and updating the characters accordingly.

- **Concurrency Management Algorithm:**
  - The usage of threads (`std::thread`) to simulate multiple raindrops concurrently involves a concurrency management algorithm. This algorithm ensures that each raindrop operates independently and does not interfere with others, allowing for a smoother animation experience. [2]

## Problem Solving
A problem i encountered while created my Digital Rain project was when i would run it after applying the threads a lot of the droplets were becoming disformed fortunetly thanks to the help of fellow classmate Dennis Costello I was able to quickly overcome this problem by modifying the structure of the printMatrixWithRaindrop function shown below. This did not fully solve my issue as although the rain started looking a lot better there was now intervals between the rain drops where the output would be a muddled mess. I solved this problem by adding a mutex shown below.

    
### Print matrix code with mutex

    // Function to print the matrix with raindrops
`std::mutex mtx;`

    void printMatrixWithRaindrop(const std::vector<std::vector<char>>& matrix) {
        try {
            for (const auto& row : matrix) {
                std::lock_guard<std::mutex> guard(mtx);
                SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 5);
                for (char pixel : row) {
                    std::cout << pixel;
                }
            }
        }
        catch (const std::exception& e) {
            std::cerr << "Exception occurred while printing matrix: " << e.what() << std::endl;
        }
    }
    

The code snippet employs std::lock_guard to manage a mutex, ensuring that only one thread can access memory at a time, which prevents the screen from displaying random bursts of characters. This mechanism automatically unlocks the mutex when the lock_guard object goes out of scope. This is a fundamental aspect of RAII (Resource Acquisition Is Initialization) in C++, guaranteeing that resources are properly released, even if an exception occurs within the scope. This automatic unlocking mechanism prevents deadlocks and ensures thread safety by releasing the lock when the scope in which the lock_guard was created is exited.

## Modern C++ 
My code showcases various aspects of Modern C++ design philosophy, leveraging the language's standard library features and modern programming paradigms to achieve concurrency, robustness, and maintainability.

**Standard Library Containers (`std::vector`):**
  - The code utilizes `std::vector` for managing two-dimensional data (matrix) representing the raindrop animation. Vectors provide dynamic arrays with automatic memory management, allowing for flexible storage of elements.

- **Modern Random Number Generation (`std::rand()` replaced with `<random>`):**
  - Instead of using the older `rand()` function, the code employs `std::rand()` and `<random>` header for random number generation. This modern approach provides better randomness and flexibility in generating random numbers.

- **Concurrency with Threads (`std::thread`):**
  - The code uses `std::thread` to create multiple threads for simulating raindrops concurrently. This approach leverages modern C++ features for multithreading, enhancing performance and efficiency. [2]

- **Exception Handling (try-catch blocks):**
  - The code incorporates exception handling using `try-catch` blocks to gracefully handle runtime errors, such as exceptions thrown during the raindrop simulation or matrix printing. This ensures robustness and reliability of the program.

- **Synchronization with Mutex (`std::mutex`):**
  - The code employs `std::mutex` for synchronization, ensuring that access to shared resources (such as the console output) is properly coordinated among multiple threads. This prevents data races and ensures thread safety.

- **Use of Standard Library Utilities:**
  - Standard library utilities like `std::this_thread::sleep_for` for introducing delays and `SetConsoleTextAttribute` for changing console text color are used. This demonstrates the utilization of existing C++ standard library functionalities to achieve desired behavior efficiently.

- **Modern C++ Features (auto keyword, range-based for loops):**
  - The code utilizes modern C++ features such as the `auto` keyword for automatic type deduction and range-based for loops for concise iteration over elements of containers. These features improve code readability and maintainability.

- **Encapsulation with Namespaces:**
  - All functions and data related to the raindrop animation are encapsulated within the `MatrixFall` namespace. This promotes modularity, prevents naming conflicts, and enhances code organization and clarity.
All functions and data related to the raindrop animation are encapsulated within the MatrixFall namespace. This promotes modularity, prevents naming conflicts, and enhances code organization and clarity.
## References
[1] M. Lynch. “C++”, [Lecture], <mark>Digital Signal Processing</mark>, Galway-Mayo Institute of Technology, Galway, 2024.

[2] C++ REference “std::thread”, [Online], <mark>cpp reference.com</mark>, Available: https://en.cppreference.com/w/cpp/thread/thread [Accessed]22/3/24




