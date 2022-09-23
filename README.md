# Minesweeper Demo
### This repository contains demonstrations of my [Android Minesweeper Project](https://github.com/EricYoung37/cse5236Minesweeper.git).
**Author: Eric Young**<br>
**Project Contributors: Eric Young, Yuqi You, Jiashu Zhang**

## Overview
This is an Android App that resembles the classic Windows puzzle video game Minesweeper.<br>

### Use Cases (applicable to both portrait and landscape views):
Screenshots displayed here are mostly for easy-mode games. You may check the screenshots directory for more hard-mode game screenshots.
You may also check the demo video taken at checkpoint 5 (all other contents in this repository are for checkpoint 6 / the final checkpoint).
1. **The user starts a new game from the main menu.**
![Main Menu](screenshots/1_1_main_menu.jpg)
2. **The user views the time elapsing in the upper right corner.**
3. **The user clicks a cell to open it.**
![Clear Mode, Easy Game](screenshots/2_1_easy_clear.jpg)
4. **The user clicks the flag tool icon in the upper left corner to enter flag mode.**
![Flag Mode, Easy Game](screenshots/2_2_easy_flag.jpg)
5. **In flag mode, the user clicks a cell to place a flag on it, and clicks the same cell again to
   reclaim the flag. (The number of flags available is shown below the flag tool icon and will
   change as the user places or reclaims flags.)**
![Reclaim Flag, Easy Game](screenshots/2_3_easy_reclaim_flag.jpg)
6. **The user submits his/her record to the leaderboard if he/she successfully finishes the
   game. (Depending on the difficulty of the game the user completes, the record will be
   submitted to the “easy game” database table or the “hard game” database table. Also, if
   the submitted player name already exists in the table, the record will be updated;
   otherwise, a new record is created.)**
![Victory, Easy Game](screenshots/2_4_easy_Victory.jpg)
![Before Submitting a Record, Easy Game](screenshots/2_5_easy_before_submitting_record.jpg)
![Submitted a Record, Easy Game](screenshots/2_6_easy_submitted_record.jpg)
7. **The user restarts the game by clicking the restart button at the bottom of the screen in
   the game page.**
8. **The user goes to the leaderboard page from the homepage.**
![Leaderboard, Easy Game](screenshots/3_1_easy_leaderboard.jpg)
9. **The user switches between the “easy” leaderboard and the “hard” leaderboard with the
   navigation buttons at the bottom of the leaderboard page.**
![Leaderboard, Hard Game](screenshots/3_3_hard_ld.jpg)
10. **The user deletes a record on the leaderboard page.**
![Delete_Record, Easy Game](screenshots/3_2_easy_ld_delete.jpg)
11. **The user checks the settings page.**
![Settings](screenshots/5_1_settings.jpg)
12. **The user checks the difficulty configuration page.**
![Difficulty Config](screenshots/6_1_difficulty_config.jpg)
13. **The user changes the game difficulty to easy.**
![Change Difficulty to Easy](screenshots/6_2_change_to_easy.jpg)
14. **The user changes the game difficulty to hard.**
![Change Difficulty to Hard](screenshots/6_3_change_to_hard.jpg)
15. **The user checks the theme configuration page.**
![Theme Config](screenshots/7_1_theme_config.jpg)
16. **The user set the theme to the default.**
![Set Theme to Default](screenshots/7_2_set_default_theme.jpg)
17. **The user set the theme to dark mode.**
![Set_Theme_to_Dark](screenshots/7_3_set_dark_theme.jpg)

### Non-functional requirements:
1. **Data States Preservation between Configuration Changes**<br>
With ViewModel, data states for game field (e.g., cells opened, cells marked), flag status,
and timer are all maintained in screen rotation.
![Easy Game before Rot](screenshots/9_1_easy_before_rot.jpg)
![Easy Game aft Rot](screenshots/9_2_easy_aft_rot.jpg)
![Hard Game before Rot](screenshots/9_3_hard_before_rot.jpg)
![Hard Game aft Rot](screenshots/9_4_hard_aft_rot.jpg)
![Easy Board before Rot](screenshots/9_5_easy_board_before_rot.jpg)
![Easy Board aft Rot](screenshots/9_6_easy_board_aft_rot.jpg)
![Hard Board before Rot](screenshots/9_7_hard_board_before_rot.jpg)
![Hard Board aft Rot](screenshots/9_8_hard_board_aft_rot.jpg)
2. **Design and display enhancement**<br>
2 themes are now available: the default and the dark themes. The dark theme is newly
added to accommodate app use in dark environments so that the screen light will not
hurt the user’s eyes.
![Main Menu, Dark](screenshots/8_1_main_menu_dark.jpg)
![Easy Game, Dark](screenshots/8_2_easy_game_dark.jpg)
![Hard Game, Dark](screenshots/8_3_hard_game_dark.jpg)
![Easy Leaderboard, Dark](screenshots/8_4_easy_ld_dark.jpg)
![Hard Leaderboard, Dark](screenshots/8_5_hard_ld_dark.jpg)
3. **Resilience against Network Failures**<br>
Firebase local caching is enabled to ensure the app functions under network failures.
### Highlights:
- Model-View-ViewModel (MVVM) architecture implemented to separate interface control and business logic.
- Online leaderboard backed by NoSQL (Firebase Realtime Database) and local caching to ensure resilience against network failures.
- Data dispatch process in the leaderboard ViewModel to better synchronize the background thread that runs the leaderboard RecyclerView adapter with the main (UI) thread.