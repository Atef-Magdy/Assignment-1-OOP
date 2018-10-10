# Assignment-1-Progaming 2
Overloading Operators on matrices

#include <iostream>

using namespace std;

struct matrix
{
    int** data;
    int row, col;
};

void createMatrix (int row, int col, int num[], matrix& mat);

matrix operator+  (matrix mat1, matrix mat3); // Add if same dimensions
matrix operator-  (matrix mat1, matrix mat3); // Sub if same dimensions
matrix operator*  (matrix mat1, matrix mat3); // Multi if col1 == row2
matrix operator+  (matrix mat1, int scalar);  // Add a scalar
matrix operator-  (matrix mat1, int scalar);  // Subtract a scalar
matrix operator*  (matrix mat1, int scalar);  // Multiple by scalar

matrix operator+= (matrix& mat1, matrix mat2); // mat1 changes & return
matrix operator-= (matrix& mat1, matrix mat2); // mat1 changes + return new
matrix operator+= (matrix& mat, int scalar);   // change mat & return new matrix
matrix operator-= (matrix& mat, int scalar);   // change mat & return new matrix
void   operator++ (matrix& mat);               // Add 1 to each element ++mat
void   operator-- (matrix& mat);    	       // Sub 1 from each element --mat
istream& operator>> (istream& in, matrix& mat);

ostream& operator<< (ostream& out, matrix mat);
bool   operator== (matrix mat1, matrix mat2);
bool   operator!= (matrix mat1, matrix mat2);
bool   isSquare   (matrix mat);
bool   isSymetric (matrix mat);
bool   isIdentity (matrix mat);
matrix transpose(matrix mat);

int main()
{
    matrix mat1 , mat2;
    int num[9]={1,2,3,2,1,6,3,6,1};
    int num2[9]={1,2,3,4,5,6,7,8,9};
    createMatrix(3,3,num,mat1);
    createMatrix(3,3,num2,mat2);

    return 0;
}
void createMatrix (int row, int col, int num[], matrix& mat)
{
    mat.row = row;
    mat.col = col;
    mat.data = new int* [row];
    for (int i = 0; i < row; i++)
        mat.data[i] = new int [col];

    for (int i = 0; i < row; i++)
        for (int j = 0; j < col; j++)
            mat.data[i][j] = num[i * col + j];
}
matrix operator+(matrix mat1, matrix mat3)
{
    matrix mat2;
    mat2.row=mat1.row=mat3.row;
    mat2.col=mat1.col=mat3.col;
    int data[mat2.row*mat2.col];
    createMatrix(mat2.row,mat2.col,data,mat2);
    for(int i=0; i<mat1.row; ++i)
    {
        for(int j=0; j<mat1.col; ++j)
        {
            mat2.data[i][j]=mat1.data[i][j]+mat3.data[i][j];
        }
    }
    return mat2;
}
matrix operator-(matrix mat1, matrix mat3)
{
    matrix mat2;
    mat2.row=mat1.row=mat3.row;
    mat2.col=mat1.col=mat3.col;
    int data[mat2.row*mat2.col];
    createMatrix(mat2.row,mat2.col,data,mat2);
    for(int i=0; i<mat1.row; ++i)
    {
        for(int j=0; j<mat1.col; ++j)
        {
            mat2.data[i][j]=mat1.data[i][j]-mat3.data[i][j];
        }
    }
    return mat2;
}
matrix operator*(matrix mat1, matrix mat2)
{
    matrix mat3;
    mat3.row=mat1.row;
    mat3.col=mat2.col;
    int data[mat3.row*mat3.col];
    createMatrix(mat3.row,mat3.col,data,mat3);
    int sum=0;
    for(int i=0; i<mat3.row; ++i)
    {
        for(int j=0; j<mat3.col; ++j)
        {
            for(int p=0; p<mat2.row; ++p)
            {
                sum+=mat1.data[i][p]*mat2.data[p][j];
            }
            mat3.data[i][j]=sum;
            sum=0;
        }
    }
    return mat3;
}
matrix operator+ (matrix mat1, int scalar)
{
    matrix mat2;
    mat2.row=mat1.row;
    mat2.col=mat1.col;
    int data[mat2.row*mat2.col];
    createMatrix(mat2.row,mat2.col,data,mat2);
    for(int i=0; i<mat1.row; ++i)
    {
        for(int j=0; j<mat1.col; ++j)
        {
            mat2.data[i][j]=mat1.data[i][j]+scalar;
        }
    }
    return mat2;
}
matrix operator-(matrix mat1, int scalar)
{
    matrix mat2;
    mat2.row=mat1.row;
    mat2.col=mat1.col;
    int data[mat2.row*mat2.col];
    createMatrix(mat2.row,mat2.col,data,mat2);
    for(int i=0; i<mat1.row; ++i)
    {
        for(int j=0; j<mat1.col; ++j)
        {

                mat2.data[i][j]=mat1.data[i][j]-scalar;
        }
    }
    return mat2;
}
matrix operator*  (matrix mat1, int scalar)
{
    matrix mat2;
    mat2.row=mat1.row;
    mat2.col=mat1.col;
    int data[mat2.row*mat2.col];
    createMatrix(mat2.row,mat2.col,data,mat2);
    for(int i=0; i<mat1.row; ++i)
    {
        for(int j=0; j<mat1.col; ++j)
        {
            mat2.data[i][j]=scalar*mat1.data[i][j];
        }
    }
    return mat2;

}

//----------------------------------------------------------------------

matrix operator+= (matrix& mat1, matrix mat3)
{
    for(int i=0; i<mat1.row; ++i)
    {
        for(int j=0; j<mat1.col; ++j)
        {
            mat1.data[i][j]+=mat3.data[i][j];
        }
    }
    return mat1;
}
matrix operator-= (matrix& mat1, matrix mat2)
{
    for(int i=0; i<mat1.row; ++i)
    {
        for(int j=0; j<mat1.col; ++j)
        {
            mat1.data[i][j]-=mat2.data[i][j];
        }
    }
    return mat1;
}
matrix operator+= (matrix& mat, int scalar)
{
    for(int i=0; i<mat.row; ++i)
    {
        for(int j=0; j<mat.col; ++j)
        {
            if(i==j)
                mat.data[i][j]+=scalar;
        }
    }
    return mat;
}
matrix operator-= (matrix& mat, int scalar)
{
    for(int i=0; i<mat.row; ++i)
    {
        for(int j=0; j<mat.col; ++j)
        {
            if(i==j)
                mat.data[i][j]-=scalar;
        }
    }
    return mat;
}
void operator++ (matrix& mat)
{
    for(int i=0; i<mat.row; ++i)
    {
        for(int j=0; j<mat.col; ++j)
        {
            mat.data[i][j]++;
        }
    }
}
void operator-- (matrix& mat)
{
    for(int i=0; i<mat.row; ++i)
    {
        for(int j=0; j<mat.col; ++j)
        {
            mat.data[i][j]--;
        }
    }
}
istream& operator>> (istream& in, matrix& mat)
{
    for(int i=0; i<mat.row; ++i)
    {
        for(int j=0; j<mat.col; ++j)
        {
            in>>mat.data[i][j];
        }
    }
    return in;
}

//--------------------------------------------------------

ostream& operator<< (ostream& out, matrix mat){
    for(int i=0 ; i<3 ;i++){
        for(int j=0 ; j<3 ; j++){
            out << mat.data[i][j] << " ";
        }
        out << endl;
    }
 return out;
}
bool operator== (matrix mat1, matrix mat2){
    int check=0;
    if((mat1.row==mat2.row) && (mat1.col==mat2.col)){
        for(int i=0 ; i<mat1.row ; i++){
            for(int j=0 ; j<mat1.col ; j++){
                if(mat1.data[i][j] != mat2.data[i][j]) check+=1;
    }}
    }
    else{
        cout << "The two matrices not have the same row and col" << endl;
        check=1;
    }
    return !check;
}
bool operator!= (matrix mat1, matrix mat2){
    int check=0;
    if((mat1.row==mat2.row) && (mat1.col==mat2.col)){
        for(int i=0 ; i<mat1.row ; i++){
            for(int j=0 ; j<mat1.col ; j++){
                if(mat1.data[i][j] != mat2.data[i][j]) check+=1;
    }}
    }
    else{
        cout << "The two matrices not have the same row and col" << endl;
        check=1;
    }
    return check;

}
bool isSquare   (matrix mat) {
    if (mat.row == mat.col) return true;
    else return false;
}
bool isSymetric (matrix mat){

    if(mat == transpose(mat)) return true;
    else return false;

}
bool isIdentity (matrix mat){
    if(!isSquare(mat)) return false;
    for(int i=0 ; i< mat.row ; i++){
                if(mat.data[i][i]!=1) return false;
        }
        return true;
}
matrix transpose(matrix mat){
    matrix trans;
    int arr[mat.col * mat.row];
    createMatrix((mat.col),(mat.row) , arr , trans);
    for(int i=0 ; i<(mat.row) ; i++){
        for(int j=0 ; j<(mat.col) ; j++)
            trans.data[j][i] = mat.data[i][j];
    }
    return trans;
}
