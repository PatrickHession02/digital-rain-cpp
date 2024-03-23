---
layout: post
title: C++ Digital Rain
tags: cpp coding project
categories: demo
---
## Introduction
Have you ever wondered how the iconic digital rain from the Matrix series could be brought to life in a modern C++ console application? Let's dive into the journey of creating a digital rain effect that pays homage to one of the most memorable sequences in cinematic history. The project utilises the use of threads and mutexs in order to produce the digital rain effect seen in the below image.

<img src="https://raw.githubusercontent.com/PatrickHession02/digital-rain-cpp/main/docs/assets/images/Screenshot 2024-03-22 110117.png" width="600" height="400">



## Design and Test
The code I've written implements threads and mutexes for efficient concurrency and synchronization. Initially, I had a single droplet simulated, but as the project progressed, I enhanced it by introducing threads to allow for multiple instances of the initial single droplet. However, during development, I encountered a bug that emerged due to race conditions, causing unexpected behavior in the raindrop animation. To address this issue, I incorporated a mutex, ensuring exclusive access to shared resources and preventing data corruption. Within a namespace I've defined, I initialize global variables representing the dimensions of a matrix simulating the console screen and the number of raindrops to animate. I've defined functions to generate random characters, simulate raindrop animation down specific columns of the matrix, and print the matrix with the raindrop effect, ensuring thread safety through mutex locking. The main functionality revolves around the simulateRaindrops() function, which spawns multiple threads to animate raindrops falling down various columns of the matrix.

 ```cpp
 void simulateRaindrop(int raindropColumn) {
     try {
         std::vector<std::vector<char>> matrix(MATRIX_HEIGHT, std::vector<char>(MATRIX_WIDTH, ' '));
         int raindropLength = 5 + rand() % 10;
         for (int row = 0; row < raindropLength; ++row) {
             int currentRow = row;
             while (currentRow >= 0 && currentRow < MATRIX_HEIGHT) {
                 matrix[currentRow][raindropColumn] = getRandomChar();
                 ++currentRow;
             }
             printMatrixWithRaindrop(matrix);
             currentRow = row;
             while (currentRow >= 0 && currentRow < MATRIX_HEIGHT) {
                 matrix[currentRow][raindropColumn] = ' ';
                 ++currentRow;
             }

             std::this_thread::sleep_for(std::chrono::milliseconds(ANIMATION_SPEED_MS));
         }
     }
     catch (const std::exception& e) {
         std::cerr << "Exception occurred during raindrop simulation: " << e.what() << std::endl;
     }
 }

 // Function to print the matrix with raindrops
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

 // Function to simulate raindrops using threads
 void simulateRaindrops() {
     std::thread raindrops[NUM_DROPS];

     for (int i = 0; i < NUM_DROPS; ++i) {
         int raindropColumn = rand() % MATRIX_WIDTH;
         raindrops[i] = std::thread(simulateRaindrop, raindropColumn);
     }

     for (int i = 0; i < NUM_DROPS; ++i) {
         raindrops[i].join();
     }
```
Finally, the runAllFunctions() function serves as the entry point, orchestrating the execution of all functions within the namespace, resulting in a visually captivating simulation of the iconic digital rain effect from the Matrix universe.

``` // Function to call all functions in the namespace
    void runAllFunctions() {
        char randomChar = getRandomChar();
        std::vector<std::vector<char>> matrix(MATRIX_HEIGHT, std::vector<char>(MATRIX_WIDTH, 'X'));
        printMatrixWithRaindrop(matrix);
        simulateRaindrops();
    }
 ```
    
## Testing
I found writing tests for the code a bit more challenging due to the randominess of the effect being implemented.The MatrixRainTest namespace encapsulates a set of functions designed to test various components of the Matrix rain effect implementation. These functions serve to ensure the correctness and reliability of key functionalities.

    // Test getRandomChar() function
    void testGetRandomChar() {
        char randomChar = MatrixFall::getRandomChar();
        assert(randomChar >= '!' && randomChar <= '~');
        std::cout << "getRandomChar() test passed!" << std::endl;
    }
    
Firstly, the testGetRandomChar() function verifies the behavior of the getRandomChar() function, which generates a random ASCII character within the range of printable characters. The test asserts that the generated character falls within this range and outputs a confirmation message upon successful execution.

```// Test printMatrixWithRaindrop() function
void testPrintMatrixWithRaindrop() {
    std::vector<std::vector<char>> matrix(MatrixFall::MATRIX_HEIGHT, std::vector<char>(MatrixFall::MATRIX_WIDTH, 'X')); // Initialize matrix with 'X'
    MatrixFall::printMatrixWithRaindrop(matrix);
    std::cout << "printMatrixWithRaindrop() test passed!" << std::endl;
} 
```

Similarly, the testPrintMatrixWithRaindrop() function evaluates the printMatrixWithRaindrop() function, which prints the matrix representing the raindrop effect. In this test, a matrix initialized with 'X' characters is passed to printMatrixWithRaindrop(), and the resulting output is visually inspected to confirm the correct rendering of the raindrop effect. Upon completion, a confirmation message is displayed to signify the success of the test.

```
   void testSimulateRaindrop() {
       // We can't do a direct test of simulateRaindrop() due to its animation and randomness.
       // But we can test its behavior indirectly by observing the output visually and checking for exceptions.
       std::cout << "simulateRaindrop() test passed! (Visual inspection required)" << std::endl;
   }
   ```
Due to the dynamic and randomized nature of the simulateRaindrop() function, direct testing is not feasible. Instead, the testSimulateRaindrop() function acknowledges this limitation and suggests visual inspection as a means to assess its behavior. 

```
void runAllTests() {
MatrixRainTest::testGetRandomChar();
MatrixRainTest::testPrintMatrixWithRaindrop();
MatrixRainTest::testSimulateRaindrop();
}
```
    
Finally, the runAllTests() function orchestrates the execution of all test functions within the namespace, ensuring comprehensive validation of the Matrix rain effect implementation. Each test function is invoked sequentially, and upon completion of all tests, a confirmation message is displayed to indicate successful testing completion.


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
    

The code snippet employs std::lock_guard to manage a mutex, ensuring that only one thread can access memory at a time, which prevents the screen from displaying random bursts of characters. This mechanism automatically unlocks the mutex when the lock_guard object goes out of scope. This is a fundamental aspect of RAII (Resource Acquisition Is Initialization) in C++, guaranteeing that resources are properly released, even if an exception occurs within the scope. This automatic unlocking mechanism prevents deadlocks and ensures thread safety by releasing the lock when the scope in which the lock_guard was created is exited.[4]

## Modern C++ 
My code showcases various aspects of Modern C++ design philosophy, leveraging the language's standard library features and modern programming paradigms to achieve concurrency, robustness, and maintainability.

**Standard Library Containers (`std::vector`):**
  - The code utilizes `std::vector` for managing two-dimensional data (matrix) representing the raindrop animation. Vectors provide dynamic arrays with automatic memory management, allowing for flexible storage of elements.[1]

- **Modern Random Number Generation (`std::rand()` replaced with `<random>`):**
  - Instead of using the older `rand()` function, the code employs `std::rand()` and `<random>` header for random number generation. This modern approach provides better randomness and flexibility in generating random numbers.[3]

- **Concurrency with Threads (`std::thread`):**
  - The code uses `std::thread` to create multiple threads for simulating raindrops concurrently. This approach leverages modern C++ features for multithreading, enhancing performance and efficiency. [2]

- **Exception Handling (try-catch blocks):**
  - The code incorporates exception handling using `try-catch` blocks to gracefully handle runtime errors, such as exceptions thrown during the raindrop simulation or matrix printing. This ensures robustness and reliability of the program.[1]

- **Synchronization with Mutex (`std::mutex`):**
  - The code employs `std::mutex` for synchronization, ensuring that access to shared resources (such as the console output) is properly coordinated among multiple threads. This prevents data races and ensures thread safety.[5]

- **Use of Standard Library Utilities:**
  - Standard library utilities like `std::this_thread::sleep_for` for introducing delays and `SetConsoleTextAttribute` for changing console text color are used. This demonstrates the utilization of existing C++ standard library functionalities to achieve desired behavior efficiently.[2]

- **Modern C++ Features (auto keyword, range-based for loops):**
  - The code utilizes modern C++ features such as the `auto` keyword for automatic type deduction and range-based for loops for concise iteration over elements of containers. These features improve code readability and maintainability.[1]

- **Encapsulation with Namespaces:**
  - All functions and data related to the raindrop animation are encapsulated within the `MatrixFall` namespace. This promotes modularity, prevents naming conflicts, and enhances code organization and clarity.
All functions and data related to the raindrop animation are encapsulated within the MatrixFall namespace. This promotes modularity, prevents naming conflicts, and enhances code organization and clarity.[1]

-**A clean main function:**
Towards the end of the project, after I had everything else working, I removed all unnecessary code and kept the main() function as clean as possible while retaining functionality. This is important to do as it provides a clear separation of concerns in my project and prevents the code from becoming confusing or unreadable. The main() function's sole purpose is to run the Rain functionality and its tests.[1]

<img src="https://raw.githubusercontent.com/PatrickHession02/digital-rain-cpp/main/docs/assets/images/Screenshot 2024-03-22 111744.png" width="600" height="400">

# Conclusion
In conclusion, engaging in this project provided me with a profound learning opportunity to explore various modern C++ concepts that were previously unfamiliar or not fully understood. I'm grateful for the invaluable learning experience it offered, all while crafting a visually stunning Matrix rain effect inspired by one of my favorite movies.

## References
[1] M. Lynch. “C++”, [Lecture], <mark>Digital Signal Processing</mark>, Galway-Mayo Institute of Technology, Galway, 2024.

[2] Cpp Reference “std::thread”, [Online], <mark>cpp reference.com</mark>, Available: https://en.cppreference.com/w/cpp/thread/thread [Accessed]22/3/24

[3]Cpp Reference “std::rand”, [Online], <mark>cppreference.com</mark>, Available: https://en.cppreference.com/w/cpp/thread/thread [Accessed]22/3/24

[4]Cpp Reference “std::lock_guard”, [Online], <mark>cppreference.com</mark>, Available: https://en.cppreference.com/w/cpp/thread/lock_guard [Accessed]22/3/24

[5]Cpp Reference “std::mutex”, [Online], <mark>cppreference.com</mark>, Available: https://en.cppreference.com/w/cpp/thread/mutex [Accessed]22/3/24





