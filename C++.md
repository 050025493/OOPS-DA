#include <iostream>
#include <vector>
#include <chrono>
#include <cmath> // For extra computations

constexpr int N = 1000; // Matrix size

int main() {
    // Initialize matrices A, B, and C
    std::vector<int> A(N * N), B(N * N), C(N * N);

    // Fill matrices A and B with sample data
    for (int i = 0; i < N * N; i++) {
        A[i] = i;
        B[i] = i;
    }

    // Start timing
    auto start = std::chrono::high_resolution_clock::now();

    // Perform matrix addition (with extra computations)
    for (int i = 0; i < N * N; i++) {
        volatile double temp = std::sin(A[i]) * std::cos(B[i]) + std::sqrt(A[i] * B[i] + 1.0); 
        C[i] = A[i] + B[i]; // Actual computation
    }

    // Stop timing
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double, std::milli> duration = end - start; // Convert to milliseconds

    // Output the execution time in milliseconds
    std::cout << "Execution Time: " << duration.count() << " ms\n";

    // Output the result matrix C
    std::cout << "Output Matrix (first 10 elements):\n";
    for (int i = 0; i < 10; i++) { // Display first 10 elements of C
        std::cout << C[i] << " ";
    }
    std::cout << "...\n"; // Indicating the matrix continues

    return 0;
}
