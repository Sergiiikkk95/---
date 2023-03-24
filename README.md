# ---
Крестики-нолики в с++. В цветовом оформлении


#include<iostream>
#include<windows.h>
#include<stdlib.h>
#include<time.h>


using namespace std;

const int BOARD_SIZE = 3;

// Функция для инициализации игрового поля
void init_game(char board[][BOARD_SIZE])
{
    for (int r = 0; r < BOARD_SIZE; ++r)
    {
        for (int c = 0; c < BOARD_SIZE; ++c)
        {
            board[r][c] = '-';
        }
    }
}

// Функция для вывода игрового поля на экран
void print_game(const char board[][BOARD_SIZE])
{
    for (int r = 0; r < BOARD_SIZE; ++r)
    {
        for (int c = 0; c < BOARD_SIZE; ++c)
        {
            cout << board[r][c] << " ";
        }
        cout << endl;
    }
}

// Функция для проверки, можно ли поставить знак на заданную позицию
bool is_move_valid(const char board[][BOARD_SIZE], int r, int c)
{
    if (r < 0 || r >= BOARD_SIZE || c < 0 || c >= BOARD_SIZE)
    {
        return false; // Проверка выхода за границы поля
    }
    if (board[r][c] != '-') 
    {
        return false; // Проверка наличия уже поставленного знака
    }
    return true;
}

// Функция для проверки, есть ли выигрышная комбинация на поле
bool check_win(const char board[][BOARD_SIZE], char player)
{
    for (int r = 0; r < BOARD_SIZE; ++r)
    {
        if (board[r][0] == player && board[r][1] == player && board[r][2] == player) 
        {
            return true; // Проверка по горизонтали
        }
    }

    for (int c = 0; c < BOARD_SIZE; ++c)
    {
        if (board[0][c] == player && board[1][c] == player && board[2][c] == player) 
        {
            return true; // Проверка по вертикали
        }
    }

    if (board[0][0] == player && board[1][1] == player && board[2][2] == player) 
    {
        return true; // Проверка по диагонали
    }

    if (board[2][0] == player && board[1][1] == player && board[0][2] == player) 
    {
        return true; // Проверка по диагонали
    }

    return false;
}

// Функция, реализующая игру крестики-нолики для двух игроков
void play_game()
{
    char board[BOARD_SIZE][BOARD_SIZE];
    char player1 = 'X';
    char player2 = 'O';
    int row, col;
    bool player1_turn = true;

    // Инициализация игрового поля и вывод его на экран
    init_game(board);
    print_game(board);

    // Основной цикл игры
    while (true) {
        if (player1_turn) 
        {
            cout << "\n\x1b[32mХод игрока 1 ('X'):\x1b[36m " << endl;
        }
        else {
            cout << "\n\x1b[33mХод игрока 2 ('O'):\x1b[36m " << endl;
        }

        // Попросить игрока выбрать позицию на поле
        cout << "\n\x1b[37mВведите строку:\x1b[36m ";
        cin >> row;
        cout << "\x1b[37mВведите столбец:\x1b[36m ";
        cin >> col;
        cout << "\n";

        // Проверить, можно ли поставить знак на выбранную позицию
        if (!is_move_valid(board, row, col)) 
        {
            cout << "\n\x1b[31mНекорректный ход\x1b[36m" << endl;
            continue;
        }

        // Выполнить ход
        if (player1_turn) 
        {
            board[row][col] = player1;
        }
        else {
            board[row][col] = player2;
        }

        // Вывести игровое поле на экран
        print_game(board);

        // Проверить наличие выигрышной комбинации или ничью
        if (check_win(board, player1)) 
        {
            cout << "\n\x1b[33mИгрок 1 выиграл!\x1b[36m" << endl;
            break;
        }
        else if (check_win(board, player2)) 
        {
            cout << "\n\x1b[34mИгрок 2 выиграл!\x1b[36m" << endl;
            break;
        }
        else if (board[0][0] != '-' && board[0][1] != '-' && board[0][2] != '-' &&
            board[1][0] != '-' && board[1][1] != '-' && board[1][2] != '-' &&
            board[2][0] != '-' && board[2][1] != '-' && board[2][2] != '-') 
        {
            cout << "\n\x1b[32mНичья!\x1b[36m" << endl;
            break;
        }

        // Передать ход другому игроку
        player1_turn = !player1_turn;
    }
}

int main()
{
	setlocale(LC_ALL, "ru");
	HANDLE hConsoleHandle = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleTextAttribute(hConsoleHandle, 11);
	srand(time(NULL));
   
    
    play_game();


	return 999;
	
}
