# sort_elements
use quicksort to sort array of 10000 elements

// header file

    #include<iostream>
    using namespace std;

    class qSort
    {
    public:
        void print() const;
        int partition(int first, int last);
        void swap(int first, int second);
        void recQuickSort(int first, int last);
        void quickSort();
        void recQuickInsertionSort(int first, int last);
        void quickInsertionSort();
        int choosePivot(int first, int last);
        void insertionSort(int first, int last);
        qSort();
        int piv();

    private:
        int arr[10000];
        int pivotChoice;
    };

// implementation file

    #include "proj4.h"

    qSort :: qSort()
    {
    srand(time(0));

    for(int j = 0 ; j < 10000; j++)
    {
        int i = rand() %1000;
        if(i != i-1)
            arr[j]=i;
        else
        {
            i = rand() % 1001;
            arr[j] = i;
        }
    }
    }

    int qSort :: piv()
    {
    int ans;
    cout << "Press" << endl;
    cout << "1 to use pivot as middle element of the array." << endl;
    cout << "2 to use pivot as median of first, last, and middle elements of the array:" << endl << endl;
    cin >> ans;
    pivotChoice = ans;
    }

    int qSort :: choosePivot(int first, int last)
    {
    int pivot;
    int f = arr[first];
    int l = arr[last];
    int m = arr[(first + last)/2];

    if(pivotChoice == 1)
        pivot = (first + last) / 2;
    else if(pivotChoice == 2)
    {
        if(m < f)
            swap(f, m);
        if(l < f)
            swap(f, l);
        if(l < m)
            swap(m, l);

        swap(m, arr[last-1]);
        return arr[last-1];
    }

      return pivot;
    }



    void qSort::print() const
    {
    for (int i = 0; i < 10000; i++)
        cout << arr[i] << " ";

    cout << endl;
    }

    int qSort :: partition(int first, int last)
    {
    int pivot, index, smallIndex;

    pivot = choosePivot(first, last);
    swap(first, pivot);

    pivot = arr[first];

    smallIndex = first;

    for(index = first+1; index <= last; index++)
        if(arr[index] < pivot)
        {
            smallIndex++;
            swap(smallIndex, index);
        }
    swap(first, smallIndex);

    return smallIndex;
    }

    void qSort :: swap(int first, int second)
    {
    int temp;

    temp = arr[first];
    arr[first] = arr[second];
    arr[second] = temp;
    }

    void qSort :: recQuickSort(int first, int last)
    {
    int pivotLocation, choice;

    if(first < last)
    {
        pivotLocation = partition(first, last);
        recQuickSort(first, pivotLocation - 1);
        recQuickSort(pivotLocation + 1, last);
    }
    }

    void qSort :: quickSort()
    {
    recQuickSort(0, 9999);
    }

    void qSort :: recQuickInsertionSort(int first, int last)
    {
    int pivotLocation;

    if(first < last)
    {
        if((last - first) < 20)
        {
            insertionSort(first, last);
        }
        else
        {
            pivotLocation = partition(first, last);
            recQuickInsertionSort(first, pivotLocation - 1);
            recQuickInsertionSort(pivotLocation + 1, last);
        }
    }
    }

    void qSort :: quickInsertionSort()
    {
    recQuickInsertionSort(0, 9999);
    }

     void qSort :: insertionSort(int first, int last)
    {
    int i, j ,temp;
    for (i = first; i < last; i++)
    {
        j = i;
        while (j > 0 && arr[j - 1] > arr[j])
        {
            temp = arr[j];
            arr[j] = arr[j - 1];
            arr[j - 1] = temp;
            j--;
        }
    }
    }

// main file

    #include<iostream>
    #include<ctime>
    #include<cstdlib>
    #include "proj4_imp.h"
    using namespace std;

    int main()
    {
    int choice;
    unsigned t1, t2;
    qSort myArray;

    cout << "This program uses quicksort to sort an array of 10,000 elements." << endl << endl;

    myArray.piv();
    cout << endl;

    cout << "Press" << endl << "1 to use normal quicksort" << endl;
    cout << "2 to use quicksort and switch to insertion sort" << endl;
    cout << "  when the length of either of the sublists is less than 20." << endl << endl;
    cin >> choice;
    cout << endl;

    t1 = clock();

    if(choice == 1)
        myArray.quickSort();
    else if(choice == 2)
        myArray.quickInsertionSort();
    t2 = clock() - t1;

    double timeUsed = ((double)(t2)) / CLOCKS_PER_SEC;

    cout << "The iteration process took " << timeUsed << " seconds." << endl;

    return 0;
    }

