#include <iostream>
#include <fstream>
#include <iomanip>
#include <stdlib.h> //srand,rand(random function)
#include <time.h>   //current time
#include <string>
//
using std::cout;
using std::cin;
using std::endl;
using std::string;
using std::fstream;
using std::ios;
using std::setw;
using std::setfill;
using std::left;
using std::right;
//

//Declare Constant
#define MAX 200
#define MAXROW 100
#define MAXCOL 100

//Declare Prototype Function
//1.
void createMatrix(int A[][MAXCOL], int& row, int& col);
void createRandomMatrix(int A[][MAXCOL], int& row, int& col);
void createMatrixFromFile(int A[][MAXCOL], int& row, int& col, string directory);
void displayMatrix(int A[][MAXCOL], int row, int col);
//2.
float averageTheEdge(int A[][MAXCOL], int row, int col);
float averageTheMainDiagonal(int A[][MAXCOL], int row, int col);
float averageTheAuxiliaryDiagonal(int A[][MAXCOL], int row, int col);
//9.
void createTheRightRotationMatrixF(int A[][MAXCOL], int row, int col, int F[][MAXCOL], int k);

//10.
void swap(int& a, int& b);
void sortintoTheAscendingArray(int arr[], int n);
void createTheDescendingSpiralMatrixI(int A[][MAXCOL], int row, int col, int I[][MAXCOL]);


void Menu();
//1.
//Ham tao ma tran bang cach nhap cac gia tri tu ban phim
void createMatrix(int A[][MAXCOL], int& row, int& col) {
    cout << "Tao ma tran " << endl;
    do {
        cout << "Hay nhap so dong cua ma tran :\t";
        cin >> row;
    } while (row <= 0);
    do {
        cout << "Hay nhap so cot cua ma tran :\t";
        cin >> col;
    } while (col <= 0);
    cout << "Hay nhap gia tri cho ma tran" << endl;
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            cout << "Nhap gia tri cho phan tu [" << i << "," << j << "] := "; cin >> A[i][j];
        }
    }
}
//Ham tao ma tran voi cac gia tri ngau nhien 
void createRandomMatrix(int A[][MAXCOL], int& row, int& col) {
    srand(time(NULL));
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            A[i][j] = rand() % 100;
        }
    }
}
//Ham tao ma tran voi cac gia tri doc tu file
void createMatrixFromFile(int A[][MAXCOL], int& row, int& col, string directory) {
    fstream file;
    file.open(directory, ios::in);//read mode
    if (file.is_open()) {
        file >> row;
        file >> col;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                file >> A[i][j];
            }
        }
        file.close();
    }
    else {
        cout << "Khong tim thay file !" << endl;
    }
}
//Ham xuat ma tran
void displayMatrix(int A[][MAXCOL], int row, int col) {
    cout << "XUat ma tran : " << endl;
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            cout << "\t " << A[i][j];
        }
        cout << endl;
    }
}
//2.
//Ham tinh trung binh cong o 4 bien
float averageTheEdge(int A[][MAXCOL], int row, int col) {
    float sum = 0;
    int count = 0;
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            if (i == 0 || j == 0 || i == row - 1 || j == col - 1) {
                sum += A[i][j];
                count++;
            }
        }
    }
    return sum / (1.0 * count);
}
//Ham tinh trung binh cong tren duong cheo chinh
float averageTheMainDiagonal(int A[][MAXCOL], int row, int col) {
    int sum = 0;
    int count = 0;
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            if (i == j) {
                sum += A[i][j];
                count++;
            }
        }
    }
    return sum / (1.0 * count);
}
//Ham tinh trung binh cong tren duong cheo phu
float averageTheAuxiliaryDiagonal(int A[][MAXCOL], int row, int col) {
    int sum = 0;
    int count = 0;
    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            if (i + j == row - 1) {
                sum += A[i][j];
                count++;
            }
        }
    }
    return sum / (1.0 * count);
}
//Từ A đã cho hãy tạo và xuất ra 2 ma trận C, D (có cùng kích thước) sao cho: C chứa toàn số dương và D chứa toàn
//số âm(các vị trí trống còn lại trên C, D để số 0).
void creatMatrixPositive_C(int arr[][MAXCOL], int row, int col)
{
    int n = row * col;
    for (int i = 0; i < n; i++)
    {
        arr[i / col][i % col] = (arr[i / col][i % col] < 0 ? (arr[i / col][i % col]) - (arr[i / col][i % col]) : arr[i / col][i % col]);
    }
}
void creatMatrixNegative_D(int arr[][MAXCOL], int row, int col)
{
    int n = row * col;
    for (int i = 0; i < n; i++)
    {
        arr[i / col][i % col] = (arr[i / col][i % col] > 0 ? (arr[i / col][i % col]) - (arr[i / col][i % col]) : arr[i / col][i % col]);
    }
}
//Đếm số lần ma trận E xuất hiện trong A
void creatNewMatrix_E(int E[][MAXCOL], int& erow, int& ecol)
{
    do {
        cout << "Hay nhap so dong cua ma tran : ";
        cin >> erow;
    } while (erow <= 0);
    do {
        cout << "Hay nhap so cot cua ma tran :";
        cin >> ecol;
    } while (ecol <= 0);
    cout << "Hay nhap gia tri cho ma tran" << endl;
    for (int i = 0; i < erow; i++)
    {
        for (int j = 0; j < ecol; j++)
        {
            cout << "Nhap gia tri cho phan tu [" << i << "," << j << "] := "; cin >> E[i][j];
        }
    }
}

int ESubnetAMatrix(int E[][MAXCOL], int arr[][MAXCOL], int erow, int ecol, int row, int col)
{
    bool flg;
    int dem = 0;
    for (int iarow = 0; iarow <= row - erow; iarow++)
    {
        for (int iacol = 0; iacol <= col - ecol; iacol++)
        {
            flg = true;
            for (int ierow = 0; ierow < erow; ierow++)
            {
                for (int iecol = 0; iecol < ecol; iecol++)
                {
                    if (E[ierow][iecol] != arr[iarow + ierow][iacol + iecol])
                    {
                        flg = false;
                        break;
                    }
                }
                if (flg == false)
                {
                    break;
                }
            }
            if (flg == true)
            {
                dem++;
            }
        }
    }
    return dem;

}
//9.
//Ham tao ma tran F duoc dich phai xoay vong cac cot theo truc dung voi chieu tu trai sang phai
void createTheRightRotationMatrixF(int A[][MAXCOL], int row, int col, int F[][MAXCOL], int k) {

    if (k > 0) {
        int z = 0;
        for (int i = 0; i < row; i++) {
            for (int j = k % col; j < col; j++) {
                F[i][z] = A[i][j];
                z++;
            }
            if (i != row - 1) {
                z = 0;
            }
        }
        if (k % col != 0) {
            int temp = z;
            for (int i = 0; i < row; i++) {
                for (int j = 0; j < k % col; j++) {
                    F[i][z] = A[i][j];
                    z++;
                }
                if (i != row - 1) {
                    z = temp;
                }
            }
        }
    }
    else {
        cout << "Khong the tao ma tran F dich phai xoay vong voi k <= 0" << endl;
    }
}

//10.
//Ham chuyen doi
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
//Ham sap xep mang 1 chieu
void sortintoTheAscendingArray(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (arr[i] > arr[j]) {
                swap(arr[i], arr[j]);
            }
        }
    }
}
//Ham tao ma tran I xoan oc tang dan nguoc chieu kim dong ho
void createTheDescendingSpiralMatrixI(int A[][MAXCOL], int row, int col, int I[][MAXCOL]) {
    //Neu row hoac col == 1 => Khong the co ma tran xoan oc
    if (row != 1 || col != 1) {
        int Temp[MAX];
        int n = row * col;
        for (int i = 0; i < n; i++) {
            Temp[i] = A[i / col][i % col];
        }
        sortintoTheAscendingArray(Temp, n);
        //
        int Top_Limit = 0;
        int Bottom_Limit = row - 1;
        int Left_Limit = 0;
        int Right_Limit = col - 1;
        int i = 0;
        int j = 0;
        int z = 0;
        //
        I[0][0] = Temp[0];
        while (Bottom_Limit >= Top_Limit && Right_Limit >= Left_Limit) {
            //Left to Right 
            if (i == Top_Limit && j < Right_Limit) {
                while (i == Top_Limit && j < Right_Limit) {
                    z++;
                    j++;
                    I[Top_Limit][j] = Temp[z];
                    if (j == Right_Limit) {
                        Top_Limit++;
                    }
                }
            }
            //Top to Bottom 
            else if (i < Bottom_Limit && j == Right_Limit) {
                while (i < Bottom_Limit && j == Right_Limit) {
                    z++;
                    i++;
                    I[i][Right_Limit] = Temp[z];
                    if (i == Bottom_Limit) {
                        Right_Limit--;
                    }
                }
            }//Right to Left
            else if (i == Bottom_Limit && j > Left_Limit) {
                while (i == Bottom_Limit && j > Left_Limit) {
                    z++;
                    j--;
                    I[Bottom_Limit][j] = Temp[z];
                    if (j == Left_Limit) {
                        Bottom_Limit--;
                    }
                }
            }
            //Bottom to Top
            else if (i > Top_Limit && j == Left_Limit) {
                while (i > Top_Limit && j == Left_Limit) {
                    z++;
                    i--;
                    I[i][Left_Limit] = Temp[z];
                    if (i == Top_Limit) {
                        Left_Limit++;
                    }
                }
            }
        }
    }
    else {
        cout << "Khong the tao ma tran xoan oc giam dan !" << endl;
    }
}

//Ham menu
void Menu() {
    cout << "Welcome to Menu !" << endl;
    cout << "0.\tExit" << endl;
    cout << "1.\tTao ma tran nhap tu phim/ngau nhien/doc tu file" << endl;
    cout << "2.\tXuat ma tran" << endl;
    cout << "3.\tTinh gia tri trung binh cong cua 4 bien/duong cheo chinh/duong cheo phu" << endl;
    cout << "4.\t" << endl;
    cout << "5.\t" << endl;
    cout << "6.\t" << endl;
    cout << "7.\tTu ma tran A tao hai ma tran C va D voi C toan duong va D toan am"<< endl;
    cout << "8.\tTim ma tran E trong A" << endl;
    cout << "9.\t" << endl;
    cout << "10.\tTu ma tran A tao ma tran F duoc dich phai xoay vong cac cot theo truc dung voi chieu tu trai sang phai" << endl;
    cout << "11.\tTu ma tran A tao va xuat ra mot ma tran I duoc xoan oc giam dan nguoc chieu kim dong ho" << endl;
}
int main() {
    int A[MAXROW][MAXCOL];
    int E[MAXROW][MAXCOL];
    int F[MAXROW][MAXCOL];//Cau 9
    int I[MAXROW][MAXCOL];//Cau 10
    int row = 0;//So Hang
    int col = 0;//So Cot
    int erow, ecol;
    A[0][0] = 0;
    int option = 0;
    int dem = 0;
    string directory = "Text1.txt";
    int choice;
    int k;
    do {
        system("cls");
        Menu();
        cout << "Hay chon phuong thuc : "; cin >> option;
        system("cls");
        switch (option) {
        case 0:cout << "Goodbye  ! :<" << endl;
            break;
        case 1:
            cout << "1. Tao ma tran bang cach nhap tu ban phim \n2. Tao ma tran voi cac gia tri ngau nhien \n3. Tao ma tran voi cac gia tri cua ma tran duoc lay tu file" << endl;
            cout << "Hay chon 1 tong cac phuong thuc tren (1, 2 or 3) : "; cin >> choice;
            system("cls");
            if (choice == 1) {
                createMatrix(A, row, col);
                system("cls");
            }
            else if (choice == 2) {
                cout << "Tao ma tran ngau nhien" << endl;
                do {
                    cout << "Hay nhap so dong cua ma tran :\t";
                    cin >> row;
                } while (row <= 0);
                do {
                    cout << "Hay nhap so cot cua ma tran :\t";
                    cin >> col;
                } while (col <= 0);
                createRandomMatrix(A, row, col);
                cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" << endl;
                cout << "         Khoi tao gia tri ngau nhien hoan tat !" << endl;
                cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" << endl;
            }
            else if (choice == 3) {
                cout << "Lay gia tri cua ma tran tu file ! \nNhap duong dan : "; cin >> directory;
                createMatrixFromFile(A, row, col, directory);
                cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" << endl;
                cout << "       Lay gia tri cua ma tran tu file hoan tat !" << endl;
                cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" << endl;
            }
            else {
                cout << "Khong hop le !" << endl;
            }
            break;
        case 2:
            displayMatrix(A, row, col);
            break;
        case 3:
            cout << "Tinh gia tri trung binh cong cua :" << endl;
            cout << "1. 4 bien \n2. Duong cheo chinh \n3. Duong cheo phu" << endl;
            cout << "Hay chon 1 tong cac phuong thuc tren (1, 2 or 3) : "; cin >> choice;
            if (choice == 1) {
                cout << "Gia tri trung binh cong cua 4 bien la : " << averageTheEdge(A, row, col) << endl;
            }
            else if (choice == 2) {
                cout << "Gia tri trung binh cong cua duong cheo chinh la : " << averageTheMainDiagonal(A, row, col) << endl;
            }
            else if (choice == 3) {
                cout << "Gia tri trung binh cong cua duong cheo phu la : " << averageTheAuxiliaryDiagonal(A, row, col) << endl;
            }
            else {
                cout << "Khong hop le !" << endl;
            }
            break;
        case 4:
            break;
        case 5:
            break;
        case 6:;
            break;
        case 7:
            creatMatrixPositive_C(A, row, col);
            creatMatrixNegative_D(A, row, col);
            break;
        case 8:
            creatNewMatrix_E;
            dem = ESubnetAMatrix(E, A, erow, ecol, row, col);
            break;
        case 9:
            break;
        case 10:
            cout << "Hay nhap so lan k(k>0) : "; cin >> k;
            while (k <= 0) {
                cout << "Nhap sai, Hay nhap lai so lan k(k>0) : "; cin >> k;
            }
            cout << "Ma tran F da duoc tao" << endl;
            createTheRightRotationMatrixF(A, row, col, F, k);
            displayMatrix(F, row, col);
            break;
        case 11:
            createTheDescendingSpiralMatrixI(A, row, col, I);
            displayMatrix(I, row, col);
            break;
        default:cout << "Phuong thuc khong hop le !! :<" << endl;
        }
        cout << setfill('-');		// set fill ' ' to '-'
        cout << setw(115) << "-" << endl;	// fill 80 char '-'
        // reset fill to ' '
        cout << setfill(' ');
        system("pause>0");
    } while (option != 0);
    return 0;
