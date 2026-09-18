# Buscaminas-en-Asembler

The code implements a complete Minesweeper game written in 32-bit ARM Assembly for Linux, utilizing system calls (svc #0) and structured into modules for memory management, console I/O, game logic, and record persistence.

Architecture and Core Features

Initial Setup and Map Selection: The program prompts for the player's name, difficulty level (1, 2, or 3), and board size (option 1 for an 8x8 grid with 64 cells, or option 2 for a 12x12 grid with 144 cells). Mines are calculated dynamically as a percentage of the total surface area.

Pseudorandom Generation and Mine Distribution: It uses a linear congruential generator routine (generar_random) seeded via gettimeofday (syscall 78) to place mines on the hidden map (mapa_oculto) without duplicates, filling the remaining space with empty cells (_).

Game Loop and Coordinates: It iteratively requests row and column inputs, validating ranges based on the board size and preventing repeated selections. If the user selects a safe cell, it calculates adjacent mines using precalculated offset tables (offsets8 and offsets12) and updates the visible board (mapa_visible) using colored ANSI rendering.

Win and Loss Conditions:

Loss: Stepping on a mine (*) triggers a red color indicator, reveals the position of all mines using the mostrar_minas function, and ends the execution.

Win: Uncovering all required safe cells (objetivo_ganar) records the elapsed time, formats the score, and updates the persistent records file.

Persistent Ranking System (ranking.txt): It features a complete routine for file reading, line-by-line parsing (using 36-byte memory structures with text buffers), sorting by playtime (ordenar_ranking), and systematically rewriting the best times to the local file system to display the Top 3.
