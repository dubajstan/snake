#include <iostream>
#include <queue>
#include <Windows.h>
#include <time.h>
#include <conio.h>

using namespace std;
enum Direction { LEFT, UP, RIGHT, DOWN };
Direction dir;
bool gameover = false;

void wypisz(char pole[20][20],int wynik)
{
	if (gameover == false)
	{
		for (int i = 0; i < 13; i++)
		{
			cout << char(219);
		}
		cout << '\n' <<char(219)<<"           "<<char(219)<< '\n';
		cout << char(219) << "     " << wynik;
		if(wynik<10)
			cout << "     " << char(219) << '\n';
		else if(wynik>=10&&wynik<100)
			cout << "    " << char(219) << '\n';
		else
			cout<< "   " << char(219) << '\n';
		cout << char(219) << "           " << char(219) << '\n';

		for (int i = 0; i < 42; i++)
			cout << char(219);
		cout << '\n';

		for (int i = 0; i < 20; i++)
		{
			cout << char(219);
			for (int j = 0; j < 20; j++)
			{
				if (pole[j][i] == 'p') cout << "  ";
				if (pole[j][i] == 'w') cout << " o";
				if (pole[j][i] == 'o') cout << " o";
				if (pole[j][i] == 'j') cout << " x";
			}
			cout << char(219) << '\n';
		}
		for (int i = 0; i < 42; i++)
			cout << char(219);
	}
	else
	{
		cout << "Chcesz grac dalej?" << '\n';
		cout << "Jesli tak, kliknij t" << '\n';
		cout << "Jesli nie, kliknij n" << '\n';
		char gra_dalej;
		cin >> gra_dalej;
		if (gra_dalej == 't')
			gameover = false;
		else
			gameover=true;
	}
}

void setup(char pole[20][20])
{
	for (int i = 0; i < 20; i++)
		for (int j = 0; j < 20; j++)
			pole[j][i] = 'p';
}
void kierunek()
{
	if (_kbhit())
	{
		switch (_getch())
		{
		case 'a':
			dir = LEFT;
			break;
		case'w':
			dir = UP;
			break;
		case'd':
			dir = RIGHT;
			break;
		case's':
			dir = DOWN;
			break;
		case'p':
			gameover = true;
			break;
		default:
			break;
		}
	}
}

int main()
{
	srand(time(NULL));
	char pole[20][20];
	int x, y;
	int owocX, owocY;
	queue <int> historiaX;
	queue <int> historiaY;
	int dlugosc = 4;
	int wynik = 0;

	cout << "Sterujesz W, A, S, D" << '\n';
	cout << "PAUZA - p";
	Sleep(3000);
	system("cls");
	setup(pole);

	x = rand() % 19;
	y = rand() % 19;
	owocX = rand() % 19;
	owocY = rand() % 19;

	while (!gameover)
	{
		pole[owocX][owocY] = 'j';
		historiaX.push(x);
		historiaY.push(y);

		if (pole[x][y] == pole[owocX][owocY])//jak zje jablko
		{
			dlugosc++;
			pole[owocX][owocY] = 'w';
			owocX = rand() % 20;
			owocY = rand() % 20;
			wynik++;
		}

		kierunek();
		switch (dir)
		{
		case LEFT:
			x--;
			break;
		case UP:
			y--;
			break;
		case RIGHT:
			x++;
			break;
		case DOWN:
			y++;
			break;
		default:
			break;
		}

		if (x == 20) x = 0;
		else if (x == -1) x = 19;
		else if (y == 20) y = 0;
		else if (y == -1) y = 19;

		if (historiaX.size() == dlugosc || historiaY.size() == dlugosc)
		{
			pole[historiaX.front()][historiaY.front()] = 'p';
			historiaX.pop();
			historiaY.pop();
		}

		if (pole[x][y] == 'w')
		{
			system("cls");
			cout << "PRZEGRALES";
			Sleep(500);
			gameover = true;
		}

		pole[x][y] = 'w';
		system("cls");
		wypisz(pole,wynik);
		Sleep(50);
	}
	return 0;
}
