# OOPS-DA
#include <CL/sycl.hpp>
#include <iostream>
#include <chrono>

using namespace sycl;

constexpr int N = 1000; // Matrix size

int main() {
    queue q;

    // Allocate memory for matrices A, B, and C
    int* A = static_cast<int*>(malloc_shared(N * N * sizeof(int), q));
    int* B = static_cast<int*>(malloc_shared(N * N * sizeof(int), q));
    int* C = static_cast<int*>(malloc_shared(N * N * sizeof(int), q));

    // Initialize matrices A and B
    for (int i = 0; i < N * N; i++) {
        A[i] = i;
        B[i] = i;
    }

    // Start timing
    auto start = std::chrono::high_resolution_clock::now();

    // Perform matrix addition using SYCL parallel_for
    q.parallel_for(range<1>(N * N), [=](id<1> i) {
        C[i] = A[i] + B[i];
        }).wait();

    // Stop timing
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double, std::milli> duration = end - start; // Convert to milliseconds

    // Output the execution time in milliseconds
    std::cout << "Execution Time: " << duration.count() << " ms\n";

    // Output the result matrix C
    std::cout << "Output Matrix (first 10 elements):\n";
    for (int i = 0; i < 10; i++) { // Display first 10 elements of C to avoid large output
        std::cout << C[i] << " ";
    }
    std::cout << "...\n"; // Indicating the matrix continues

    // Optionally, print the full matrix C (but be cautious for large N)
    // for (int i = 0; i < N; i++) {
    //     for (int j = 0; j < N; j++) {
    //         std::cout << C[i * N + j] << " ";
    //     }
    //     std::cout << "\n";
    // }

    // Free allocated memory
    free(A, q);
    free(B, q);
    free(C, q);
    return 0;
}
