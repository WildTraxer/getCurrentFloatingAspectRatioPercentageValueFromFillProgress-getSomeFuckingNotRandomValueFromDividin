// ConsoleApplication1.cpp: определяет точку входа для консольного приложения.

//

#include "stdafx.h"

#include <iostream>

#include <iomanip>

#include "time.h"

using namespace std;

void print_vector(double* vect, int n, const char* name = NULL)

{ // добавить комментарии

	if (name)

		cout << "Вектор " << name << endl;

	for (int i = 0; i < n; i++)

		cout << setw(10) << setprecision(2) << vect[i] << " ";

	cout << endl;

}

double* create_vector(int n, const char* name = NULL)

{

	double* vect = new double[n]; // выделение динамической памяти

	// переменная vect - это указатель на double

	// new гарантирует что этот указатель ссылается на область,

	// достаточную для хранения n чисел double

	// заполним случайными числами

	for (int i = 0; i < n; i++)

		vect[i] = rand() / 30000.0 - 0.5;

	if (name)

		print_vector(vect, n, name);

	return vect;

}

void print_matrix(double** matr, int m, int n, const char* name)

{ // добавить комментарии

	cout << "Матрица " << name << endl;

	for (int i = 0; i < m; i++)

		print_vector(matr[i], n);

	cout << endl;

}

double** create_matrix(int m, int n, const char* name = NULL)

{

	double** matr = new double* [m]; // выделение динамической памяти

	// ВЫШЕ выделяется память под m штук указателей на строки матрицы

	for (int i = 0; i < m; i++) // что здесь выделяется?

		matr[i] = create_vector(n);

	if (name)

		print_matrix(matr, m, n, name);

	return matr;

}

void delete_vector(double* vect)

{

	delete vect;

}

void delete_matrix(double** matr, int m)

{

	for (int i = 0; i < m; i++) //

		delete_vector(matr[i]);

	delete matr; //

}

void mult_matr_numb(double** A, int m, int n, double v, double** Res)

{// напишите!!!

}

void subt_matr_matr(double** A, int m, int n, double** B, double** Res)

{

}

void mult_matr_matr(double** A, int m, int n, // N == n

	double** B, int N, int k,

	double** Res, int M, int K // M == m, K == k

)

// используйте только m, n, k

{

}

void mult_matr_vect(double** A, int m, int n,

	double* vect, int N, // N == n

	double* res_vect, int M // M == m

)

{

}

int _tmain(int argc, char* argv[])

{

	srand(time(0));

	setlocale(LC_ALL, "Rus");

	cout << "Лаб. 9. Динамическая память для матриц и операции с ними\n\n";

	cout << "Введите размер матриц и векторов n >>> ";

	int n;

	cin >> n;

	//double vect[n]; // ошибка - n должно быть постоянным

	// такая память наз. статической

	double* vect = create_vector(n, "V");

	//print_vector(vect, n, "V");

	// вар. 25

	// четыре матрицы - A, B, C, D

	double** A = create_matrix(n, n, "A");

	//print_matrix(A, n, n, "A");

	double** B = create_matrix(n, n, "B");

	double** C = create_matrix(n, n, "C");

	double** D = create_matrix(n, n, "D");

	double u = rand() / 30000.0 - 0.5;

	double v = rand() / 30000.0 - 0.5;

	double** Av = create_matrix(n, n);

	mult_matr_numb(A, n, n, v, Av);

	double** Bu = create_matrix(n, n);

	mult_matr_numb(B, n, n, v, Bu);

	double** Av_Bu = Bu; // здесь новая память не выделяется

	subt_matr_matr(Av, n, n, Bu, Av_Bu); //

	// и даже никакие элементы не копируются, а просто копируются адреса(указатели), 4байта

	double** Av_Bu_C = Av; // здесь новая память не выделяется

	mult_matr_matr(Av_Bu, n, n, C, n, n, Av_Bu_C, n, n); //

	double** E = D; // здесь новая память не выделяется

	subt_matr_matr(Av_Bu_C, n, n, D, E); //

	print_matrix(E, n, n, "E (результат)");

	cout << "Вторая часть\n\n";

	cout << "Умножение матрицы на вектор\n\n";

	double* res_vect = create_vector(n);

	mult_matr_vect(A, n, n, vect, n, res_vect, n); //

	print_vector(res_vect, n, "A*V");

	// надо будет сравнить результаты с Excel или c MathCAD

	delete_vector(vect);

	delete_vector(res_vect);

	delete_matrix(A, n);

	delete_matrix(B, n);

	delete_matrix(C, n);

	delete_matrix(D, n);

	delete_matrix(Av, n);

	delete_matrix(Bu, n);

	system("pause");

	return 0;
}
