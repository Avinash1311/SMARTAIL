// Program to identify the smallest distance between the two points in a plane containing 100 points

#include <iostream.h> 
#include <float.h> 
#include <stdlib.h> 
#include <math.h> 

using namespace std;

struct Point 
{ 

    int x, y; 
};  

int compareX(const void* a, const void* b) 
{ 

    Point *p1 = (Point *)a,  *p2 = (Point *)b; 

    return (p1->x - p2->x); 
} 

int compareY(const void* a, const void* b) 
{ 

    Point *p1 = (Point *)a,   *p2 = (Point *)b; 

    return (p1->y - p2->y); 
} 

float dist(Point p1, Point p2) 
{ 

    return sqrt( (p1.x - p2.x)*(p1.x - p2.x) + 

                 (p1.y - p2.y)*(p1.y - p2.y) 

               ); 
} 

float bruteForce(Point P[], int n) 
{ 

    float min = FLT_MAX; 

    for (int i = 0; i < n; ++i) 

        for (int j = i+1; j < n; ++j) 

            if (dist(P[i], P[j]) < min) 

                min = dist(P[i], P[j]); 

    return min; 
} 

float min(float x, float y) 
{ 

    return (x < y)? x : y; 
} 

float stripClosest(Point strip[], int size, float d) 
{ 

    float min = d; 

    for (int i = 0; i < size; ++i) 

        for (int j = i+1; j < size && (strip[j].y - strip[i].y) < min; ++j)

            if (dist(strip[i],strip[j]) < min) 

                min = dist(strip[i], strip[j]); 

  

    return min; 
} 

float closestUtil(Point Px[], Point Py[], int n) 
{ 

    if (n <= 3) 

        return bruteForce(Px, n); 

    int mid = n/2; 

    Point midPoint = Px[mid]; 

    Point Pyl[mid]; 

    Point Pyr[n-mid]; 

    int li = 0, ri = 0;  

    for (int i = 0; i < n; i++) 

    { 

      if (Py[i].x <= midPoint.x && li<mid) 

         Pyl[li++] = Py[i]; 

      else

         Pyr[ri++] = Py[i]; 

    } 

    float dl = closestUtil(Px, Pyl, mid); 

    float dr = closestUtil(Px + mid, Pyr, n-mid); 

    float d = min(dl, dr); 

    Point strip[n]; 

    int j = 0; 

    for (int i = 0; i < n; i++) 

        if (abs(Py[i].x - midPoint.x) < d) 

            strip[j] = Py[i], j++; 

    return stripClosest(strip, j, d); 
} 

float closest(Point P[], int n) 
{ 

    Point Px[n]; 

    Point Py[n]; 

    for (int i = 0; i < n; i++) 

    { 

        Px[i] = P[i]; 

        Py[i] = P[i]; 

    } 

  

    qsort(Px, n, sizeof(Point), compareX); 

    qsort(Py, n, sizeof(Point), compareY);  

    return closestUtil(Px, Py, n); 
} 

int main() 
{ 

    Point P[] = {{2,13}, {12,3}, {4,15}, {5,12}, {12,15}, {3,4}{9,2}, {10,8}, {17,4}, {16,7}, {14,4}, {8,9}, {24,25}, {22,8}, {3,14}, {14,16}, {20,3}, {8,16}, {11,4}, {7,21}, {23,4}, {5,20}, {21,15}, {13,22}, {16,23}, {1,9}, {10,17}, {24,1}, {13,9},{11,21}, {4,5}, {8,3}, {22,24}, {1,5}, {4,9}, {21,3}, {14,2}, {16,3}, {7,17}, {25,20}, {23,9}, {15,14}, {2,2}, {25,5}, {13,1}, {9,4}, {19,21}, {6,10}, {20,12}, {1,1}, {19,6}, {18,22}, {15,9}, {24,17}, {10,12}, {14,25}, {20,22}, {2,5}, {18,13}, {6,17}, {9,9}, {24,12}, {9,16}, {19,14}, {3,9}, {23,13}, {3,1}, {15,22}, {23,23}, {2,7}, {11,19}, {22,6}, {12,21}, {21,24}, {25,7}, {6,3}, {4,2}, {10,14}, {25,12}, {17,15}, {5,9}, {11,12}, {22,16}, {5,3}, {12,25}, {15,18}, {21,16}, {4,12}, {7,4}, {13,12}, {19,20}, {20,24}, {6,14}, {17,9}, {8,24}, {16,8}, {17,21}, {7,9}, {18,1}, {18,8}}; 

    int n = sizeof(P) / sizeof(P[0]); 

    cout << "The smallest distance is " << closest(P, n); 

    return 0; 
}
