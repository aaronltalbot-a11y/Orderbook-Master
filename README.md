# Orderbook-Master

# High-Performance C++20 Limit Order Book

A multithreaded, high-frequency limit order book engine built in C++20 that processes order submissions, modifications, cancellations, and order matching across price levels[cite: 4, 5, 6].

## Visual Overview & Demo

![Orderbook Execution Demo](https://raw.githubusercontent.com/username/orderbook/main/assets/demo.gif)

* **Live Interactive Demo**: [Orderbook Engine Sandbox](https://github.com/username/orderbook-demo)[cite: 3]
* **Build Status**: Compiled and verified under MSVC Toolset `v143` (Visual Studio 2022).

## Technical Stack

| Component | Technology / Feature | Description |
| :--- | :--- | :--- |
| **Language Standard** | C++20 (`stdcpp20`) | Employs standard libraries including `<format>`, `<chrono>`, dynamic pointers, and atomic flags[cite: 4, 5, 8]. |
| **Build System** | MSVC / Visual Studio 2022 | Configured for `x86` and `x64` architectures in Debug/Release modes[cite: 7, 8]. |
| **Concurrency** | `<thread>`, `<mutex>`, `<condition_variable>` | Multithreaded safety with asynchronous background order pruning[cite: 5, 6]. |
| **Data Structures** | `std::map`, `std::unordered_map`, `std::list` | Priority price levels using sorted maps and constant-time order iterators[cite: 4, 6]. |

## Key Engineering Challenges Solved

* **Order Types & Execution Logic**: Implemented support for Market, GoodTillCancel (GTC), FillAndKill (FAK), FillOrKill (FOK), and GoodForDay (GFD) orders[cite: 4, 5]. Market orders automatically adjust to GTC orders based on worst available price limits[cite: 4, 5].
* **Asynchronous EOD Order Pruning**: Integrated a background thread (`PruneGoodForDayOrders`) that calculates time remaining until end-of-day (16:00)[cite: 5, 6]. It uses `std::condition_variable::wait_for` to wake up and prune active GFD orders without impeding main matching operations[cite: 5].
* **Price-Time Priority Matching**: Utilized `std::greater` for descending Bids and `std::less` for ascending Asks[cite: 6]. An auxiliary map (`data_`) tracks aggregate volumes and level order counts dynamically[cite: 5, 6].
* **Thread Safety & Graceful Shutdown**: Protected order state operations using `std::scoped_lock` and standard mutexes[cite: 5, 6]. Designed RAII destruction to signal threads via atomic `shutdown_` flags and join background processes cleanly[cite: 5, 6].

## Running the Code Locally

### Prerequisites
* Windows OS with Visual Studio 2022 installed[cite: 7, 8].
* C++20 Desktop Development Workload[cite: 8].

### Build Instructions
1. Clone the repository:
   ```bash
   git clone [https://github.com/username/orderbook.git](https://github.com/username/orderbook.git)
   cd orderbook
