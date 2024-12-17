# Sollution
Use map to count the element that occurs in the array before.
if the number equal to 1 mean that if it is the first time this element occur
in the array, it will be print out to the screen.



``` C++
#include <bits/stdc++.h>
using namespace std;

int main() {
    string n;
    getline(cin, n);

    // Split the input into integers
    stringstream ss(n);
    int num, ma=0;
    vector<int> array;
    while (ss >> num) {
        array.push_back(num);
        if (ma < num) ma = num;
    }

    // Use a map to count occurrences
    map<int, int> frequency_map;
    for (int i = 0; i < array.size(); i++) {
        frequency_map[array[i]]++;
        if (frequency_map[array[i]] == 1) cout<<array[i]<<" ";
    }


    return 0;
}
```
# Flag: HTB{r3m0v1ng_dup5_15_s0_345y_1F_y0u_kn0w_h0w_t0_c0d3!_3e2c36d9731eee3ce39d60ee5fd4e1fb}
