# Orderbook-Master

A fast C++ limit order book that matches buyers and sellers in real time. Built over summer 2026 to practice C++20 and concurrent programming.

## Overview

This engine simulates the core matching logic used by financial and crypto exchanges. It accepts buy and sell orders, sorts them by price and time, and instantly executes trades when prices overlap.

## Key Features

* **Real-Time Order Matching:** Pairs the highest buy offers with the lowest sell offers.
* **Price-Time Priority:** Uses a "first-come, first-served" rule for orders placed at the exact same price.
* **Flexible Order Types:**
  * **Market Order:** Executes immediately at the best available current price.
  * **Good 'Til Cancel (GTC):** Remains open until manually canceled.
  * **Fill and Kill (FAK):** Fills as much as possible right now and deletes any leftover amount.
  * **Fill or Kill (FOK):** Fills the entire order immediately or cancels the whole thing.
  * **Good For Day (GFD):** Remains active throughout the session and automatically expires at market close (4:00 PM).
* **Background Housecleaning:** A background thread tracks the clock and automatically purges expired GFD orders at 4:00 PM without interrupting live trading.
* **Thread-Safe Operations:** Uses mutex locks and atomic flags so multiple threads can process incoming trades safely without race conditions or memory crashes.

## Tech Stack

| Component | Technology | Role |
| :--- | :--- | :--- |
| **Language Standard** | C++20 | Leverages `<format>`, `<chrono>`, smart pointers, and atomic flags. |
| **Build System** | MSVC (Visual Studio 2022) | Configured for `x86` / `x64` architectures in Debug/Release modes. |
| **Concurrency** | Threads & Locks | Utilizes `std::thread`, `std::scoped_lock`, and `std::condition_variable`. |
| **Data Structures** | Maps & Lists | Fast priority matching via `std::map`, `std::unordered_map`, and `std::list`. |

## How It Works

1. **Order Intake:** Incoming orders enter the system and get categorized by buy (bid) or sell (ask).
2. **Priority Sorting:** Bids are sorted highest-to-lowest, while Asks are sorted lowest-to-highest.
3. **Execution:** The engine checks if the top buy price meets or exceeds the top sell price. If matched, trades execute instantly.
4. **Async Pruning:** A dedicated thread waits until 16:00 to remove expired GFD orders without locking down the main matching engine.
